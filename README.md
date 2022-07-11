# Mockito

## Stub Test

```js
public class TodoBusinessImplStubTest {

	@Test
	public void testRetriveTodosRelatedToSpring_usingStub() {
		TodoService todoServiceStub = new 
				TodoServiceStub();
		TodoBusinessImpl todoBusinessImpl = new 
				TodoBusinessImpl(todoServiceStub);

		List<String> filtered = todoBusinessImpl
				.retrieveTodosRelatedToSpring("Dummy");

		assertEquals(2, filtered.size());
	}

}
```

## Mocking List Interface

```js
public class ListTest {

	// return single value
	@Test
	public void letsMockListSizeMethod() {
		List listMock = mock(List.class);
		when(listMock.size()).thenReturn(2);

		assertEquals(2, listMock.size());
	}

	// return multiple values sequentially
	@Test
	public void letsMockListSizeMethod_ReturnMultipleValues() {
		List listMock = mock(List.class);
		when(listMock.size()).thenReturn(2).thenReturn(3);

		assertEquals(2, listMock.size());
		assertEquals(3, listMock.size());
	}

	@Test
	public void letsMockListGetMethod_ReturnMultipleValues() {
		List listMock = mock(List.class);
		when(listMock.get(0)).thenReturn("djsk");

		assertEquals("djsk", listMock.get(0));

		// non stubbed method returns default value
		assertEquals(null, listMock.get(1));
	}

	@Test
	public void letsMockListGetMethod_ReturnMultipleValuesArgumentMatcher() {
		List listMock = mock(List.class);

		// Argument Matcher
		when(listMock.get(anyInt())).thenReturn("djsk");

		assertEquals("djsk", listMock.get(0));

	}

	@Test(expected = RuntimeException.class)
	public void letsMockList_ThrowException() {
		List listMock = mock(List.class);

		// Argument Matcher
		when(listMock.get(anyInt())).thenThrow(new RuntimeException("Something"));

		listMock.get(0);
	}
}
```

## Mockito Basics

```js
public class TodoBusinessImplMockTest {

	@Test
	public void testRetriveTodosRelatedToSpring_usingMock() {
		TodoService todoServiceMock = mock(TodoService.class);
		
		List<String> todos = Arrays.asList("Learn Spring MVC", "Learn Spring", "Learn to Dance");
		given(todoServiceMock.retrieveTodos("Dummy")).willReturn(todos);

		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoServiceMock);

		List<String> filtered = todoBusinessImpl.retrieveTodosRelatedToSpring("Dummy");

		assertEquals(2, filtered.size());
	}

	@Test
	public void testRetriveTodosRelatedToSpring_usingMockBDD() {
		// Given
		TodoService todoServiceMock = mock(TodoService.class);
		
		List<String> todos = Arrays.asList("Learn Spring MVC", "Learn Spring", "Learn to Dance");
		given(todoServiceMock.retrieveTodos("Dummy")).willReturn(todos);

		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoServiceMock);

		// When
		List<String> filtered = todoBusinessImpl.retrieveTodosRelatedToSpring("Dummy");

		// Then
		assertThat(filtered.size(), is(2));
	}

	// Verify
	@Test
	public void testDeleteTodosNotRelatedToSpring_usingMockBDD() {
		// Given
		TodoService todoServiceMock = mock(TodoService.class);
		
		List<String> todos = Arrays.asList("Learn Spring MVC", "Learn Spring", "Learn to Dance");
		given(todoServiceMock.retrieveTodos("Dummy")).willReturn(todos);

		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoServiceMock);

		// When
		todoBusinessImpl.deleteNotRelatedToSpring("Dummy");

		// Then
		// verify(todoServiceMock).deleteTodo("Learn to Dance");
		then(todoServiceMock).should().deleteTodo("Learn to Dance");

		// verify(todoServiceMock, never()).deleteTodo("Learn Spring");
		then(todoServiceMock).should(never()).deleteTodo("Learn Spring");

		verify(todoServiceMock, times(1)).deleteTodo("Learn to Dance");
	}
	
	// capturing passed arguments
	@Test
	public void testDeleteTodosNotRelatedToSpring_CaptureArguments() {
		// Step 1 : Declare Argument Captor
		ArgumentCaptor<String> stringArgumentCaptor = ArgumentCaptor.forClass(String.class);

		// Given
		TodoService todoServiceMock = mock(TodoService.class);
		
		List<String> todos = Arrays.asList("Learn Spring MVC", "Learn Spring", "Learn to Dance");
		given(todoServiceMock.retrieveTodos("Dummy")).willReturn(todos);

		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoServiceMock);

		// When
		todoBusinessImpl.deleteNotRelatedToSpring("Dummy");

		// Then

		// Step 2 : Define Argument captor on specific method call
		then(todoServiceMock).should().deleteTodo(stringArgumentCaptor.capture());

		// Step 3 : Capture the argument
		assertThat(stringArgumentCaptor.getValue(), is("Learn to Dance"));
	}
	
	@Test
	public void testDeleteTodosNotRelatedToSpring_CaptureArgumentsMultipleTimes() {
		// Step 1 : Declare Argument Captor
		ArgumentCaptor<String> stringArgumentCaptor = ArgumentCaptor.forClass(String.class);

		// Given
		TodoService todoServiceMock = mock(TodoService.class);
		
		List<String> todos = Arrays.asList("Learn Rock n Roll", "Learn Spring", "Learn to Dance");
		given(todoServiceMock.retrieveTodos("Dummy")).willReturn(todos);

		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoServiceMock);

		// When
		todoBusinessImpl.deleteNotRelatedToSpring("Dummy");

		// Then

		// Step 2 : Define Argument captor on specific method call
		then(todoServiceMock).should(times(2)).deleteTodo(stringArgumentCaptor.capture());

		// Step 3 : Capture the argument
		
		// it will fail
		// assertThat(stringArgumentCaptor.getValue(), is("Learn to Dance"));
		
		assertThat(stringArgumentCaptor.getAllValues().size(), is(2));
	}
}
```
## Hamcrest
```js
import static org.junit.Assert.*;

import java.util.Arrays;
import java.util.List;

import org.junit.Test;
import static org.hamcrest.Matchers.*;

public class Hamcrest_matcher {

	@Test
	public void test() {
		List<Integer> scores = 
				Arrays.asList(99, 100, 101,105);
		
		assertThat(scores, hasSize(4));
		assertThat(scores, hasItems(99, 100));
		assertThat(scores, everyItem(greaterThan(90)));
		assertThat(scores, everyItem(lessThan(920)));
		
		assertThat("", isEmptyString());
		assertThat(null, isEmptyOrNullString());
		
		Integer [] marks = {1,2,3};
		assertThat(marks, arrayWithSize(3));
		assertThat(marks, arrayContaining(1,2,3));
	}

}
```

## Annotations
```js
	@Rule
	public MockitoRule mockitoRule = MockitoJUnit.rule();
	
	@Mock
	TodoService todoServiceMock;
	
	@InjectMocks
	TodoBusinessImpl todoBusinessImpl;
	
	@Captor
	ArgumentCaptor<String> stringArgumentCaptor;
```

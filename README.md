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

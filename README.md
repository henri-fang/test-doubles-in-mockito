# test-doubles-in-mockito


## Dummy

Dummy is simple of all. It’s a placeholder required to pass the unit test.
```
UserService dummyUserService = mock(UserService.class);
```

## Fake

Fake is used to simplify a dependency so that unit test can pass easily.
A Fake is an object that has a concrete implementation that works similar to the actual implementation. It is a 
simplified version of production code.

An example could be using AWS S3 in production but adding a fake that saves files to disk for testing purposes.

```
class FakeUserRepository extends UserRepository {

@override
public User findOne(String userId){
return new User(userId, "fakeUserName");
}

}
```

## Spy

When using spy objects, since it is a real object with real methods, when you are not stubbing the method, then it will 
call the real method behavior. If you want to change and mock the method, then you need to stub it.

We should use spies where we don't care about the return values of functions but want to verify what functions were 
called, with what arguments, when, and how often.


```
UserService userService = new UserServiceImpl();
UserService spyUserService = Mockito.spy(userService);  
```
or annotation
```
@Spy
UserService userService = new UserServiceImpl();
```

Example:
```
@Test
public void whenStubASpy_thenStubbed() {
    List<String> list = new ArrayList<String>();
    List<String> spyList = Mockito.spy(list);

    assertEquals(0, spyList.size());//no stub

    Mockito.doReturn(100).when(spyList).size();//stub method size()
    assertEquals(100, spyList.size());// verify the stub
}
```

## Mock

When using mock objects, it gives you the full control over the behavior of the mocked objects. the default behavior of 
the method when not stub is do nothing.
Mock is similar to a stub, but the behaviour of the mocked interface can be changed dynamically based on scenarios.
A mock can be used anywhere we use a stub, but mostly be used for behavior verification.

```
UserService mockUserService = mock(UserService.class);  
```
or annotation
```
@Mock
```

## Stub

A stub is an object that provides (canned) hardcoded values to method calls. It always returns the same output 
regardless of the input.
Instead of the actual object, we can introduce a stub and define what data should be returned, just like Repository:
```
when(gradeRepository.findGrades(studentId)).thenReturn(List.of(1, 2, 3));
```
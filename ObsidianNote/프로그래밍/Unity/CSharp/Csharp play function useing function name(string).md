#language/CSharp 

### string으로 함수실행 시키는 법

[stack overflow](https://stackoverflow.com/questions/540066/calling-a-function-from-a-string-in-c-sharp)

내용:
```Csharp
Type thisType = this.GetType();
MethodInfo theMethod = thisType.GetMethod(TheCommandString);
theMethod.Invoke(this, userParameters);
```

함수는 Public이여야한
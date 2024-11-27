
에디터에서 선택지 나오게 하는 기능

```C#
NameButtonSetup script = (NameButtonSetup)target;  
  
  
string[] options = new string[] { "Option A", "Option B", "Option C" };  
//과거 선택된 옵션 인덱스가져오기
int selectedIndex = System.Array.IndexOf(options, script.selectoption);  

//현제 선택도니 옵션 가지고오기
selectedIndex = EditorGUILayout.Popup("Select Option", selectedIndex, options);  

//코드에 값 설정
script.selectoption = options[selectedIndex];

```

다른 방식으로는 enum에서 값을 받아서   Editor class에서 사용하는 방법이 있다.

```c#
switch (script.mode)  
{  
    case NameButtonSetup.Mode.ModeA:   
        GUILayout.Button("test");  
        break;  
      
    case NameButtonSetup.Mode.ModeB:   
        GUILayout.Button("test2");  
        break;  
}
```

이런식으로 조건에 따라 UI추가 랜더링하는 기능이 있다.

#language/CSharp #unity  
## 쓸만한 Attribute 정리

### Header
인스펙터창에다가 제목을 달수있다 구분할때 주로 쓰인다
```CSharp
[Header("Headername")]
```

### Range
변수값을 해당범위 안으로 조정시키는 바가생긴다
```CSharp
[Range(0f,1f)]
```

### CreateAssetMenu
scriptable object를 만들때 사용한다
```CSharp
[CreateAssetMenu(fileName = "파일이름", menuName = "위치 / atttribute이름")]
```
scriptable object를 사용하고싶으면 ScriptableObject를 상속받아야한다. [[Unity scriptable object]]

### SerializeField
해당 변수를 직렬화 시키고 인스펙터에서 public처럼 보이게한다.
```CSharp
[SerializeField]
```

### System.Serializable
해당클레스를 인스펙터에서 리스트로써 조정할수잉ㅆ다
```CSharp
[System.Serializable]
public class type {}
```
#unity #optimization #scriptableObject
[관련내용 중간부분부터 봐야함](https://ansohxxn.github.io/unity%20lesson%203/ch5-1/)

데이터를 저장하는데 사용할수있는 데이터 컨테이너 에셋

프리펩을 사용해서 오브젝트를 찍어낼경우

오브젝트A1 - 데이터1
오브젝트A2 - 데이터1
오브젝트A3 - 데이터1
오브젝트B1 - 데이터2
오브젝트B2 - 데이터2
이렇게 각각 데이터 하나당 오브젝트 하나에 배치되는데
같은 데이터가 각각 3번 2번 비효율적에게 여러번 사용했다.

하지만 스크립터블 오브젝트를 사용하면
오브젝트A1 - \
오브젝트A2 -  - 데이터1
오브젝트A3 - /
오브젝트B1 - \
오브젝트B2 -  - 데이터2
이렇게 하나를 참조해서 사용해서 사용한다 따라서 메모리를 아낄수있다 = 최적화
사본들이 여러게 생성되는것 방지


스크립터블오브젝트를 만드는 방법이다
CreateAssetMenu를 사용해서 만들수있다
```csharp
[CreateAssetMenu(fileName = "New Item", menuName = "New Item/item")]
public class something: ScriptableObject{

}
```
menuName 에 다가 에디터에서 사용될 create안에 잇는 곳에 버튼을 만들수있다
ex) 우클릭 > create > New Item > item
그후 생성된오브젝트의 이름이 New Item이 된다.
또한 fileName  이 type의 이름이 된다


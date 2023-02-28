#unity #CSharp #TextMeshPro #assemblies #assembly
해당문제는
내가 파악했을때는 다른 어셈블리 파일로 묶여 있어서 그런거 같다
이것이 발생하는것은 UnitTest를 하기위해 어셈블리 레퍼런스를 사용한경우 발생할수있다.

이걸 해결하기위해서는 자신의 스크립트가 있는 파일에
어셈블리파일에 > Assembly Definition References에 TextMeshPro.dll 어셈블리 파일을 추가시켜줘야한다

아마도 이것이 발생한이유는 TextMeshPro가 import해서 쓰는거여서 외부 기능이여서
unity기본기능은 다른 어셈블리 파일에 묶여도 사용가능한데 TextMeshPro사용할수있게 참조해야하는거 같다.
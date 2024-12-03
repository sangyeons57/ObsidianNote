script는 Ediotr에 영향을 받는 클래스

```C#
script.selectoption = EditorGUILayout.TextField("Enter Name", script.selectoption);
```
텍스트 인풋
```C#
script.health = EditorGUILayout.FloatField("Health", script.health);
```
int 인풋
```c#
script.health = EditorGUILayout.FloatField("Health", script.health);
```
float인풋
```C#
script.position = EditorGUILayout.Vector3Field("Position", script.position);
```
Vector3인풋
```C#
script.isActive = EditorGUILayout.Toggle("Is Active", script.isActive);
```
toggle인풋
```C#
script.speed = EditorGUILayout.Slider("Speed", script.speed, 0f, 100f);
```
slider형태 인풋
```C#
script.difficulty = (MyComponent.Mode)EditorGUILayout.EnumPopup("Difficulty", script.difficulty);
```
popup인풋

```C#
serializedObject.ApplyModifiedProperties()
```
이 메서드는 편집된 값을 실제 클레스에 적용해주는 역할을 한다.
SeriallizeObject는 원본객체의 데이터를 수정하고 즉시 반영하지 않고 버퍼에 저장하는데
해당 메서드를 호출하면 실제 객체에 적용한다.

- Undo/Redo시스탬에 반영된다.
- 성능최적화
- 데이터 동기화
등의 이유를 해당 메서드를 값변경후 호출해야한다.


### Player Boundary
사용자를 특정 조건에 따라 입장 불가의 경계를 만들어내는 기능

#### Inspcetor
Auto Boundary Mode: 자동으로 
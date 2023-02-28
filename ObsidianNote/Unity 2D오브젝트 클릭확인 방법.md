#unity #CSharp 

```CSharp
Vector2 mousePosition = Camera.main.ScreenToWorldPoint(Input.mousePosition);
hit = Physics2D.Raycast(mousePosition, Vector2.zero);

if (hit.collider == collider && Input.GetMouseButtonUp(0))
{
	//실행할 코드
}
```
hit.colllider == collider는  instantiate로 생성한경우
한오브젝트를 클릭하면 다른 instance에도 클릭되지않았는데 실행되는 부분을 막기위해존재한다.
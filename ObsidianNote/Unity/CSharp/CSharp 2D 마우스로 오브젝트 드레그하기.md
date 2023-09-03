
```CSharp

public class game : MonoBehaviour {

	Vector2 difference = Vector2.zero;

	private void OnMouseDown()
	{
		difference = (vector2)Camera.main.ScreenToWorldPoint(Input.mousePosition) - (Vector2)tranfrom.position;
	}

	private void OnMouseDrag()
	{
		transform.position = (Vector2)Camera.main.ScreenToWorldPoint(Input.mousePosition) - difference;
	}
}

```

또한 컴포넌트에 콜라이더(collider) 를 추가해야한다.


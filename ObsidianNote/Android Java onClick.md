#And #Java #onClick 
**Q**
android java 에서 onclick은 왜사용하지 말라는 거야?

**A**
`onClick` 메소드는 개발자가 뷰 요소(예를 들어, 버튼)에서 사용자가 클릭 행위를 감지하고 이를 처리하는 방법으로 자주 사용됩니다.

하지만, 개발자는 이를 사용하지 말아야 하는 경우도 있습니다. 예를 들어, 액티비티에서 많은 수의 onClick 메소드가 있을 경우 , 액티비티 클래스의 코드가 복잡해지고 유지보수가 어려워질 수 있습니다.

대신, 개발자는 액티비티에서 뷰 요소의 onClick 속성을 설정하지 말고, 버튼에 대한 클릭 이벤트를 처리하는 데 사용되는 리스너 객체를 사용하십시오. 이는 코드의 유지보수와 가독성을 향상시켜주고, 액티비티의 책임을 분리하여 더 깔끔하고 간결한 코드를 작성할 수 있도록 도와줍니다.

### 정리
onClick 이 많아지면 클레스의 코드가 복잡해진다 따라서 이벤트 리스너 방식을 지향해야한다

---
**Q**
그러면 리스너 객체 사용방법에 대해 설명해줘

**A**
Android Java에서 "onClick"을 사용하지 말라는 것은 권장일 뿐, 강제하지는 않습니다. 대신 버튼 클릭에 대한 이벤트 처리를 하기 위해서는 View.OnClickListener 인터페이스를 구현한 리스너 객체를 사용하는 것이 권장됩니다.

리스너 객체 사용 방법은 다음과 같습니다.

1.  리스너 객체를 생성합니다.

javaCopy code

```Java
View.OnClickListener listener = new View.OnClickListener() {
	@Override
	public void onClick(View view) {
		// 처리할 로직 작성 
	}
};
```

2.  버튼에 리스너 객체를 등록합니다.

javaCopy code

```Java
Button button = findViewById(R.id.button); button.setOnClickListener(listener);
```

이렇게 하면 버튼이 클릭될 때마다 `onClick` 메소드가 호출되어 버튼 클릭 이벤트에 대한 처리를 할 수 있습니다.

### 정리
```Java
Button button = findViewById(R.id.button);
button.setOnClickListener(new View.OnClickListener() {
	public void onClick(View view) {
		//이벤트 코드
	}
});
```



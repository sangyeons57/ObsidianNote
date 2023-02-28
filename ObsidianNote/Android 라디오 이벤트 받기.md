#Android #Java #radio

**Q**
라디오 그룹에서 받는 이벤트 다 말해봐
**A**
안드로이드 자바에서 라디오 그룹에서 이벤트를 받는 방법에 대해 알려드리겠습니다.

1.  RadioGroup에 OnCheckedChangeListener 추가: RadioGroup 객체에 OnCheckedChangeListener를 추가하여 라디오 그룹의 선택이 변경될 때마다 이벤트를 받을 수 있습니다.
```Java
RadioGroup radioGroup = (RadioGroup) findViewById(R.id.radioGroup);
radioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(RadioGroup group, int checkedId) {
        // 라디오 그룹의 선택이 변경될 때 호출되는 메서드
    }
});
```

2.  RadioButton에 OnClickListener 추가: 각 RadioButton에 OnClickListener를 추가하여 RadioButton이 클릭될 때마다 이벤트를 받을 수 있습니다.
```Java
RadioButton radioButton1 = (RadioButton) findViewById(R.id.radioButton1);
radioButton1.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        // RadioButton이 클릭될 때 호출되는 메서드
    }
});

```
이 두 가지 방법 모두 RadioButton의 상태 변경에 대한 이벤트를 받을 수 있습니다. 어떤 방법을 사용할지는 앱의 구조와 기능에 따라 결정하시면 됩니다.

#### 결론
위2가지 방법으로 라디오 이벤트를 받을수 있는데
1. 위방법은 그룹에서 라디오 버튼의 클릭의 변화를 체크하는거고
2. 아레방법은 각각 라디오 버튼에 이벤트를 만들어서 각 라디오버튼이 클릭 됬는지 확인하는거다

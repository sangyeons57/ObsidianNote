
[[http 통신]]
예제
```html
<form action="submit_form.php">
```
action: 폼 데이터를 서버로 전송할떄 사용할 URL

```HTML
<form method="post">
```
method: HTTP 메서드를 지정 일반적으로 GET과 POST가 있다.

```HTML
<form enctype="">
```
폼 데이터를 서버로 전송할떄 이코딩 타입

```HTML
<form target="_blank">
```
제출 결과를 표시할 위치
- \_self : 현제 창 (기본 값)
- \_blank: 새 창
- \_parent: 부모 프레임
- \_top : 최상위 프레임

### form 내부에서 사용되는 관련 태그 요소

input: 입력 필드 생성
```HTML
<input type="text" name="username" placeholder="Enter your username" required>
```
- type: 입력 필드의 타입 지정
- name: 서버로 전송될떄의 필드의 이름
- value: 입력 필드의 초기값 설정
- placeholder: 힌트값 설정

- required: 필수 입력 필드 지정
- readonly: 읽기 전용 필드 지정
- disabled: 입력 불가능 필드 지정


label: 이력 필드에 대한 설명 제공 + 선택시 포커스 이동
``` HTML
<label for ="username">Username:</label>
```
for: 연결할 입력 필드의 ID 값지정 (선택시 해당 입력 필드로 이동)


textarea: 텍스트 입력 필드 생성
```HTML
<textarea name="comments" rows="4" cols="50"></textarea>
```
row, columns: 초기 행렬의 크기를 설정한다.


select, option: 드롭다운 목록 생성
```HTML
<select name="choices" multiple>
    <option value="option1">Option 1</option>
    <option value="option2">Option 2</option>
    <option value="option3">Option 3</option>
</select>
```

mutiple: 다중선택 허용 기능

### 중요
Html form을 전성하기 위해서는
`<input type="submit">` 또는 `<button type="submit">` 요소가 필요하다.
document.getElementById("myForm").submit(); 이런식으로  javascriopt에서 전송하는것도 가능하긴 하다.

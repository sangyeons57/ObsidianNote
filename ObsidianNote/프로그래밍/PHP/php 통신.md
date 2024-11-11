```php
<?php
header();
/**
header()는 서버가 클리아이언트 에게 응답하기 위해서 사용된다.
콩텐츠 유형 설정, 캐싱제어, 강제화. 등 다양한 작업을 할수잇다.
*/

$_SERVER[]  //클리아언트로 부터 받은 데이터를 처리하기 위해 사용된다.
json_decode(file_get_contents('php://input'),true);
// php://input 은 전체 데이터를 받기위해 사용한다.
// 위처럼 2개의 메서드 까지 합하면 파일
file_get_contents() // 전체 파일을 String으로 읽는 메서드
?>
```



mysql 데이터 읽기 밑 저장을 위해서
```PHP
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "example_db";

$conn = new mysqli($servername, $username, $password, $dbname);
if($conn->connect_error){
 echo "Error";
}
```
이 사용된다.

sql  쿼리 사용방법
읽기
```PHP
$sql = "SELECT * FROM test_table";
$result = $conn->query($sql);

if ($result->num_rows > 0){
	while($row = $result->fetch_assoc()) {
		echo "id: " . $row["id"]. " - Name: " . $row["username"]. " - Email: " . $row["email"]. "<br>"; 
	}
}
```
$result ->fetch_assoc()
는 연관 배열을 반환하는데 만약에 값이 없다면 false를 반환해서 반복을 종료한다.
는 값을


쓰기
```PHP
$sql = "SELECT * FROM test_table";
$result = $conn->query($sql);
```
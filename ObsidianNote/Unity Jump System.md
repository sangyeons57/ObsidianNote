#CSharp #unity 

```CSharp

bool isGround

void Update()
{
isGround = Physics2D.OverlapCircle(
	(Vector2)transform.position +
	 new Vector2(0, -0.5f), 0.07f,
	  1 << LayerMask.NameToLayer("Ground"));

if ( isGround &&  (Input.GetKeyDown(KeyCode.W) ||
	 Input.GetKeyDown(KeyCode.UpArrow)
	)) pv.RPC("JumpRPC", RpcTarget.All);
}
```

OverlapCircle(위치, 크기, 필터)
필터는 레이어 번호인데
0 : 필터없음
1: 0번레이어 default
2: 1번레이어
4: 2번레이어
8: 3번레이어
16: 4번레이어

이런식으로 2배식 늘어난다
따라서 5번레이어를 체크하고 싶으면 16을써야한다.
1 << LayerMask.NameToLayer("Groud") 는 이걸 표현한거다
Ground레이어의 번호가 들어오면
비트연산을 해서 자동으로 2배씩 계산해준다

pv.RPC() 는
[[Unity Photon]]에서쓰는 서버전체 캐릭터점프이다
#unity #CSharp #Photon2

[튜토리얼 한국어 영상](https://www.youtube.com/playlist?list=PL3KKSXoBRRW3YE4UMnRH762vOhSHLdnpK)

[포톤서버](https://dashboard.photonengine.com/)
에서 CREATE A NEW APP 을하고 App Id를 복사한다

unity 에서 Photon Asset 을 import하고 

App Id를 생성해준다

---
MonoBehaviourPunCallbacks
#MonoBehavour대신에 쓰는고 다양함 함수를 override해서 쓰기위해 사용한다.

```CSharp
PhotonNetwork.NetworkClientState.ToString();
//내트워크 연결 상태를 준다
PhotonNetwork.LocalPlayer.NickName =
//이걸로 클라이언트 닉네임을 설정한다.
//OnConnectedToMaster에서 상용한다.


PhotonNetwork.ConnectUsingSettings();
//connect연결
public override void OnConnectedToMaster() {}
//connect되면실핼하는함수


PhotonNetwork.Disconnect();
//열결끊기
public override void OnDisconnected() {}
//연결끊길때 실행


PhotonNetwork.JoinLobby();
//로비접속
public override void OnJoinedLobby() {}
//로비 접속했을대 실행

```
방을 만들거나 참가할때는 connect되어있던가 Lobby에 있어야한다.

```CSharp
PhotonNetwork.CreateRoom(roomName, new RoomOptions{Maxlayers = 2}, null);
//룸을 만든다 Room Options밑에 설명
//만드는데성공한경우 OnCreatreRoom, OnJoinedRoom을 실행한다.

PhotonNetwork.JoinRoom(roomName);
//rooname으로 방참가
PhotonNetwork.JoinOrCreateRoom(roomName, new RoomOptions { MaxPlayers = 2}, null);
//방이없다면 방을만들고, 있다면 참가한다.
PhotonNetwork.JoinRandomRoom();
//랜덤한 룸에 입장
PhotonNetwork.LeaveRoom();
//룸떠나기
//룸에서떠나면 로비가아닌 connect상태로 들어간다.


public override void OnCreatedRoom()
//방이 만들어진 경우
public override void OnJoinedRoom()
//방이 들어가진 경우

public override void OnCreatedRoomFailed()
//방이 만들어진걸 실패한 경우
public override void OnJoinedRoomFailed()
//방이 들어가진걸 실패한 경우
public override void OnJoinedRandomFailed()
//랜덤방 참가 실패

```
RoomOption 들
[MaxPlayers](https://doc-api.photonengine.com/en/pun/v2/class_photon_1_1_realtime_1_1_room_options.html#afe8e03921d811f4fe536cfd9a71dd9bb)
[PlayerTtl](https://doc-api.photonengine.com/en/pun/v2/class_photon_1_1_realtime_1_1_room_options.html#a209447cfcc1477298f53bd59d7771e88)
[EmptyRoomTtl](https://doc-api.photonengine.com/en/pun/v2/class_photon_1_1_realtime_1_1_room_options.html#a01b2d365fd7a9fea82a9dc1e055801c9)
[CustomRoomProperties](https://doc-api.photonengine.com/en/pun/v2/class_photon_1_1_realtime_1_1_room_options.html#acf4d20de1d1a094f4b5253acdf5d9ce0)
[CustomRoomPropertiesForLobby](https://doc-api.photonengine.com/en/pun/v2/class_photon_1_1_realtime_1_1_room_options.html#a9664878066f389ecb8979c636ac78d32)
[Plugins](https://doc-api.photonengine.com/en/pun/v2/class_photon_1_1_realtime_1_1_room_options.html#aab3dbb987830f0c38b167d8b5de6a08f)

### Room info
```CSharp
[ContextMenu("정보")]
void Info()
{

if (PhotonNetwork.InRoom)
{
	Debug.Log("현재 방 이름: " + PhotonNetwork.CurrentRoom.Name);
	Debug.Log("현재 방 인원수: " + PhotonNetwork.CurrentRoom.PlayerCount);
	Debug.Log("현재 방 최대인원수: " + PhotonNetwork.CurrentRoom.MaxPlayers);

	string playerStr = "방에 있는 플레이어 목록: ";
	for (int i = 0; i < PhotonNetwork.PlayerList.Length; i++)
		playerStr += PhotonNetwork.PlayerList[i].NickName + ", ";
	Debug.Log(playerStr);
}
else
{
	Debug.Log("접속한 인원수: " + PhotonNetwork.CountOfPlayers);
	Debug.Log("방 개수: " + PhotonNetwork.CountOfPlayers);
	Debug.Log("모든 방에 있는 인원 수: " + PhotonNetwork.CountOfPlayersInRooms);
	Debug.Log("로비에 있는지: " + PhotonNetwork.InLobby);
	Debug.Log("연결됐는지: " + PhotonNetwork.IsConnected);
}

}
```
ContextMenu("정보")
이렇게 하면 networkManeger에서 우클릭해서 "정보"를 클릭해 실행할수있다.


### falied되었을때 returnCode
**Common errors:**  
-   OperationNotAllowedInCurrentState = -3 (rare)
-   InvalidOperation = -2 (should not happen in PUN)
-   InternalServerError = -1 (rare)

  
**CreateRoom operation / OnCreateRoomFailed:**  
-   ServerFull = 32762 (rare)
-   GameIdAlreadyExists = 32766
-   PluginReportedError = 32752 (in case you use server plugin)
-   PluginMismatch = 32751 (in case you use server plugin)
-   SlotError = 32742 (in case you use ExpectedUsers)

  
**JoinRoom operation / OnJoinRoomFailed:  
-   GameFull = 32765
-   GameClosed = 32764
-   GameDoesNotExist = 32758
-   PluginReportedError = 32752 (in case you use server plugin)
-   PluginMismatch = 32751 (in case you use server plugin)
-   JoinFailedPeerAlreadyJoined = 32750
-   JoinFailedFoundInactiveJoiner = 32749
-   JoinFailedWithRejoinerNotFound = 32748 (for Rejoin)
-   JoinFailedFoundExcludedUserId = 32747 (I think not used currently)
-   JoinFailedFoundActiveJoiner = 32746
-   SlotError = 32742 (in case you use ExpectedUsers)
  
**JoinRandomRoom operation / OnJoinRandomFailed:  
-   NoRandomMatchFound = 32760


### Photon View
특정 무언가를 동기화 할때 쓰인다.
동기화 할려면 photon View는 기본적으로 있어야한다.

photon translate view : 위치 크기 이동 동기화 기능
photon Animator View : 애니메이터 동기화 기능
master인 client에서 움직여야 master가 아닌 클라이언트에서 도움직인다.
master가 아닌 클라이언트 에서 움직인 오브젝트는 master가 아니므로 이동이 동기화가 안된다.

### PhotonNetwork Instantiate
모든 클라이언트에 오브젝트를 생성해야할때 쓰는 함수다
그러니까 Instatiate에 공유형이라고 생각하면 된다
```CSharp
public override void OnJoinedRoom()
{
	PhotonNetwork.Instantiate("prefeb이름", 위치값, 각도값)
}
```
prefeb이름은 Resources 폴더안에 있는 prefeb만 작동한다. 
또한 움직임 등을 공유하고 싶을때는
prefeb에 Photon View 와  Photon transform view를 추가해야한다.

또한 PhotonNetwork Instantiate로 생성된 오브젝트는 master가 게임을 나갈경우 해당 master인 오브젝트는 사라진다.

### Photon master 구별된 행동
```CSharp
public PhotonView pv;

void Update ()
{
	if (pv.isMine)
	{
		//실행코드
	}
}
```
pv에서 동기화 하기위해 만들었던 photonView 컴포넌트를 넣어준다.
이렇게하면 모든 해당스크립트를 가진 각 클리이언트에서 만들어진 오브젝트가 움직이느게 아니라 마스터인 오브젝트만 움직인다.

### PhtonView.RPC
[관련문서](https://doc.photonengine.com/fusion/current/manual/rpc)
PhotonView.RPC는 해당 PhotonView가 포함된 모든 오브젝트 스크립트 에서 특정 함수를 실행시키는 함수이다.
```CSharp
public PhotonView pv;

void Update ()
{
	if (pv.isMine)
	{
		pv.RPC("SpecialFunctionRPC", RpcTarget, parameter);
	}

}
[PunRPC]
void SpecialFunctionRPC (int parameter)
{
	Debug.Log("WOW");
}
```
일반적으로 이렇게 쓰인다.
우선 해당 master에서 RPC 를 부르면 
	따라서 if (pv.isMine) 을 해야함
다른 클리이언트에서 해당 PhotonView를 포함한 오브젝트에 함수를 실행시킨다
RPC(함수이름, RpcTarget. 해당 함수의 인자들);

그리고 RPC에서 불릴 함수에는 ```[PunRPC]```가 쓰여야한다.


서버에 접속할때 player오브젝트에 player 스크립트가 생긴체로 생성된다고하면
1. 플레이어가 한명일때는 자신에 것만있으니 1개
2. 플레이어가 두명일때는 A의 것과 B의것 A에 표시되는 B의것 B에 표시되는A의 것     총 2x2 = 4 4개 가있다.
3. 즉 3명은 3x3 = 9개 의 스크립트를 가지게 된다.

RpcTarget종류
-   `All`: can be sent / is executed by all peers in the session (including the server).
-   `Proxies`: can be sent / is executed by a peer who does not have either Input Authority or State Authority over the object.
-   `InputAuthority`: can be sent / is executed by the peer with Input Authority over the object.
-   `StateAuthority`: can be sent / is executed by the peer with State Authority over the object.
RpcTarget.All : 즉시 호출되어 사라짐 (방전체)
RpcTarget.AllBuffered : 재접속될때 호출됨 


## 변수 sync
IPunObservable를 추가로 구현해야한다.
```CSharp
public void OnPhotonSerializeView(PhotonStream stream, PhotonMessageInfo info)
{
	if(stream.IsWriting) stream.SendNext(something);
	else something = (type)stream.ReceiveNext();
}
```

stream.IsWriting과
stream.IsReading로 주는것과 받는걸 확인할수있다
여러게 할때는
```CSharp
public void OnPhotonSerializeView(PhotonStream stream, PhotonMessageInfo info)
{
	if(stream.IsWriting)
	{
		stream.SendNext(something);
		stream.SendNext(something2);
	}
	else
	{
	 something = (type)stream.ReceiveNext();
	 something2 = (type)stream.ReceiveNext();
	}
}
```
이렇게 하면된다.

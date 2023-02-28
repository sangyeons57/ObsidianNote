#Photon2 #unity #CSharp 
예제)
부르는곳
```CSharp
 PhotonNetwork.Instantiate("Bullet", transform.position, Quaternion.identity)
	.GetComponent<PhotonView>().RPC("DirRPC", RpcTarget.All);
```
사용되는곳 외부
```CSharp
[PunRPC]
void DirRPC()
{
	dir = ((Vector2)(Camera.main.ScreenToWorldPoint(Input.mousePosition) - transform.position)).normalized;
	//dir = dir.normalized;
}
```

```CSharp
 PhotonNetwork.Instantiate(리소스, 포지션, Quaternion.identity)
	.GetComponent<PhotonView>().RPC(RPC이름, RpcTarget.타겟종류);
```

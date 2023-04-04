#unity #Project/Minecraft 

## Player
```CSharp
private void FixedUpdate()
{
	CalculateVelocity();

	if (jumpRequest)
		Jump();

	transform.Rotate(Vector3.up * mouseHorizontal);
	cam.Rotate(Vector3.right * -mouseVertical);
	transform.Translate(velocity, Space.World);
}
```
1. 속도계산
2. 점프
3. 화면 회전
vector.up = (0,1,0) 이므로 mousehorizontal 은 -1 ~ 1 까지의 값
mouseHorizontal은 GetAxis("Mouse x") 로  input maneger에서 회전값을 가지고 온다.
4. 그후 속도갑으로 움직임

```CSharp
private void Update()
{
	GetPlayerInputs();
}

private void GetPlayerInputs()
{
	horizontal = Input.GetAxis("Horizontal");
	vertical = Input.GetAxis("Vertical");
	mouseHorizontal = Input.GetAxis("Mouse X");
	mouseVertical = Input.GetAxis("Mouse Y");

	if (Input.GetButtonDown("Sprint"))
		isSprinting = true;
	if (Input.GetButtonUp("Sprint"))
		isSprinting = false;

	if (isGround && Input.GetButtonDown("Jump"))
		jumpRequest = true;
}
```
플레이어 인풋받기  위처럼 되어있음 큰 어려운 부분은 없었다.


```CSharp
void Jump()
{
	verticalMomentum = jumpForce;
	isGround = false;
	jumpRequest = false;
}
```
위아레 축( verticlaMomentum)으로 힘을 준다 
```CSharp
private void CalculateVelocity()
{
	//Affect Vertical momention with gravity
	if (verticalMomentum > gravity)
		verticalMomentum += Time.fixedDeltaTime * gravity;

	// if we're sprinting, use the sprint multiplier
	if (isSprinting)
		velocity = ((transform.forward * vertical) + (transform.right * horizontal)) * Time.fixedDeltaTime * sprintSpeed;
	else
		velocity = ((transform.forward * vertical) + (transform.right * horizontal)) * Time.fixedDeltaTime * walkSpeed;

	//Apply vertical momentum (falling / jumping)
	velocity += Vector3.up * verticalMomentum * Time.fixedDeltaTime;


	if ((velocity.z > 0 && front) || (velocity.z < 0 && back))
		velocity.z = 0;
	if ((velocity.x > 0 && right) || (velocity.x < 0 && left))
		velocity.x = 0;

	if (velocity.y < 0)
		velocity.y = checkDownSpeed(velocity.y);
	else if (velocity.y > 0)
		velocity.y = checkUpSpeed(velocity.y);
}
```
verticalMomentum : 세로로 가해지는 힘
gravitiy = -9.8 기때문에 가해지는 중력보다 +이면 가해진 시간 * 즁력의 힘 만큼 세로힘을 더한다

isSprinting 은 달리기이다
가로축 * 가로 기본 백터 + 세로축 + 세로 기본 백터 * 고정시간 * (걷기속도 || 뛰기 속도)

velocity 에다가 상향방향 기본백터에다가 * 상향축 힘 * 고정시간
속도에다가 상향축 힘을 적용

속도가 0보다 작은 경우 즉, 떨어지고 있는 경우
	떨어질수있는 상황인지 고려해서 떨어지는 속도는 넣는다
속도가 0보다 큰 경우 즉 , 즉 점프하고있는 경우
	점프할수있는상황, 즉 위에 무언가 없는 상화인경우 점프하는 속도를 적용한다.


```CSharp
private float checkDownSpeed(float downSpeed)
{
	if (
		world.CheckForVoxel(new Vector3(transform.position.x - playerWidth, transform.position.y + downSpeed, transform.position.z - playerWidth))||
		world.CheckForVoxel(new Vector3(transform.position.x + playerWidth, transform.position.y + downSpeed, transform.position.z - playerWidth))||
		world.CheckForVoxel(new Vector3(transform.position.x + playerWidth, transform.position.y + downSpeed, transform.position.z + playerWidth))||
		world.CheckForVoxel(new Vector3(transform.position.x - playerWidth, transform.position.y + downSpeed, transform.position.z + playerWidth)))
	{
		isGround = true;
		return 0;
	}
	else
	{
		isGround = false;
		return downSpeed;
	}
}
private float checkUpSpeed(float upSpeed)
{
	if (
		world.CheckForVoxel(new Vector3(transform.position.x - playerWidth, transform.position.y + 2f + upSpeed, transform.position.z - playerWidth))||
		world.CheckForVoxel(new Vector3(transform.position.x + playerWidth, transform.position.y + 2f + upSpeed, transform.position.z - playerWidth))||
		world.CheckForVoxel(new Vector3(transform.position.x + playerWidth, transform.position.y + 2f + upSpeed, transform.position.z + playerWidth))||
		world.CheckForVoxel(new Vector3(transform.position.x - playerWidth, transform.position.y + 2f + upSpeed, transform.position.z + playerWidth)))
	{
		return 0;
	}
	else
	{
		return upSpeed;
	}
}
```
점프와 떨어지는 커 체크하는 알고리즘
각각 4방향으로 가상의 부피를 만들어서 해당 좌표에서 블럭이 있는지 확인한다.

```CSharp
public bool front
{
	get
	{
		return (
			world.CheckForVoxel(new Vector3(transform.position.x, transform.position.y, transform.position.z + playerWidth))||
			world.CheckForVoxel(new Vector3(transform.position.x, transform.position.y + 1f, transform.position.z + playerWidth)));
	}
}
public bool back
{
	get
	{
		return (
			world.CheckForVoxel(new Vector3(transform.position.x, transform.position.y, transform.position.z - playerWidth))||
			world.CheckForVoxel(new Vector3(transform.position.x, transform.position.y + 1f, transform.position.z - playerWidth)));
	}
}
public bool left
{
	get
	{
		return (
			world.CheckForVoxel(new Vector3(transform.position.x - playerWidth, transform.position.y, transform.position.z))||
			world.CheckForVoxel(new Vector3(transform.position.x - playerWidth, transform.position.y + 1f, transform.position.z)));
	}
}
public bool right
{
	get
	{
		return (
			world.CheckForVoxel(new Vector3(transform.position.x + playerWidth, transform.position.y, transform.position.z))||
			world.CheckForVoxel(new Vector3(transform.position.x + playerWidth, transform.position.y + 1f, transform.position.z)));
	}
}
```
자기자과, 자기자신보다 1칸 높이보와
자기가 가려는 방향(+1칸) 에 블럭이 존재하는지 확인하는 코드

움직임을 계산하는코드에서 만약 블력이있을 경우 해당 방향으로 이동을 0으로 바꾼다

```CSharp
public bool CheckForVoxel(Vector3 pos)
{
	ChunkCoord thisChunk = new ChunkCoord(pos);

	if (!IsVoxelInWorld(pos) || pos.y < 0 || pos.y > VoxelData.chunkHeight)
		return false;

	if (chunks[thisChunk.x, thisChunk.z] != null && chunks[thisChunk.x, thisChunk.z].isVoxelMapPopulated)
		return blockTypes[chunks[thisChunk.x, thisChunk.z].GetVoxelfromGlobalVector3(pos)].isSolid;

	return blockTypes[GetVoxel(pos)].isSolid;
}
```
들어오는 포지션 을 chunk로 바꾼후
해당위치 가 월드안에 존제하지않는 경우 false를 리턴

월드안에 존제하는 경우
	해당위치에 청크가 없지 않고 활성화되어있다면
	행당위치에 청크에 복셀에 타입에 블록타입을 가지고와서 solid한지 확인한다
	|
전부아니면 해당위치에 복셀을 가지고와서 블록타입이 solid인지 확인한다

## DebugScreen.cs
상태창 표시하는부분인데 크게 어려운 부분이 없어서 설명안함
월드와 voxelData 값들을 사용자 canvas 에 표시해줒는거임

## World 
부분 최적화를 했는데 오히려 느려짐
수정된부분만 올림
```CSharp
private void Update()
{
	playerChunkCoord = GetChunkCoordFromVector3(player.position);

	if (!playerChunkCoord.Equals(playerLastChunkCoord))
		CheckViewDistance();

	if (chunksToCreate.Count > 0 && !isCreatingChunks)
		StartCoroutine("CreateChunks");

	if (Input.GetKeyDown(KeyCode.F3))
		debugScreen.SetActive(!debugScreen.activeSelf);

}
```
현재 플레이어의 청크의 위치 와 마지막 플레이어 청크의 위치가 같으면
checkviewDistance() 실행
생성되 청크가 0보다 크고 청크가 생성중이 아니라면
청크하나를 추가로 생성


```CSharp
IEnumerator CreateChunks()
{
	isCreatingChunks = true;

	while(chunksToCreate.Count > 0)
	{
		chunks[chunksToCreate[0].x, chunksToCreate[0].z].Init();
		chunksToCreate.RemoveAt(0);
		yield return null;
	}

	isCreatingChunks = false;
}
```
이게 만들어진 이유는 update가 한번할때 1프레임이 실행된다
즉, 1update는 = 1프레임
그레서 모든 프레임에 모든 픽셀이 동시생성되는것은
순각적으로 한프레임을 끈기게 만들수가있다

따라서 무조건적으로 1프레임에 1개의 청크만 생성되게해서
한프레임에 명령어가 모야 끈기는 현상을 줄일수있다.
IEnumerator를 활용해서 작업 할게 있는경우 
위치에 청크를 설정하고 작없을 끝낸것을 제거하고 일을끝낸다

CheckViewDistance()는 모든 청크 가 다시로도되지 않게 한다

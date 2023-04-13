#unity #Project/Minecraft 

## 블럭 제거밑 생성
```CSharp
public void EditVoxel (Vector3 pos, byte newID)
{

	int xCheck = Mathf.FloorToInt(pos.x);
	int yCheck = Mathf.FloorToInt(pos.y);
	int zCheck = Mathf.FloorToInt(pos.z);

	int Player_xCheck = Mathf.FloorToInt(world.player.position.x);
	int Player_yCheck = Mathf.FloorToInt(world.player.position.y);
	int Player_yUpCheck = Mathf.FloorToInt(world.player.position.y) + 1;
	int Player_zCheck = Mathf.FloorToInt(world.player.position.z);


	if (newID != 0 && (xCheck == Player_xCheck && (yCheck == Player_yCheck || yCheck == Player_yUpCheck) && zCheck == Player_zCheck)) return ;


	xCheck -= Mathf.FloorToInt(chunkObject.transform.position.x);
	zCheck -= Mathf.FloorToInt(chunkObject.transform.position.z);

	voxelMap[xCheck, yCheck, zCheck] = newID;

	UpdateSurroundingVoxels(xCheck, yCheck, zCheck);
	// Update Surrounding Chunks;

	UpdateChunk();
}

void UpdateSurroundingVoxels(int x, int y, int z)
{
	Vector3 thisVoxel = new Vector3(x, y, z);

	for (int p = 0; p < 6; p++)
	{
		Vector3 currentVoxel = thisVoxel + VoxelData.faceCheckers[p];
		if(!IsVoxelInChunk( (int)currentVoxel.x, (int)currentVoxel.y, (int)currentVoxel.z ))
		{
			world.GetChunkFromVector3(currentVoxel + position).UpdateChunk();

		}
	}
}
```
EditVoxel 은 해당 위치에 복셀을 주어진 ID로 ID를 교체한다
해당 위치가 플레이어 위치랑겹치면 해당스폰을 막는다

UpdateSurroudingVoxels 주변에 둘러싼 6명의 복셀중 외부 청크가 있으면 해당청크도 다기 계산하계해주는 코드
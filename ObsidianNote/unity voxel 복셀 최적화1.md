복셀의 한면을 표현하기위해 fragment2개가 필요하다
3 + 3 = 6개의 vertex가 필요하다.
이때 voxel 을 처리하기위해
vertices와 triangles 가 필요한데
vertieces는 vertex의 위치를
triangles는 vertex를 연결하기 위해 vertex의 순서를 쓰는거다

면 2개를 만들기위해 triangles는 6개의 값이 필요하지만
실제로는 vertex값을 2개 공유해쓰기때문에
vertex는 4개의 값만 쓰면 된다
```CSharp
    public static readonly int[,] voxelTris = new int[6, 4]
    {
        {0,3,1,2}, // Back Face
        {5,6,4,7}, // Front Face
        {3,7,2,6}, // Top Face
        {1,5,0,4}, //Bottom Face
        {4,7,0,3}, //Right Face
        {1,2,5,6},
    };

    public static readonly Vector2[] voxelUvs = new Vector2[4] {
        new Vector2(0f,0f),
        new Vector2(0f,1f),
        new Vector2(1f,0f),
        new Vector2(1f,1f),
    };


for(int p = 0; p < 6; p++)
{
	if(!CheckVoxel(pos + VoxelData.faceCheckers[p]))
	{
		//4개의 vertex값만 저장
		vertices.Add(pos + VoxelData.voxelVerts[VoxelData.voxelTris[p,0]]);
		vertices.Add(pos + VoxelData.voxelVerts[VoxelData.voxelTris[p,1]]);
		vertices.Add(pos + VoxelData.voxelVerts[VoxelData.voxelTris[p,2]]);
		vertices.Add(pos + VoxelData.voxelVerts[VoxelData.voxelTris[p,3]]);
		//uv도 4개 값만 저장
		uvs.Add(VoxelData.voxelUvs[0]);
		uvs.Add(VoxelData.voxelUvs[1]);
		uvs.Add(VoxelData.voxelUvs[2]);
		uvs.Add(VoxelData.voxelUvs[3]);

		//triangles의 순서는 6개
		//뒤에붙는 숫자는 vertex순서를 표시
		triangles.Add(vertexIndex);
		triangles.Add(vertexIndex + 1);
		triangles.Add(vertexIndex + 2);
		triangles.Add(vertexIndex + 2);
		triangles.Add(vertexIndex + 1);
		triangles.Add(vertexIndex + 3);

		// vertex가 6개가 아니라 4개가 추가된다
		vertexIndex += 4;
	}
}
```
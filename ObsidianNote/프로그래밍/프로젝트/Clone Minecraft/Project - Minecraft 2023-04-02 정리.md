#unity #Project/Minecraft 
맵만드는것을 했는데 알고리즘을 보자면

-----
### world부분
우선 world에서 시작한다.
```CSharp
private void Start()
{
	Random.InitState(seed);

	spawnPosition = new Vector3(
		(VoxelData.WorldSizeInChunks * VoxelData.chunkWidth) / 2f,
		 VoxelData.chunkHeight,
		(VoxelData.WorldSizeInChunks * VoxelData.chunkWidth) / 2f);

	GenrateWorld();
	playerLastChunkCoord = GetChunkCoordFromVector3(player.position); 
}

```
Random.InistState를 해주는 이유는 random이 perlinoise 에 영향을 준다고했는데
챶아보니 실제로는 영향이 없다고 한다
그냥 perlinoise는 무한하니 offset을 추가하라고 한다.

그다음 spawnposition을 정한다.
그다음으로 GenerateWorld() 로 world를 생성 한다
그리고 마지막의 플레이어가있었던 청크좌표를 기억한다.


```CSharp
void GenrateWorld ()
{
	for (int x = (VoxelData.WorldSizeInChunks / 2) - VoxelData.ViewDistanceInChunks;
	 x < (VoxelData.WorldSizeInChunks / 2) + VoxelData.ViewDistanceInChunks; x++)
	{
		for (int z = (VoxelData.WorldSizeInChunks / 2) - VoxelData.ViewDistanceInChunks ; z < (VoxelData.WorldSizeInChunks / 2) + VoxelData.ViewDistanceInChunks; z++)
		{
			CreateNewChunk(x, z);
		}
	}

	player.position = spawnPosition;
}
```
주어진 크기만틈 청크를 생성한다  그후 플레이어 위치를 설정한다.



```CSharp
private void Update()
{
	playerChunkCoord = GetChunkCoordFromVector3(player.position);
	if (!playerChunkCoord.Equals(playerLastChunkCoord))
		CheckViewDistance();
}
```
플레이어가 있는 청크의 좌표를 계속 계산한다 최적화를 위해 계속해서
일정거리이상의 청크는 inactive 하기 위해서 이다.

플레이어의 청크좌표와 마지막 청크좌표가 달라질경우 check worldDistance를 실행한다.


```CSharp
void CheckViewDistance()
{
	ChunkCoord coord = GetChunkCoordFromVector3(player.position);
	List<ChunkCoord> previouslyActiveChunks = new List<ChunkCoord>(activeChunk);

	for ( int x = coord.x - VoxelData.ViewDistanceInChunks;
	 x < coord.x + VoxelData.ViewDistanceInChunks; x++)
	{
		for ( int z = coord.z - VoxelData.ViewDistanceInChunks;
		 z < coord.z + VoxelData.ViewDistanceInChunks; z++)
		{
			if (IsChunkInWorld(new ChunkCoord(x, z)))
			{
				if (chunks[x, z] == null)
					CreateNewChunk(x, z);
				else if (!chunks[x, z].isActive)
				{
					chunks[x,z].isActive = true;
					activeChunk.Add(new ChunkCoord(x, z));
				}
			}

			for (int i = 0; i< previouslyActiveChunks.Count; i++)
			{
				if (previouslyActiveChunks[i].Equals(new ChunkCoord(x,z)))
					previouslyActiveChunks.RemoveAt(i);
			}
		}
	}

	foreach(ChunkCoord c in previouslyActiveChunks)
	{
		chunks[c.x, c.z].isActive = false;
	}
}


bool IsChunkInWorld(ChunkCoord coord)
	=> (coord.x > 0 && coord.x < VoxelData.WorldSizeInChunks - 1 && coord.z > 0 && coord.z < VoxelData.WorldSizeInChunks - 1);


```
플레이어가볼수있는 거리만큼 x z좌표를 검사한다

해당청크의 좌표가 월드안에 존제한다면
  chunk가 아직 생성되지않았으면 그냥 생성하고
  비활성화 되어있다면 활성화 시키고 활성화된 청크리스트에 추가시킨다
그리고 이전에 활성화된 청크리스트에서 현제활성화되어있는 청크들을 뺀다

그리고 남아있는 이전에 활성화된 청크리스트에 청크들을 비활성화 시킨다.



```CSharp
void CreateNewChunk (int x, int z)
{
	chunks[x,z] = new Chunk(new ChunkCoord(x, z), this);

	activeChunk.Add(new ChunkCoord(x, z));

}
```
청크생성 코드이다 청크배열에다가 해당 청크의 정보를 입력한다
그리고 활서된 청크에 추가시킨다



```CSharp
public byte GetVoxel (Vector3 pos)
{
	int yPos = Mathf.FloorToInt(pos.y);

	/* IMMUTABLE PASS */

	// If outside world, retain air
	if (!IsVoxelInWorld(pos))
		return 0;

	//IF bottom block of chunk, return bedrock
	if (yPos == 0)
		return 1;


	/* BAIC TERAIN PASS */
	int terrainHeight = Mathf.FloorToInt(biome.terrainHeight * Noise.Get2DPerlin(new Vector2(pos.x, pos.z), 0, biome.TerrainScale )) + biome.solidGroundHeight;
	byte voxelValue = 0;

	if (yPos == terrainHeight)
		voxelValue = 3;
	else if (yPos < terrainHeight && yPos > terrainHeight - 4)
		voxelValue = 5;
	else if (yPos > terrainHeight)
		return 0;
	else
		voxelValue = 2;

	if (voxelValue == 2)
	{
		foreach (Lode lode in biome.lodes)
		{
			if (yPos > lode.minHeight && yPos < lode.maxHeight)
				if (Noise.Get3DPerlin(pos, lode.noiseOffset, lode.scale, lode.threshold))
					voxelValue = lode.blockID;
		}
	}
	return voxelValue;


}
```
청크는 여러개의 복셀들로 이루어져있다.
이 복셀에 값을 가지고 오는 함수이다

위치값을 입력하면 해다위치에 GetVoxel알고리즘을 적용시킨후 어떤 타입의 복셀인지 
byte를 반환 시킨다 byte는 0 ~255까지 표현할수있는 숫자 데이터 이다
그것과 비슷한 SByte는 -128~127까지 표현할수있다.

현재 알고리즘은
world밖은 전부 공기복셀로 체우고
가장 아레 바닥은 badrock으로 채운다

그후, (펄린노이즈를 생성하고 0~1 사이의 값) * (터레인 스케일을 곱한다 0~1 -> 0 ~ 터레인스케일) 이것을 int형으로 바꾼후 + 기본높이을 더해준다

이렇게 해서 나온 높이와 같으면 잔디블럭
그아레부터 4칸은 흙 블럭

터레인 기높이 보다 높으면 공기블럭으로 한다.

그외는 모레블럭으로 하는데
모레블럭일 경우 정해진 높이 사이에서 3dperlin noise로 바이옴을 표현 한다.
그렇게 해서 만들어진 해당 바이옴을 해당 ID값으로 변환시킨다

남은 공간은 기본값이 공기블로 채워져있고 그후 결과값을 프린트 한다.


### BlockType
```CSharp
[System.Serializable]
public class BlockType
{
    public string blockName;
    public bool isSolid;

    [Header("Texture Values")]
    public int backFaceTexture;
    public int frontFaceTexture;
    public int topFaceTexture;
    public int bottomFaceTexture;
    public int leftFaceTexture;
    public int rightFaceTexture;

    //Back Front Top Bottom Left Right

    public int GetTexutureID (int faceTexture)
    {
        switch (faceTexture)
        {
            case 0: return backFaceTexture;
            case 1: return frontFaceTexture;
            case 2: return topFaceTexture;
            case 3: return bottomFaceTexture;
            case 4: return leftFaceTexture;
            case 5: return rightFaceTexture;

            default:
                Debug.Log("Error in GetTextureID; invalid face index");
                return 0;
        }
    }
}
```
`[System.Serializable]`  이걸하면 유니티 에디터에서 해당 클레스를 리스트로 받을때
클레스 내부의 요소도 다 따로따로 받을수있게 만들수있다.

`[Header("Texture Values")]`  이걸쓰면 유니티에디터에서 들오는값을 나눠서 제목을 지울수있다

이부분에서 하는것은 한복셀에 각 면에대한 텍스쳐 값을 할당하고
특정 번째의 면을 달라고 하면 해당명에 텍스쳐에 해당하는 명의 값을 반환하는 것이다.


-----
### Biom / BiomAttributes
```CSharp
[CreateAssetMenu(fileName = "BiomeAttributes", menuName = "MinecraftTutorial/Biome Attribute")]
public class BiomeAttributes : ScriptableObject
{
    public string biomName;

    public int solidGroundHeight;
    public int terrainHeight;
    public float TerrainScale;

    public Lode[] lodes;

}


[System.Serializable]
public class Lode
{
    public string nodeName;
    public byte blockID;
    public int minHeight;
    public int maxHeight;
    public float scale;
    public float threshold;
    public float noiseOffset;
}
```
이객체는 스크립터블 오브젝트이다 [[Unity scriptable object]] 에서 해당 내용을 볼수있다
여기서는 바이옴와 정보와  바이옴의 이름 지형을 생성한때 사용할
지형의 높낮이 차와 터레인의 그레디언트 크기에 대한 정보를 가지고 있다

바이옴의 정보는 
- 바이옴이름
- 바이옴에 블럭ID
- 최대생성높이
- 최저생성높이
- 크기
- 시작점
- 노이즈 시작점 이있다

----
### Noise / perlinNoise

```CSharp
public class Noise
{
    public static float Get2DPerlin (Vector2 position, float offset,float scale)
    {
        return Mathf.PerlinNoise( 
            (position.x + 0.1f) / VoxelData.chunkWidth * scale  + offset,
            (position.y + 0.1f) / VoxelData.chunkWidth * scale  + offset);
    }

    public static bool Get3DPerlin(Vector3 position, float offset,float scale, float threshold)
    {
        float x = (position.x + offset + 0.1f) * scale;
        float y = (position.y + offset + 0.1f) * scale;
        float z = (position.z + offset + 0.1f) * scale;

        float AB = Mathf.PerlinNoise(x, y);
        float BC = Mathf.PerlinNoise(y, z);
        float AC = Mathf.PerlinNoise(x, z);

        float BA = Mathf.PerlinNoise(y, x);
        float CB = Mathf.PerlinNoise(z, y);
        float CA = Mathf.PerlinNoise(z, x);

        return (AB + BC + AC + BA + CB + CA) / 6f > threshold;

    }
}
```

노이즈는 현제 2d perlinnoise와 3d perlinnoise 2개를 가지고 잇다
2d  펄린 노이즈는 일반 텍스쳐 처럼 펄린 노이즈가 있고 해당 값을
복셀 1개의 크기로 바꾼후
높낮이크기를 곱해준다 
그후 추가값을 더해준다

0.1f  를 더해주는 이유는 정수 좌표해서 펄린 노이즈는 항상 같은값을 주기때문이라고 한다.

3d펄린노이즈는 유니티에서 기본 제공해주지 않아서 직접 만드는데
위처럼 뭔가 신발끈 공식(외적) 처럼 생겼다 [[MATH 내적과 외적]]

----
### VoxelData
여기는 말그데로 복셀 데이터를  가지고있다

```CSharp
public static readonly Vector3[] voxelVerts = new Vector3[8] {
	new Vector3(0f, 0f, 0f),
	new Vector3(1.0f, 0f, 0f),
	new Vector3(1.0f, 1.0f, .0f),
	new Vector3(0f, 1.0f, 0f),

	new Vector3(0f, 0f, 1.0f),
	new Vector3(1.0f, 0f, 1.0f),
	new Vector3(1.0f, 1.0f, 1.0f),
	new Vector3(0f, 1.0f, 1.0f),
};
```
이렇게 정6면체를 이루기위한 8개의 좌표를 가지고 있고

```CSharp
public static readonly Vector3[] faceCheckers = new Vector3[6]
{
	new Vector3(0f,0f,-1f),
	new Vector3(0f,0f,1f),
	new Vector3(0f,1f,0f),
	new Vector3(0f,-1f,0f),
	new Vector3(-1f,0f,0f),
	new Vector3(1f,0f,0f),
};
```
다음에 복셀이 있는지 확일할때 쓰기위하 6개의 추가 좌표도있고

```CSharp
public static readonly int[,] voxelTris = new int[6, 4]
{
	//Back Front Top Bottom Left Right
	{0,3,1,2}, // Back Face
	{5,6,4,7}, // Front Face
	{3,7,2,6}, // Top Face
	{1,5,0,4}, //Bottom Face
	{4,7,0,3}, //Left Face
	{1,2,5,6}, //Right Face
};
```
면을 만들기 위해 좌표의 표현순서를 가지고 있고

```CSharp
public static readonly Vector2[] voxelUvs = new Vector2[4] {
	new Vector2(0f,0f),
	new Vector2(0f,1f),
	new Vector2(1f,0f),
	new Vector2(1f,1f),
};
```
해당 복셀에 텍스쳐를 표현하기위한 uv좌표값도 가지고있다
uv좌표값은 면에 표현할때 사용되는 좌표값과 같다

한면에 좌표 4개 uv도 4개

----
### Chunk 청크
실제로 복셀을 생성해서 플레이어 눈에 보이도록 관리하는 부분이다

```CSharp
    public Chunk(ChunkCoord coord, World world)
    {
        this.coord = coord;
        this.world = world;
        chunkObject = new GameObject();
        meshFilter = chunkObject.AddComponent<MeshFilter>(); 
        meshRenderer = chunkObject.AddComponent<MeshRenderer>();

        meshRenderer.material = world.material;
        chunkObject.transform.SetParent(world.transform);
        chunkObject.transform.position = new Vector3(coord.x * VoxelData.chunkWidth, 0, coord.z * VoxelData.chunkWidth);
        chunkObject.name = $"Chunk {coord.x}, {coord.z}";

        PopulateVoxelMap();
        CreateMeshData();
        createMesh();
    }
```
청크가 하나 생성이 되면

우선 이 청크의 좌표값과 월드를 가지고
게임오브젝트와 컴포넌트를 생성한다

청크의 머티리얼 이름 위치를 설정한다

그후 복셀값들을 활성화시키고
매쉬를 데이터를 생성한후
해당 매쉬데이터로 실제 매쉬를 생성한다


```CSharp
void PopulateVoxelMap ()
{
	for (int y = 0; y < VoxelData.chunkHeight; y++)
	{
		for (int x = 0; x < VoxelData.chunkWidth; x++)
		{
			for (int z = 0; z < VoxelData.chunkWidth; z++)
			{
				voxelMap[x,y,z] = world.GetVoxel(new Vector3(x, y, z) + position);
			}
		}
	}
}
```
world부분에있던 GetVoxel로 voxelmap을 활성화 시킨다


```CSharp
void CreateMeshData()
{
	for (int y = 0; y < VoxelData.chunkHeight; y++)
	{
		for (int x = 0; x < VoxelData.chunkWidth; x++)
		{
			for (int z = 0; z < VoxelData.chunkWidth; z++)
			{
				if (world.blockTypes[voxelMap[x,y,z]].isSolid)
					AddvoxelDataToChunk(new Vector3(x, y, z));
			}
		}
	}
}
```
주어진 복셀의 타입이 고체인경우 청크에다가 복셀 데이터를 추가한다.

```CSharp
void AddvoxelDataToChunk(Vector3 pos)
{
	for(int p = 0; p < 6; p++)
	{
		if(!CheckVoxel(pos + VoxelData.faceCheckers[p]))
		{
			byte blockID = voxelMap[(int)pos.x, (int)pos.y, (int)pos.z];


			for (int num = 0; num < 4; num++)
			{
				vertices.Add(pos + VoxelData.voxelVerts[VoxelData.voxelTris[p,num]]);
			}

			AddTexture(world.blockTypes[blockID].GetTexutureID(p));

			triangles.Add(vertexIndex);
			triangles.Add(vertexIndex + 1);
			triangles.Add(vertexIndex + 2);
			triangles.Add(vertexIndex + 2);
			triangles.Add(vertexIndex + 1);
			triangles.Add(vertexIndex + 3);

			vertexIndex += 4;
		}
	}
}
```
각 6면이 면을 추가할수있는 상태인경우에만
	현제복셀에서 면을 추가할련는 부분 방향에 복셀이 없는 경우

각 면에 vertex와 해당 vertex에 uv좌표를 부여하고 면을 추가한다
uv 는 특정 블럭에 특정면의 값( gGetTextureID(p) )을 을 인수로 넣어서 가져온다

그후에 vertexIndex를 추가해 다음 에 면을 추가하기위한 준비를 한다.
앞에서 버텍스 4개능 이미 할당되어서 다음 4개로 자동사용되기 때문

```CSharp
bool IsVoxelInChunk ( int x, int y, int z) 
	=> !(x < 0 || x > VoxelData.chunkWidth -1 ||
	     y < 0 || y > VoxelData.chunkHeight -1||
	     z < 0 || z > VoxelData.chunkWidth - 1);


bool CheckVoxel(Vector3 pos)
{
	int x = Mathf.FloorToInt(pos.x);
	int y = Mathf.FloorToInt(pos.y);
	int z = Mathf.FloorToInt(pos.z);

	if (!IsVoxelInChunk(x, y, z))
		return world.blockTypes[world.GetVoxel(pos + position)].isSolid;

	return world.blockTypes[voxelMap[x, y, z]].isSolid;
}
```
들어온 위치에 복셀이 청크 밖에 있는경우
	모든 복셀을 청크가 다르긴 하지만 같은 알고리즘으로 생성 됨으로
	다른위치에 청크의 해당 복셀이 solid인경우 voxel 이 있다고한다
들어온 위치에 복셀이 청크 안에 있는경우 자신의 voxelmap에서 해당좌표의 값이 solid확인하고
solid인경우 voxel이 있다고 한다.

```CSharp
void AddTexture (int textureID)
{
	float y = textureID / VoxelData.TextureAtlasSizeInBlocks;
	float x = textureID - (y * VoxelData.TextureAtlasSizeInBlocks);

	x *= VoxelData.NormalizedBlockTextureSize;
	y *= VoxelData.NormalizedBlockTextureSize;

	y = 1f - y - VoxelData.NormalizedBlockTextureSize;

	uvs.Add(new Vector2 (x, y));
	uvs.Add(new Vector2 (x, y + VoxelData.NormalizedBlockTextureSize));
	uvs.Add(new Vector2 (x + VoxelData.NormalizedBlockTextureSize, y));
	uvs.Add(new Vector2 (x + VoxelData.NormalizedBlockTextureSize,
	y + VoxelData.NormalizedBlockTextureSize));
}
```
텍스쳐를 추가하는 부분이다
텍스쳐는 4 * 4 모양의 텍스쳐고 해당 텍스쳐의 좌표를 찾아주기위해서 이 함수가 존재한다
왼쪽의부터 0,0 으로 시작함으로

y값은 해당 아이디 / 4단위를 해서 y좌표를 구하고
x 값 y가 간만큰  x 값을 제거해서 x좌표를 구한다

텍스쳐는 왼쪽 아레부터 좌표를 시작 함으로
1에서 뺀후 시작이 0임으로 1단위룰 뺀다

그리고 나온 x + y값에  오른쪽 위로 갈수록 값이 증가하니 나며지 3값도 값을 + 해서 
좌표 4개를 설정한다.


```CSharp
void createMesh()
{
	Mesh mesh = new Mesh();
	mesh.vertices = vertices.ToArray();
	mesh.triangles = triangles.ToArray();
	mesh.uv = uvs.ToArray();

	mesh.RecalculateNormals();

	meshFilter.mesh = mesh;
}
```
매쉬를 생성하고 해당 매쉬에
위에서 만든 메쉬데이터를 넣은다
차례로 vertex값 / 면을 표현하기위한 vertex순서 값 / 면에 텍스쳐를 표현하기위한 uv값

그후 화면에 표시하기 위해서 노멀을 재계산 해준다
그리고 메쉬필터에 만든 메쉬를 한당한다
메쉬필터는 그냥 매쉬가 있게하려한다면 있어야하는 거다 
메쉬를 표현해주는놈?

#Project/Minecraft

복셀최적화를 했다
vertex데이터 6 -> 4개로 적게 사용하기
내부 데이터 표현안하기

그중 vertex 사용 줄이기는 [[unity voxel 복셀 최적화1]] 에 서 보면 될거같다

```CSharp
public MeshRenderer meshRenderer;
public MeshFilter meshFilter;

int vertexIndex = 0;
List<Vector3> vertices = new List<Vector3>();
List<int> triangles = new List<int>();
List<Vector2> uvs = new List<Vector2>();

bool[,,] voxelMap = new bool[VoxelData.chunkWidth, VoxelData.chunkHeight, VoxelData.chunkWidth];


void Start()
{

	PopulateVoxelMap();
	CreateMeshData();
	createMesh();
}
```
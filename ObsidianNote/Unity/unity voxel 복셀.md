#unity/voxel 

voxel 복셀은 3차원 공간에서 정규 격자 단위의 값을 나타낸다.

vetex 여러개가 연결되어서 face가 한개 말들어 진다
일반적으로 3개의 vertex 사용한다.

이때 face 한 면에서만 face를 볼수있는데 
이때 시계방향으로 랜더링 방향이 결정된다


정사각형이 존재한다고 했을때
각각
vertices =  (0,0), (1,0), (0,1), (0,1)
triangles = 0, 1, 2 , 2, 1, 3

uv vertices와 1대1 대칭 되는거 같다.

이렇게 표현한다

아마도 vertices는 vertex의 위치를 triangles는 vertex의 순서를 표현하는거 같다.


여기서는 시계방향을 이용했다.
정6면체 만들기
```CSharp
public class VoxelData
{
//정6면체의 좌표 총 8개개
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

//면 만들기 삼각형 2개로 한 면을 만들면 1숫자당 vertex좌표를 표현한다.
    public static readonly int[,] voxelTris = new int[6, 6]
    {
        {0,3,1,1,3,2}, // Back Face
        {5,6,4,4,6,7}, // Front Face
        {3,7,2,2,7,6}, // Top Face
        {1,5,0,0,5,4}, //Bottom Face
        {4,7,0,0,7,3}, //Right Face
        {1,2,5,5,2,6},
    };

//uv맵 좌표를 표현하며 이것도 삼각형 2개로 표현했다.
    public static readonly Vector2[] voxelUvs = new Vector2[6] {
        new Vector2(0f,0f),
        new Vector2(0f,1f),
        new Vector2(1f,0f),
        new Vector2(1f,0f),
        new Vector2(0f,1f),
        new Vector2(1f,1f),
    };
}
```

```CSharp
public class Chunk : MonoBehaviour
{
    public MeshRenderer meshRenderer; // 매쉬를 랜더링 하는데 쓰인다. 
    public MeshFilter meshFilter; // 실제메쉬를 저장한다.
    
    void Start()
    {
        int vertexIndex = 0; //vertex에 
        List<Vector3> vertices = new List<Vector3>();
        List<int> triangles = new List<int>();
        List<Vector2> uvs = new List<Vector2>();

        for(int p = 0; p < 6; p++)
        {
            for(int i = 0; i < 6; i++)
            {
			//면을 표현하기위한 vertex의 위치값을 불러온다
                int triangleIndex = VoxelData.voxelTris[p, i]; 
            //vertex값을 불러온다.
                vertices.Add(VoxelData.voxelVerts[triangleIndex]);
				
				// vertex의 순번을 tragles에 저장한다.
                triangles.Add(vertexIndex);

				//uvs 해당 위치의 uv좌표를 저장한다.
                uvs.Add(VoxelData.voxelUvs[i]);

				// 계산하는 vertex의 번째 1추가
                vertexIndex++;
            }

        }

        Mesh mesh = new Mesh(); //unit buit-in 클레스 Mesh를 가지고 온다.
        mesh.vertices = vertices.ToArray(); // 정점위치를 설정하낟.
        mesh.triangles = triangles.ToArray(); // 면 위치를 설정한다.
        mesh.uv = uvs.ToArray(); // uv map을 설정한다.

        mesh.RecalculateNormals(); // normal 재계산 빛계산

        meshFilter.mesh = mesh; // meshFilter에 설정적용
    }
}
```
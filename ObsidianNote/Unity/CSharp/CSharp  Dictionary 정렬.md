#language/CSharp #Linq #정렬 #order

```CSharp
//사전 정렬
var sortingDict = from pair in playerPointNote
			orderby pair.Value descending
			select pair;
```

linq를 활용해 정렬했다 orderby가 정렬할때쓰는거고
뒤에나오는 값으로 정렬한다
```CSharp
orderby word.length
```
롤 하면 길이로 정렬 된다

descending은 앞에있는 값을 내림차순으로
ascending은 앞에있는 값을 오름차순으로 정렬한다.

여러기준으로 정렬할수도있다
```CSharp
orderby pair.Value, pair.Value.length descending
```
롤 하면 pari.Value기준으로 오름차순으로 정렬하고 pair.Value.lenth를 내림차순으로 정렬한다.

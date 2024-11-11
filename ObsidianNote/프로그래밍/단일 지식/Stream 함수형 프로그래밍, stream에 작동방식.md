Stream은 java에서 함수형 프로그래밍을 구현하는데 사용하는 메서드이다.

나는 이 stream이 일반적인 반복 처럼 동작하지 않는다는것을 알고 어떤 식으로 동작 하는지 알고 싶어져서 찾아봤다.

ex)
```Java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

names.stream()
     .filter(name -> {
         System.out.println("Filtering: " + name);
         return name.startsWith("A");
     })
     .map(name -> {
         System.out.println("Mapping: " + name);
         return name.toUpperCase();
     })
     .forEach(name -> System.out.println("Final: " + name));

```

```makefile
Filtering: Alice
Mapping: Alice
Final: ALICE
Filtering: Bob
Filtering: Charlie
Filtering: David
```

stream은 이런식으로 데이터를 
하나의 메서드에서 전부 처리하고 다음 메서드로 넘기는 방식을 상용하지 않는다.

Stream은 파이프라인을 구성하는데
이것은
종단 연산이 실행될때 까지 중간 연산을 실행시키지 않고
시작 부터 종단 연산까지 해당 연산은 먼저 연속적인 파이프 라인으로 만든다.

그후 이 연산은 하나처럼 동작해서
하나의 요소가 하나의 메서드(연산) 를 지나듯 여러개의 연산은 연결된 체인 처럼 동작한 후에 다음 요소가 연산하기 시작 한다.

마치 반복문 하나를 도는데 그 안에 연산이 많아 진것과 같은것이다. 
즉, 반복문을 여러게 쓰지않고 한 반복문 안에서 전부 끝네는 느낌이다.

아까 말했듯이 중간 연산은 지연 평가를 하는데
중간연산이 정의 될떄는 아무일도 일어 나지 않고 종단연사이 호출 되어야지 데이터 처리르 시작한다.
이를 통해 불필요한 연산을 피하고 효율적으로 작동할수 있다.

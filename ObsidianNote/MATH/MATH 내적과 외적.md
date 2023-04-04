#MATH/내적-dot_product #MATH/외적-cross_produc

## 내적 : dot product
결과값 = 스칼라
[관련영상1](https://youtu.be/2PrSUK1VrKA)

두백터사이에 각도를 구하는 방법이다

A 내적 B 는
(백터 A의 길이) * (백터 B의 길이)  * cos세타
이다
유니티에서는 이렇게쓸수있다
Vector3.Dot(A,B) =  Vector3.Magnitude(A) * Vector3.Magnitude(B) * Mathf.Cos(세타)

이렇게 볼수도있다
Vector3.Dot(A,B) /  Vector3.Magnitude(A) / Vector3.Magnitude(B) = Mathf.Cos(세타)

cos을 제거하기위해 Acos을 사용하면 이렇게된다
Mathf.Acos(Vector3.Dot(A,B) /  Vector3.Magnitude(A) / Vector3.Magnitude(B)) =세타(사이각도)

이때 세타는 radian 값이다 따라서 Mathf.Rad2Deg 가 필요할수도있다.
[[파이와 디그리 그리고 라디안]] 관련내용

그리고 내적 공식도 따로있다
	(x1, y1, z1) / (x2, y2, z2) 두개가 일을때
	내적은 = x1 * x2 + y1 * y2 + z1 * z2
 이다 2d는
	(x1, y1) / (x2, y2) 두개가 일을때
	내적은 = x1 * x2 + y1 * y2

Dot은 정확히 같은 방향을 가리키면 1을, 완전히 반대 방향을 가리키면 -1을, 벡터가 수직이면 0을 반환합니다.

내적이 반환 하는것은 두 각도 사이에 코사인이다

내적은 곱한 두 벡터의 크기와 그 사이 각도의 코사인을 곱이다
내적 = 백터A 크기 *  백터B크기 * 그사이각의 코사인곱

---
## 외적 : cross product
결과값: = 백터
두백터를가지고 두백터에 수직인 백터구하는 방법이다
계산 순서를 바꾸면 반대방향백터가 나오는데
이때 2종류에 백터가나오는데 오른손법칙을쓰냐 왼손법칙을쓰냐에 따라 다르다

외적공식 : 신발끈공식
	(vx,vylvz) / (wx,wy,wz)
	 V x W = (vy * wz - wy * vz , vz * wx - wz * vx, vx * wy - wx * vy )

A, B의 외적 = A백터의길이 * B백터의길이 * sin(두백터가 이루는 각도) * 두백터의 수직인 단위백터


위백터는 다른차원에 있다 3차원

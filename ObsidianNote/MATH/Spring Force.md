#MATH/Physics

3개의 물체가 존제한다고 할때 물체들이 서로 일정 거리를 유지 하도록 하는 기능을 필요로 할떄 사용하는 기능이다.
(물체의 거리 - 스프링의 길이) * 스프링 탄성 상수: 를 해서 스프링이 가해질 힘을 정한다

double fx = springForce * (dx / distance);
double fy = springForce * (dy / distance);
이 부분은 (가해질 힘 * 방향)  이다.
이 과정이 필요한 이유는

double springForce = springConstant * (distance - naturalLength);
여기에서 계산한 힘이 distance를 기준으로 한 힘의 양이여서
x좌표와 y좌표로 가해저야할 힘의 양을 (x좌표 / 전체힘) 이런식으로 계산해서 구하는 거다.

```java
import java.util.ArrayList;
import java.util.List;

public class Object {
    double x, y; // 물체의 좌표
    double vx, vy; // 물체의 속도
    double ax, ay; // 물체에 작용하는 가속도
    double mass; // 물체의 질량

    public Object(double x, double y, double mass) {
        this.x = x;
        this.y = y;
        this.mass = mass;
    }

    // 물체에 작용하는 힘 계산
    public void applyForce(double fx, double fy) {
        ax += fx / mass;
        ay += fy / mass;
    }

    // 물체의 속도 업데이트
    public void updateVelocity(double dt) {
        vx += ax * dt;
        vy += ay * dt;
    }

    // 물체의 위치 업데이트
    public void updatePosition(double dt) {
        x += vx * dt;
        y += vy * dt;
    }
}

public class MainObject {
    public static void main(String[] args) {
        double springConstant = 0.01; // 스프링 상수
        double naturalLength = 100; // 스프링의 기본 길이

        Object obj1 = new Object(200, 200, 1);
        Object obj2 = new Object(300, 300, 1);
        Object obj3 = new Object(400, 400, 10); // 조정 가능한 물체

        List<Object> objects = new ArrayList<>();
        objects.add(obj1);
        objects.add(obj2);
        objects.add(obj3);

        double dt = 0.1; // 시간 간격

        while (true) {
            // 각 물체에 대해 작용하는 힘 계산
            for (int i = 0; i < objects.size(); i++) {
                Object obj = objects.get(i);
                for (int j = i + 1; j < objects.size(); j++) {
                    Object other = objects.get(j);
                    // 두 물체 간의 거리 계산
                    double dx = other.x - obj.x;
                    double dy = other.y - obj.y;
                    double distance = Math.sqrt(dx * dx + dy * dy);
                    // 스프링 힘 계산
                    double springForce = springConstant * (distance - naturalLength);
                    double fx = springForce * (dx / distance);
                    double fy = springForce * (dy / distance);
                    obj.applyForce(fx, fy);
                    other.applyForce(-fx, -fy);
                }
            }

            // 조정 가능한 물체의 속도 및 위치 업데이트
            obj3.updateVelocity(dt);
            obj3.updatePosition(dt);

            // 모든 물체의 속도 및 위치 업데이트
            for (Object obj : objects) {
                obj.updateVelocity(dt);
                obj.updatePosition(dt);
            }

            // 화면에 물체 그리는 코드 (생략)

            // 잠시 대기
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```
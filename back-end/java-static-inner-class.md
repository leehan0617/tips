우선, 이 글을 쓰게 된 이유는 Java에서 inner class를 사용하지만 자세히 알지 못하기 때문에 쓴다.

자바의 Inner Class는 선언 방식에 따라 크게 non static, static 클래스로 나눌 수 있다. 

이중에서 static inner class에 대해서 알아본다.

우선 static inner class는 보통 아래와 같이 생겼다.

```
public class Outer {
  public static class Inner {
  }
}
```
그리고 생성 또한 아래와 같이 한다.
```
// 파일 상단에 import를 했다고 가정.
Outer.Inner innerInstance = new Inner();
```

여기까지는 왠만한 검색을 통해서 다 찾을 수 있다.

궁금했던건 static inner class인데 jvm에 메모리 할당이 어떻게 되는지 이였다.

보통 static 메소드 or 변수들은 jvm상에서 Method영역에 생성된다고 알고있다.

그러면 static inner 클래스도 Method 영역에 있을것인가?

아래 구문을 보면 IDE자체에서 에러가 나는 구문이다.

```
public static class Outer {
}
```
단순 일반 클래스에 static 키워드를 붙이면 에러가 난다. 
처음에 나는 class가 static으로 생성할 수 없다고만 생각했지만, 생각을 잘못하고 있었다.
class는 자바가 실행될때, class에 대한 정보를 이미 Method영역에 할당하고 있었던 것이다. 그래서 static 키워드를 붙여도 의미가 없어서 에러가 나는게 아닌듯 싶다.
다시, inner static 클래스는 그러면 멀까? 우선 inner static 클래스를 생성할때 new 키워드를 사용한다. 이것으로 볼때 new 키워드로 생성되는순간 jvm상에서는
heap 영역에 생성된다고 볼 수 있다.

우선, 이 글을 쓰게 된 이유는 Java에서 inner class를 사용하지만 자세히 알지 못하기 때문에 쓴다.
또한 회사에서 아래와 비슷한 코드를 봤는데 정확히 이해가 안가는 부분들이 있었다.
예를들면.. 아래와 같은 클래스
```
public class xxxDTO {
  public static class GetXXXDTO {
  }
  
  public static class PutXXXDTO {
  }
}
```
위와 같이 static 클래스로 DTO를 만들어 버리면 나중에 엄청많은 DTO가 생기면 OOM이 발생되지 않을까?
근데 만약에 된다면 왜 되는걸까? 
이게 궁금했다.

여러가지를 찾아보다가 내가 찾는 답을 얻지못해서 내 나름대로 정리해서 쓴 글입니다. 잘못된정보는 지적해 주시면 감사하겠습니다. 꼭!!

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
그러면 그냥 inner 클래스랑 왜 나뉘어져 있을까? 라는 생각을 할 수 있다. 우선 inner class를 Oracle 공식 Docs에서는 Nested Class라고 하는데 이 문서에서는 Nested Class를 사용하는 이유가, 비슷한 로직끼리 묶을라고, 캡슐화를 가능하게 하려고, 유지보수성을 향상시키려고... 라고 설명되있다. (영어가 서툴러서 오역이 있을 수 있음 참고한 링크: [여기](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)

그래서 보통 Spring을 예로들면 Dto안에 여러개의 static inner class를 볼 수 있다.
바깥의 Outer 클래스는 단순히 namespace역할을 한다.
혹시라도 static이 붙어있어서 thread safe하지 않을까라는 의문이 들 수도 있다. 
Spring을 예로들면, Bean으로 등록된 하나의 메소드안에서 생성이 되고 메소드 종료후 더이상 사용되지 않는다면 이것 역시 GC의 대상이 되기 때문에, 
사용해도 된다. 

글이 좀 주저리 주저리된거 같은데, 결론은 inner static클래스를 사용해도 메모리상 성능은 크게 차이 없을 것이다.


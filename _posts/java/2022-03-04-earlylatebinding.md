---
title:  "바인딩: Early binding and late binding"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
- java
tags:
- BackEnd
---

 자바에서 하위클래스의 객체를 상위 클래스를 받는 레퍼런스에 담을 수 있고

이 레퍼런스에 함수 실행을 할 경우에 상위 클래스의 함수가 아닌 하위클래스의 함수를 실행한다.

자주 쓰이는 Shape 클래스 예제로 보도록 하자.

```
abstract class Shape {
   private int x;
   private int y;

   Shape(int newx, int newy) {
      moveTo(newx, newy);
   }

   int getX() { return x; }
   int getY() { return y; }
   void setX(int newx) { x = newx; }
   void setY(int newy) { y = newy; }

   void moveTo(int newx, int newy) {
      setX(newx);
      setY(newy);
   }
   void rMoveTo(int deltax, int deltay) {
      moveTo(getX() + deltax, getY() + deltay);
   }

   abstract void draw();
}
```

Rectangle 클래스는 Shape 클래스를 상속한다.

```
class Rectangle extends Shape {
   private int width;
   private int height;

   Rectangle(int newx, int newy, int newwidth, int newheight) {
      super(newx, newy);
      setWidth(newwidth);
      setHeight(newheight);
   }

   int getWidth() { return width; }
   int getHeight() { return height; }
   void setWidth(int newwidth) { width = newwidth; }
   void setHeight(int newheight) { height = newheight; }

   void draw() {
      System.out.println("Drawing a Rectangle at:(" + getX() + ", " + getY() +
         "), width " + getWidth() + ", height " + getHeight());
   }
}
```

Shape를 받는 레퍼런스에 Rectangle 객체를 생성하여서 할당하고 draw() 함수를 실행시켜보자

```
public class MainFunc {
    public static void main(String[] args) {
        Shape shape = new Rectangle(1, 2, 3, 4);
        shape.draw();
    }
}
```

결과값은 다음과 같다.

```
> Task :MainFunc.main()
Drawing a Rectangle at:(1, 2), width 3, height 4
```

이게 가능한 이유는 자바가 Late binding을 이용하기 때문이다.

여기서 Binding은 메소드 콜과 메소드 정의를 연결해주는(mapping) 것을 의미한다.

자바에서 메소드 콜이 일어나면 프로그램은 메소드 정의(또는 메소드 바디)가 저장되어있는 메모리 주소 연걸해준다.

이 바인딩은 Early(static) binding과 Late(Dynamic) binding으로 나눌 수 있다.

Early binding은 컴파일에서 이루어진다.

Late binding은 런타임때 이루어진다.

자바에서 Early binding은 priavte, static, final과 같은 키워드를 쓸 때 일어난다.

그 외에 기본값으로는 Late binding이 일어난다.

리플렉션이나 메소드 오버라이딩이 Late binding의 좋은 예제이다.

이유는 자바의 객체지향인 언어 디자인 때문이다.

Early binding을 쓰면 Late binding에 비해서 일반적으로는 퍼포먼스가 좀 더 좋다.

Late binding을 쓰면 위의 예제처럼 객체를 더 유연하게 사용을 할 수 있다.

다른 타입들을 런타임에 적용할 수 있다는 것은 이에 연관된 코드를 덜 짜도 되게 만들어준다.










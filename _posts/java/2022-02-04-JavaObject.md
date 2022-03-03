---
title:  "자바 Object와 toString, equals, hashCode"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - java
tags:
  - BackEnd
---
  모든 자바의 클래스는 Object 클래스를 상속받는다.
  
새로운 클래스를 만들면 Object의 함수들을 Override 해야할 경우가 많이 생기는데

대표적인 함수들이 toString, equals, hashCode이다.

toString은 String 형태의 객체나 클래스를 반환하라는 함수이다.

예제는 다음과 같다.

```
public class Employee {
    Integer id;
    String name;
    Date hireDate;

    public Employee() {
    }

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", hireDate=" + hireDate +
                '}';
    }
}
```

```
public class Employee {
    Integer id;
    String name;
    Date hireDate;

    public Employee() {
    }

    @Override
    public String toString() {
        return "Employee{id=%d, name='%s', hireDate=%s}".formatted(id, name, hireDate);
    }
}
```

equal는 해당 객체나 클래스가 파라미터에 들어가는 객체나 클래스와 같은지

비교해달라는 함수이다.
```
@Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Employee)) return false;
        Employee employee = (Employee) o;

        if (id != null ? !id.equals(employee.id) : employee.id != null) return false;
        if (name != null ? !name.equals(employee.name) : employee.name != null) return false;
        return hireDate != null ? hireDate.equals(employee.hireDate) : employee.hireDate == null;
    }
```

hashCode는 O(1) 상수 시간복잡도로 해시테이블을 사용해야할 때 해쉬값을 만들어주는 함수이다.

```
@Override
    public int hashCode() {
        int result = id != null ? id.hashCode() : 0;
        result = 31 * result + (name != null ? name.hashCode() : 0);
        result = 31 * result + (hireDate != null ? hireDate.hashCode() : 0);
        return result;
    }
```

equals와 hashCode가 중요한 이유는 보통

자바에서 객체의 덩어리(Collection)을 표기하는 인터페이스인 Set 인터페이스를 사용하기 위함인데

Set은 중복된 값이 없고 순서가 없는 집합이다.

Set을 이용하는 이유는 해시 알고리즘으로 빠른 검색을 하기 위해서인데

이때 equals와 hashCode가 이용된다.

그래서 새로운 클래스를 만들 때 toString, equals, hashCode는 자바에서 자주 쓰이는

기능들을 위해서 Override 해주는 것이 좋다.

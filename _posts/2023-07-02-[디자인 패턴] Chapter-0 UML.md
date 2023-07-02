---
title: "[디자인 패턴] Chapter-0 UML"
date: 2023-07-02
---

### static 변수

- **프로세스가 메모리에 로드 되는 순간** 정적변수 영역(데이터 영역)에 생성되는 변수
- **객체 라이프 스타일과 다르게 접근할 수 있다.** (다른 라이프 스타일을 표현하기 위해서, 밑줄)
    - 즉, 객체가 생성되지 않았을 때에도 접근이 가능하다.

### 기울림꼴 = 추상 클래스와 인터페이스를 의미한다.

ex) *ParentClass*, *Printable*

- 필드 중에서 **static**만 인터페이스를 선언할 수 있다.
- 즉, static이 아닌 필드와 메서드가 있으면 인터페이스가 아니다.

점선 = 상속

실선 = 구현

### **aggregation** → 집약 = 위임했다. = 가지고 있다. (그 녀석한테 기능을 위임한다.)

Basket은 배열을 가지고 있다.

무조건 가지고 있으면 다이아몬드로 표현해준다.

### **객체** = “**속성**과 **동작**을 캡슐화한(가지고 있는) 것”

### 클래스 = 변수와 메서드를 가지고 있는 것

### 메서드와 함수 차이

- 동작의 주어가 없으면, 함수
    - 함수를 호출하는 객체가 없는 경우
- 동작의 주어가 있으면, 메서드
    - 함수를 호출하는 객체가 있는 경우를 메서드라고 말한다.

```java
public class Wallet {
	private int money; //속성은 드러날 일이 없다.

	public int getMoney() {
		return money;
	}

	public void setMoney(money){
		this.money = money;
	}
}

public int move(){
	return 1;
}

Wallet.getMoney(); //메서드
move(); //함수
```

### 접근 제어자

- private : inner class도 가능

```java
public class Wallet {
	public int money;
}

public class Staff {
	void clac(Product p, Wallet w){
		if(p.price <= w.money){
			//결제 기능
		}
	}
}
```

```java
public class Wallet {
	private int money; //속성은 드러날 일이 없다.

	public int getMoney() {
		return money;
	}

	public void setMoney(money){
		this.money = money;
	}
}

public class Staff {
	void clac(Product p, Wallet w){
		if(p.price <= w.getMoney()){
			//결제 기능
		}
	}
}
```

```java
public class Wallet {
	private int money; //속성은 드러날 일이 없다.

	public void pay(Product p){
		if(p.price > this.money){
			throw new Exception();	
		}
}

public class Staff {
	void clac(Product p, Wallet w){
		w.pay(p);
	}
}
```

### 클래스의 속성을 private로 선언해서 getter, setter를 사용하는 이유

속성은 노출될 일이 없다. → 다른 곳에서 알아서는 안된다. → 직접 속성을 노출하지 말아야 한다.

→ 동작이 있으니까, 속성이 숨겨져 있다.

→ 동작만 드러나게 되어 있다.

→ 속성은 거의 대부분 private라서 속성의 접근 제어자를 이해할 필요가 없고, 동작만 여러 접근 제어자가 필요하다.

- **privateMethod** = **서브 프로시저** → 작게 할 때 ⇒ 코드를 간결하게 할 때
- **protectedMethod** = 같은 패키지에서 사용해라. → **상속해서 사용하라**는 것을 의미한다.
- **publicMethod** = **내가 공개한 API**

### 시퀀스 다이어그램

- 각각의 **역할**이 가장 중요하다.
    - ex) :Client, :Server, :Device
    - **실선은 요청, 점선은 응답**
        - 이것만 기억하면, 굳이 흐름의 번호를 붙이지 않아도 쉽게 이해할 수 있다.
    - 네모는 라이프 사이클을 의미한다.
        - 만약에 점선을 받아도 네모가 끝나지 않으면 아직 해당 라이프 사이클이 끝나지 않았음을 의미한다.

# 221215 내부클래스와 익명클래스
https://harutocoding.tistory.com/185

<h3>내부클래스의 용도</h3>
<ol>
  <li>중요한 정보를 캡슐화해서 접근 방식에 제한을 둘 수 있다. 내부클래스를 사용하기 위해서는 반드시 외부클래스를 사용해야 하기 때문이다.</li>
  <li>외부클래스 안에 내부클래스가 있다는 포함관계를 확실히 한다.</li>
</ol><br>
<h3>내부클래스의 종류</h3>
<p>변수처럼 생각하자!</p>
<ol>
  <li>instance class : 외부클래스의 멤버변수 위치</li>
  <li>static class : 외부클래스의 멤버변수 위치</li>
  <li>local class : 외부 클래스의 메서드나 초기화 블럭 안에 위치 </li>
  <li>anonymous class : 클래스의 선언 + 객체의 생성 동시에 하는 이름없는 클래스. 일회용.</li>
</ol><br>

```
class Outer {			        // 외부 클래스
    class InstanceInner { }	    	// 인스턴스 클래스 (내부)
    static class StaticInner { }	// 스태틱 클래스 (내부)

    void myMehtod {
        class LocalInner { }  		// 로컬 클래스 (내부)
    }
}
```

# 익명클래스
<ul>
  <li>클래스의 선언과 객체의 생성을 동시에 한다.</li>
  <li>단 한번만 사용되고, 오직 하나의 객체만을 생성 가능하다.</li>
  <li>한번만 사용될 거라 이름이 없고, 조상이나 인터페이스의 이름을 사용한다.</li>
  <li>그래서 어떤 조상/인터페이스를 참조했는지를 확실히 알 수 있다.</li>
  <li>인터페이스는 객체 생성이 불가하므로, 인터페이스를 상속받은 무명클래스(=하위클래스)를 정의한 뒤 형변환하는 방식으로 진행해야 한다.</li>
</ul><br>

<h3>익명클래스 문법</h3>
<table>
  <tr>
    <td>new 조상클래스이름( ) { <br>
         // 멤버 선언<br>
        }
    </td>
    <td>new 구현인터페이스이름( ) { <br>
         // 멤버 선언<br>
        }
    </td>
  </tr>
</table>

```
interface i1  { void m1() }

class Ano implement i1 {
        @override
        void m1() {}
}

i1 i1 = new i1() {
        @override
        public void m1() {
                // 재정의 했어요!
        }
}
```

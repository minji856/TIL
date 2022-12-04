# 221202 React.js

https://harutocoding.tistory.com/167

# 리액트란?
- 하나의 파일 안에 html, js, css를 묶어서 component로 만든다.
- 공통형식의 태그를 미리 구현하고, 다른 부분은 매개변수로 전달하여 처리한다.
- 재사용이 가능한 라이브러리이다.
- SPA : single page application 이다.

**환경 설정**
1. node.js 설치
2. VSCode 설치
3. cmd 명령문 실행 : 티스토리 참고
4. npm start  !!!!!!!!!!!!!!!!

node.js란?
- 서버이다.
- 우리는 톰캣 서버, 노드 서버 있다. 
- 노드는 리액트 전용으로 사용한다.

<br><br>

# JSX (return)
- 확장된 자바스크립트 문법이다.
- 소문자 태그를 사용한다.
- 반드시 닫는 태그가 있어야 한다.
- 두 단어가 합쳐진 것이라면 단어 앞 글자는 모두 대문자로 적는다. (ex. MyApp, BackgroundcColor)
- 주석은 {/* */} 형태로 적는다. return 외부는 //로 적는다.
- 변수는 { str1 } 형식으로 적는다.

- return 할 때 반드시 하나의 태그 안에 내용이 들어가야 한다. <div></div>로 감싸면 됨
- 태그가 많다면 return ```(<div><h1></h1><p></p><img src="logo192.png" alt="react"/></div>);``` 처럼 ( 괄호 ) 안에 넣는다.
- return 문 안에서 if문 혹은 반복문 사용 불가하다. html 내에서 실행할 수 없기 때문이다.
- 삼항연산자를 사용하자.
<br><br>
**스타일 지정**
```
<h1 style={{color:"blue", border:"2px solid pink"}}>첫번째 방법</h1>

let style1 = {color:"pink", backgroundColor:"silver"}; 				
<h1 style={style1}>두번째 방법</h1>		
```
<br><br>
**함수 호출하면서 값 전달**
```
import Calculator from './Calculator';

root.render(
  <React.StrictMode>
    <Calculator su1="10 su2="20"/>
  </React.StrictMode>
);
```
```
function Calc(param) {      // <Calculator su1="10" su2="10"/>
    // param.su1 = 10 꺼낼 수 있다.
    let su1 = parseInt(param.su1);
    let su2 = parseInt(param.su2);

    return (<div>
        <h1 style={style1}>{su1} + {su2} = {su1+su2}</h1>
    </div>)
}
```
<br><br>

# 리액트 컴포넌트의 기본 문법
```
import React from "react";

class Employee extends React.Component{
    render() {
        return();
    }
}
```

<br><br>
# 리액트에서 이벤트 처리하는 법
- 리액트는 jsx라서 DOM 구조가 없다. 이벤트가 발생한 그 객체는 ev 라는 파라미터를 받아서 표현하자.
**함수 방식으로 정의**
```
const GreetingClass = () => {
    const welcome = () => {alert("안녕하세요!")}
    return(<input type="button" value="인사" onClick={welcome}/>);
}
```


**class 방식으로 정의**
```
class GreetingClass extends React.Component{
    render() {
        const welcome = () => { alert("안녕하세요!")}
        return(<input type="button" value="인사" onClick={welcome}/>);
    }
}
```







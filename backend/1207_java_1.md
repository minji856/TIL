# 221207 자바의 개요

https://harutocoding.tistory.com/172

# 배운 것
1. 자바의 역사
2. 변수
3. 환경설정
4. 연산자
5. 조건문
<br />

**모든 내용은 티스토리에 있습니다.**
**이 문서에서는 System.out.printf, if-else, switch 코드를 다룹니다.**

<br/><br/><hr/><br/>
# System.out.printf
1. format에 맞춰 출력한다.
2. d인 이유는 decimal(10진수)의 첫번째 글자라서 그렇다!!! int의 i로 해 주지 아쉽

<br>


```
String location = "새싹 용산캠퍼스";
String title = "클라우드 기반 웹서버 과정";
int week = 3;

System.out.printf("%d을 표현합니다. \n", 10);
System.out.printf("%f을 표현합니다. \n", 3.14);
System.out.printf("%c을 표현합니다.", 'a').println();
System.out.printf("%s을 표현합니다.", "name").println();
System.out.printf
("우리는 %s에서 %s수업을 %n%d주 동안 열심히 들어요. \n새나라의 아이들.\n", location, title, week);
```
```
10을 표현합니다. 
3.140000을 표현합니다. 
a을 표현합니다.
name을 표현합니다.
우리는 새싹 용산캠퍼스에서 클라우드 기반 웹서버 과정수업을 
3주 동안 열심히 들어요. 
새나라의 아이들.
```
<br><br><br><br>

# if-else와 switch
- 이름, 국어점수, 영어점수, 수학점수, 합계, 평균 점수를 받아서 printf로 출력하자.
- if-else와 switch 두 가지로 실행한다.
- 평균이 100점 이하 90점 이상이라면 A, 90 미만 80 이상이라면 B, 80미만 이상이라면 C, 70미만 60이상이라면 D, 60점 미만이라면 낙제를 준다.

**if-else**
```
  String grade = "";

  if (avgInt >= 90 && avgInt <= 100) {
    grade = "A등급";
  } else if (avgInt >= 80) {
    grade = "B등급";
  } else if (avgInt >= 70) {
    grade = "C등급";
  } else if (avgInt >= 60) {
    grade = "D등급";
  } else {
    grade = "낙제";
  }
```

**switch**


```
  String grade2 = "";
  
  switch (avgInt/10) {
  case 10 : 
  case 9 : {
    grade2 = "A등급";
    break;
  }
  case 8 : {
    grade2 = "B등급";
    break;
  }
  case 7 : {
    grade2 = "C등급";
    break;
  }
  case 6 : {
    grade2 = "D등급";
    break;
  }
  default:
    grade2 = "낙제";
  }
```









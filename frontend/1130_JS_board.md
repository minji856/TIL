# 221201 boardlist 과제 내용

https://harutocoding.tistory.com/162

**배운 것**
1. 자바스크립트의 함수
2. 자바스크립트의 객체
3. 자바스크립트의 배열
<br><br>

**어려웠던 점**
1. 객체에 값을 넣는 것. 버튼을 누르면 인풋값이 객체로 전달되고 객체가 생성되어야 한다.
2. 1번의 객체는 배열의 요소로 저장되어야 한다.
3. 배열에 저장된 요소를 반복문을 통해 list의 형태로 출력해야 한다.
<br><br>

**해결방법**
1. 객체는 new 객체명(매개변수)로 만드는 걸 잊지 말자.
2. 배열에 저장하기 위해서는 배열명.push(값)의 형태를 사용해야 한다.
3. **1번과 2번의 과정을 합해서, boardlist.push(new Emp(seq, title, contents)); 의 형태로 코드를 작성한다.**
4. 배열의 i번째 요소는 배열명[i]로 꺼낸다.
5. 배열의 요소는 배열명.요소명으로 꺼낸다.
6. **4번과 5번의 과정을 합해서, boardlist[i].seq 와 같은 형태로 작성한다.**
7. 값을 모두 꺼내야 하는데, for in 이라는 array 함수를 사용한다.
8. **for(index in boardlist) { boardlist[i].seq } 와 같은 형태로 작성하면 해당 인덱스의 해당 요소들이 반환된다.**
9. 5개 이상 만들면 더이상 추가 불가능하다 -> boardlist.length <= 4 로 조건을 걸었다. 
<br><br>
 
**추가사항**
1. 조회 버튼 이벤트에 innerHTML = " "; 추가. 조회 버튼을 연속해서 누르면 조회 결과도 연속해서 떴다. html을 비우고 다시 만들기 위한 코드였다.
<br><br>


**결과물**
<img width="707" alt="image" src="https://user-images.githubusercontent.com/107450834/204934877-6c4ba260-d0fa-4f91-ab45-9768741895f9.png">


```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="/js/jquery/jquery-3.6.1.min.js"></script>
<script type="text/javascript">
	$(document).ready(function() {
		function Board(seq, title, contents){
			this.seq = seq,
			this.title = title,
			this.contents = contents
			this.time = new Date().toLocaleDateString();
		}
	
	let boardlist = [];
	
	// 게시물 저장 함수
	// 배열 갯수
	document.getElementById("save").onclick = function() {
		let seq1 = document.getElementById("seq").value;
		let title1 = document.getElementById("title").value;
		let content1 = document.getElementById("contents").value;
		
		if(boardlist.length <= 4) {				// index
			let new_board = new Board(seq1, title1, content1);
			boardlist.push(new_board);
			console.log(boardlist);
		} else {
			document.getElementById("saveresult").innerHTML = "게시물의 추가 저장이 불가능합니다. 리스트를 출력하세요.";
		}
	}
	
	// 게시물 조회 함수
	document.getElementById("output").onclick = function() {
		document.getElementById("list").innerHTML = "";
		for(i in boardlist) {
			document.getElementById("list").innerHTML += boardlist[i].seq + " : " + boardlist[i].title + " : " + boardlist[i].contents + " : " + boardlist[i].time + "<br>";
		}
	}
	});
</script>
<style>
	table, tr, td, th {
		border: solid black 1px;
		border-collapse: collapse;
	}
</style>
</head>
<body>
	번호:<input type=text id="seq">
	제목:<input type=text id="title">
	내용:<input type=text id="contents">
	<input type=button id="save" value="게시물저장">
	<div id="saveresult"></div>
	<input type=button id="output" value="게시물조회">
	<div id="list"> 5개 게시물 입력 결과 출력<br> </div>
</body>
</html>
```

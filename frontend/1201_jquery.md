# 221202 JQUERY의 함수와 이벤트
https://harutocoding.tistory.com/164


**오늘 배운 것**
1. 제이쿼리 선택자 연습
2. 배열로 값 뽑아서 넣는 연습[i[
3. 함수들 .attr(), .html(), .val(), .prop() ... 
4. 정규표현식 연습
5. 이벤트 처리 $("").on("click", function() {...} );
<br><br>

키코드 관련 : event1.html<br>
e.preventDefault(); , 정규표현식 관련 : a_submit.html, 과제들(allcheck, idcheck, idinput)
<br><br><br><br>

# 체크박스 선택과 해제
- 우리는 제이쿼리의 이벤트를 $("요소").on("이벤트", function() {} ); 와 같은 식으로 적을 것이다.
- 전체 선택 체크박스(#all)이 체크되지 않은 상태에서 체크하면 모든 체크박스들이 체크된다. #all이 체크된 상태에서 체크하면 모든 체크박스들이 체크 해제된다.
- #all을 제외한 체크박스들이 모두 체크된다면 #all도 체크한다. 만약 체크박스들 중 하나라도 체크가 해제된다면 #all의 체크를 해제한다.
- ("#text1").val() 의 값을 들고 온다. 만약 아무것도 입력되지 않아 ''가 반환된다면, submit 이벤트를 막는다.
- ("input:checkbox:checked").length를 조회한다. 만약 길이가 0이라면 submit 이벤트를 막는다.
- 위의 두 조건 중 하나라도 만족하지 않는다면 submit 이벤트를 막는다.
<br><br>
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
	<!-- 모두 제목 내용 작성자 중 한개 이상 선택하고 검색어를 입력한 다음에 검색 버튼 클릭하면 search.jsp로 전송된다
	    1>이 때 모두를 선택시 제목, 내용, 작성자 자동으로 모두 선택되어야 하고
	    모두 선택 해제시 제목, 내용, 작성자 자동으로 모두 선택해제되도록 구현한다
	    2> 만약 제목 내용 작성자 가운데 아무 것도 선택되지 않거나 검색어 입력되지 않으면 search.jsp로 전송되지 않도록 구현한다
	 -->
<script src="jquery-3.6.1.min.js"></script>
<script type="text/javascript">
	$(document).ready(function() {
		$("#all").on('click', function() {
			if($("#all").prop("checked")) {
				$("input[name=chbox]").prop("checked", true);
			} else {
				$("input[name=chbox]").prop("checked", false);
			}
		})
	
		$("input[name=chbox]").on("click", function() {
			let total = $("input[name=chbox]").length;
			let checked = $("input[name=chbox]:checked").length;
			if (total == checked) {
				$("#all").prop("checked", true);
			} else {
				$("#all").prop("checked", false)
			}
		})
		
		$("#searchform").on("submit", function(e) {
			if ($("#text1").val() == "" || $("input:checkbox:checked").length == 0) {
				alert("검색조건을 선택한 후 검색어도 입력해 주세요!")
				e.preventDefault();
			}
		})
	
	});
</script>
</head>
<body>
<h1>검색 대상과 검색어를 입력하세요.</h1>
  <form action="search.jsp" id="searchform">
    <input type=checkbox value="모두" id="all" name="all">모두
    <input type=checkbox value="제목" id="title" name="chbox">제목
    <input type=checkbox value="내용" id="contents" name="chbox">내용
    <input type=checkbox value="작성자" id="writer" name="chbox">작성자
    <input type=text id="text1" placeholder="검색어를 입력하세요!" />
    <input type=submit value="검색" />
  </form>
</body>
</html>


```

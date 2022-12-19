# 221219 Colletion, Compare, Stack 알고리즘

https://harutocoding.tistory.com/191



# list1과 list2의 데이터를 비교하라 

```
package test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class SameSentence {
	String[] compare(List<String> list1, List<String> list2){
	// list1과 list2 저장 데이터 갯수 비교한다
	// 갯수 일치하면 list1, list2 모든 데이터를 각 인덱스별 비교하며, 동일하면 String 배열 저장하고 리턴
	// 갯수 일치하지 않으면 list1, list2 더 작은 갯수의 list 갯수만큼 데이터를 각 인덱스별 비교하며, 동일하면 String 배열 저장하고 리턴
		
		int size = list1.size() < list2.size() ? list1.size() : list2.size();
		ArrayList<String> arrList = new ArrayList();
		
		if(list1.size() == list2.size()) {
			for(int i = 0; i < list1.size(); i++) {
				if(list1.get(i).equals(list2.get(i))) {
					System.out.println("같습니다");
					arrList.add(list1.get(i));
				}
			}
		} else {
			for(int i = 0; i < size; i++) {
				if(list1.get(i).equals(list2.get(i))) {
					System.out.println("다릅니다");
					arrList.add(list1.get(i));
				}
			}
		}
		String[] arr = new String[arrList.size()];
		arrList.toArray(arr);
		
		return arr;
	}
}

public class SameSentenceTest {
	public static void main(String[] args) {
		ArrayList<String> list1 = new ArrayList<String>();
		list1.add("자바는 객체지향 언어입니다");
		list1.add("우리는 용산캠퍼스에서 자바를 배우는 중입니다");
		list1.add("오늘은 컬렉션 프레임워크를 배우죠! ");
		list1.add("내일은 스레드를 배울 예정입니다 ");
		
		ArrayList<String> list2 = new ArrayList<String>();
		list2.add("자바는 객체지향 언어입니다");
		list2.add("우리는 청계천에서 자바를 배우는 중입니다");
		list2.add("오늘은 콜렉션 프레임워크를 배우죠! ");
		list2.add("내일은 스레드를 배울 예정입니다 ");
		
		ArrayList<String> list3 = new ArrayList<String>();
		list3.add("자바는 객체지향 언어입니다");
		list3.add("우리는 용산에서 자바를 배우는 중입니다");
		list3.add("오늘은 컬렉션 프레임워크를 배우죠! ");
		
		SameSentence ss = new SameSentence();
		System.out.println("list1, list2에서 같은 내용을 출력합니다");
		System.out.println(Arrays.toString(ss.compare(list1, list2)));
		
		System.out.println("================================");
		
		System.out.println("list1, list3에서 같은 내용을 출력합니다");
		System.out.println(Arrays.toString(ss.compare(list1, list3)));

	}
}
```


<br><br>
# Stack 괄호 맞추기 알고리즘
**1. EmptyStackException 사용 방법**

```
public class ExpText2 {
	public static void main(String[] args) {
		String expressions [] = {
				"(5-(2+3)*2)/4", "(2+3)*1)+3", "((2+3)*1)+3", "(5-(2+3)*2)/4)"};
		Stack st = new Stack();
		// 만약 pop할 게 없다면 확인 후 break 시키고 괄호불일치 문장 출력
		// 만약 짝괄호가 없다면 확인 후 break 시키고 괄호불일치 문장 출력
		// catch(EmptyStackException e) 이거 썼는데 오류 발생하는 순간 코드가 종료돼서 배열을 더 받을 수 없다

		for(int i = 0; i < expressions.length; i++) {
			for (int j = 0; j < expressions[i].length(); j++) {
				if (expressions[i].charAt(j) == '(') {
					st.push('(');
				} else if (expressions[i].charAt(j) == ')') {
					try {
						st.pop();
					} catch (EmptyStackException e) {
						st.push(')');
					}
				}
			}
			if(st.isEmpty()) {
				System.out.println(expressions[i]);
			}
			st.clear();
		}
	}
}
```
<br><br><br>
**2. size() 사용방법**
```
public class ExpText {
	public static void main(String[] args) {
		String expressions [] = {
				"(5-(2+3)*2)/4", "(2+3)*1)+3", "((2+3)*1)+3", "(5-(2+3)*2)/4)"};
		Stack st = new Stack();
		for(int i = 0; i < expressions.length; i++) {
			String str = expressions[i];
			for (int j = 0; j < str.length(); j++) {
				char ch = str.charAt(j);
				if (ch == '(') {
					st.push(ch);
				} else if (ch == ')' && st.size() != 0) {
					st.pop();
				} else if (ch == ')') {
					st.push(ch);
				}
			}
			if(st.size() == 0) {
				System.out.println(expressions[i]);
			}
			st.clear();
		}
	}
}
```

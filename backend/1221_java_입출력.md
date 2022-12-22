# 221222 자바의 
<br>
<h3>scanner, while, if문을 사용한 계산기 만들기</h3>
<br>

```
package test;

import java.util.ArrayList;
import java.util.Scanner;

class Calcu {
	int su1, su2;
	String operator;
	
	public Calcu(int num1, int num2, String operator) {
		super();
		this.su1 = num1;
		this.su2 = num2;
		this.operator = operator;
	}

	@Override
	public String toString() {
		int result = 0;
		if (operator.equals("+")) {
			result = su1 + su2;
		} else if (operator.equals("-")) {
			result = su1 - su2;
		} else if (operator.equals("*")) {
			result = su1 * su2;
		} else if (operator.equals("/")) {
			result = su1 / su2;
		}
		return su1 + operator + su2 + "=" + result;
	}
}

public class Calculator {

	public static void main(String[] args) {
		ArrayList<String> menu = new ArrayList<>();
		menu.add("덧셈");
		menu.add("뺄셈");
		menu.add("곱셈");
		menu.add("나눗셈");
		menu.add("프로그램 종료");
		
		while(true) {
		System.out.println("계산기를 시작합니다. \r\n"
				+ "종료하려면 q를 입력하세요. \r\n");
		
		for(int i = 0; i < menu.size(); i++) {
			System.out.println((i+1) + "." + menu.get(i));
		}
		System.out.println();
		System.out.println("선택번호 입력: ");
		
		Scanner sc = new Scanner(System.in);
		
		String num = sc.next();
		if(num.equals("5") || num.equals("q") | num.equals("exit")) {
			System.out.println("프로그램이 종료되었습니다.");
			break;
		} else {
			System.out.println(menu.get(Integer.parseInt(num)-1) + "할 데이터1 입력 : ");
			int num1 = sc.nextInt();
			System.out.println(menu.get(Integer.parseInt(num)-1) + "할 데이터2 입력 : ");
			int num2 = sc.nextInt();
			String operator = null;
			if (num.equals("1")) {
				operator = "+";
			} else if(num.equals("2")) {
				operator = "-";
			} else if(num.equals("3")) {
				operator = "*";
			} else if(num.equals("4")) {
				operator = "/";
			}
			Calcu calcu = new Calcu(num1, num2, operator);
			System.out.println(calcu);
			System.out.println();
		}
		}
	}
}
```
<br><br>
<h3>scanner로 입력받은 것을 student.txt로 출력한다.</h3>
느낀 점 : 입출력 메소드는 정말 많다
        : buffer를 잘 사용하면 효율적인 코드를 짤 수 있

```
package test;

import java.io.BufferedInputStream; 수많은 stream
import java.io.BufferedOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.OutputStream;
import java.io.OutputStreamWriter;
import java.io.PrintStream;
import java.io.PrintWriter;
import java.io.Serializable;
import java.util.ArrayList;
import java.util.Scanner;

class Student implements Serializable {
	String name;
	int kor;
	int math;
	int eng;
	int totalScore;
	double avg;
	
	public Student(String name, int kor, int math, int eng) {
		super();
		this.name = name;
		this.kor = kor;
		this.math = math;
		this.eng = eng;
		this.totalScore = kor + math + eng;
		this.avg = (double)(kor + math + eng)/3;
	}
	
	@Override
	public String toString() {
		return "Student [이름=" + name + ", 국어=" + kor + ", 수학=" + math + ", 영어=" + eng + ","
				+ " 총점="+ totalScore + ", 평균=" + avg + "]";
	}
	
}
public class ScoreRunner {

	public static void main(String[] args) throws IOException, ClassNotFoundException {

		Scanner sc = new Scanner(System.in);
		
		System.out.println("학생정보관리시스템입니다.");
		System.out.println("이름을 입력하세요.");
		String name = sc.next();
		System.out.println("국어성적을 입력하세요.");
		int kor = sc.nextInt();
		System.out.println("수학성적을 입력하세요.");
		int math = sc.nextInt();
		System.out.println("영어성적을 입력하세요.");
		int eng = sc.nextInt();

		Student student = new Student(name, kor, math, eng);
		System.out.println(student);
		sc.close();

		// 방법 1
		// 그런데 뒤에 append가 안 돼서 앞에 게 계속 사라짐
//		PrintWriter pw = new PrintWriter("student.txt");
//		pw.print(student);
//		pw.close();
		
//		 방법 2
//		 File, FileWriter
//		 Student 객체의 toString을 재정의했다. 
//		 .write는 String을 받는다. student.toString을 매개변수로 보냈다.
//		 여기서 student라고만 했을 때 toString()이 자동으로 되지 않는다.
//		 !!! print에서만 toString()이 자동으로 된다. !!!
		File file = new File("student.txt");
		FileWriter fw = new FileWriter(file, true);
		fw.write(student.toString());
		fw.close();
		
		// 방법3
		// student 객체 자체를 outPut한다.
		// 객체를 스트림에 사용하기 위해선느 직렬화(serialization)을 해야 한다.
		// 직렬화를 쉽게 말하자면 객체를 byte 형태로 변환하는 것이다.
		// JVM의 메모리에 상주되어 있는 객체 데이터를 바이트로 변환한다.
		// 역직렬화는 바이트로 변환된 데이터를 객체로 변환시켜서 JVM으로 상주시키는 형태이다.
		// 이를 위해서는 class에 implements Serializable 해야한다.
		
		// 객체 스트림이란?
		// 프로그램 메모리상에 존재하는! 객체를 직접 입출력해줄 수 있는 스트림이다. 영속성(?)을 제공한다.
		// 자바에서 객체 안의 내용을 파일로 저장하거나 네트워크를 통해 다른 곳으로 전송하려면 일일히 바이트로 분해해야 한다.
		// 이를 대신 하는 것(?)이 객체 스트림이다.
		
		/*
		 * https://okky.kr/articles/224715
		 * 간단하게 이야기 드리면 서버가 다중화(여러개존재) 되어 있고 세션 클러스터링을 통해 세션관리를 하는 환경에서 도메인 객체가 세션에 저장이 될때 
		 * 도메인 객체에 Serializable 인터페이스 클래스를 구현하기(implements) 해야지 정상적으로 세션에 저장하고 꺼내올수 있기 때문입니다. 
		 * 
		 * */
		
	
// class Student implements Serializable
// 방법 3. 객체 전달
//		File file = new File("student.txt");
//		
//		FileOutputStream fos = new FileOutputStream(file, true);
//		BufferedOutputStream bos = new BufferedOutputStream(fos);
//		ObjectOutputStream oos = new ObjectOutputStream(bos);
//		oos.writeObject(student);
//		oos.writeUTF(student.toString());		// UTF면 뭐해 객체 전달이 아니라 String을 전달받는 걸 ... 아닌가 이렇게 하는 게 맞나 ... ?
//		
//		oos.close();
//		bos.close();
//		fos.close();

	}

}


```


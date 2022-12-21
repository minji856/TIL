# 람다식과 멀티스레드

https://harutocoding.tistory.com/193
https://harutocoding.tistory.com/194
<br>
<h3>멀티스레드</h3>

```
package test;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.function.Function;

//1개 서버 - 다수개 클라이언트 요청 - 처리
public class Server {
	public static void main(String[] args) {
    // 네 개의 스레드를 동시에 실행한다.
		LoginClient c1 = new LoginClient("java", "java");
		LoginClient c2 = new LoginClient("java", "1234");
		RegisterClient c3 = new RegisterClient(new Date());
		BoardClient c4 = new BoardClient();
		
		c1.start();
		c2.start();
		c3.start();
		c4.start();
		
		// 리턴타입, 매개변수 없는 람다식 사용
    // 날짜 date 형식 표현 3
		Thread c5 = new Thread( () -> {System.out.println("람다스레드");});
		c5.start();
		
		// 리턴타입, 매개변수 모두 있는 람다식 사용
		// 날짜포맷과 매개변수를 전달하면 날짜형식이 완성되는 형태
		Function<String, String> mydate = str -> new SimpleDateFormat(str).format(new Date());
		String now =  mydate.apply("yyyy년도 MM월 dd일");
		System.out.println(now);
	}
}
//1.멀티스레드클래스 정의
//2.생성자 정의/ 필드변수 정의
class LoginClient extends Thread{
	String id, pw;
	
	public LoginClient(String id, String pw) {
		this.id = id;
		this.pw = pw;
	}
	
	@Override
	public void run() {
		System.out.println("로그인 아이디 " + id + "를 입력받습니다");
		System.out.println("로그인 암호를 확인합니다");
		if(pw.equals("java")) {
			System.out.println("로그인 암호 맞습니다");
		} else {
			System.out.println("로그인 암호 올바르지 않습니다");
		}
	}
}

class RegisterClient extends Thread{
	String dateStr;
	
	public RegisterClient(Date date) {
//		방법 1
//		SimpleDateFormat formatter = new SimpleDateFormat("yyyy년도 MM월 dd일"); 
//		String format = formatter.format(date);
//		this.date = format;
		
//		방법 2
		dateStr = new SimpleDateFormat("yyyy년도 MM월 dd일").format(date);
	}

// 0.5초마다 실행되도록 한다.
// Thread.sleep을 사용해서 0.5초동안
	@Override
	public void run() {
		System.out.println(dateStr + "에 회원가입요청합니다. \n회원가입요청 처리중입니다.");
		try {
    // 3초간 중지했다가 다시 실행되게 한다.
			Thread.sleep(3000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("회원가입요청 처리완료입니다");
	}

}

class BoardClient extends Thread{
	@Override
	public void run() {
		for(int i = 1; i <= 5; i++) {
			System.out.println("게시물 " + i + "번 작성합니다.");
			try {
      // 0.5초동안 멈췄다 실행되기를 반복되게 한다.
				Thread.sleep(500);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}
}

```













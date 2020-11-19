### Notice
if-else 는 하드코딩? <br/>
Try-catch는 어떤 에러인지 확인 가능
<br/>

```java
package exeption;
import java.util.InputMismatchException;
import java.util.Scanner;

/* Execption 정리
 * 1. input 값이 4인 경우 종료시킨다. (Done)
 * 2. input 값이 정수가 아닌 경우 Throw (Done)
 * 4. 영화 추가 시 movie array를 넘어가는 인덱스가 입력될 경우 Throw (Done)
 * 5. 영화 추가 시 영화 장르와 등급 선택 과정에서 1~4의 값이 입력되지 않는 경우 Throw(Done)
 * 6. 4,5번에서 input 값이 정수가 아닌 경우 Throw (Done)
 * 7. 영화 제거 시 제거할 영화 인덱스가 array의 index를 초과할 경우 Throw (Done)
 * 8. 영화 추가 시에 exception이 발생하면 추가되지 않는다. (Done)
 */

public class Ex08 {
	private static String[] movie = new String[5];
	private static String[] movie_type = new String[5];
	private static int[] movie_age = new int[5];
	private static String[] type = { "액션", "스릴러", "멜로", "공포" };
	private static int[] age = { 9, 12, 15, 18 };
	static Scanner sc = new Scanner(System.in);
	
	public static void main(String[] arg) {
	int menu = 0;
	
	for (int i = 0; i < 5; i++) {	// 초기화
		movie[i] = "";
		movie_type[i] = "";
		movie_age[i] = 0;
	}
	
	// 종료 보안 관리자 설정
	System.setSecurityManager(new SecurityManager(){
		@Override
		public void checkExit(int input) {
			if(input != 4) {
	          throw new SecurityException(); // input이 4라면 프로세스 종료
	        }else {
	          System.out.println("시스템 종료!");
	        }
		}
    	}
	);
	
	while (menu != 4) {
		try {
			System.out.println("");
			System.out.println("영화관 관리 프로그램 ");
			System.out.println("1.조회");
			System.out.println("2.영화 추가");
			System.out.println("3.영화 제거");
			System.out.println("4.종료");
			System.out.print("입력 : ");
			menu = sc.nextInt();
			
			if (menu == 1) {
				view();
			} else if (menu == 2) {
				try {
					insert();
				}catch(Exception ex) {}
			} else if (menu == 3) {
				try {
					delete();
				}catch(Exception ex) {}
			} else if (menu == 4) {
				try {
					System.exit(menu);
				}catch(SecurityException e) {}
			}
		}catch(InputMismatchException ex){	// Input이 integer가 아닌 경우 catch
			System.err.printf("%nException: %s%n", ex);
			sc.nextLine();
			System.out.printf("You must enter integers. Please try again.%n%n");
		}
	}
	
	}
	
	public static void view() {
		System.out.println("상영관\t\t 영화 제목\t\t 영화 장르\t 영화 등급");
		
		for (int i = 0; i < 5; i++) {
			if (movie[i] == "") {
				continue;
			} else {
				System.out.print(i + 1 + "관 \t\t");
				System.out.print(movie[i] + "\t\t");
				System.out.print(movie_type[i] + "\t");
				System.out.print(movie_age[i] + "\t");
			}
			System.out.println();
		}
	}
		
	public static void insert() {
		int insert = 0;
		System.out.println("비어 있는 상영관");
		
		for (int i = 0; i < 5; i++) {
			if (movie[i] == "") {
				System.out.print(i + 1 + " ");
			}
		}
		
		try {
			System.out.println();
			System.out.print("영화를 추가할 상영관 입력 : ");
			insert = sc.nextInt();
			checkNumber2(insert);
		}catch(InputMismatchException ex){	// Input이 integer가 아닌 경우 catch
			System.err.printf("%nException: %s%n", ex);
			sc.nextLine();
			System.out.printf("You must enter integers. Please try again.%n%n");
		}catch(Exception ex) {
			movie[insert-1]="";
			System.out.printf("%nException: 1에서 5 사이의 값을 입력하세요%n");
		}
		
		if (movie[insert - 1] != "") {
			System.out.println("해당 상영관에 상영 중인 영화가 있습니다. 다시 작업해 주세요");
		}else {
			sc.nextLine();
			System.out.print("영화 제목 : ");
			movie[insert - 1] = sc.nextLine();
			
			try {
				System.out.print("영화 장르 (1.액션, 2.스릴러, 3.멜로, 4.공포) : ");
				int data = sc.nextInt();
				checkNumber(data);
				movie_type[insert - 1] = type[data - 1];
				System.out.print("영화 등급 (1.9, 2.12, 3.15, 4.18 ): ");
				data = sc.nextInt();
				checkNumber(data);
				movie_age[insert - 1] = age[data - 1];
				System.out.println("등록이 완료 되었습니다.");
			}catch(InputMismatchException ex){	// Input이 integer가 아닌 경우 catch
				sc.nextLine();
				System.out.printf("You must enter integers. Please try again.%n%n");
			}catch(Exception ex) {
				movie[insert-1]="";
				System.out.printf("%nException: 1에서 4 사이의 값을 입력하세요%n");
			}
		}
	}
	
	public static void delete() {
		int delete = 0;
		System.out.println("상영중인 상영관");
		
		for (int i = 0; i < 5; i++) {
			if (movie[i] != "")
			System.out.print(i + 1 + " ");
		}
		
		System.out.println();
		try {
			System.out.print("영화를 제거할 상영관 입력 : ");
			delete = sc.nextInt();
			checkNumber2(delete);	
		}catch(Exception ex){
			System.out.printf("%nException: 1~5사이의 상영중인 상영관을 입력하세요%n");
		}
		
		
		if (movie[delete - 1] == "") {
			System.out.println("해당 상영관은 비어 있습니다.");
		} else {
			movie[delete - 1] = "";
			movie_type[delete - 1] = "";
			movie_age[delete - 1] = 0;
			System.out.println("제거 완료 되었습니다.");
		}
	}
	
  // 이렇게 Exception 을 Throws하는 함수를 만들 수 있음
	public static void checkNumber(int input) throws Exception{
		if(input>4||input<0) {
			throw new Exception();
		}
	}
	
	public static void checkNumber2(int input) throws Exception{
		if(input>5||input<0) {
			throw new Exception();
		}
	}
}

```

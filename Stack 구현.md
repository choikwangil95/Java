## Stack
2차원 배열을 stack으로 구현하고 matrix의 요소를 stack으로 변경하고 출력한다

```java
// ArrayStack 클래스

public class ArrayStack {
	int stack_top;	// 객체들이 다른 값을 가져야 하니, static ㄴㄴ
	int[] stack;
	int size;
	
	public ArrayStack(int size) {	// 생성자로 변수 초기화
		stack_top = -1;
		stack = new int[size];
		this.size = size;
	}
	
	public int top() {
		if (stack_top==-1) {
			System.out.println("Empty!");
		}
		return stack[stack_top];
	}
	
	public void push(int value) {	// 객체에서 호출해야 하니까 static ㄴㄴ
		if (stack_top==size) {
			System.out.println("Full!");
		}
		
		stack[++stack_top] = value;
	}
	
	public int pop() {
		if (stack_top==-1) {
			System.out.println("Empty!");
		}
		return stack[stack_top--];
	}
}
```

```java
// matrix

package stack;
import java.util.Scanner;
import stack.*;

public class matrix {
	public static void main(String[] args) {
		
		int i, j, number, change, store, save, stack_top, peak;
		String is;
		int[] array = {51, 22, 62, 27, 72, 85, 38, 2, 46, 9, 75, 21, 83, 61, 5, 13};
		ArrayStack stack = new ArrayStack(16);
		
		for(int element: array) {
			stack.push(element);
		}
		
		stack_top = stack.stack_top;
		
		Scanner in = new Scanner(System.in);
		System.out.print("행렬 A는 다음과 같다 ");
		System.out.printf("%n");
		
		print_matrix(stack);
		System.out.print("행렬 A는 다음과 같다 ");
		
		while(true) {
			System.out.print("행렬 A에서 바꾸고 싶은 원소 A의 행의 위치 i를 입력: ");
			i = in.nextInt();
			System.out.print("행렬 A에서 바꾸고 싶은 원소 A의 열의 위치 j를 입력: ");
			j = in.nextInt();
			
			if (i < 1 || i > 4 || j < 1 || j > 4) {
				System.out.printf("4X4 matrix 입니다. 1에서 4 사이의 값을 입력하세요 %n");
				continue;
			}
			
			number = (4-i)*4 + (4-j);
			
			while(true) {
				
				if(stack.stack_top == number) {
					System.out.printf("%n선택한 원소는 %d 입니다%n", stack.top());
					peak = stack.top();
					break;
				}else {
					stack.pop();
					continue;
				}
			}
			
			System.out.println("맞으면 yes를 입력하고, 다시 입력하고 싶으면 no를 입력하세요 : ");
			is = in.next();
			
			if (is.equals("yes")) {
				System.out.printf("%d를 선택하셨습니다%n", peak);
				System.out.print("바꾸고 싶은 원소를 입력하세요: ");
				change = in.nextInt();
				stack.stack[stack.stack_top] = change;
				
				System.out.printf("바꾼 원소를 적용하여 다음과 같이 행렬을 갱신했습니다. %n");
				stack.stack_top = stack_top;
				print_matrix(stack);
				
				System.out.print("더 이상 변경할 원소가 없으면 yes를 입력하고, 변경할 원소가 있다면 no를 입력하세요: ");
				is = in.next();
				
				if (is.equals("yes")) {
					System.out.print("프로그램이 종료되었습니다.");
					break;
				} else {
					continue;
				}
				
			}else if (is.equals("no")) {
				continue;
			}
		}
		
		
		
	}

	static void print_matrix(ArrayStack matrix) {
		
		int k = 0;
		int setup = matrix.stack_top;
		
		while(true) {
			
			if (matrix.stack_top == -1) {
				matrix.stack_top = setup;
				break;
			}
			
			k += 1;
			System.out.printf("%d ", matrix.pop());

			if (k%4 == 0) {
				System.out.printf("%n");
			}
		}
	}	
}

```

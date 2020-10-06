## 클래스 상속
클래스 상속을 통해 상속 없이 구현한 코드를 튜팅해보자

### without Inheritance
```java
// manager.java
package virtualMachine;

public class manager {
	int cpu;
	int ram;
	int disk;
	int number=0;
	virtualmachine[] list = new virtualmachine[10];
	
	public manager(int cpu, int ram, int disk) {
		this.cpu = cpu;
		this.ram = ram;
		this.disk = disk;
	}
	
	public void createVM(String name, int cpu, int ram, int disk) {
		this.cpu -= cpu;
		this.ram -= ram;
		this.disk -= disk;
		
		virtualmachine vm = new virtualmachine(name, cpu, ram, disk);
		list[number]=vm;
		number++;
	}
	
	public void deleteVM(String name) {
		for (int i=0; i<number; i++) {
			if(list[i].name.equals(name)) {
				this.cpu += list[i].cpu;
				this.ram += list[i].ram;
				this.disk += list[i].disk;
				
				for(int j=i; j<number; j++) {
					if(j == number-1) {
						break;
					}
					list[j] = list[j+1];
				}
				break;
			}
		}
		this.number--;
	}
	
	public void printVM() {
		for(int i=0; i< this.number; i++) {
			System.out.printf("  %s   %d     %d     %d%n", list[i].name, list[i].cpu, list[i].ram, list[i].disk);
		}
	}
}
```

```java
// virtualmachine.java
  package virtualMachine;

public class virtualmachine {
	String name;
	int cpu;
	int ram;
	int disk;
	
	public virtualmachine(String name, int cpu, int ram, int disk) {
		this.name=name;
		this.cpu=cpu;
		this.ram=ram;
		this.disk=disk;
	}
}
```

```java
// main.java

package virtualMachine;
import java.util.Scanner;

public class main {
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		System.out.printf("가상머신 관리자가 처음 실행되었습니다. 가상 자원을 초기화 하십시오. 가상 머신 생성에 할당할 자원을 설정하십시오%n");
		System.out.print("CPU는 몇 개를 설정할까요 (현재 0개 남음)? ");
		int cpu = in.nextInt();
		System.out.print("RAM은 몇 GB를 설정할까요 (현재 0GB 사용가능, 단위: GB)? ");
		int ram = in.nextInt();
		System.out.print("DISK는 몇 GB를 설정할까요 (현재 0GB 사용가능, 단위: GB)? ");
		int disk = in.nextInt();
		
		manager manager = new manager(cpu, ram, disk);
		System.out.printf("가상머신 생성을 위한 자원 설정이 완료되었습니다.");
		
		while(true) {
			System.out.printf("%n%nName | CPU | RAM | DISK%n");
			if(manager.number > 0) {
				manager.printVM();
				System.out.printf("%n");
			}
			
			System.out.printf(">> 가상머신 관리자 화면입니다.%n%n");
			System.out.printf("현재 CPU %d개, RAM %dGB, DISK %dGB 가 사용가능 합니다. 현재 구동중인 가상머신은 %d개 입니다.%n",
				manager.cpu, manager.ram, manager.disk, manager.number);
			System.out.print("어떤 작업을 하시겠습니까? (1: 가상머신 생성, 2: 가상머신 삭제, 3: 가상머신 목록, 4: 종료)");
			
			int number;
			number = in.nextInt();
			
			if(number < 0 || number > 4) {
				continue;
			}
			
			if(number == 4) {
				System.out.print("프로그램을 종료합니다.");
				break;
			}
			
			switch(number) {
				case 1:
					if (manager.cpu <= 0 ) {
						System.out.print("할당 가능한 CPU 자원이 없습니다. 가상머신을 생성할 수 없습니다.");
						break;
					}
					if (manager.ram <= 0) {
						System.out.print("할당 가능한 RAM 자원이 없습니다. 가상머신을 생성할 수 없습니다.");
						break;
					}
					if (manager.disk <= 0) {
						System.out.print("할당 가능한 DISK 자원이 없습니다. 가상머신을 생성할 수 없습니다.");
						break;
					}
					System.out.print("가상머신을 생성합니다.");
					System.out.print("가상머신의 이름은 무엇입니까?");
					String name = in.next();
					
					System.out.printf("CPU는 몇 개를 할당할까요 (현재 %d개 남음)? ", manager.cpu);
					int get_cpu = in.nextInt();
					if (manager.cpu < get_cpu ) {
						System.out.print("할당 가능한 CPU 자원이 없습니다. 가상머신을 생성할 수 없습니다.");
						break;
					}
					
					System.out.printf("RAM은 몇 GB를 할당할까요 (현재 %dGB 사용가능)? ", manager.ram);
					int get_ram = in.nextInt();
					if (manager.ram < get_ram) {
						System.out.print("할당 가능한 RAM 자원이 없습니다. 가상머신을 생성할 수 없습니다.");
						break;
					}
					
					System.out.printf("DISK는 몇 GB를 할당할까요 (현재 %dGB 사용가능)? ", manager.disk);
					int get_disk = in.nextInt();
					if (manager.disk < get_disk) {
						System.out.print("할당 가능한 DISK 자원이 없습니다. 가상머신을 생성할 수 없습니다.");
						break;
					}
									
					manager.createVM(name, get_cpu, get_ram, get_disk);
					System.out.print("가상머신이 생성되었습니다.");
					break;
				
				case 2:
					if (manager.number == 0) {
						System.out.print("삭제할 가상머신이 없습니다.");
						break;
					}else {
						System.out.print("삭제할 가상머신의 이름은 무엇입니까? ");
						String deleteName = in.next();
						System.out.printf("가상머신 %s 이 삭제되어 자원이 회수되었습니다.", deleteName);
						manager.deleteVM(deleteName);	// 이 함수 수정 필요
						break;
					}
				
				case 3:
					break;
			}
		}
	}
}
```

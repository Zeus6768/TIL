# 클래스와 멤버 변수, 지역변수

```java
public class Student {

	public int studentNumber;
	public String studentName;
	public int grade;
	
	public String showStudentInfo() {
		return studentName + "학생의 학번은 " + studentNumber + "이고 " + grade + "학년입니다.";

	}
}
```

<br>

위에서 `studentNumber`, `studentName`, `grade`는 멤버 변수이다. 
멤버 변수는 초기화를 하지 않아도 null, 0 등으로 컴파일러가 알아서 기본 생성자를 통해 초기화를 해준다. 

하지만 만약 showStudentInfo() 메소드에 다음과 같이 변수를 추가한다고 하면, 

<br>

```java
public String showStudentInfo() {
    int i;

    return studentName + "학생의 학번은 " + studentNumber + "이고 " + grade + "학년입니다." + i;
	}
```

<br>

이는 지역 변수이므로 초기화가 되지 않아 오류가 나게 된다. 

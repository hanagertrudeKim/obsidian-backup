
# Big 3

## 복사 생성자
-  

## 복사 대입 연산자 
- 

## 소멸자

# others

## 이동 생성자 
- 항상 `noexcept` 로 수식
	- 효과가 충분히 있기위해 반드시 필요
- move-and-swap idiom

## 이동 대입 연산자


## 복사 생략
- copy elision
- 컴파일러가 최적화 과정에서 불필요한 생성자의 호출 제거함



## Rule of Zero
-  BIg 5 직접 정의 x => 클래스 설계
- how
	1. 사용할수없도록 만들기
	2. 진짜 필요없도록
		-  ex) vector 사용

## problem
### 1. find the first nonrepeated character
```ad-question
Write an efficient function to find the first nonrepeated character in a string. For instance, the first nonrepeated character in “total” is 'o' and the first nonrepeated character in “teeter” is 'r'. Discuss the efficiency of your algorithm.
```
- 반복되지 않는 첫번째 글자를 찾는 문제

my solution
- 가장 빠른 문자열을 찾아야하므로, 일단 처음 문자를 기준으로 모든 문자에 똑같은 문자가 있는지 검사
- 있다면 다음 문자로 넘어감
- 없다면 return 해당 문자

고려 사항
- 모두가 중복되는 문자일때도 고려
- 문자가 1글자일경우 => 그냥 Return 문자

```c++
char findNonrepeatedChar(std::stirng string){
	if(string.length <= 1) return string

	for(int i = 0; i < string.length; ++i){
		for(int j = 0; j < string.length; ++j){
			if(string[i] == string[j]){
				break;
			} 
			if(j == string.length - 1) return string[i];
		}
	}
	return error?
}
```
-> 최악의 경우로 알고리즘을 짠것같다. 이렇게 되면 o(n^2)의 시간복잡도를 가짐

===> 더 나은 solution
1. 배열이나 해시 테이블을 활용할수있도록 해라 
2. 해시 테이블을 이용한다면
	1. 문자카운트 해시 테이블을 생성
		1. => 해시 테이블을 쓰는 이유는???
		2. => c++ 에서는 어케 쓰이는거냐
	2. 마지막 해시 테이블에서 카운트가 1인것을 반환

```c++
char firstNonRepeated(const std::string& str) {
    std::unordered_map<char, int> charHash;
    int length = str.length();
    char c;

    // 문자열을 스캔하며 해시 테이블 작성
    for (int i = 0; i < length; i++) {
        c = str[i];
        if (charHash.find(c) != charHash.end()) {
            // c에 대한 카운트 증가
            charHash[c]++;
        } else {
            charHash[c] = 1;
        }
    }

    // 원래 문자열의 문자 순서로 해시 테이블 검색
    for (int i = 0; i < length; i++) {
        c = str[i];
        if (charHash[c] == 1)
            return c;
    }

    return '\0'; // 더 이상 반복되지 않은 문자가 없는 경우
}
```
=> 예상 시간복잡도 O(n);


### 2. Remove specified charicter
```ad-question
Problem Write an efficient function that deletes characters from an ASCII string. Use the prototype

string removeChars( string str, string remove );

where any character existing in remove must be deleted from str. For example, givenastrof"Battle of the Vowels: Hawaii vs. Grozny"andaremoveof "aeiou",thefunctionshouldtransformstrto“Bttl f th Vwls: Hw vs. Grzny”. Justify any design decisions you make, and discuss the efficiency of your solution
```
- remove string에 있는 문자들을 str에서 다 제거해야 하는 문제


my solution
- 어케 풀거냐
	- remove string을 char 로 쪼개서 순환
	- 그 안에 str을 순회하면서 삭제

```c++
string removeChars(string str, string remove){
	for(int i = 0; i < remove.length; ++i){
		char removeChar = remove[i];
		for(int j = 0; j < str.length; ++i){
			if(removeChar == str[j]){
				str.remove(i, i);
			}
		}
	}
	return str;
}
```
- 이렇게 되면 시간복잡도는 O(n) 아니면 O(n^2) 이 될듯함

===> 더 나은 solution
1. 모든 str 배열을 false로 설정
2. 제거 문자열을 반복하면서 검사 => 검사 걸리면 true로 바꾸기
3. false인 문자열들을 다른 result array에 복사하기
4. return result array

```c++
#include <iostream>
#include <unordered_set>

std::string removeChars(const string& str, const string& remove) {
    std::unordered_set<char> removeSet(remove.begin(),remove.end());
    std::string result;

    for (char c : str) {
	    //c요소 not found => end() 반환
        if (removeSet.find(c) == removeSet.end()) {
            result.push_back(c);
        }
    }

    return result;
}
```
- string 참조로 넘겨주는 이유
	- 복사값을 줄이려고
	- 원본 수정 가능
- std::unordered_set
	- hash table기반 
		- 요소의 검색, 삭제, 추가 시간복잡도는 O(1)
	- 중복된 값을 허용하지않음 => 각 요소는 유일
	- begin(), end()로 반복 순환 가능
	- 동적 크기 조절

### 3. Reverse Words
```ad-question
Write a function that reverses the order of the words in a string. For example, your function should transform the string "Do or do not, there is no try." to "try. no is there not, do or Do". Assume that all words are space delim- ited and treat punctuation the same as letters.
```
- 띄어쓰기 단위로 끊어서 문자열 뒤집기 문제

 my solution
- 반복자 돌려서 띄어쓰기 부분 찾기
- 띄어쓰기 나오면 => 문자열 시작부터 띄어쓰기까지 끊기
	- 다음 자르기를 위해 startidx 갱신
- => 끊은 문자열 Result 문자열 맨 앞에 붙이기
- return 문자열
```c++
string reverseWord(const string str&){
	string result;
	int startIdx = 0;
	for(int i = 0; i < str.length(); ++i){
		//띄어쓰기 찾기
		if(str[i] == " " || i == str.length()-1){
		  string word = str.substr(startIdx, i);
		  startIdx = i + i;
		  //word를 result 앞에 붙임
		  result.insert(result.begin(), word.begin(), word.end());
		}
	}
	if(result.length() == 0)
		return str;
	return result;
}
```
===> 더 나은 solution
- 뒤에서부터 스캔함
	- 순서가 뒤부터 진행됨으로 굳이 insert를 할 필요가 없음
	- => 좀 더 효율적이다.
- 띄어쓰기가 나오면 =>  띄어쓰기 복사
- 문자 나오면 => 다음 띄어쓰기가 나올때까지 문자 그대로 복사
- 반복

```c++
void reverseWords(char str[]) {
    int length = strlen(str);
    int tokenReadPos = length - 1;
    int writePos = 0;

    while (tokenReadPos >= 0) {
	    //띄어쓰기 나오면 그대로 복사
        if (str[tokenReadPos] == ' ') {
            str[writePos++] = str[tokenReadPos--];
        } else {
            int wordEnd = tokenReadPos;
            // 띄어쓰기 만나기 전까지, 위치 --index;
            while(tokenReadPos >= 0 && str[tokenReadPos] != ' ')
            {
                tokenReadPos--;
            }
            int wordStart = tokenReadPos + 1;

			// wordStart부터 wordEnd 까지 string문자열 앞에 추가하기
            while (wordStart <= wordEnd) {
                str[writePos++] = str[wordStart++];
            }
        }
    }
	//마지막 null 포인트 넣어서 문자열 끝이라는걸 알려야함..
    str[writePos] = '\0';
}
```
=> 이렇게 되면, o(n) 시간복잡도


### 4. Integer/String Conversions
```ad-question
Write two conversion routines. The first routine converts a string  
to a signed integer. You may assume that the string contains only digits and the minus character ('-'), that it is a properly formatted integer number, and that the number is within the range of an int type. The second routine converts a signed integer stored as an int back to a string.
```
- 두가지의 conversion 루틴 정의
	- int to string
	- string to int

- string to int
```c++
int strToInt(const std::string& str) {
    int i = 0, num = 0;
    bool isNeg = false;
    int len = str.length();

    if (str[0] == '-') {
        isNeg = true;
        i = 1;
    }

    while (i < len) {
        num *= 10;
        num += (str[i++] - '0');
    }

    if (isNeg) {
        num = -num;
    }

    return num;
}

int main() {
    // 테스트
    std::string str1 = "137";
    std::string str2 = "-42";

    int result1 = strToInt(str1);
    int result2 = strToInt(str2);

    std::cout << "Result 1: " << result1 << std::endl;  // Output: 137
    std::cout << "Result 2: " << result2 << std::endl;  // Output: -42

    return 0;
}
```

- int to string
```c++
const int MAX_DIGITS = 10;

std::string intToStr(int num) {
    int i = 0;
    bool isNeg = false;

    // Buffer big enough for largest int and - sign
    char temp[MAX_DIGITS + 1];

    // Check to see if the number is negative
    if (num < 0) {
        num = -num;
        isNeg = true;
    }

    // Fill buffer with digit characters in reverse order
    do {
        temp[i++] = static_cast<char>((num % 10) + '0');
        num /= 10;
    } while (num != 0);

    std::string result;
    if (isNeg) {
        result += '-';
    }

    while (i > 0) {
        result += temp[--i];
    }

    return result;
}

int main() {
    // 테스트
    int num1 = 137;
    int num2 = -42;

    std::string result1 = intToStr(num1);
    std::string result2 = intToStr(num2);

    std::cout << "Result 1: " << result1 << std::endl;  // Output: 137
    std::cout << "Result 2: " << result2 << std::endl;  // Output: -42

    return 0;
}
```


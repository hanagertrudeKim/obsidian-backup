
## 1.  완주하지 못한 선수
###### 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.
마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.
##### 제한사항
- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.
##### 입출력 예
| participant | completion | return |
| ---- | ---- | ---- |
| ["leo", "kiki", "eden"] | ["eden", "kiki"] | "leo" |
| ["marina", "josipa", "nikola", "vinko", "filipa"] | ["josipa", "filipa", "marina", "nikola"] | "vinko" |
| ["mislav", "stanko", "mislav", "ana"] | ["stanko", "ana", "mislav"] | "mislav" |

### Solution
해싱은 찾기 연산에 O(1)로 시간복잡도를 최소화 할 수 있다.
해싱 함수를 이용하여서 선수 이름을 찾고 없으면 return하면 좋을것같다.
그리고 해싱은 충돌에 유념해야하기 때문에, 충돌이 될만한 동명이인 케이스를 조심하자.

중복인 케이스가 있으므로 c++ stl 의 unordered_multiset을 사용함
participant 배열을 순회하여 mutiset에 넣기
multiset에 completion 인원을 찾기
있으면 => multiset에서 삭제하기
없으면 => Return

테스트 케이스 4개정도 실패했다.
예외 처리가 안된 부분은 어디일까 생각해보니

1. 동명이인이 여러명일경우 erase를 쓰면 dataset에 존재하는 모든 동명이인이 지워진다.
	1. => key 대신에 iterator 를 전달하면 하나만 지워진다.

solve code
```c++
#include <string>
#include <vector>
#include <unordered_set>

using namespace std;

string solution(vector<string> participant, vector<string> completion) {
    unordered_multiset<string> allParticipant;
    string answer = "";
    
    if(participant.size() < 1) return "";
    
    for(int i=0; i < completion.size(); ++i){
        allParticipant.insert(completion[i]);
    }
    
    for(int i=0; i < participant.size(); ++i){
        if (allParticipant.find(participant[i]) != allParticipant.end()){
            allParticipant.erase(allParticipant.find(participant[i]));
        } else {
            answer = participant[i];
        }
    }
    
    return answer;
}
```

```ad-note
해싱연산의 삽입, 삭제, 찾기 연산은 O(1) 이다.

multiset에서 중복된 값을 지우고싶을땐,
1. key값을 넘겨주면 key의 모든 data가 삭제
2. key의 iterator를 넘겨주면 하나의 data만 삭제
```

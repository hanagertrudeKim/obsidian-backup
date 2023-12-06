---
tags:
  - linux
---

## linux 명령어

```python
$ pwd #현재 디렉토리 확인

$ ls #현재 디렉토리 폴더, 파일 보여줌

$ mkdir #디렉토리 생성

$ cd #디렉토리 이동 (상위 폴더 이동 $ cd ..)

$ cp file1 file1_cp #디렉토리 카피 (폴더 카피시 cp -r folder folder_cp)

$ mv testfile dir1/ #디렉토리 이동 (폴더 이동시 mv -r folder )

$ rm #삭제 (폴더 삭제 rm -r folder)

$ touch file2 #업데이트 일자를 현재 시간으로 변경
```
- options
`-f` : force로 강제 실행
`-r` : 서브디렉토리를 포함한 모든 내용을 재귀적으로 실행
`-i` : 파일 삭제 전에 사용자 확인하도록 설정


## linux alias 설정

alias 명령어 : 명령어를 단축어를 설정해주는 쉘내부 명령어
```python
$alias #현재 설정된 alias 모두 확인

$alias gic = "git clone" #alias 명령어 설정

$unalias gic #gic이라는 alias 명령어 삭제

```
** 주의해둘점이, alias 설정은 다시 프로그램 재실행하면 초기화됨 => ~/.bashrc 파일에서 건들여주는게 나음 
=> [alias 영구등록](https://coding-factory.tistory.com/800) 참고하기


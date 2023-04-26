# CLI 기본 명령어 (2)

<br>
<br>

## 기본 명령어

<br>

### less: 파일을 열어서 탐색
```bash
less [file_name]
```
less는 한 번에 한 화면 페이지씩 텍스트 파일을 필터링하고 보는데 사용합니다.  
전체 파일을 로드하지 않고도 매우 긴 텍스트 파일을 세그먼트해 읽을 수 있습니다.  
파일을 열어서 탐색 가능하며, q를 눌러 빠져나올 수 있습니다.

<br>

### head: 파일의 앞 10줄을 출력
```bash
head [file_name]
head -n 5 [file_name] # 앞 5줄을 출력합니다.
head -5 [file_name] # 앞 5줄을 출력합니다.
```
head는 파일의 앞 10줄을 출력합니다.  
옵션으로 원하는 줄의 갯수를 지정할 수 있습니다.

<br>

### tail: 파일의 뒤 10줄을 출력
```bash
tail [file_name]
tail -n 5 [file_name] # 뒤 5줄을 출력합니다.
tail -5 [file_name] # 뒤 5줄을 출력합니다.
```
tail는 head와 반대로 파일의 뒤 10줄을 출력합니다.  
tail 또한 옵션으로 줄 수를 지정할 수 있습니다.

<br>

### wc: 파일과 출력의 줄/단어/글자 수를 출력
```bash
wc [file_name]
```
word count의 약자입니다.  
파일 내부에 텍스트 줄, 단어, 글자가 얼마나 있는지를 보여줍니다.  
출력은 줄, 단어, 글자 순서대로 나타납니다.

<br>

### >: 파일에 출력 내용 덮어 쓰기
```bash
head [file_name] > [new_file_name or existing_file_name]
```
등호의 왼쪽에 있는 출력을 등호의 오른쪽에 입력한 파일에 저장합니다.  
새로운 파일 이름으로 입력했다면 등호 왼쪽 출력을 내용으로 담은 새로운 파일이 생성되며, 기존에 있던 파일 이름으로 입력했다면 기존 내용은 모두 지워지고 새로운 출력이(등호 왼쪽 출력) 내용에 덮어 쓰이게 됩니다.  
기존에 있던 파일 내용을 유지하고 추가하기 원한다면 다음 명령어를 사용하면 됩니다.

<br>

### >>: 파일 끝에 출력 내용 추가
```bash
tail [file_name] >> [existing_file_name or new_file_name]
```
등호의 왼쪽에 있는 출력을 등호의 오른쪽에 입력한 파일 끝에 추가합니다.  
기존에 있던 파일의 내용은 유지되며, 단지 왼쪽의 출력이 끝에 추가됩니다. 등호 오른쪽에는 기존에 있던 파일 이름뿐만 아니라, 새로 생성하고자 하는 파일 이름을 입력해도 파일이 생성됩니다.

<br>

### grep: 특정 문자열 패턴을 검색해 출력
```bash
grep [option] [pattern] [file_name]
grep -i 'time' test.txt
# test.txt 파일에서 대소문자 상관 없이 'time'이 들어가 있는 라인을 모두 찾습니다.
```
`grep`은 파일의 내용에서 특정 문자열 패턴을 찾고자 할 때 사용하는 명령어입니다.  
기본적으로 대소문자를 구분함에 주의하세요.  

자주 사용되는 옵션은 아래와 같습니다.
```bash
-c : 문자열이 포함된 라인 개수를 표시
-i : 문자열의 대소문자를 구분하지 않음
-h : 파일 이름을 출력하지 않음
-l : 문자열이 일치한 파일의 이름만 출력
-v : 입력한 패턴이 포함되지 않은 문자열을 출력
-r : 서브 디렉터리의 파일까지 모두 출력
-n : 일치한 문자열이 포함된 라인 번호를 출력
-w : 입력한 문자열이 독립된 단어로 존재하는 경우만 출력
```
`grep --help`를 사용하면 더 많은 옵션을 볼 수 있습니다.

<br>

### |(파이프): 기호 왼쪽 표준 출력을 오른쪽 명령어 뒤에 붙이기
```bash
cat test.txt | head -4
# cat 명령어로 test.txt의 파일의 출력값이 파이프 오른쪽으로 넘어가고, 그 값의 앞부분 4줄이 출력됩니다.
cat test.txt | head -4 | wc
# cat 명령어로 test.txt의 파일의 출력값이 파이프 오른쪽으로 넘어가고, 그 값의 앞부분 4줄이 다시 파이프 오른쪽으로 넘어가, wc 명령어를 만나게 됩니다.
# 최종적으로 앞부분 4줄에 해당하는 줄, 단어, 글자 수를 출력합니다.
```
`|`(파이프:pipe)는 둘 이상의 명령을 결합하는 데 도움을 주며 명령어에서 입력/출력 개념으로 사용됩니다.  
파이프 여러개를 사용해 연쇄적으로도 사용 가능합니다.

<br>

### diff: 두 파일의 차이를 출력
```bash
diff [file_name1] [file_name2]
```
differences의 약자입니다.  
파일 2개를 비교해 일치하지 않는 라인을 출력합니다.

<br>

### ps: 실행중인 프로세스의 목록 출력
```bash
ps 
ps [option]
ps aux | grep 'django'
# 현재 실행중인 프로세스 중에서 'django' 라는 문자열이 있는 프로세스만 보여줍니다.
```
process 약자입니다.  
현재 실행중인 프로세스 목록을 보여줍니다.

<br>

### kill: 실행중인 프로세스 종료
```bash
kill [OPTIONS] [PID]
```
`kill`은 실행중인 프로세스를 pid를 통해 종료할 수 있는 명령어입니다.

<br>
<br>
<br>

## Reference
- [Linux Pipe Command with Examples](https://linuxhint.com/linux-pipe-command-examples/)  
- [grep 예제와 옵션](https://madplay.github.io/post/grep-command-example-options)
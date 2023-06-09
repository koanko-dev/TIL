# CLI 기본 명령어 (1)

<br>

## 준비
### macOS - 터미널(Terminal) 앱 실행하기
macOS를 사용한다면 추가로 설치하지 않아도 기본적으로 터미널 앱이 내장되어 있습니다.  
이 터미널과 같은 프로그램을 사용해 컴퓨터와 소통하는 방식을 CLI(Command-Line Interface)라고 합니다. 우리가 편하게 컴퓨터에서 그래픽을 보며 하는 모든 작업들은, 이 CLI 환경에서도 모두 할 수 있습니다.  
단지, 화면을 클릭하는 행위가 텍스트를 입력하는 행위로 바뀔 뿐 똑같이 작동합니다.

<br>

**실행하는 방법**
- `command` + `spacebar` 를 누르면 나오는 Spotlight 검색창에 Terminal을 검색해 앱을 실행합니다.  
또는
- 하단의 dock에서 런치패드를 클릭 후, Terminal 앱을 찾아 실행합니다.
  
프로그램이 실행되면 아래와 같은 화면이 뜰 것입니다.

|![terminal initial screen](./img/initial.png)|
|:--:|
|터미널 앱 초기 화면|

<br>
<br>

## 기본 명령어

<br>

### pwd: 현재 위치 출력
```bash
pwd
```
print/present working directory의 약자입니다. 디렉토리는 폴더를 의미합니다.  
현재 작업중인 디렉토리 위치를 출력합니다.

<br>

### ls: 파일 목록 출력
```bash
ls
```
list의 약자입니다.  
현재 위치해있는 디렉토리의 파일 목록을 보여줍니다.

옵션을 붙여 사용할 수 있습니다.  
```bash
ls -a # all의 약자로, 숨김파일 또는 폴더를 모두 보여줍니다.
ls -l # long의 약자로, 파일 목록을 상세하게 보여줍니다.
ls -t # time의 약자로, 수정한 시간 역순으로 보여줍니다.(=최신순 나열)
```
옵션의 경우 혼합하여 한번에 사용할 수 있습니다.  
```bash
ls -alt # 모든 파일을 상세하게 시간 역순으로 보여줍니다. 옵션을 입력하는 순서는 상관없습니다.
```

<br>

### cd: 디렉토리 이동(변경)
```bash
cd [이동할 디렉토리 이름 or 경로]
```
change directory의 약자입니다.  
`cd` 뒤에 이동할 곳을 입력하면 위치기 해당 디렉토리로 변경됩니다.

<br>

### touch: 파일 생성
```bash
touch [지정할 파일 이름]
```
`touch` 뒤에 `test.txt`를 입력하면 test.txt 파일이 현재 디렉토리에 생성됩니다.

<br>

### mkdir: 디렉토리 생성
```bash
mkdir [지정할 디렉토리 이름]
```
make directory의 약자입니다.  
`mkdir` 뒤에 `test_dir`를 입력하면 test_dir 디렉토리가 현재 디렉토리에 생성됩니다.

-p 옵션을 사용하면 디렉토리를 생성하는 동시에 하위의 디렉토리를 한번에 생성할 수 있습니다.

```bash
mkdir -p test_dir/c1/c2/c3
# test_dir가 현재 디렉토리에 생성되고 c1, c2, c3가 차례대로 하위폴더로 생성됩니다.
```

<br>

### mv: 파일 이동 및 이름 변경
```bash
mv [이동할 파일 이름] [이동할 디렉토리]
```
move의 약자입니다.  
파일이 해당 디렉토리로 이동하게 됩니다.

`mv` 명령어로 파일의 이름을 변경할 수도 있습니다.
```bash
mv [변경 전 파일 이름] [변경 후 파일 이름]
mv before.txt after.txt
# before.txt 파일이 after.txt로 이름이 변경됩니다.
```

<br>

### cp: 파일 복사 / 디렉토리 복사
```bash
cp [원본 파일 이름] [복사된 파일 이름]
```
copy의 약자입니다.  
새로 지정한 이름으로 파일이 복사됩니다.

디렉토리를 복사하는 방법은 다음과 같습니다.
```bash
cp -r [디렉토리 이름] [디렉토리 이름]
# 디렉토리 내부를 순환하면서 내부의 모든 파일을 함께 복사합니다.
```

<br>

### rmdir: 디렉토리 삭제
```bash
rmdir [삭제할 디렉토리 이름]
```
remove directory의 약자입니다.  
해당 디렉토리가 삭제됩니다. 디렉토리 내부에 파일이 있다면 삭제되지 않습니다.  
파일을 가지고 있는 디렉토리를 삭제하려면 다음에 소개되는 명령어를 참고하면 됩니다.

<br>

### rm: 파일 삭제 / 파일이 있는 디렉토리 삭제
```bash
rm [삭제할 파일 이름]
```
remove의 약자입니다.  
해당 파일이 삭제됩니다.

파일이 있는 디렉토리 삭제하는 방법 또한 `rm`을 사용합니다.
```bash
rm -r [삭제할 디렉토리 이름]
# 파일이 있는 디렉토리를 삭제할 때 사용합니다. 내부의 파일들까지 모두 삭제됩니다.
```

<br>

### echo: 표준 출력
```bash
echo 'hello world'
# hello world
```
`echo`는 뒤에 오는 문자열을 출력합니다.  
python의 print()와 비슷하다고 생각하면 됩니다.
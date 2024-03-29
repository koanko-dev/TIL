# "Watch Mode” & 전체 프로젝트 컴파일하기

## "Watch Mode” 사용하기

타입스크립트 파일을 작성하고 매번 `tsc <file-name>.ts` 명령어를 치는 건 번거로운 일입니다.

Watch Mode를 사용하면, 타입스크립트 파일에 변경사항이 있을 때마다 자동으로 저장할 수 있습니다.
```bash
tsc app.ts --watch
# or
tsc app.ts -w
```
Watch Mode의 단점은 특정한 파일만을 대상으로 한다는 것입니다. 큰 프로젝트에서는 문제가 됩니다.  
프로젝트 내부의 모든 타입스크립트 파일을 한꺼번에 컴파일 할 수는 없을까요?

<br/>

## 프로젝트 전체를 컴파일하기 (여러 ts 파일 컴파일)

특정한 파일이 아니라 프로젝트 폴더를 보고, 변경사항이 있으면 그 안에 있는 모든 타입스크립트 파일을 컴파일 할 수는 없을까요?

그렇게 하기 위해선, 먼저 아래 명령어로 초기화해야 합니다.

```bash
tsc --init
```

명령어를 입력하면 tsconfig.json 파일이 생성됩니다.  
이 파일은 타입스크립트 컴파일 관련 설정을 하는 파일입니다.

이런 초기화는 프로젝트를 시작할 때 한번만 실행하면 됩니다. 이 명령어는 특정 파일을 가리키는 게 아니라 그냥 tsc를 실행합니다.

명령어를 입력할 때 위치했던 폴더가 프로젝트 폴더로 지정되고, 그 폴더 안에 있는 타입스크립트 파일들을 관리하게 될 겁니다.

이제 특정한 파일 명을 입력하는 대신, 아래의 명령어로 폴더 내부의 모든 타입스크립트 파일을 한번에 컴파일 할 수 있습니다.

```bash
tsc
```

Watch Mode와 연결해서 사용할 수도 있습니다.

```bash
tsc --watch
# or
tsc -w
```

<br/>
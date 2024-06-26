### 데이터 카테고리 / 다양한 종류의 데이터 이해

application => 소스코드가 실행되는 곳 / 코드+환경

제공되는곳 => 개발자

이코드와 전체환경이 빌드 단계에 이미지에 추가됨

즉 이미지가 빌드할때 코드가 이미지에 복사가된다. => 이 설정을 위한게 dockerfile이였음

이미지를 기반으로 컨테이너를 실행할때

컨테이너는 제공된 환경에서 우리의 코드를 사용

모든것들은 이미지에 복사하면 더이상 변경 불가.=> 이미지는 readonly 명령 실행되면 잠김

즉 새로운것을 변경하려면 이미지를 빌드해야됨

임시 애플리케이션 데이터 => 우리 소스코드가아닌 애플리케이션이 실행되는 동안 생성된 데이터를 의미

ex\_ 폼에 뭔가 입력하고 실행중인 서버컨테이너에서 처리하면 그 사용자 데이터가 임시데이터 => 코드 변수에 저장되거나 데이터베이스에 저장될수있음

일시적일뿐이니 컨테이너가꺼지면 사라져도됨

이미지가 아닌 컨테이너에 저장된다는 것은 부가 extra 레이어에 대해서 이야기하는것임

이미지 위에 도커에 의해 추가된 것

부가 레이어는 기본적으로 로직이라고 할 수 있는데 이미지를 인식하고 이미지 파일 시스템을 인식하여

복사하지않고 파일 시스템을 미러링함

이미지에서 변경안하고 부가레이어에서 실제로 read-write를한다. 배후에 일어나는일이긴함

마지막으로 영구 애플리케이션 데이터 => 지속되어야 하는 데이터 실행중인 컨테이너에서 가져와 생성함

저장되어야 되고 컨테이너 중지되고 다시실행되도 있어야함.

영구앱 데이터는 앱이 실행되는 동안 작성되지만 영구적이어야됨. 그게 **볼륨**

dockerizing 도커화

컨테이너나 이미지와 로컬파일시스템 간에는 서로 연결되어있지않음

이미지를 초기화할때 파이의 로컬 폴더랑 스냅샷을 복구할수있지만 연결이없기떄문에 그걸로 끝

만일 도커 컨테이너를 삭제하지않고 stop만하면 껏다켰을때 파일 존재.

이 격리 개념

이미지를 빌드하는 dockerfile

### 볼륨 소개

도커네는 볼륨이라는 내장 기능이 있음 => 데이터 유지를 돕는다.

애플리케이션에서 볼륨을 어떻게 활용하는가

볼륨은 호스트 머신의 폴더임

컨테이너나 이미지에 있는게아님 호스트 컴퓨터에 장착된 하드 드라이브에 존재하여 사용가능하거나, 컨테이너로 매핑되는것임

볼륨은 도커가 인식하는 호스트머신인 우리의 컴퓨터에있는 폴더로써.

도커 컨테이너 내부의 폴더에 매핑됨

like sound dockerfile Copy

하지만 이 명령어는 파일의 스냅샷을 취하고 이미지에 복사하는게 전부임

볼륨은 다름 컨테이너 내부의 폴더를 호스트 머신상의 컨테이너 외부 폴더에 연결 가능

두 폴더의 변경사항은 다른 폴더에 반영

호스트머신에 파일을 넣으면 컨테이너 내부에서 액세스 할 수 있고 컨테이너가 매핑된 경로에 파일을 추가하면 컨테이너 외부 즉 호스트 머신에서도 사용할 수 있다.

볼륨은 컨테이너가 종료된 이후에도 존재.

컨테이너가 제거되어도 볼륨은 유지된다.

### 볼륨 추가

볼륨을 추가하는 가장 쉬운 방법은 dockerfile에 특수한명령어를 넣는것

VOLUME 명령어

역시나 string[]을 취함

VOLUME ["컨테이너내부경로(생존할데이터의위치)"]

노드의 fs. rename 메서드는 다수의장치를 거쳐 이동하기에 적합하지않다.

fs.rename => fe.copyFile
++ await fs.unlink(tempFilePath);

본질적으로 파일을 복사하고 수동으로 삭제

뭐야 안되잖아

### 저장 메커니즘

도커에는 두가지 외부 저장 메커니즘이있음

볼륨 / 바인드 마운트

볼륨은 도커에 의해 관리되고 바인드 마운트는 나에의해.

볼륨에는
named 볼륨이 있고 익명 볼륨이있음

그전에는 내부 경로만 정하고 호스트머신의 경로는 정하지않음 그래서 미러링된 폴더의 위치를 우리는 모름

이 볼륨에 엑세스하는 유일한 방법은 docker volume 명령어
docker volume ls 하면 볼륨 볼수있음 이역시 아이디값 처럼 들어가있다.

익명볼륨과 모든 바인드 마운트는 하나의 핵심 개념을 공유하게됨

컨테이너에 정의된 경로는 어떤 볼륨에 매핑이됨

그래서 호스트 머신상 생성경로로 연결됨

VOLUME ["컨테이너내부경로(생존할데이터의위치)"]

이건 실제로 호스트머신의 어딘가에 매핑이됨(우리는 그 경로를 모름)

named 볼륨을 쓰면 컨테이너가 종료된 후에도 볼륨이 유지됨 => 하드드라이브 가 그대로 유지됨

그뒤에 다시 컨테이너가 실행되면 볼륨이 복구되고 모든 데이터를 여전히 사용가능

그래서 named 볼륨은 영구적이어야 하는 데이터나 편집하거나 직접 볼 필요가 없는 중요한 데이터에 적합함

실질적으로 호스트 머신의 폴더에 액세스하지 않을 것이기때문에

docker run -v ${named}:/app/feedback

이런식으로 매핑

:으로 분기

컨테이너가 제거되면 익명불름은 자동으로 제거가됨 --rm 옵션 일경우 그렇다

--rm옵션이없다면 익명볼륨은 제거되지 않는다.

그리고 새로시작하면 이전에만들어뒀던게 아니라 또 새로운걸 만들기떄문에 볼륨이쌓임

docker volume rm NAME명령어나

docker volume prune 명령어로 정리

### 바인드 마운트

바인드 마운트는 볼륨과 비슷하지만
도커에 의해관리되는 볼륨의 위치를 우리는 모르지만
바인드 마운트의 경우는 우리가 알고있음

우리가 개발자로써 호스트 머신 상에 매핑될 컨테이너의 경로를 설정하기 떄문임

바인드 마운트는 영구적이고 편집가능한 데이터에 적합함

named 볼륨도 영구적이지만 이건 편집불가능한 데이터 (어디에 저장되어있는지도모르니까)

바인드 마운트 추가 하는법? => dockerfile은 아님

이미지가 아니라 컨테이너에만 적용되기떄문에

컨테이너가 실행될때 터미널 내부에서 바인드 마운트를 설정해야됨

-v named 말고 두번쨰 볼륨을 또 추가 가능

이떄이전과는 다르게 : 앞에 절대경로를 붙여야된다.

절대경로에 double qoute 로 감싸는것도 좋음 경로에 공백이나 특이한게있으면 ㅇㅇ

도커의 기본설정에 액세스해서

FILE SHARING에 공유하고있는 폴더가 리스팅되어있는지 확인 필요

왜없나 했더니 윈도우에는 없음
--rm 옵션이있으면 끄고나서 -ps 로도 안잡힘

```
macOS / Linux: -v $(pwd):/app

Windows: -v "%cd%":/app

```

컨테이너가 볼륨 및 바인드 마운트와 상호작용하는 방식?

컨테이너 내부의 어떤 폴더가 호스트 머시느이 폴더에 마운트 되거나 연결됨

컨테이너 내부에 이미 파일이 있다고 가정하면 외부 볼륨에도 파일이 존재.

토커가 호스트 머신의 로컬 파일을 덮어쓰지않음 => 그랬다간 실수로 컴퓨터에서많은 것들이 삭제될수도있음

그래서 로컬호스트 폴더를 덮어쓰는게아니라 로컬호스트 폴더안에 있는 컨텐츠가 컨테이너를 뒤덮어씀

이로인해서 이 경우 node_moudles가 삭제됨

그래서 도커에게 덮어쓰지말아야될 부분이있다는것을 알려줘야됨

익명의 볼륨을 하나더추가하는 방식으로 해결 가능 -v app/node_modules

도커는 항상 컨테이너에 설정하는 모든 볼륨을 평가함

- 경로에 "" 처리해줬는데 잘못해서 app앞까지만했는데 app까지 해줘야됨

이렇게 바인딩까지해주면 html 변경사항같은것들도 바로 반영된다.

익명 볼륨을 넣어주면 바인드 마운트 폴더의 내용물로 node_modules가 덮어쓰여지지않기떄문에 가능함

### Node mon 사용

콘솔로그 추가했는데 로그에 안찍히는이유? => 런타임

웹서버 재시작해야됨

컨테이너를 재시작할 필요없고 서버만 재시작하면됨 그러나 간단하지않음...

이게 nodejs의 문제

파일시스템을 감지해서 변경이있을떄마다 서버 재시작하는 nodemon 사용하면편함

"scripts": {
"start": "nodemon server.js"
},
"devDependencies": {
"nodemon": "2.0.4"
}

윈도우에선 안될수있다...

### 요약

docekr run -v /app/data => 익명
docker run -v data:/app/data => 기명
docker run -v /path:/app/code => 바인드

기명 / 바인드 마운트는 여러 컨테이너랑 공유가능하고 rm으로 지워도 안사라짐

도커 명령으로는 삭제불가능

이것들은 전부 컨테이너 내부 데이터를 관리하기위한 주요 방식임.

익명 볼륨은 외부 경로보다 컨테이너 내부 경로의 우선순위를 높이는데 사용가능

### 읽기전용 볼륨

볼륨은 기본적으로 read-write

-v path:/app:ro

변경해서는 안되는 컨테이너 내부의 파일을 실수로 변경하지않도록 명시적으로 방지해라

### 도커볼륨 관리하기

볼륨은 도커에의해 관리됨

docker volume ls

도커에의해 관리된다는것은 컨테이너를 실행할때 볼륨이 존재하지않는다면 생성하는 의미이기도함

docker volume create 명령어로 자체 볼륨 생성도 가능

docker volume create [OPTIONS] [VOLUME]

docker volume inspect

언제만들어졌는지 mountPoint , 라벨 등이 나옴

docker volume rm

이역시 쓰고있는애가 중지되어야 삭제 가능

### COPY vs 바인드

바인드가있으면 copy를 뭐하러? => 그래서 지우고 실행

여전히 잘 작동함.

바인드마운트가있으면 copy를 제거할수있음

하지만 docker run 은 개발중에 사용하는 명령어

개발을 마친뒤에는 이 컨테이너를 가져와 서버에 넣음

서버에서 바인드로 실행하진않음.

데이터가 유지되도록 볼륨을 쓸수있어도 바인드는 사용안할거임
서버상의 제품상태로 실행중이라면 실행되는 동안 실시간으로 연결된 소스코드가없을것임

프로덕션에서는 항상 코드의 스냅샷을 가지고싶을것이고 그걸 해결해주는게 COPY 명령어

### docker ignore

복사되는 내용을 제한할수있는데

.dockerignore를 작성하면 됨 마치 git 처럼

이런 것들 작성가능
Dockerfile
.git

### 인수 & env파일

인수를 사용하면 도커파일에서 특정 도커파일 명령으로 다른값을 추출하는데 사용할 수 있는 변수를 사용가능

--build arg 옵션으로

환경변수는 dockerfile내부에서 사용

dockerfile내부 ENV옵션으로 전달 가능 아니면 run 시 --env

ENV PORT 80

EXPOSE $PORT

이런식으로 $을써서 도커에게 알리고 쓸수있음

run 시에는 run --env PORT=8000 의 형태로 사용가능 dockerfile 내부에 연결됨 8000으로

--env말고 -e 로가능 -v마냥 여러개 -e -e 추가 가능

### 빌드인수

빌드타임인수? => 이미지를 빌드할때 다른 값을 플러그인할수있음

dockerfile ARG

인수는 도커파일내부에서만 사용가능한데 CMD 에서는 사용불가

```
FROM node

ARG DEFAULT_PORT=80

WORKDIR /app

COPY package.json /app
# 패키지 제이슨이 무겁기떄문에 한단계 캐싱
```

FROM node

ARG DEFAULT_PORT=80

WORKDIR /app

COPY package.json /app

# 패키지 제이슨이 무겁기떄문에 한단계 캐싱 시켜두는 작업

RUN npm i

COPY . /app

ENV PORT $DEFAULT_PORT

EXPOSE $PORT

# VOLUME [ "/app/feedback" ]

COPY . .

CMD [ "npm","start" ]

```시켜두는 작업

RUN npm i

COPY . /app


# VOLUME [ "/app/feedback" ]

COPY . .

ARG DEFAULT_PORT=80

ENV PORT $DEFAULT_PORT

EXPOSE $PORT


CMD [ "npm","start" ]

```

build 시에 --build-arg DEFAULT_PORT=8000 같은 형태로 사용

여기서 순서조정

왜냐면 ARG나 ENV역시도 레이어에 추가되어서 변경점이 생기는순간 뒤에것드도 재실행되기떄문에 후순위로.

### 요약

컨테이너가 읽고쓸슁ㅆ고 제거후에 살아남는애들

직접적인 상호적용 => 바인드

컨테이너는도커의 핵심 / 데이터읽고쓰기가능

컨테이너는 이미지 위에 read-write레이어추가 그러나 제거되면 데이터 손실

격리 개념은 유용하지만 아닐때도잇음



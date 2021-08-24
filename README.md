# 📦 openmeetings 환경 설정 기록

## 사용법
1. `docker` 와 `docker-compose` 패키지가 필요하다. 데비안/리눅스 기준
```bash
  sudo apt install docker docker-compose -y
```
2. 환경변수 `EXTERNAL_IP` 를 설정할 필요가 있다. 혹은, 이 레포 클론 루트 폴더에 `.env` 파일을 만들어서 다음과 같이 적어주자
```.env
EXTERNAL_IP=[외부에서 이 서버에 접근 가능한 아이피 여기에 적기]
```
3. 이 레포 클론 루트 폴더에 `./data/coturn` 폴더 생성
4. 해당 폴더 안에 `turnserver.conf` 생성 (예제는 아래 메모에 있음)
5. `sudo openssl rand -hex 32` 를 입력하던지 아니면 아래 메모 파트의 스크립트를 브라우저 콘솔에서 실행해서 랜덤한 안전한 32비트 비밀번호를 만들어준다.
6. `turnserver.conf` 편집
  - `#use-auth-secret` -> `use-auth-secret` 주석 해제
  - `#static-auth-secret=north` -> `static-auth-secret=[좀전에 생성한 32비트 비밀번호]` 주석 해제 후 static 암호 입력
  - `#realm=mycompany.org` -> `realm=[도메인 혹은 아이피]` 주석 해제 후 서버 도메인 혹은 아이피 입력
  - `#stale-nonce=600` -> `stale-nonce=0` nonce 수명을 0으로 설정해 클라이언트 인증이 만료되지 않도록 설정
7. 빌드된 openmeetings 폴더 안에서*(설명추가필요) `./webapps/openmeetings/WEB-INF/classes/openmeetings.properties` 파일에서 다음 라인 수정
  - `kurento.turn.url=` -> `kurento.turn.url=[도메인 혹은 아이피]:3478`
  - `kurento.turn.secret=` -> `kurento.turn.secret=[좀전에 생성한 32비트 비밀번호]`
8. 루트 폴더 안에서 다음 명령어로 시작
```bash
  docker-compose up -d
```

## 메모
- ./data/coturn/turnserver.conf 예제
  - https://github.com/coturn/coturn/blob/master/examples/etc/turnserver.conf
- 브라우저 콘솔을 이용해서 안전한 32비트 랜덤 비밀번호 생성
```javascript
(function(){
  var buf = new Uint8Array(32);
  window.crypto.getRandomValues(buf);
  console.log(Array.prototype.slice.call(buf).map((v) => v.toString(16).padStart(2, '0')).join(''));
})();
```

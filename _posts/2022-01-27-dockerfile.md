---
title:  "Docker 이미지 파일 만들때 주의점"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
- BackEnd
tags:
- BackEnd
---
 
 간단하게 자바스크립트로 짜인 앱을 이미지화 시키는 예제이다.


app.js

```
const http = require('http');
const os = require('os');

console.log("Kubia server starting...");

var handler = function(request, response) {
  console.log("Received request from " + request.connection.remoteAddress);
  response.writeHead(200);
  response.end("You've hit " + os.hostname() + "\n");
};

var www = http.createServer(handler);
www.listen(8080);
```

Dockerfile

```
FROM node:7
ADD app.js /app.js
ENTRYPOINT ["node", "app.js"]
```

위의 두 파일을 한 디렉토리에 넣고

```
$ docker build -t kubia .
```

(끝에 '.'은 현재 디렉토리를 뜻한다.)

위의 커맨드를 해당 디렉토리에서 실행한 경우

다음과 같은 출력이 나와야한다.

```
yeonginkim@Yeongins-MacBook-Air kubia % docker build -t kubia .
[+] Building 2.6s (8/8) FINISHED                                                
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 234B                                       0.0s
 => [internal] load .dockerignore                                          0.0s
 => => transferring context: 2B                                            0.0s
 => [internal] load metadata for docker.io/library/node:7                  2.5s
 => [auth] library/node:pull token for registry-1.docker.io                0.0s
 => [internal] load build context                                          0.0s
 => => transferring context: 168B                                          0.0s
 => [1/2] FROM docker.io/library/node:7@sha256:af5c2c6ac8bc3fa372ac031ef6  0.0s
 => CACHED [2/2] ADD app.js /app.js                                        0.0s
 => exporting to image                                                     0.0s
 => => exporting layers                                                    0.0s
 => => writing image sha256:01b039af02f7f04247ac70b31c122f6f50de39d0f7033  0.0s
 => => naming to docker.io/library/kubia                                   0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them

```

문제점은 Dockerfile은 확장자 이름을 가지지 않는다.

Dockerfile이나 dockerfile이라도 도커가 둘다 인식을 하지만

Dockerfile에 '.'으로 시작하는 확장자가 붙을 경우 다음과 같은 에러가 난다.

```
yeonginkim@Yeongins-MacBook-Air kubia % mv Dockerfile Dockerfile.dockerfile
yeonginkim@Yeongins-MacBook-Air kubia % ls
Dockerfile.dockerfile	app.js
yeonginkim@Yeongins-MacBook-Air kubia % docker build -t kubia .            
[+] Building 0.0s (1/2)                                                         
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 2B                                         0.0s
failed to solve with frontend dockerfile.v0: failed to read dockerfile: open /var/lib/docker/tmp/buildkit-mount705266051/Dockerfile: no such file or directory
```

위 예제에서는 파일이름을 맥의 zsh에서 mv를 이용하여 바꾸었다.

Dockerfile이 없는 경우 위와 같은 에러 메세지를 돌려주는데

다시 강조하지만 Dockerfile에 확장자 이름이 붙어서는 안된다.

도커 커맨드로 특정 파일을 도커파일로 쓰도록 하는 방법도 있다.

이것은 아래의 스택오버플로우 링크를 걸어두겠다.

[Stack Overflow dockerfile](https://stackoverflow.com/questions/64985913/failed-to-solve-with-frontend-dockerfile)

출처: Kubernetes in Action by Marco Luksa
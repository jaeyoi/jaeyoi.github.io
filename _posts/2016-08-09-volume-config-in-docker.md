---
layout: post
title: docker에서 volume의 적용 시점
---
## 파일 예제

### 포함된 파일 목록
* app.py (Flask 실행 파일)
* docker-compose.yml
* Dockerfile
* requirement.txt

### Dockerfile
```
FROM python:3.5

COPY requirements.txt /code/requirements.txt
WORKDIR /code
RUN pip install -r requirements.txt
COPY . /code

CMD python app.py
```

### docker-compose.yml
```
version: '2'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
```

## 설명
* docker-compose.yml의 volumes는 host의 현재 폴더를 container의 /code 경로로 mount 시켜준다. 그 시점이 언제인지 아리송해서 기록해둔다.
* COPY 및 RUN은 mount 되기 전의 container 폴더에 적용됨
* CMD는 mount된 이후에 적용됨
* 개발환경에서는 Dockerfile을 다음과 같이 해도 동작하나, 실 환경을 고려해서 위의 예제처럼 보통 사용. 아래 같은 경우, docker-compose.yml에서 volume 설정이 빠지면(실환경에서는 뺌) /code 폴더에 아무것도 안 들어있게 됨.

### Dockerfile 변형 (개발용에 적용 가능)
```
FROM python:3.5

COPY requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

WORKDIR /code
CMD python app.py
```

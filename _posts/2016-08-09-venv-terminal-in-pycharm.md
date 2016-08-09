---
layout: post
title: pycharm에서 virtual env 적용된 내부 terminal 열기
---
참고: http://stackoverflow.com/questions/22288569/how-do-i-activate-a-virtualenv-inside-pycharms-terminal

* project 폴더에 설정파일을 하나 만든다. (예: .pycharmrc)
```
source ~/.bashrc
source ~/.venv/py3/bin/activate
```

* Preference/Settings > Tools > Terminal 화면에서 다음처럼 설정한다.
```
Shell path: /bin/bash --rcfile .pycharmrc
```

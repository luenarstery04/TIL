# DB에 저장된 자료 HTML로 가져오기
Django를 통해 project와 app을 생성하는 명령어

## 1. project 생성하기

```cmd
django-admin startproject project_name
```

지정한 이름의 project 폴더가 생성된다.

```cmd
django-admin startapp app_name
```

지정한 이름의 app 폴더가 생성된다.

## 2. settings.py, urls.py 수정하기

project를 생성했다면 반드시 settings.py에 들어가 app을 추가해줘야 한다.

만약 project이름을 'my_project'라고 했다면, 하위 폴더에 동명의 폴더가 더 생기고 그곳에 setting.py가 존재한다.

my_project/my_project/settings.py

34번 줄의 INSTALLED_APPS에 여러 Django 클래스들이 있는데, 맨 아래에 내가 생성한 app들의 이름을 써주면 된다.

urls.py에 들어가 urlpatterns 내에 path를 설정해야 한다.

```py
from django.urls import path, include

path('', include('my_app.urls'))
```

include가 없다면 반드시 import 해줘야 한다.

## 3. 서버 열리는 지 확인하기

아무것도 없을 지라도 일단 서버가 열리는 지 확인해야 한다.

명령어를 실행하는 위치가 project 내부임을 확인한 후 실행하자.

```cmd
python manage.py runserver
```
#### 1. 소개

django를 활용하여 할 수 있는 것이 무엇이 있을까 생각하다가 날씨를 알려주는 사이트를 만들면 재미있을 것 같다는 생각이 들었다.

마침 openweathermap 이라는 사이트에서 전세계 각 도시의 기상 정보를 알려주는 API를 제공하는 것을 확인하여 바로 개발해보았다.

게다가!!! 좀더 이 API를 쉽게 사용할 수 있도록 어느 개발자분들이 pyowm이라는 python 패키지를 만들어 두셔서 관련 정보를 좀 더 쉽게 다룰 수 있었다.

그리고 더불어 특정 도시의 경도, 위도 값을 받아오기 위해 구글 크롬에서 제공하는 geocoding API를 사용하였다.

이 역시도 googlemaps라는 python 패키지가 존재하여 쉽게 API를 이용할 수 있었다.



#### 2. django 기본설정

이미 필자가 [무작정 따라하는 django tutorial 1편](https://kingmurky.github.io/%EA%B0%9C%EB%B0%9C/django2/) 에서 다룬 부분이지만 짤막하게나마 다뤄보도록 하겠다.

우선 django와 관련 IDE가 모두 설치가 되어있다고 가정하겠다.

우선 이해가 안되더라도 terminal에서 다음 과정을 따라해보자.



1. cd <프로젝트를 생성할 디렉토리 주소> **django 프로젝트를 만들 디렉토리로 이동**
2. django-admin startproject <프로젝트 이름> **django 프로젝트 생성**
3. py manage.py startapp <하위 프로그램 이름> **하위 프로그램 생성**
4. py manage.py migrate **DB파일 생성 기본적으로는 django에서 제공하는 sqlite3으로 생성된다**
5. py manage.py createsuperuser **관리자 계정 생성**
6. py manage.py runserver **서버 실행**



일단 기본 설정은 끝났다!



#### 3. settings.py 와 urls.py 설정하기

settings.py는 기본적으로 django 파일의 다양한 설정을 관리하는 파일이라고 할 수 있다.

이곳에서 많은 것들을 설정 할 수 있지만 우선 우리는 일부만을 다루도록 하겠다.

일단 INSTALLED_APPS 에서 우리가 만든 하위 앱을 추가하자.

```(.python)
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'weather'
]
```

 

'weather' 앱을 추가한 것을 확인 할 수 있다.

그 다음 언어와 시간대 설정을 해보자.

```(.python)
LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/Seoul'
```

필자는 한국어를 사용하고 한국에 사는 한국인 이므로 이렇게 설정해주었다.



다음은 url을 연결해주는 작업을 해주기 위해 project 디렉토리 하위에 위치한 urls.py 파일을 열어주도록 하자.

초기에는 이런 모습일 것이다.

```(.python)
urlpatterns = [
    path('admin/', admin.site.urls),
]
```



여기에 우리가 프로젝트 하위에 만든 하위 앱의 url을 연결하여 원하는 대로 view가 표시되도록 하자.

우선

```(.python)
from django.urls import path,include
```

을 입력하여 include 함수를 import 해주자.

include 함수는 django 서버가 url을 쉽게 찾을 수 있도록 도와준다. 



그리고 나서 urlpatterns에 다음과 같이 적으면 된다.

```(.python)
path('weather/', include('weather.urls')),
```

나 같은 경우 하위 앱의 이름이 weather 이었기 때문에 weather.urls로 경로 설정을 해주었다.



이때 서버를 작동시키면 우리가 원하는 화면이 등장하지 않는다.

우선 view가 구성되지 않았기 때문이고, 하위 앱에 urls.py가 존재하지 않기 때문에, 'weather.urls'가 존재하지 않는 파일로 인식되어 오류를 발생시키기 때문이다.

우선 하위 앱 weather 하위에 urls.py 파일을 생성하자



이 파일에는 하위 앱 weather의 다양한 view를 연결할 urlpatterns를 생성하도록 한다.

다음과 같이 입력하자.

```(.python)
from django.urls import path
from . import views

urlpatterns = [

]

```



여기까지 하면 기본적인 url세팅을 모두 마치게 되었다.

다음 시간에는 본격적으로 우리가 사이트에 마주할 화면을 담당하는 views.py 파일을 다뤄보고 API에 대해 살펴보도록 하겠다.


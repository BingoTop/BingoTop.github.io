---
layout: post
title: Django 배포하기 전 점검하기(SECRET_KEY 발급과 관리)
categories: Django
comments: true
---

----

개강 때문에 정신이 없어서 오늘 포스팅하게 되네요ㅠㅠ
그래도 이번에 두 번째 프로젝트로 이미지를 업로드할 수 있고 댓글까지 disqus로 구현을 해보았는데요.
처음으로 프론트엔드에서 정말 곤혹을 겪었습니다ㅠㅠ(CSS..)
오늘은 배포하기 전에 점검하는 것을 포스팅해보도록 하겠습니다.

## 왜 SECRET_KEY를 쓸까?
저는 처음에 이런 궁금증이 들었는데요, 저의 검색 실력 미달으로 그런지 한글로 된 글들은 확실한 대답을 보지는 못 했습니다.
그래서 저의 미천한 영어실력으로 검색을 해봤는데요. 결과는 이렇습니다.
~~~
The secret key is used for:
All sessions if you are using any other session backend than django.contrib.sessions.
backends.cache, or are using the default get_session_auth_hash().
All messages if you are using CookieStorage or FallbackStorage.
All PasswordResetView tokens.
Any usage of cryptographic signing, unless a different key is provided.
If you rotate your secret key, all of the above will be invalidated.
Secret keys are not used for passwords of users and key rotation will not affect them.
~~~

SECRET_KEY는 다음을 위해 사용된다.
django.contrib 이외의 다른 세션 백엔드를 사용하는 경우 모든 세션들은
backends.cache 또는 기본 get_session_auth_hash()를 사용하고 있다.
CookieStorage 또는 FallbackStorage를 사용하는 경우 모든 메시지
모든 PasswordResetView 토큰.
다른 SECRET_KEY가 제공되지 않는 한, 암호 서명의 사용된다.
SECRET_KEY를 돌리면 위의 모든 것이 무효가 된다.
SECRET_KEY는 사용자의 비밀번호에 사용되지 않으며 키 로테이션에 영향을 미치지 않는다.

파이썬 터미널에서
~~~
python manage.py check --deploy
~~~

![스크린샷, 2019-09-10 00-45-17](https://user-images.githubusercontent.com/40786401/64575768-698c0680-d3b0-11e9-8545-7e8540e230d2.png)

노란색 경고문이 뜨면 정상적으로 되는 것이니 너무 당황하지 않고


[django secret key generator](https://www.miniwebtool.com/django-secret-key-generator/) 들어가서
![시크릿키 제네레이터](https://user-images.githubusercontent.com/40786401/64575102-1ebcbf80-d3ad-11e9-84ee-5cac07ea8663.png)

시크릿 키 제네레이터 홈페이지 메인화면에서 Generate Django SECRET_KEY를 누르면
![발급](https://user-images.githubusercontent.com/40786401/64575141-4e6bc780-d3ad-11e9-9972-6a7930308d4d.png)
길이가 50인 문자를 받을 수 있습니다.(스크린샷의 시크릿 키는 배포 용도로 쓰이지 않았습니다.)
배포하실 때는 절대로 시크릿 키를 맘대로 보여주시면 안 되요!

다음으로 manage.py있는 곳에 secrets.json파일을 생성해서
~~~
{
"SECRET_KEY" :"발급 받은 키 입력"
}
~~~

프로젝트/settings.py 수정
~~~python

from django.core.exceptions import ImproperlyConfigured # import

secret_file = os.path.join(BASE_DIR,'secrets.json')

with open(secret_file) as f:
    secrets = json.loads(f.read())

def get_secret(setting, secrets=secrets):
    try:
        print(secrets[setting]) # 시크릿키 테스트 출력 원하지 않으면 주석 처리
        return secrets[setting]
    except KeyError:
        error_msg = "Set the {0} enviroment variable".format(setting)
        raise ImproperlyConfigured(error_msg)


SECRET_KEY = get_secret("SECRET_KEY")
~~~

터미널에서
~~~
python manage.py shell
~~~
입력해서 시크릿 키가 잘 출력 되는 걸 확인 한다면 성공적으로 잘 되었는 걸 확인할 수 있습니다.


### Tip
github에 코드를 push할 때 시크릿 키를 절대 배포해서는 안 되는데. push할 때 지정한 파일만 무시하고 올려 줄 수 있습니다.
__.gitignore__ 메모장을 생성하셔서 이와 같이 입력한 다음
~~~
secrets.json
~~~
파일 이름을 입력하면 끝!

배포하기 전에 확인도 안 하고 배포할뻔 했습니다ㅠㅠ 여러분도 배포할 때 체크사항을 확인하고 배포합시다!

## __Reference__

https://wayhome25.github.io/django/2017/07/11/django-settings-secret-key/

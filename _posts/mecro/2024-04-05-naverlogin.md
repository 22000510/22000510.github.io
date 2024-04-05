---
title: 메크로, 파이썬 selenium으로 네이버 자동 로그인 하기
date: 2024-04-05 15:08:22 +09:00
categories: [파이썬, 메크로]
tag: [파이썬, 메크로, 자동, 코딩]
---

## selenium 이용해 네이버 자동로그인 해보기
<br>
파이썬 selenium 코드를 이용해서 네이버 자동로그인 하는 방법을 알아보도록 하겠습니다.<br>
어렵지 않은 코드지만 네이버 자동 로그인과 자동입력 방지 문자 안나오도록 하는 법에 대해 포스팅 해보겠습니다.

(저는 jupyter notebook 이용해서 연습했습니다.)
<br>
<br>
우선 제가 작성한 코드입니다.(급하신 분들을 위해..)

    from selenium import webdriver
    from selenium.webdriver.common.by import By
    from selenium.webdriver.common.keys import Keys
    from selenium.webdriver.support.select import Select
    import pyperclip
    import time

    user_id = '자기 아이디' //자기 아이디와 비밀 번호 넣어주세요
    user_pw = '자기 비밀번호'

    driver = webdriver.Chrome()
    driver.get('https://nid.naver.com/nidlogin.login?mode=form&url=https://www.naver.com/')
    driver.implicitly_wait(5)
    pyperclip.copy(user_id)
    driver.find_element(By.ID, 'id').send_keys(Keys.CONTROL, 'v')
    time.sleep(1)
    pyperclip.copy(user_pw)
    driver.find_element(By.ID, 'pw').send_keys(Keys.CONTROL, 'v') 
    time.sleep(1)
    driver.find_element(By.ID, 'log.login').click()
    driver.implicitly_wait(5)

## 과정 설명

#### 1. selenium 모듈과 webdriver 설치

먼저 selenium 모듈을 설치해주셔야 됩니다.<br>
이전 버전의 selenium은 별도로 자신의 환경에 맞는 버전의 크롬 드라이버를 설치했어야 했습니다.<br>
이번에 selenium 4.0부터는 따로 크롬 드라이버를 설치하지 않아도 됩니다.<br>
터미널 창에 아래와 같은 명령어를 이용해 설치해주세요.

    pip3 install selenium webdriver_manager

* 참고: <https://kibua20.tistory.com/228>
<br>

#### 2. from과 import 입력해주기

    from selenium import webdriver
    from selenium.webdriver.common.by import By
    from selenium.webdriver.common.keys import Keys
    from selenium.webdriver.support.select import Select
    import pyperclip
    import time

from과 import를 입력 해주어야 웹드라이버와 By, Key 등의 기능들을 사용할수 있습니다.<br>

#### 3. 브라우저 열기

저는 크롬 브라우저를 이용했습니다.<br>

    driver = webdriver.Chrome()
    driver.get('https://nid.naver.com/nidlogin.login?mode=form&url=https://www.naver.com/')
    driver.implicitly_wait(5)

드라이버를 크롬으로 입력해주고 "driver = webdriver.Chorome", 가고자 하는 url을 get 안에 넣어줍니다. "drivere.get(가고자하는 url)" <br>
저는 네이버 로그인 창을 url로 지정해주었습니다.<br>
"driver.implicitly_wait(5)" 이 코드는 페이지가 뜰때 까지 기다리기 위한 코드입니다.

#### 4. 로그인 하기

먼저 아이디 입력하는 칸의 id가 어떻게 할당되는지 알아봐야 합니다. <br>

<img src="https://private-user-images.githubusercontent.com/82192901/319904565-13e8b043-3085-40f6-9458-9ce225760be0.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTIzMDQxMTMsIm5iZiI6MTcxMjMwMzgxMywicGF0aCI6Ii84MjE5MjkwMS8zMTk5MDQ1NjUtMTNlOGIwNDMtMzA4NS00MGY2LTk0NTgtOWNlMjI1NzYwYmUwLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA0MDUlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNDA1VDA3NTY1M1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWY2NGM3NDE0ZTY1NDMxNzNlNjU5MDA2YmIzNjk0ZjVlOGMxYTk0MTYyNmQ1MjhiMzk1M2RmMmQzYWQxYmZiMmYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.IYcemOyY5NoAkwV9QlC1KtSO426Zw-X6OOzMoL3fucQ" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="screen"></img><br>

네이버 로그인 페이지 창을 열어 f12키를 눌러줍니다.<br>
위의 사진에 파란색으로 동그라미 쳐진 부분을 클릭 해주시면 마우스를 올려놓은 곳이 어떻게 작성되어져 있는지 확인 가능합니다.<br>
네이버 아이디 칸에 마우스를 올리고 옆의 스크립트를 통해 아이디 칸의 input 필드의 id가 "id"라는 것을 확인할 수 있습니다.(그림 속 파란색 밑줄)<br>
비밀번호의 id도 동일한 방법으로 찾을수 있습니다.<br>
<br>
각 칸의 id를 확인하고 처음에는 밑에 작성된 코드로 자동 로그인을 시도했습니다.

    driver.find_element(By.ID, 'id').send_keys('자기 아이디') 
    driver.find_element(By.ID, 'pw').send_keys("자기 비밀번호")

하지만 위의 코드로 실행시 자동입력 방지 문자가 나오는 과정이 생겨 로그인을 할 수가 없었습니다. 아마 너무 빨리 입력되서 자동방지문자가 뜨는 거 같습니다.<br>
그래서 저는 아이디와 비밀번호를 복사 붙여넣기 방식을 이용해서 문제를 해결했습니다.

    pyperclip.copy(user_id)
    driver.find_element(By.ID, 'id').send_keys(Keys.CONTROL, 'v')
    time.sleep(1)
    pyperclip.copy(user_pw)
    driver.find_element(By.ID, 'pw').send_keys(Keys.CONTROL, 'v')
    time.sleep(1)
    driver.find_element(By.ID, 'log.login').click()

pyperclip 모듈을 이용해 아이디와 비밀 번호를 복사후 붙여넣기 할 수 있었습니다. time.sleep(1)을 하는 이유는 복사 붙여넣기도 너무 빠르게 연속으로 될 경우 자동입력 방지 문자가 뜨기에 사이 사이에 텀을 주기위해 넣었습니다.
 <br>

#### 3.마무리

이렇게 오늘은 selenium을 이용해 네이버 자동로그인을 해보았습니다. 아마 제가 한 것보다 완벽하게 작성된 코드가 많이 있을거라고 생각합니다.<br>
더 공부해서 더 좋은 코드 가지고 올 수 있도록 하겠습니다. 감사합니다.







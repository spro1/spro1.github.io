---
layout: post
title:  "파이썬을 이용한 질병관리본부 코로나19 데이터 파싱"
date:   2020-03-19
categories: Python Crawler
---
코로나바이러스19(COVID-19)가 국내외로 많이 퍼져 사회적으로 큰 이슈가 되고있다.

코로나19가 국내로 유입되어 확산된 이후 많은 개발자분들이 데이터들을 모아 간단하고 직관적이게 확인할 수 있게 웹사이트를 개발하여 제공하고 있다.

국내 감염현황을 활용하여 웹사이트를 만들어보려고 간단하게 [질병관리본부 사이트](http://ncov.mohw.go.kr/)를 활용하여 국내의 감염현황을 파싱해봤다.

----------------------------------------

[Github code](https://github.com/spro1/python_script/blob/master/script/corona_crawler.py)

request를 이용해 질병관리본부의 페이지를 크롤링하고 beautifulSoup를 활용하여 html 데이터를 파싱했다.

```
url = "http://ncov.mohw.go.kr/"
fp = urllib.request.urlopen(url)
data = fp.read()
fp.close()

soup = BeautifulSoup(data, 'html.parser')
```
beautifulSoup는 태그나 클래스 값을 이용해서 속성 값이나 텍스트 값을 가져올 수 있다.

```
content = soup.find('div' ,class_='live_left')

update_date = content.find('span',class_='livedate').text
print("업데이트 날짜 : %s" % update_date)
```
최종적으로 아래와 같은 데이터를 얻을 수 있다.

    업데이트 날짜 : (3.18. 00시 기준, 1.3 이후 누계)
    확진자 : 8413
    완치 : 1540
    치료 중 : 6789
    사망 : 84
    누적 검사수 : 295647
    누적 검사완료수 : 279301
    누적 확진률 : 3.01%
    도시 : 서울, 확진자 : 270
    도시 : 부산, 확진자 : 107
    도시 : 대구, 확진자 : 6,144
    ...(생략)
    도시 : 제주, 확진자 : 4

추후 데이터 format을 정해서 DB에 저장하여 웹사이트에 활용할 예정이다.

하루라도 빨리 코로나 사태가 진정되기를 기원한다.
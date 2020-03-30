---
layout: post
title:  "IT feed 모아보는 웹사이트"
date:   2020-03-30
categories: Web Python Django React
---
IT rss를 보려고 rss 목록을 검색하던 중 따로 검색하지 않고 리더기에 등록하지 않아도 IT RSS feed들을 보여주는 웹사이트가 있으면 좋을 것 같아서 만들어 봤다.

기업이 운영하는 rss 들과 개인 블로그, IT NEWS rss에서 데이터를 파싱해왔고 추가적으로 회사원들은 경제에 관해서도 관심이 많을 것 같아 경제관련한 rss도 추가했다.

사용한 기술 스택은 아래와 같다.
Python, Django, React

Python crawler를 통해 매 1회 오전 9시에 rss 리스트들을 방문하여 새로 업로드된 글 리스트들을 DB에 저장한다.
Django는 DB에 저장된 RSS 목록을 json 형태로 뿌려주고 react에서는 해당 데이터들을 받아 front로 뿌려준다.


---------------
[Github code](https://github.com/spro1/react_project)

주요 몇가지 코드만 간단하게 설명한다.

crawler는 feedparser 라는 라이브러리를 이용해 구성했다.
기본적으로 rss 들은 버전에 따라 같은 양식이다. 

간단한 코드만 보면 feedparser 라이브러리를 활용해 각종 데이터를 dict 형태로 받아온다.

DB에 넣기 위해 string 형태의 날짜 데이터를 datetime 타입으로 변경해준다.

```
import feedparser
import datetime

feed = feedparser.parse(url)
author = feed["channel"]["title"]
date = i["published"]

time_format = "%a, %d %b %Y %H:%M:%S %z"
date = datetime.datetime.strptime(date, time_format)
```

Django 에서는 RSS 데이터를 저장할 모델을 만들고 react에서 데이터를 사용할 수 있게 rest_framework 를 활용한다.

model.py
```
class Rss(models.Model):
    author = models.CharField(max_length=100)
    title = models.TextField()
    href = models.TextField()
    category = models.CharField(max_length=30)
    upload = models.DateTimeField()
```

serializers.py
```
class RssSerializer(serializers.ModelSerializer):
    class Meta:
        fields = (
            'author',
            'title',
            'href',
            'category',
            'upload',
        )
        model = Rss
```

views.py
```
class ListRss(generics.ListCreateAPIView):
    queryset = Rss.objects.filter().exclude(category="Economy NEWS").order_by('-upload')
    serializer_class = RssSerializer
```

urls.py
```
urlpatterns = [
    path('rss', views.ListRss.as_view()),
    path('rss/<int:pk>/', views.SortRss.as_view()),
    path('rss/company', views.CompanyRss.as_view()),
    path('rss/news', views.NewsRss.as_view()),
    path('rss/solo', views.SoloRss.as_view()),
    path('rss/economy', views.EconomyRss.as_view()),
]
```

Front 단에서는 django에서 데이터를 호출하여 map 형태로 저장해사용한다.
```
async componentDidMount() {
        if(this.state.path === "/news"){
            this.state.api = "http://127.0.0.1:8000/api/rss/news";
        }else if(this.state.path === "/company") {
            this.state.api = "http://127.0.0.1:8000/api/rss/company";
        }else if(this.state.path === "/solo") {
            this.state.api = "http://127.0.0.1:8000/api/rss/solo";
        }else if(this.state.path === "/economy"){
            this.state.api = "http://127.0.0.1:8000/api/rss/economy";
        }else{
            this.state.api = "http://127.0.0.1:8000/api/rss";
        }
        try {
            const res = await fetch(this.state.api);
            const rss = await res.json();

            this.setState({
                rss,
                filter:rss
            });
        } catch (e) {
            console.log(e);
        }
    }
```

--------------------------------
이렇게 해서 만들어진 웹사이트가 아래와 같다.
![feed_main](https://user-images.githubusercontent.com/12808575/77892636-e24fa300-72ad-11ea-9320-ab9228cc57c0.PNG)

전체보기에는 IT관련한 RSS만 지원하며 경제와 관련된 RSS는 따로 탭을 만들어 보여준다.

![feed_economy](https://user-images.githubusercontent.com/12808575/77895440-bf26f280-72b1-11ea-97dc-9874d2e3afcc.PNG)

호스팅을 위해 현재 URL을 삿지만 서버가 아직 세팅이 되지않아서 아직 공개전이다.
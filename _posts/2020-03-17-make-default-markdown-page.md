---
layout: post
title:  "github blog markdown 기본 페이지 만들기"
date:   2020-03-17
categories: Python
---
github blog를 시작하기 전에 포스트에 사용될 기본 markdown 형식을 일일이 작성하기 번거로워 
기본 마크다운 파일을 생성하는 간단한 스크립트를 만들었다.

------------------------
[Github code](https://github.com/spro1/python_script/blob/master/script/make_mdfile.py)

파일명, 제목, 날짜, 카테고리 항목의 데이터를 입력받아 파일을 만드는 스크립트이다.

    ex)
    ---
    layout: post
    title:  "github blog markdown 기본 페이지 만들기"
    date:   2020-03-17
    categories: Python
    ---

먼저 현재 날짜 데이터를 받아온다.<br/>
필요한 데이터는 연-월-일이므로 뒤에 시간데이터는 제거해준다.

```
now = datetime.datetime.now()
filename_date = str(now).split(" ")[0]
```
파일명을 입력받아 날짜데이터와 합쳐 파일명을 만들어준다.
```
filename = input("input filename : ").replace(" ","-")
filename = "%s-%s.md"%(filename_date,filename)
print("filename : %s"%(filename))
```

다음으로 글의 제목과 카테고리 데이터를 input함수를 사용해 받아온다.
```
#title input
title = input("input title : ")
print("title : %s"%title)

#categories input
category = input("input category : ")
print ("category : %s"%category)
```
마지막으로 입력받은 데이터들을 하나의 문자열로 합쳐 파일을 생성하여 저장한다.
```
data = """---
layout: post
title:  "%s"
date:   %s
categories: %s
---
"""%(title, filename_date, category)

print(data)
#make file
# path = "C:\test\"
# filename = path+filename
f = open(filename,'w', encoding='utf-8')
f.write(data)
f.close()
```

아직 제목, 시간, 카테고리만 저장하는 간단한 스크립트이지만 추후 고도화를 통해 마크다운형식에 맞게 글을 입력할 수 있고 자동으로 github에 commit과 push가 가능하게 변경할 예정이다.

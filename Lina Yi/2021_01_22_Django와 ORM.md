﻿ 
Application들은 다양한 데이터 저장방법을 가지고 있다.
데이터베이스(RDBMS, NoSQL …등)에 저장하거나 파일(로컬, 외부 정적 스토리지), 캐시서버(메모리캐시, redis…) 에 저장할 수 있다
.
일반적인 서비스의 경우에는 데이터베이스에 저장하는 경우가 많다. 정적 파일의 경우에는 파일 시스템을 이용한다.

데이터베이스의 종류로는 RDBMS(ex. PostgreSQL, MySQL, SQLite, MSSQL, Oracle.)과 NoSQL(MongoDB ...) 이 있으며 데이터베이스에서 쿼리에 접근하기 위한 언어인 SQL을 사용한다.



ORM을 이야기하기에 앞서 우선 SQL에 대해 알아야 한다.

SQL(Structured Query Language)란
관게형(relational) 데이터베이스에서 수정, 삭제 혹은 정보를 가져올 때 등의 동작을 할 때 사용되는 언어이다.

즉, MySQL, 혹은 PostgresQl같은 데이터베이스를 사용하기 위해서는 해당 SQL언어를 알아야 한다.

직접 SQL문을 작성하는 방법도 있지만 ORM(Object Relational Mapping)을 통해서 프로그래밍 언어로 SQL문을 생성하거나 실행할 수 있다. 하지만 ORM을 사용하더라도 자신의 ORM 코드가 어떤 SQL문을 뜻하는지 알아두는 것이 좋다.

ORM이란, 객체(Object)의 관계(Relational)를 연결(Mapper)해주는 것을 뜻한다. 객체 지향적인 방법을 사용하여 데이터베이스의 데이터를 쉽게 조작할 수 있게 해주는 것이다.

즉, Django의 ORM이란, 파이썬과 데이터베이스의 SQL사이의 통역사 역할을 해준다.

Django는 아래 SQL 코드를 위에 파이썬 언어로써 사용할 수 있게 해주는 것이다.


데이터베이스의 Table과 파이썬의 Class는 1대1로 매핑된다.

모델 클래스명은 단수형으로 지정해야 한다. (ex. Posts(X), Post(O)

예시 models.py 
```
from django.db import models
class Post(models.Model):
     title = models.CharField(max_length=100)
     content = models.TextField()
     created_at = models.DateTimeField(auto_now_add=True)
```

### Model 생성, 활용 순서
#### 모델 클래스 작성

1. 모델 클래스의 migration 파일 생성 (python manage.py makemigrations)
2. migration 파일을 DB에 적용 (python manage.py migrate)
3. 모델 활용!
( 외부에서 DB 형상을 관리할 경우에는 inspectdb 명령으로 활용할 수 있습니다)

#### migration을 통해 생기는 DB의 테이블명
DB 테이블명 : 기본값으로 “AppName_ModelName”
ex. Blog앱의 Post모델의 테이블 명 : blog_post

커스텀으로 지정하고자 할 때는 Meta클래스를 생성하여 설정하면 된다.

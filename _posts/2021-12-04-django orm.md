---
title:  "[Django] django ORM으로 DB CURD 구현"
excerpt: "django ORM으로 DB CURD 구현"

categories:
- Django

toc: True
toc_sticky: True

date: 2022-12-04
last_modified_at: 2022-04-02
---


<br>

## QuerySet, Model Manager

QuerySet을 이용해서 별도로 SQL을 작성할 필요 없이 DB로 부터 데이터를 가져오고 추가, 수정, 삭제 등이 가능

- Post.objects.all() : SELECT * FROM post~ 와 같은 SQL문 생성.
- Post.objects.create() : INSERT INTO post VALUES(~)와 같은 SQL문 생성.

## Models.py

```python
class Menu(models.Model):
    name = models.CharField(max_length=30)

class Category(models.Model):
    menu = models.ForeignKey('Menu', on_delete=models.CASACADE)
    name = models.CharField(max_length=30)
```

## DB 데이터 추가(CREATE)

- Menu 추가

```bash
# django shell 실행
python manage.py shell

# models.py 불러오기
>>> from products.models import Menu

# create()
>>> Menu.objects.create(name='Coffe')
# bluk_create() : 하나의 query로 여려개의 object 생성 가능

>>> a1 = Menu(name='상품')
>>> a2 = Menu(name='카드')
>>> Menu.objects.bulk_create([a1, a2])
```

<br>

- Category 추가

```bash
>>> from products.models import Category

# Menu의 id를 확인하고 추가('Coffe' 메뉴에 추가)
>>> Category.objects.create(name='아메리카노', menu=Menu(id=1))
```

<br>

## DB 데이터 조회 (READ)

```bash
# 테이블 데이터 전체 출력
>>> Category.objects.all()

# Category 데이터 중 Coffe Menu를 참조하고 있는 데이터만 출력
>>> Category.objects.filter(menu_id=1) # Category.objects.filter(menu__name='음료')

# Coffe Menu를 참조하는 모든 데이터의 name 값을 출력
>>> for category in Category.objects.filter(menu__name='Coffe'):
 print(category.name)

### Coffe Menu를 참조하는 모든 카테고리 출력
>>> a1 = Menu.objects.get(name='Coffe')
>>> a1.category_set.all()

### Coffe Menu를 참조하는 모든 카테고리 개수 출력
>>> a1 = Menu.objects.get(name='Coffe')
>>> a1.category_set.count()

### 아메리카노 카테고리가 참조하고 있는 메뉴의 name값 출력
>>> a2 = Category.objects.get(name='아메리카노')
>>> a2.menu.name
```

- AND 조건

```python
queryset = 모델클래스명.objects.all()
queryset = queryset.filter(조건필드1=조건값1, 조건필드2=조건값2)
queryset = queryset.filter(조건필드3=조건값3)


for model_instance in queryset:
    print(model_instance) 
```

- OR 조건(Q객체 필요)

```python
from django.db.models import Q

모델클래스명.objects.all().filter(Q(조건필드1=조건값1) | Q(조건필드2=조건값2)) # or 조건
모델클래스명.objects.all().filter(Q(조건필드1=조건값1) & Q(조건필드2=조건값2)) # and 조건
```

ex) blog.views.post_list 에서 검색 구현
```python
def post_list(requests):
    qs = Post.objects.all()   # Post 모델클래스의 전체 데이터 저장
    q = requests.GET.get('q','')   # get파라미터중 q라는 값 저장
    if q:  # 만약 값이 있을 경우
        qs = qs.filter(title__icontains=q)  # 대소문자 구별없이, title에 값을 찾는다.
    return render(requests, 'blog/post_list.html', {
        'post_list' : qs,  
        'q':q,})   

#blog/templates/blog/post_list.html
<form action="" method="get">
    <input type="text" name="q" />
    <input type="submit" value="검색" />
</form>
```

- 특정 필드만 가져오기

```python
# values() : 해당 컬럼의 key,value 쌍의 리스트 반환
>>> Room.objects.values("city")
[{"city":"seoul"}, {"city":"newyork"}, ...]

# values_list() : key,value 형태가 아닌 tuples 형태의 리스트로 만환
[("city","seoul"), ...]

# values_list(flat=True) : value의 리스트 형태로 반환
["seoul","newyork", ...]
```

<br>

## DB 데이터 수정 (UPDATE)

```bash
# filter() 메소드로 아메리카노를 카페라떼로 변경, 한번에 여러 데이터 수정 가능
>>> Category.objects.filter(name='아메리카노').update(name='카페라떼')
```

<br>

## DB 데이터 삭제 (DELETE)

```bash
### 단일 삭제
>>> data = Category.objects.get(name='카페라떼')
>>> data.delete()

### 다중 삭제
>>> Category.objects.filter(name='카페라떼').delete()
```
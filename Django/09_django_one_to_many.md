# RDBMS - One to many Relationships

## 테이블에 새로운 컬럼을 추가한다면
장고 ORM은 기본적으로 테이블에 null 값이 들어가지 않도록 합니다.  
즉, 빈 값이 허용되지 않습니다.  

모델을 생성할 때  
`content = models.TextField()` 이렇게 생성한다고 하면,  
사실 기본 값은 `content = models.TextField(null=False)`이기에, 빈 값을 허용하지 않도록 설정됩니다.  

새로운 열을 모델링 이후에 추가하게 되면, 기존 값들에 null 이 생기기 때문에 문제가 됩니다.  
이를 해결하기 위한 방법은

1. `(null=True)`로 바꿔주거나
2. 기존에 있던 값들을 채워주거나

이렇게 두 가지가 있을 것입니다.

<br>

값을 설정할 때 사용할 수 있는 속성이 있는데, 바로 `default` 속성입니다. 이 속성은 위의 2번을 가능하게 합니다.  
```python
content = models.TextField(default='default value')
```
content라는 새로운 열이 추가되면, 기존에 있던 레코드에 빈 값이 생기는데 `default`는 이 값들을 채워주는 역할을 합니다.  
따라서, 추가로 새로운 컬럼이 등장할 때는 `default` 속성을 지정해줘야 합니다.

<br>
<br>

## DB의 버저닝
migrations 폴더 안을 보면 `makemigrations` 할 때마다 0001..., 0002... 와 같은 파일들이 쌓입니다.  
이 파일들은 데이터베이스를 버저닝을 하고 있는 것이라고 생각하면 됩니다. 0001..., 0002... 버전으로 저장되고, 돌아가길 원한다면 이것으로 롤백할 수 있습니다.  
실제로 DB가 변화하는 과정은, 파이썬 코드에서 시작해 버전이 쌓이는 version migration을 거쳐 DB가 조작되는 순서입니다. 마치, 깃의 버전관리와 비슷한 모습입니다.  

때문에, `makemigrations`를 하나 할 때마다 커밋도 하나씩 해주는 것이 버전관리를 하기에 좋습니다.  
헷갈림 없이 DB의 버전을 명확하게 기록할 수 있기 때문입니다.  


### DB 정보를 수정하고 싶다면
아래의 순서로 하는 것이 좋습니다.  
모델 수정 -> `makemigrations` -> `migrate` -> `git commit`

<br>
<br>

## 기존 테이블과 연결되는 새로운 테이블
기존 테이블과 새로운 테이블을 연결하는 방법은, 새로운 테이블에 하나의 열을 만들어 그 열에 연결될 키를 넣는 것입니다.  
아티클 테이블과 연결할 새로운 댓글 테이블을 생성한다면, 각각의 댓글 레코드 안에 아티클의 id를 추가하면 됩니다.  
이 아티클의 id를 Primary Key(고유키)라고 합니다. Primary Key(고유키)값을 댓글 레코드의 Foreign Key(외래키) 자리에 넣으면 됩니다.  

<br>

### 새로운 테이블 생성
하나의 아티클에 N개의 댓글이 달리는 경우, 이 경우를 1 대 N 관계라고 합니다.  
댓글 테이블을 생성하면 각각의 댓글이 어떤 아티클에 달려있는지를 나타내기 위해 아티클 아이디 정보를 담는 열을 하나 추가해서 생성합니다.  

해당 데이터를 `ForeignKey`로 사용하기 원할 때, `ForeignKey()`을 씁니다.  
첫번째 인자로 프라이머리 키가 있는 모델을 넣고,  
두번째 인자로 해당 레코드가 삭제되면 같이 살제할건지 설정할 수 있습니다.`(on_delete=models.CASCADE)`
```python
class Comment(models.Model):
    content = models.CharField(max_length=200)
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
```
위의 설정으로, 해당 게시글이 지워지면 그 게시글에 달린 댓글도 지워질 것입니다.

<br>

### `ForeignKey`
위의 코드에서 article은 실제로 article을 저장한다는 의미가 아닙니다.  
`ForeignKey`를 사용하면 DB의 실제 컬럼 이름은 `article_id` 가 됩니다. `_id` 가 자동으로 붙는 것입니다.  

순서대로 살펴보자면,
1. Comment를 생성할 때, article에는 article 인스턴스 그 자체를 넣어줍니다.
2. article을 전달받은 ForeignKey는 article의 id만 추출하여 DB에 저장합니다.(article_id 컬럼으로)
3. 특정 comment가 article_id가 아닌 article을 호출하면, orm이 article_id 값으로 해당하는 article을 찾아와 반환합니다.
   
**즉, article의 데이터가 저장되는 것이 아니라, id 값만이 저장되는 것입니다.**

아래의 코드로 어떤 방식으로 호출할 수 있는지 알아볼 수 있습니다.
```python
a1 = Article.objects.create(title='Test Title', content='this is test content')

c1 = Comment()
c1.article = a1
c1.content = 'comment 1'
c1.save()
c2 = Comment.objects.create(article=a1, content='comment 2')
c3 = Comment.objects.create(article=a1, content='comment 3')

# c1 댓글 내용
c1.content  # 'comment 1'

# c1 댓글이 달린 게시글 id
c1.article_id  # 3

# c1 댓글이 달린 게시글
c1.article  # <Article: Article object (3)> => 3은 아티클의 id입니다.

# c1 댓글이 달린 게시글의 제목
c1.article.title  # 'Test Title'

# a1 게시글에 달린 댓글들 전부 (종속모델명_set => ForeignKey 생성 순간 생성됩니다.)
a1.comment_set.all()  # <QuerySet [<Comment: Comment object (1)>, <Comment: Comment object (2)>, <Comment: Comment object (3)>]>

# a1 게시글에 달린 모든 댓글들의 내용을 출력
for comment in a1.comment_set.all():
    print(comment.content)
# 'comment 1'
# 'comment 2'
# 'comment 3'
```

<br>

### Tip
models.py 파일은 코드가 돌면서 계속 실행이 됩니다.  
이 파일에 나중에 참고하고 싶은 코드를 써 놓는다면, 모델 파일이 실행됨에 따라 매번 같이 실행될 것입니다.  

이를 방지하기 위해 아래와 같이 쓸 수 있습니다.
```python
if __name__ == '__main__':
    a1 = Article.objects.create(title='Title', content='content')

    c1 = Comment()
    c1.article = a1
    # ...
```
이 파일이 직접 실행 됐을 때만 `'__main__'`가 `True` 가 됩니다.  
여기서 말하는 직접 실행이란, 커멘드 라인으로 `python board/models.py` 을 입력해 실행시키는 것을 말합니다.  
따라서 직접 실행하지 않는 한, 이 if 문 안에 작성한 코드는 실행되지 않습니다.

<br>

# Many-To-Many
Many-To-Many 관계는 새로운 테이블이 하나 필요하다.
관계를 One-To-Many로 전환시켜줄.

## ManyToManyField
```python
from django.db import models

class Actor(models.Model):
    name = models.CharField(max_length=100)

class Movie(models.Model):
    title = models.CharField(max_length=100)

class ActorMovie(models.Model):
    actor = models.ForeignKey(Actor, on_delete=models.CASCADE)
    movie = models.ForeignKey(Movie, on_delete=models.CASCADE)
```
```python
from django.db import models

class Actor(models.Model):
    name = models.CharField(max_length=100)

class Movie(models.Model):
    title = models.CharField(max_length=100)
    
    # 이 줄로 새로운 테이블이 생성된다.
    actors = models.ManyToManyField(Actor)

# class ActorMovie(models.Model):
#     actor = models.ForeignKey(Actor, on_delete=models.CASCADE)
#     movie = models.ForeignKey(Movie, on_delete=models.CASCADE)
```
`models.ManyToManyField()`를 사용하면 위의 코드와 똑같이 actor_id, movie_id를 담은 테이블이 생성된다.  
위의 케이스에선 테이블 이름이 `<app_name>_movie_actors`로 생성된다. 모델 명과 지정된 변수명이 합쳐져 만들어진 것이다. 변수명은 보통 복수로 작성한다.
`models.ManyToManyField()`를 두 모델 중 어느 쪽에서 작성해도 상관 없다.  

### 새로운 테이블 채우기 (M:N 관계 추가)
1. 영화 입장에서 배우 넣기
```python
a1 = Actor.objects.get(pk=1)
a2 = Actor.objects.get(pk=2)
a3 = Actor.objects.get(pk=3)

m1 = Movie.objects.get(pk=1)

m1.actors.add(a1)
m1.actors.add(a2, a3)
```

2. 배우 입장에서 영화 넣기
`_set`을 사용. 동작은 위와 똑같이 한다.
```python
m2 = Movie.objects.get(pk=2)

a1.movie_set.add(m2)
```

영화 입장에서 배우를 넣고, 그 다음 같은 배우 입장에서 같은 영화를 넣어도 DB에서 중복된 값을 생성하지는 않는다.

### 기존 M:N 관계 삭제하기
```python
m2 = Movie.objects.get(pk=2)

m2.actors.remove(a2)
```

### 정보 가져오기
```python
m1.actors.all()
a1.movie_set.all()
```


## 추가 모델
위와 같이 아이디만 필요한 게 아니라 추가적인 컬럼이 필요하다면, 
모델을 만들어야 한다. 모델에서 추가로 확장하고자 하는 컬럼을 넣어줘야 한다.
```python
from django.db import models

class Doctor(models.Model):
    name = models.CharField(max_length=20)

class Patient(models.Model):
    name = models.CharField(max_length=20)
    doctors = models.ManyToManyField(
        Doctor,
        through='Reservation',
        through_fields=('patient', 'doctor'),
    )

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    date = models.DateField()
```

```python
# 1번 환자의 모든 예약 의사
p1.doctors.all() 
# 1번 환자의 모든 예약
p1.reservation_set.all()

# 1번 의사의 모든 예약 환자
d1.patient_set.all()
# 1번 의사의 모든 예약
d1.reservation_set.all()
```


## related_name
역참조 이름을 _set 이 아닌 다름 이름으로 지정하는 것.
모델안에 변수명을 복수로 지정하고, related_name을 상대방의 복수형으로 맞춰버리는게 헷갈리지 않는다.

```python
from django.db import models

class Doctor(models.Model):
    name = models.CharField(max_length=20)

class Patient(models.Model):
    name = models.CharField(max_length=20)
    # Patient 객체 => doctors로 접근 (정참조)
    doctors = models.ManyToManyField(
        Doctor,
        through='Reservation',
        through_fields=('patient', 'doctor'),
        # Doctor 객체 => 기본값은 patient_set 임
        # 수정하려면 related_name 사용해 지정. (역참조)
        # _set 붙은 것은 모두 역참조
        related_name = 'patients',
    )

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE, related_name='reservations')
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE, related_name='reservations')
    date = models.DateField()
```

```python
# 1번 환자의 모든 예약 의사
p1.doctors.all() 

# 1번 환자의 모든 예약
p1.reservations.all()

# 1번 의사의 모든 예약 환자
d1.patients.all()

# 1번 의사의 모든 예약
d1.reservations.all()
```
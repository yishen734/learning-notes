# Build a Data Model
### Relationship Between Models
1. One-to-one
2. One-to-many
3. Many-to-many

![image](https://user-images.githubusercontent.com/70382342/159122531-bb48e104-2726-41ee-a3a7-f3b2cb674ab2.png)
![image](https://user-images.githubusercontent.com/70382342/159122572-ce48d8cd-38dd-4a2d-9dbb-f51dbcc86748.png)

Each app should only do one thing and do it well. Instead of putting everything together in the project (Monolith), we split the project into multiple apps.
Bad example:
![image](https://user-images.githubusercontent.com/70382342/159122743-1789612c-349e-4ba1-b222-9edc4a22edf0.png)
一个崩了，有 dependency的全部崩

Cart 和 Order 不应该分开，没有 Order，有 Cart 毫无意义
所以我们需要把 highly-related 的 app 合并在一起

如何分解呢？分析如下
1. 首先 tag 不是 project-specific 的，它可以用在任何其他 project 中，所以可以把 tag 单独分离出来成一个 app
![image](https://user-images.githubusercontent.com/70382342/159122898-71fbd50f-36cd-42ee-a6b0-c3885fca2dde.png)

把所有的东西都放在一起会很难 maintain，但是把 application 拆分的过细，会导致它们之间有过多的 coupling
一个好的拆分应该是使得 apps 之间由最小的 coupling，同时保证最大的 cohension (每一个 app 只 focus 在一件事情上面)

根据上面的分析，我们现在创建 store 和 tags 两个 app
```python3
python manage.py startapp store
python manage.py startapp tags
```

### Create Models
Let's create the model for ```Product``` and ```Customer```
```python3
from django.db import models

class Product(models.Model):
    title = models.CharField(max_length=255)
    description = models.TextField()
    price = models.DecimalField(max_digits=6, decimal_places=2)
    inventory = models.IntegerField()
    last_update = models.DateTimeField(auto_now=True)


class Customer(models.Model):
    MEMBERSHIP_BRONZE = 'B'
    MEMBERSHIP_SILVER = 'S'
    MEMBERSHIP_GOLD = 'G'
    MEMBERSHIP_CHOICES = [
        (MEMBERSHIP_BRONZE, 'Bronze'),
        (MEMBERSHIP_SILVER, 'Silver'),
        (MEMBERSHIP_GOLD, 'Gold'),
    ]
    first_name = models.CharField(max_length=255)
    last_name = models.CharField(max_length=255)
    email = models.EmailField(unique=True)
    phone = models.CharField(max_length=255)
    birth_date = models.DateTimeField(null=True)
    membership = models.CharField(max_length=1, choices=MEMBERSHIP_CHOICES, default=MEMBERSHIP_BRONZE)


class Order(models.Model):
    PAYMENT_STATUS_PENDING = 'P'
    PAYMENT_STATUS_COMPLETE = 'C'
    PAYMENT_STATUS_FAILED = 'F'
    PAYMENT_STATUS_CHOICES = [
        (PAYMENT_STATUS_PENDING, 'Pending'),
        (PAYMENT_STATUS_COMPLETE, 'Complete'),
        (PAYMENT_STATUS_FAILED, 'Failed'),
    ]
    place_at = models.DateTimeField(auto_now_add=True)
    payment_status = models.CharField(max_length=1, choices=PAYMENT_STATUS_CHOICES)
```



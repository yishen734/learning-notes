# Build a Data Model
## 模型关系
- One-to-one: 

- One-to-many: 
E.g. ```Customers (one)``` and ```Orders (many)```. 在 Django 中, 我们把 ``` Customers (parent)``` 作为 ```外键 (foreign key)``` 添加到 ```Orders (child)``` 类中. 

- Many-to-many: 
E.g. ```Orders (many)``` and ```Tags (many)```. 在 Django 中, 我们把 ```Tags``` 通过 ```ManyToManyField``` 添加到 ```Orders``` 类中, 或者我们也可以通过使用两个外键来将它们关联在一起. 两种方法是等价的.

<br>

## 如何设计模型间的关系
每个 app 都应该只做一件事, 并且做好这件事. 我们应当把项目分成多个 app, 而不是把所有东西都放在一起 (Monolith).


#### 不好的设计案例
把 ```Products```, ```Orders```, ```Customers```, ```Carts``` 分离成四个不同的 apps


#### 这样设计的问题
一个 app 崩了或者被更新了, 对其有依赖的所有其他 apps 都要修改, 即牵一发而动全身. 同时, ```Cart``` 和 ```Order``` 本就不应该分开, 因为只有 ```Cart``` 而没有 ```Order``` 毫无意义. 因此, 我们需要把高度相关的 apps 合并在一起.

> 把所有的东西都放在一起会很难维护, 但是把 application 拆分的过细, 会导致它们之间有过多的 coupling.


#### 如何正确设计?
我们应该把 ```Products```, ```Orders```, ```Customers```, ```Carts``` 放在一个 app 里面, 同时创建一个新的 ```tags``` app. 之所以 ```tags``` 应该单独成为一个 app 是由于它不是 project-specific 的, 即可以用在任何其他 project 中.


#### 小结
一个好的拆分应该是使得 apps 之间由最小的 **coupling**, 同时保证最大的 **cohension (每一个 app 只 focus 在一件事情上面)**.

根据上面的分析, 我们应当创建 ```store``` 和 ```tags``` 两个 apps.
```python3
python manage.py startapp store
python manage.py startapp tags
```

<br>

## 创建模型的代码
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
    
class Address(model.Model):
    street = models.CharField(max_length=255)
    city = models.CharField(max_length=255)
    
    """
    customer 应该在 Address 被创建之前就存在
    OneToOneField 的第一个参数指的是 parent module
    on_delete 指的是当 parent 被删除之后 child 的行为是什么 (CASCADE 指的是级联删除)
    与此同时我们不再需要在 Customer 里面定义 address, 因为 django 会自动帮我们添加
    如果我们想要创建 one-to-many 关系, 就需要把 OneToOneField 改成 ForeignKey
    """
    customer = models.OneToOneField(Customer, on_delete=model.CASCADE, primary_key=True)
```



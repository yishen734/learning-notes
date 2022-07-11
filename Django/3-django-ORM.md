# Django ORM
本质上就是 project 和 数据库之间的一个沟通桥梁, 可以帮助程序员通过 python 来操作数据库, 因此就不再需要写 SQL 代码

## Managers
Django 中的每一个 ```model``` 都有一个 attribute 叫做 ```objects```, 其本质是一个 ```manager object```. ```manager``` 是数据库的 ```interface```, 就像一个遥控器一样, 通过上面的按钮去操作数据库.

## Queryset
```manager``` 的一些方程会返回一个 ```queryset```, 其本质上是一个 ```包装 SQL query```的对象. ```queryset``` 包装的语句不会被马上运行 **(懒执行机制)**, 而是只有在特定的时刻才会被解析. 之所以存在懒加载机制是因为我们可以在原 ```queryset``` 的基础上通过 ```queryset```提供的种种方法去构建构建更加复杂的 ```queryset```. 如果一旦一个 ```queryset``` 被创建就被马上执行, 就会很有可能做很多无用功.

### 懒执行必要性的案例
```python
# 如果没有懒执行机制, 那么 all() 就有可能返回上亿数据, 内存直接爆炸
query_set = Product.objects.all()
query_set.filter().filter().order_by()

# count() 不存在懒执行, 因为 count() 直接返回一个数字, 因此无法跟后序操作
query_set.count()
```

### Queryset 解析的时刻
- 当我们 iterate 一个 ```queryset``` 的时候
```python
for product in queryset:
    print(product)
```

- 当我们将 ```queryset``` 转化成为一个 ```list``` 的时候
```python
list(queryset)
```

- 当我们从 ```queryset``` 中获取元素的时候
```python
queryset[0:5]
```

## ORM 操作
### Retriving 
```python3
# 获取所有的元素
queryset = Product.objects.all()
 
# 获得一个元素, 返回一个 object (因为无法做后序操作)
# 值得注意的是 get() 如果无法获得数据会抛异常
try:
    product = Product.objects.get(pk=1)  # Django 会自动把 pk 转化成表单的 primary key
except ObjectDoesNotExist:
    pass

# 是上面的写法的简化版本, first() 可以返回 None
product = Product.objects.filter(pk=1).first()

# 检查某一个元素是否存在
exists = Product.objects.filter(pk=1).exists()
```

### Filtering 
```python3
# 下面都是 Field lookup (通过 __ 来进行精准 filter) 的案例

# 价格大于 20 的 product
queryset = Product.objects.filter(unit_price__gt=20)

# 价格在 (20, 30) 之间的 product
queryset = Product.objects.filter(unit_price__range=(20, 30))

# Collection ID = 1 的 product
queryset = Product.objects.filter(collection__id__gt=1)

# 标题里面包含 coffee 的 product (contains 是 case-sensitive, icontains 是 case-insensitive)
queryset = Product.objects.filter(title__icontains='coffee')

# 最后更新日期年份为 2021 的 product
queryset = Product.objects.filter(last_update__year=2021)

# 不包含最后更新日期的 product
queryset = Product.objects.filter(last_update__isnull=true)
```

### Q-Objects 
```python3
# 存库少于 10, 价格小于 20 的 product
from django.db.models import Q
queryset = Product.objects.filter(inventory__lt=10, unit_price__lt=20)
queryset = Product.objects.filter(inventory__lt=10).filter(unit_price__lt=20)

# 存库少于 10 或者 价格小于 20 的 product
queryset = Product.objects.filter(Q(inventory__lt=10) | Q(unit_price__lt=20))

# 存库少于 10 并且 价格不小于 20 的 product
queryset = Product.objects.filter(Q(inventory__lt=10) & ~Q(unit_price__lt=20))
```

### F-Objects 
```python3
# 存库和价格相等的 product
queryset = Product.objects.filter(inventory=F('unit_price'))

# 存库和集合 ID 相等的 product
queryset = Product.objects.filter(inventory=F('collection__id'))
```

### Sorting
```python3
# 正向排序, 反向排序 (-)
queryset = Product.objects.order_by('title')
queryset = Product.objects.order_by('-title')
queryset = Product.objects.order_by('unit_price', 'title')

# reverse() 会反转所有排序
queryset = Product.objects.order_by('unit_price', 'title').reverse()
queryset = Product.objects.order_by('unit_price')[0]

# earliest() latest() 返回 object
# 起到的也是正反向排序的作用
queryset = Product.objects.earliest('unit_price')[0]
```

### Limiting
```python3
# 利用 slicing 返回部分元素 => SQL 语句里面会添加 LIMIT 命令
queryset = Product.objects.all()[:5]
```

### Selecting Fields to Query
```python3
# 只选择 id, title fields, 返回 dict
queryset = Product.objects.values('id', 'title', 'collection__title')

# 返回 tutple
queryset = Product.objects.values_list('id', 'title', 'collection__title')

# 返回 instances, 用 only 的时候要小心, 有可能产生很多 query
queryset = Product.objects.only('id', 'title')

# in, distinct
queryset = Product.objects.filter(id__in=OrderItem.objects.values('product_id')).distinct().order_by('title')
```

### Defering Fields
```python3
# 排除 field
# 但和 only 一样, 用的时候要小心, 比如我们用 defer 排除了description
# 但是后来又尝试去访问 description, 那么就会额外产生很多 query
queryset = Product.objects.defer('description')
```

### Selecting Related Objects
```python3
# 假设我们写了一下语句, 但是在前端尝试去访问 collection__title
# 那么就会产生很多额外的 query
queryset = Product.objects.all()

# 用 select_related 可以解决这个问题
queryset = Product.objects.select_related('collection').all()

# 1 对多的时候用 prefetch
queryset = Product.objects.prefetch_related('promotion').all()
```

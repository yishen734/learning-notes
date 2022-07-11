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
```python
# ---------------- Retriving ----------------
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

# ---------------- Filtering ----------------
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


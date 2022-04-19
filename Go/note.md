# 变量, 数据类型, 操作符, 常数
*作者: Eason* <br>
*时间: 2022/04/18*

## 变量 (Variables)
### 变量的声明 (Variable Declaration)
1. ```var food int``` 一般变量的赋值在另外一个 scope 的时候用这个
2. ```var foo int = 42``` 在 package scope 的时候必须使用这个
3. ```foo := 42``` 最经常使用这个, 因为最简洁 <br>
    要注意的是 ```foo := 42.1``` 默认 ```foo``` 的类型是 ```float64```, 所以如果要使类型是 ```float32``` 则需要使用第二种命名方式

- Cannot redeclare variables, but can shadow them
- 所有被声明的变量都**必须**被使用

### Visibility
- Lower case first letter for package scope
- Upper casse first letter (in package scope) to export
- No private scope

### 命名习惯
- 一般来说越短越好，变量名的长度应该和该变量的生命周期成正比, 控制循环的变量用 ```i, j``` 就行了
- 驼峰命名, 首字母缩写词 (acronym) 大写, ```e.g. HTTP, URL```

### 类型转化
- ```e.g. float32(a)```
- 用 ```strconv``` 去转化为 strings

<br>

## 数据类型
1. *Boolean*
    ```go
    // 有两个值 true 或者 false
    // bool 和 int 不能隐形转化 (有一些语言 -1 代表false, 1 代表 true)
    var n bool // 默认值为 false
    ```
2. *Integer*
    ```go
    var a int // int 是多少位取决于 system, 最低值为 32 位
    // 有符号 int: int8, int16, int32, int64, uint8, 
    // 无符号 int: uint16, uint32
    // 两个不同类型的 int 不可以相加, 比如 int + int8 会报错
    ```
3. *Float*
    ```go
    // 只有两个类型: float32, float64
    // 下面是几种不同的 styles
    n := 1.3
    n := 13.7e72
    n := 2.1E14
    ```
4. *Complex Number*
    ```go
    // 只有两个类型: complex64, complex128
    // 默认值是 (0 + 0i)
    // 其他细节暂时省略, 因为并不常用
    ```
5. *String*
    ```go
    // string 的声明以及赋值
    s := "this is a string"  // string 是 immutable 的, 所以 s[0] => t" 会报错
    
    // string 的拼接
    s1 := "abc"
    s2 := "cdf"
    s3 = s1 + s2
    
    // 将 string 转化成为 collection of bytes
    s := "this is a string"
    b := []byte(s)
    fmt.Printf("%v, %T", b, b)  // [116 104 105 115 ........], []unit8
    ```
6. *Runes (int32)*
    ```go
    r := 'a' 
    var r rune = 'a'
    fmt.Printf("%v, %T", r, r)  // 97, int32
    ```
<br>

## 操作符
7. *运算符*
    ```go
    // 加: +
    // 减: -
    // 乘: *
    // 除: /
    // 余: %
    ```
9. *位运算*
    ```go
    // And:      &
    // Or:       |
    // XOR:      ^
    // NOT AND:  &^
    ```
11. *位移*
    ```go
    // 左移: << 
    // 右移: >> 

<br>

## 常数 (Constant)

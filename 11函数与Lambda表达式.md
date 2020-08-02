# 11、函数与Lambda表达式
## 1.函数
### 1.1定义与使用
以def关键词开头，后接函数名和圆括号()。函数执行的代码以冒号起始，并且缩进。return [表达式] 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回None。
```python
def add(a, b):
    print(a + b)


def minus(a, b):
    return a - b


add(1, 2)  # 3
print(minus(2, 1)) # 1
```
### 1.2函数文档
```python
def MyFirstFunction(name):
    "函数定义过程中name是形参"
    # 因为Ta只是一个形式，表示占据一个参数位置
    print('传递进来的{0}叫做实参，因为Ta是具体的参数值！'.format(name))


MyFirstFunction('老马的程序人生')  
# 传递进来的老马的程序人生叫做实参，因为Ta是具体的参数值！

print(MyFirstFunction.__doc__)  
# 函数定义过程中name是形参
```
### 1.3函数参数
位置、默认（需要放在位置参数之后）。
#### 可变
*args 表示可变参数组成元组。
#### 关键词
**kw 将关键词组成字典。
其中命名关键词使用*, nkw，用于限制关键词名字。
#### 1.4变量作用域
定义在函数内部的变量拥有局部作用域，该变量称为局部变量。
定义在函数外部的变量拥有全局作用域，该变量称为全局变量。
局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。
```python
num = 1


def fun1():
    global num  # 需要使用 global 关键字声明
    print(num)  # 1
    num = 123
    print(num)  # 123


fun1()
print(num)  # 123
```
#### 内嵌函数
闭包作为一个特殊的内嵌函数。
```python
def make_counter(init):
    counter = [init]

    def inc(): counter[0] += 1

    def dec(): counter[0] -= 1

    def get(): return counter[0]

    def reset(): counter[0] = init

    return inc, dec, get, reset


inc, dec, get, reset = make_counter(0)
inc()
inc()
inc()
print(get())  # 3
dec()
print(get())  # 2
reset()
print(get())  # 0
```
递归即在函数内部调用自己。
```python
def recur_fibo(n):
    if n <= 1:
        return n
    return recur_fibo(n - 1) + recur_fibo(n - 2)


lst = list()
for k in range(11):
    lst.append(recur_fibo(k))
print(lst)  
# [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```
## 2.Lambda表达式
可以将函数独立，对于相同的输入总有相同的输出。经常用于高阶函数中。
```python
def sqr(x):
    return x ** 2


print(sqr)
# <function sqr at 0x000000BABD3A4400>

y = [sqr(x) for x in range(10)]
print(y)
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

lbd_sqr = lambda x: x ** 2
print(lbd_sqr)
# <function <lambda> at 0x000000BABB6AC1E0>

y = [lbd_sqr(x) for x in range(10)]
print(y)
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]


sumary = lambda arg1, arg2: arg1 + arg2
print(sumary(10, 20))  # 30

func = lambda *args: sum(args)
print(func(1, 2, 3, 4, 5))  # 15
```
## 3.练习题
1.怎么给函数编写⽂档？
在函数中" "。
2.怎么给函数参数和返回值注解？
def accumlate(x : int, y : int) -> int:。
3.闭包中，怎么对数字、字符串、元组等不可变元素更新。
用nonlocal进行声明后修改。
4.分别根据每一行的首元素和尾元素大小对二维列表 a = [[6, 5], [3, 7], [2, 8]] 排序。(利用lambda表达式)
```python
a = [[6, 5], [3, 7], [2, 8]] 
a.sort(key = lambda x: x[0])
print(a)

a.sort(key = lambda x: x[1])
print(a)
```
5.有a、b、c三根柱子，在a柱子上从下往上按照大小顺序摞着64片圆盘，把圆盘从下面开始按大小顺序重新摆放在c柱子上，尝试用函数来模拟解决的过程。（提示：将问题简化为已经成功地将a柱上面的63个盘子移到了b柱）
```python
def hanoi(n:int, a, b, c):
    if n == 1:
        mov(a, c)
    else:
        hanoi(n - 1, a, c, b)
        mov(a, c)
        hanoi(n - 1, b, a, c)

def mov(a, c):
    print("move" + a + "-->" + c)
    
hanoi(64, 'a', 'b', 'c' )
```
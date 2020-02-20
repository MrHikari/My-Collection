#### 递归的概念

函数可以自己调用自己，称为递归调用。虽然可以写出递归，但是我并不知道它是如何得出结果。

#### 函数的递归调用

方法：

1. 首先去找临界值，就是无需计算，就能获得的值
2. 找这一次和上一次的关系
3. 假设当前函数已经可以使用了，调用自身计算上一次的运行结果，再写出这次的运行结果

例如计算1加到n的和

```js
sum(100) = sum(99) + 100;
sum(n) = sum(n - 1) + n;
```

`需要注意的是，递归会在短时间内，使内存剧增。`

```js
function sum(n){
    if (n == 1) {
        return 1;
    }
    return sum(n - 1) + n;
}
alert(sum(100));
```

结果为5050。

运行特点：

1. 必须有参数
2. 必须有return

#### 实例:

通过递归，打印**n**个**hello world**

```js
function print(n){//必须有参数
    if (n == 0) {
        return;//必须有return
    }
    document.write("hello world<br>");
    return print(n - 1);
}
print(10);
```

**判断一个函数是否为闰年**

参数：年份　　返回值：是否为闰年

```js
function leapYear(){
    var x = new Date().getYear();
    if (x % 400 == 0 || x % 4 == 0 && year % 100 != 0) {
        return true;
    } else{
        return false;
    }
}
alert(leapYear());    //今年不是闰年
```

**判断一个数是否为素数**

素数：只能被1和他本身整除的数

```js
function prime(num){
    var isYes = false;
    for(var i = 2; i < num; i++){
        if(num % i ==0){
            isYes = true;    //循环中有一次成功
            break
        }
    }
    return !isYes;
}
alert(prime(3));
```

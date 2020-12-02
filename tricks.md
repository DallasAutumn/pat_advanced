# 一些比较实用的小玩意儿

## 1.大小写互换
```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
    string s = "aAbB";
    for(int i = 0; i < s.size(); i++)
        s[i] ^= 32;

    cout << s; // 输出AaBb，杂合子

    return 0;
}
```
使用通常的tolower，toupper也可。

## 2.判断奇偶
```cpp
int is_odd(int num)
{
    return num & 1;
}
```
简单的位运算，判断机器数最后一位是否为1，是1就是奇数。通常的写法是
```cpp
int is_odd(int num)
{
    return num % 2;
}
```

## 3.判断质数
```cpp
bool isPrime(int n)
{
    if(n <= 2)
        return false;

    int sqr = (int)sqrt(1.0 * n);
    for(int i = 0; i <= sqr; i++)
        if(num % i == 0)
            return false;

    return true;
}
```

## 4.求位数和
```cpp
int getDigitSum(int n)
{
    int ans = 0;
    while(n)
    {
        ans += (n % 10);
        n /= 10;
    }
    return ans;
}
```

## 5.求最大公约数，最小公倍数
```cpp
int gcd(int a, int b)
{
    return !b ? a : gcd(b, a % b);
}

int lcm(int a, int b)
{
    int d = gcd(a, b);
    return a / d * b;
}
```
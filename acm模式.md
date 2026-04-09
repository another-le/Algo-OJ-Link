# acm模式

参考视频：[一个视频讲明白ACM模式！](https://www.bilibili.com/video/BV1KCtRzSEcg/?spm_id_from=333.337.search-card.all.click&vd_source=b1a946a5e3102f0454c4d2a036a9e703)

## 输入

### 固定式输入

> 常见形式：
>
> 1. 第一行输入三个数x,y,z
> 2. 前两行为整数x,y
> 3. 第一行为一个整数n，第二行输入一个长度为n的整数

前两种比较简单，针对第三种示例：

c++:

```cpp
int n;
cin >> n;
vector<int> a(n); 
for(int i = 0; i < n; i++) {
    cin >> a[i];
}
```

python:

```python
n = int(input()) 
array = list(map(int, input().split())) 
```

java:

```java
int n = in.nextInt(); 
int[] arr = new int[n]; 
for(int i=0; i<n; i++){
  arr[i] = in.nextInt();
} 
```

### 不定式输入分析

#### 一行内数量不确定

> 如：输入一行，若干个整数。
>
> 输入：1 2 3 4 5 6 ...

c++:

```cpp
int x, len = 0;
int data[1000]; // 注意：原图这里漏了分号，这里补上了

// cin自动判断EOF (End Of File)
while(cin >> x) {
    data[len] = x;
    len++;
}
```

java:

```java
// java
Scanner in = new Scanner(System.in);
int[] data = new int[1000];
int len = 0;

// Scanner.hasNext系列函数可判断EOF
while (in.hasNextInt()) {
    data[len] = in.nextInt();
    len++;
}
```

python:

```python
# python
# 读取一行，分割，转为int，存入列表
data = list(map(int, input().split()))
```

#### 数据组数不确定

> 多个输入组，但组数不确定，每组数据的格式是：第一行一个整数 n ，第二行 n 个整数。如：
>
> 5
>
> 1 2 3 4 5
>
> 3
>
> 1 2 3
>
> ...

c++:

```cpp
// cpp
int n;
int arr[1000];
while(cin >> n) {  // 只要还能读到 n，就继续处理一组
    for (int i = 0; i < n; i++){
        cin >> arr[i]; // 读取 n 个数据
    }
}
```

java:

```java
// java
Scanner in = new Scanner(System.in);
int[] arr = new int[1000];

while (in.hasNextInt()) { // 判断是否还有下一个整数
    int n = in.nextInt(); // 读取 n
    // 读取当前组的 n 个数据
    for (int i = 0; i < n; ++i) {
        arr[i] = in.nextInt();
    }
}
```

python:

```python
# python
import sys

for line in sys.stdin:  # 逐行读取
    # 先读取每组的第一个数 (表示该组数据个数)
    count = int(line)

    # 读取下一行数据，作为该组的具体内容
    data_line = sys.stdin.readline().strip()
    data = list(map(int, data_line.split()))
```

#### 组数和组内数据个数均不确定

> 如：
>
> 1 2 3 4
>
> 1 1 3 4 5 1 4
>
> ...

c++:

```c++
// cpp
int n, data[1000];
string line;

// getline 成功读取时返回 true, EOF 时返回 false
while (getline(cin, line)) {
    // 对本组数据处理
    int num, len = 0;

    // 关键点：使用 stringstream 将字符串变成像 cin 一样的流
    stringstream ss(line);
    while(ss >> num){
        data[len] = num; // 注意：图中这里写成了 date，应该是 data
        len++;
    }
}
```

java:

```java
// java
Scanner in = new Scanner(System.in);
int[] data = new int[1000];

// 循环读取每行，判断 EOF
while (in.hasNextLine()) {
    // 读取一整行
    String line = in.nextLine();

    // 关键点：利用新的 Scanner 解析这一行数据
    Scanner lineScanner = new Scanner(line);
    int len = 0;
    while(lineScanner.hasNextInt()){
        data[len] = lineScanner.nextInt();
        len++;
    }
}
```

python:

```python
# python
import sys
for line in sys.stdin:
    data = list(map(int, line.split()))
```

## 输出

**题目中没有的要求不要默认加上（换行、空格等）！**

### **C++ 的输出逻辑**

**核心特性**：`cout` 是“所见即所得”的流式输出。它不会自动添加空格或换行，除非你显式地告诉它。

```c++
// 输出 "1 2 3" 并换行
cout << 1 << " " << 2 << " " << 3 << "\n";
// 输出 "6"，此时光标停在 6 后面
cout << 6;
```

### **Java 的输出逻辑**

**核心特性**：Java 提供了两个主要的输出函数，区别在于是否自动换行。

- **`System.out.print()`**：只输出内容，不换行。

- **`System.out.println()`**：输出内容后自动换行。

  ```java
  // 使用 println，输出 "1 2 3" 后自动换行
  System.out.println(1 + " " + 2 + " " + 3);
  // 使用 print，输出 "6" 后不换行
  System.out.print(6);
  ```

### **Python 的输出逻辑**

**核心特性**：Python 的 `print()` 函数非常智能，但也有一些“隐式”行为需要控制。

- **多元素输出**：`print(1, 2, 3)` 会自动在元素之间插入空格，输出 `1 2 3`。

- **默认换行**：默认情况下，`print()` 结束后会自动加一个换行符（`\n`）。

- **`end` 参数**：这是 Python 输出控制的精髓。通过 `end=""` 可以覆盖默认的换行行为，让输出停留在当前行。

  ```python
  # 默认行为：元素间加空格，结尾加换行
  print(1, 2, 3)
  # 修改结尾字符：输出 6 后，结尾不是换行，而是空字符串（即不换行）
  print(6, end="")
  ```

## oj常见错误

| 缩写 | 全称                  | 含义       | 常见原因与对策                                                                     |
| ---- | --------------------- | ---------- | ---------------------------------------------------------------------------------- |
| AC   | Accepted              | 通过       | 代码逻辑正确，时间和空间复杂度符合要求。这是我们的终极目标。                       |
| WA   | Wrong Answer          | 答案错误   | 1. 逻辑错误：算法写错了。 2. 格式错误：多了或少了空格、换行（部分平台会判为 PE）。 |
| TLE  | Time Limit Exceeded   | 超时       | 程序运行时间超过了限制。通常是算法复杂度太高，或者陷入了死循环。                   |
| RE   | Runtime Error         | 运行时错误 | 程序在运行中崩溃了。常见原因：数组越界、除以零、栈溢出。                           |
| CE   | Compilation Error     | 编译错误   | 代码有语法错误，连编译都过不了。检查拼写、符号、头文件等。                         |
| MLE  | Memory Limit Exceeded | 超内存     | 程序占用的内存超过了限制。通常是开了过大的数组或使用了过于臃肿的数据结构。         |

 **关于 WA 与 PE**

- **WA** 是最常见的错误，意味着你的输出和标准答案不一致。
- 很多时候 WA 不是因为算法错了，而是因为**输出格式**不对（比如行末多了空格）。
- **PE** 专门指格式错误，但现在的很多平台（如 LeetCode, Codeforces）通常会将 PE 直接归类为 WA，所以格式一定要严格对齐。

 **关于 TLE 与语言效率**

- **时间复杂度估算**：通常认为计算机 1 秒大约能处理 $10^8$ 次运算。
- 语言差异
  - **C++**：最快，安全计算量在 $10^8 \sim 10^9$ 级别。
  - **Java**：稍慢，约为 $10^8 \sim 10^8$ 级别。
  - **Python**：较慢，约为 $5×10^6 \sim 10^7$ 级别。
- **对策**：如果你用 Python 写 $O(n^2)$ 的算法且 $n=10^5$ ，必 TLE 无疑。

 **关于 RE 的陷阱举例：**

- **数组越界**：这是新手最容易犯的错。定义 `arr[10]`，却访问了 `arr[10]`（最大下标是 9）或更大。
- **除以零**：在做除法或取模运算前，一定要检查分母是否为 0。

 **关于 MLE 与数据类型**

- **内存计算**：在 64MB 限制下，C++ 的 `int` 数组（每个 4 字节）大约能开 $1.6×10^7$个元素。
- **Python 的内存坑**：Python 的 `int` 是对象，内存开销远大于 C++ 的原生 `int`（28字节 vs 4字节）。如果题目卡内存很紧，用 Python 可能会因为内存占用过大而 MLE。
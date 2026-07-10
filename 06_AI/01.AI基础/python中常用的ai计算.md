# 标准输入list和矩阵

```python
arr = list(map(int, input().strip().split())

m = int(input().strip())
matrix = []
for _ in range(m):
    matrix.append(list(map(int, input().strip().split()))
```

# 向上向下取整，四舍五入

```python
import math

x = 2.4
y = 2.5
z = 2.6
print(math.ceil(x), math.ceil(y))   # 3 3
print(math.floor(x), math.floor(y)) # 2 2
print(round(x), round(y), round(z)) # 2 2 3

```

# np一维数组

```python
>>> arr = np.zeros(5)
>>> arr
array([0., 0., 0., 0., 0.])
>>> print(arr)
[0. 0. 0. 0. 0.]
```

# np二维数组

```python
>>> matrix = np.zeros([5, 5])
>>> print(matrix)
[[0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]]
```

# 数组求和

## 普通数组

```python
>>> arr = [1,2,3,4]
>>> print(sum(arr))
10
```

## np数组

```python
>>> arr = np.array([1,2,3,4,5])
>>> print(np.sum(arr))
15
```

# 数组或矩阵乘以一个数

```python
>>> matrix = np.array([1,2,3,4,5,6]).reshape([2,3])
>>> print(matrix)
[[1 2 3]
 [4 5 6]]
>>> print(np.multiply(matrix, 3))
[[ 3  6  9]
 [12 15 18]]
```

# np矩阵乘法

```python
>>> matrix = np.array([[1,2,3], [4,5,6]])
>>> arr = np.array([7,8,9]).reshape([3,1])
>>> print(matrix)
[[1 2 3]
 [4 5 6]]
>>> print(arr)
[[7]
 [8]
 [9]]
>>> print(np.dot(matrix, arr))
[[ 50]
 [122]]


```

# 二维数组转为一维数组

```python
>>> matrix = np.array([1,2,3,4,5,6]).reshape([2,3])
>>> print(matrix)
[[1 2 3]
 [4 5 6]]
>>> print(matrix.reshape(6))
[1 2 3 4 5 6]
>>> print(matrix.reshape([6,1]))
[[1]
 [2]
 [3]
 [4]
 [5]
 [6]]
```

# np数组排序，获取索引

```python
>>> arr = np.array([6,4,5,7,8,9,1,2])
>>> sortedIdx = np.argsort(arr)
>>> print(sortedIdx)
[6 7 1 2 0 3 4 5]
```

# np数组反转

```
>>> arr = np.array([6,4,5,7,8,9,1,2])
>>> arr = arr[::-1]
>>> print(arr)
[2 1 9 8 7 5 4 6]
```

# 日期和时间

```python

    now = datetime.datetime.now()
    print("当前时间:", now)

   # 创建特定日期
   specific_date = datetime.datetime(2023, 10, 5, 12, 0, 0)
   print("特定日期:", specific_date)

   # 计算前一天同一时刻
   yesterday_specific = specific_date - datetime.timedelta(days=1)
   print("前一天同一时刻:", yesterday_specific)

   # 计算前一周同一时刻
   last_week_specific = specific_date - datetime.timedelta(days=7)
   print("前一周同一时刻:", last_week_specific)

   # 判断星期几
   weekday_specific = specific_date.weekday()
   print("特定日期是星期:", weekday_specific)

   weekday_full_specific = specific_date.strftime('%A')
   print("特定日期是星期:", weekday_full_specific)

```

输出：

```bash
当前时间: 2025-09-03 11:48:53.970193
特定日期: 2023-10-05 12:00:00
前一天同一时刻: 2023-10-04 12:00:00
前一周同一时刻: 2023-09-28 12:00:00
特定日期是星期: 3
特定日期是星期: Thursday
```

# 定制排序

```python
data = [[1, 4],
        [5, 1],
        [7, 3],
        [2, 5],
        [9, 5]]
# 按行排序
print(np.sort(np.array(data), axis=1))
# 按列排序
print(np.sort(np.array(data), axis=0))
# 先按第二个元素排序，相等时按第一个元素排序
print(sorted(data, key=lambda x: (x[1], x[0])))

```

输出:

```bash

[[1 4]
 [1 5]
 [3 7]
 [2 5]
 [5 9]]

[[1 1]
 [2 3]
 [5 4]
 [7 5]
 [9 5]]

[[5, 1], [7, 3], [1, 4], [2, 5], [9, 5]]

```

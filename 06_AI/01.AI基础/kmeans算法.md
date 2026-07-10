# 背景知识

Kmeans聚类算法是机器学习最基础的分类算法之一，主要思路是通过特征之间的距离，把数据点归到最近的聚类中心所属的分类，更新聚类中心，不断迭代优化聚类结果。算法虽然简单，但是在实际业务场景中却能经常用到，以到20%的付出，收获80%的效果。

# 试题

## 题目

FTTR（Fiber to the room）规划中，若干用户通过光纤连接到同一个机房，其中主要成本之一为需要铺设的光纤长度，即为机房到所属用户之间的总距离，因此机房的选址应该使该距离尽可能小。

现在有一组用户的坐标和需要新建的机房个数K，不考虑机房的容量上限和覆盖距离上限，请用Kmeans算法给出每个机房规划的位置（坐标），点对之间的距离使用直线距离，即：

$$
x d_{i,j} = \sqrt{(\text{经度}_i - \text{经度}_j)^2 + (\text{纬度}_i - \text{纬度}_j)^2}
$$
					

可以假设用户的分布有相对明显的簇状结构，不会出现跨越经度180度的情况。

解答要求：

时间限制：1000ms, 内存限制：256MB

输入：

第一行，正整数K，设备的个数

第二行，K个设备的经纬度，均为浮点数，单个设备经纬度之间以英文逗号分割，设备经纬度之间以英文分号分割，如：120.0, 80;120.1,80.1;121.1,80.1;

第三行，正整数N，用户点的个数

第四行，N个设备的经纬度，均为浮点数，单个设备经纬度之间以英文逗号分割，设备经纬度之间以英文分号分割

第五行，正整数M，算法迭代次数

输出：

第一行，迭代M次之后N个设备的经纬度坐标（均保留两位小数），单个设备经纬度之间以英文逗号分割，设备经纬度之间以英文分号分割。经纬度输出顺序：按经度从小到大排序，经度相同的，按维度从小到大排序。

**输入样例1：**

2

114.0, 86.0;116.0,84.0

10

119.37,81.03;119.73,79.51;121.51,81.29;120.17,79.14;119.7,79.64;111.49,90.02;109.66,91.63;110.27,89.6;110.36,88.7;110.87,89.50

3

**输出样例1：**

110.53,89.89;120.10,80.12

四舍五入保留两位小数

**输入样例2：**

3

0,1;1,0;-1,0

15

0.16,10.74;0.18,9.66;-0.88,10.49;-0.68,9.62;0.4,10.9;4.64,-5.1;5.24,-4.82;5.5,-5.18;5.16,-4.0;4.94,-4.09;-5.31,-3.74;-3.79,-3.39;-4.75,-3.89;-6.17,-5.46;-5.42,-4.9

2

**输出样例2：**

-0.16,10.28;5.09,-4.64;-5.09,-4.28

四舍五入保留两位小数

## 题目理解

Kmeans的三个关键超参为：分类的数量K、K个分类的初始中心点坐标和迭代次数。在本题目中关键输入都已给出，只需要知道Kmeans迭代的方法即可。

## 解题思路

1）  根据初始中心点坐标，遍历所有数据点，计算出距离最近的中心点，将该点归到中心点对应的点集中

2）  根据第一轮迭代产生的点集，计算每个点集的中心，即横纵坐标的平均值，作为新的中心点坐标

3）  根据新的中心点坐标，重新计算每个点离中心的距离，并重新归类

4）  重复1\~3直至达到迭代次数上限。

# 参考代码

## python

```python
import numpy as np


def ParseInputPoints():
    pointStrings = input().strip().split(';')
    points = []
    for pointStr in pointStrings:
        points.append(list(map(float, pointStr.split(','))))
    return np.array(points)


def ParseInput():
    centerNum = int(input().strip())
    centers = ParseInputPoints()
    pointNum = int(input().strip())
    points = ParseInputPoints()
    epoch = int(input().strip())
    return centers, points, epoch


def NearPoints(points, centerIdx, centerIdxForPoint):
    result = []
    for i, point in enumerate(points):
        if centerIdxForPoint[i] == centerIdx:
            result.append(point)
    return np.array(result)


def Output(centers):
    sep = ''
    str = ''
    for center in centers:
        [x, y] = center
        str += sep + f'{round(x, 2):.2f}' + ',' + f'{round(y, 2):.2f}'
        sep = ';'
    print(str)


def Distance(p1, p2):
    return np.sqrt((p1[0] - p2[0]) ** 2 + (p1[1] - p2[1]) ** 2)


if __name__ == '__main__':
    centers, points, epoch = ParseInput() # 初始中心，训练数据，迭代次数
    n = len(points)
    centerIdxForPoint = np.array([0] * n)
    for _ in range(epoch):
        for i, point in enumerate(points): # 遍历每个点
            # 计算第i个点距离每个中心的距离  
            distance = np.array([Distance(point, center) for center in centers])
            # 打标签：第i个点距离哪个中心点更近
            centerIdxForPoint[i] = np.argmin(distance)

        # 遍历中心点
        for centerIdx, center in enumerate(centers):
            # 找出距离这个中心点更近的点
            nearPoints = NearPoints(points, centerIdx, centerIdxForPoint)
            # 把这些点的坐标均值作为新的中心点
            centers[centerIdx] = np.mean(nearPoints, axis=0)
    Output(centers)
```

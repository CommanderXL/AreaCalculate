# AreaCalculate

##地图上计算乱序点组成多边形的面积.

具体代码见html文件

#####问题描述

计算机无法计算乱序点组成的多边形的面积，因此首先解决如何将乱序点转化为顺序点，再进行多边形面积计算。

#####总体思路

 * 选出所有点中经度最小和最大的点。
 * 取一过这两点的直线，得直线的上、下两区域
 * 将直线上半区域按经度从小到大排列并按顺时针连接
 * 将直线下半区域按经度从大到校排列并按顺时针连接
 * 最后得到顺序点组成的多边形
 * 选取坐标原点，将多边形顶点和原点相连，通过计算多个三角形面积得到多边形面积
 
#####注：
  * 由于测量面积不是特别大，近似将测量曲面当作平面计算
  * 单位换算：1纬度=111千米，1经度=111千米 × COS(纬度)
  * 整个计算过程按 “度” 为单位计算，然后将计算结果的单位 ”度” 转化为距离单位(千米) 

#####具体实现步骤如下:

首先通过电脑程序获取每个巡检人员手持终端最新的定位信息，包括经纬度。每个巡检人员的定位点都不相同，即这些定位点都处于乱序状态，而计算机是无法直接通过这些乱序点将巡检面积求出。因此计算机获取到每个巡检人员的定位信息后，首先应该按照一定的顺序将这些定位点进行排序。
  步骤一：将获取到的N（假设N=7）个定位点信息，按照经度的大小值将定位点进行第一次排序，经度最小的点记为A（A5点）（如果存在经度相同的点，就取纬度值最小的点）。选取经度最大的点记为B（A3点）（如果存在经度相同的店，就取纬度值最大的点）。


![image](https://github.com/CommanderXL/AreaCalculate/raw/master//img/1.jpg)

步骤二：通过A、B两点的坐标信息可确定一条经过A、B两点的直线，因此这条直线将除了A、B两点之外的剩余点分成了三部分，这三部分点的特性分别是：将点坐标代入直线方程后计算结果为大于0的区域记为C1，点的个数记为N1，计算结果小于0的区域记为C2，点的个数记为N2，计算结果等于0的区域记为C3，点的个数为N3.（图二）

![image](https://github.com/CommanderXL/AreaCalculate/raw/master//img/3.jpg)

 步骤三：将C1区域的点按照经度从小到大的顺序进行一次排列，如果存在经度相同的几个点，则按纬度从小到大的顺序排列，并将这N1个点按排列的先后顺序依次连接，然后将A点与C1区排序后的第一个点相连，B点与排序后的最后一点相连。
  步骤四：将C2区域的点按照经度从大到小的顺序进行一次排列。如果存在经度相同的几个点，则按经度的从大到小的顺序排列，并将这N2个点按排列的先后顺序依次连接，然后将B点与C2区域排序后的第一个点相连，将A点与排序后的最后一个点相连，这样就得到一个以A为起始点和终点的顺时针方向的多边形，将多边形的顶点个数记为K（K=7），(图三)。

![image](https://github.com/CommanderXL/AreaCalculate/raw/master//img/2.jpg)

步骤五：取坐标原点，多边形的每个顶点都和坐标原点组成一个向量（图四），每个三角形的面积计算依据

向量外积公式  

多边形的面积计算公式

规定在计算过程中，矢量相乘的面积取逆时针方向为正，顺时针方向为负，分别将三角形 的面积按照公式（1）计算，其中A_1 OA_2,A_2 OA_3,A_3 OA_4,A_4 OA_5,A_5 OA_6为顺时针方向取正值，而A_6 OA_7,A_7 OA_1为逆时针方向取负，并根据公式（2），求出最后的多边形面积，此时计算结果的单位为“度”，接下来将“度”的单位转化为“平方千米”，即可得最后多边形面积。
![image](https://github.com/CommanderXL/AreaCalculate/raw/master//img/4.jpg)
![image](https://github.com/CommanderXL/AreaCalculate/raw/master//img/5.jpg)

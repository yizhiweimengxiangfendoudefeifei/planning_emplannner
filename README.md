# planning_emplannner
This is an autonomous vehicle's planning algorithm. This algorithm is based on APOLLO emplanner, based on sampling.  
转自 忠厚老实的老王https://GitHub.com/VincentWong3   
# emplanner规划过程  
## 参考线模块  
从全局路径中找到投影点 -> 匹配点 -> 从全局路径中找到匹配点对应的参考线上的所有点，具体是指往后数30个点，往前数150个点 -> 对取到的点进行平滑化处理① -> 平滑后的路径得到对应的x，y坐标 -> 在参考线上投影得到匹配点、投影点的x，y坐标  
### ①：平滑处理包含三个代价函数：  
1. 相似性代价，保证生成的路径和原路径尽可能相似。  
2. 均匀性代价，采样点每段的距离尽可能均匀。  
3. 平滑性代价，路径要尽可能平滑，减少大曲率路径的出现。  
![image](https://user-images.githubusercontent.com/39455551/163668313-5632a915-29b8-4c27-a06b-50de37194ef7.png)

## emplanner规划  
 index2s模块② -> 计算规划起点③ -> 将计算的规划起点投影到参考线得到匹配点信息、投影点的x，y坐标和航向角、曲率等消息 -> 将开始规划的点的匹配点进行坐标转换，将笛卡尔坐标系转换为frenet坐标系下 -> 障碍物坐标投影到参考线，将笛卡尔坐标系转换为frenet坐标系 -> 空间离散化，撒9行6列的点寻找凸空间 -> 在该找到的凸空间中进行二次规划寻找最优解 ->速度规划
### ②index2s模块  
这里采用数组存储笛卡尔坐标系和frenet坐标系之间的转换关系，减小计算量。  
![image](https://user-images.githubusercontent.com/39455551/163668400-201345f5-4545-4d4b-ad2d-cbc7ab40eb46.png)
### ③计算规划起点  
这里要注意规划起点并不是定位的坐标点直接投影得到的，要利用当前位置加0.1s（规划周期）的位置作为规划起点。同时要判断推断的车辆位置和定位的车辆位置的偏差，如果偏差大于一定值则利用车辆动力学进行外推。  

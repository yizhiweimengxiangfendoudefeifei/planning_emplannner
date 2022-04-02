# planning_emplannner
This is an autonomous vehicle's planning algorithm. This algorithm is based on APOLLO emplanner, based on sampling.  
转自 忠厚老实的老王https://GitHub.com/VincentWong3   
# emplanner规划过程  
## 参考线模块  
从全局路径中找到投影点 -> 匹配点 -> 从全局路径中找到匹配点对应的参考线上的所有点，具体是指往后数30个点，往前数150个点 -> 对取到的点进行平滑化处理① -> 平滑后的路径得到对应的x，y坐标 -> 在参考线上投影得到匹配点、投影点的x，y坐标
## emplanner规划  
 index2s模块② -> 计算规划起点③ -> 将计算的规划起点投影到参考线得到匹配点信息、投影点的x，y坐标和航向角、曲率等消息 -> 将开始规划的点的匹配点进行坐标转换，将笛卡尔坐标系转换为frenet坐标系下 -> 障碍物坐标投影到参考线，将笛卡尔坐标系转换为frenet坐标系 -> 空间离散化，撒9行6列的点寻找凸空间 -> 在该找到的凸空间中进行二次规划寻找最优解 ->速度规划

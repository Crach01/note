# 数据分析



## 一、matplotlib



#### 折线图

> matplotlib 用于绘制数据可视化的图表，是最流行的python底层绘图库，主要做数据可视化图表，名字取材于MATLAB，模仿MATLAB构建

> matplotlib 中的 pyplot 模块用来画折线图

假设一天中的气温每隔两小时`range(2,26,2)`的气温为 ` [15, 13, 14, 5, 17, 20, 25, 26, 24, 22, 18, 15] `

```
# 导入matplotlib 包
from matplotlib import pyplot as plt
# 导报出错时，用py -m pip uninstall matplotlib 卸载，再py -m pip install matplotlib 安装即可

x = range(2, 26, 2)
				#数据在x轴的位置，是一个可迭代对象
y = [15, 13, 14, 5, 17, 20, 25, 26, 24, 22, 18, 15]
				#数据在y轴的位置，是一个可迭代对象
				#x 轴、y轴 的数据在一起一起组成了所有要绘制出的坐标
				#坐标分别是 （2，15）,（4,13）,(6,14)......

plt.plot(x, y)  #  传入 x 和 y ，通过plot绘制出折线图

plt.show()  # 在执行程序时展示出图形

```



![Alt text](C:\Users\18451\Desktop\笔记\python笔记\images\matplotlib_01.png)

存在的问题：

* 高清无码大图
* 保存到本地
* 描述信息（x,y 表示什么，这个图表示什么）
* 调整 x，y刻度间的间距
* 线条的样式（颜色、透明度）
* 标记处`特殊的点`(告诉别人最高点、最低点)
* 给图片`添加水印`（防伪防盗用）

##### 设置图片大小

```
fig = plt.figure(figsize = (20,8), dpi = 80)
							# figure图形图标的意思，在这里指的是我们画的图
							# 通过实例化一个figure并且传入参数，能够在后台自动调用该figure实例
							#figsize表示设置图片大小
							# 在图形模糊的时候可以传入dpi参数,让图片更加清晰
```



##### 保存图片

```from matplotlib import pyplot as plt
from matplotlib import pyplot as plt
fig = plt.figure(figsize = (8,6) ,dpi = 80)

x = range(2, 26, 2)

y = [15, 13, 14, 5, 17, 20, 25, 26, 24, 22, 18, 15]

plt.plot(x, y)
plt.savefig("./sig.png") #在绘制之后进行保存图片   可以保存为svg格式，不会失真
plt.show() 

```

##### 绘制x轴的刻度

`plt.xticks(x)`

`plt.yticks(y)`

```
from matplotlib import pyplot as plt

fig = plt.figure(figsize = (12,6) ,dpi = 80)

x = range(2, 26, 2)

y = [15, 13, 14, 5, 17, 20, 25, 26, 24, 22, 18, 15]

plt.plot(x, y)

_xticks_labels = [i/2 for i in range(4, 49)] #列表推到式

plt.xticks(_xticks_labels[::3]) #步长3

plt.yticks(range(min(y), max(y)+1))#设置y轴的刻度

# plt.savefig('./s.png')

plt.show()
```



##### 




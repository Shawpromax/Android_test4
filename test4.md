# Android-实验4（Jupyter Notebook实践）
# 1.打开Anaconda，launch Jupyter Notebook
![image1](https://github.com/Shawpromax/images/blob/main/test4_1.jpg)
# 2.新建一个新的Notebook
![image2](https://github.com/Shawpromax/images/blob/main/test4_2.jpg)
# 3.执行一些代码，具体代码及结果在下方
# 4.完成数据分析的例子
  ### 4.1 下载数据分析的例子
![image3](https://github.com/Shawpromax/images/blob/main/test4_3.jpg)
  ### 4.2 检查数据集
  ### 4.3 使用matplotlib进行绘图
# 5.Jupyter Notebook扩展工具的下载
  ### 5.1 执行指令

- pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ jupyter_contrib_nbextensions

- jupyter contrib nbextension install --user

- pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ jupyter_nbextensions_configurator

- jupyter nbextensions_configurator enable --user

![image4](https://github.com/Shawpromax/images/blob/main/test4_4.jpg)
  ### 5.2 等待最后一条指令完成即可。完成之后，重新打开Jupyter Notebook启动页面,点击Nbextensions标签，勾选Hinterland
![image5](https://github.com/Shawpromax/images/blob/main/test4_5.jpg)

# 6.步骤3、4的具体操作以及结果如下:




```python
print('Hello World!')
```

    Hello World!
    


```python
import time
time.sleep(3)
```


```python
import numpy as np
def square(x):
    return x * x
x = np.random.randint(1, 10)
y = square(x)
print('%d squared is %d' % (x, y))
```

    9 squared is 81
    


```python
def partition(arr,low,high): 
    i = ( low-1 )         # 最小元素索引
    pivot = arr[high]     
  
    for j in range(low , high): 
  
        # 当前元素小于或等于 pivot 
        if   arr[j] <= pivot: 
          
            i = i+1 
            arr[i],arr[j] = arr[j],arr[i] 
  
    arr[i+1],arr[high] = arr[high],arr[i+1] 
    return ( i+1 ) 

#快速选择排序
def quickSort(arr,low,high): 
    if low < high: 
  
        pi = partition(arr,low,high) 
  
        quickSort(arr, low, pi-1) 
        quickSort(arr, pi+1, high) 
  
arr = [10, 7, 8, 9, 1, 5] 
n = len(arr) 
quickSort(arr,0,n-1) 
print ("排序后的数组:") 
for i in range(n): 
    print ("%d" %arr[i])
```

    排序后的数组:
    1
    5
    7
    8
    9
    10
    


```python
%matplotlib inline
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
df = pd.read_csv('fortune500.csv')
df.head()
df.tail()
df.columns = ['year', 'rank', 'company', 'revenue', 'profit']
len(df)
df.dtypes
non_numberic_profits = df.profit.str.contains('[^0-9.-]')
df.loc[non_numberic_profits].head()
len(df.profit[non_numberic_profits])
bin_sizes, _, _ = plt.hist(df.year[non_numberic_profits], bins=range(1955, 2006))
df = df.loc[~non_numberic_profits]
df.profit = df.profit.apply(pd.to_numeric)
len(df)
df.dtypes
group_by_year = df.loc[:, ['year', 'revenue', 'profit']].groupby('year')
avgs = group_by_year.mean()
x = avgs.index
y1 = avgs.profit
def plot(x, y, ax, title, y_label):
    ax.set_title(title)
    ax.set_ylabel(y_label)
    ax.plot(x, y)
    ax.margins(x=0, y=0)
fig, ax = plt.subplots()
plot(x, y1, ax, 'Increase in mean Fortune 500 company profits from 1955 to 2005', 'Profit (millions)')
y2 = avgs.revenue
fig, ax = plt.subplots()
plot(x, y2, ax, 'Increase in mean Fortune 500 company revenues from 1955 to 2005', 'Revenue (millions)')
def plot_with_std(x, y, stds, ax, title, y_label):
    ax.fill_between(x, y - stds, y + stds, alpha=0.2)
    plot(x, y, ax, title, y_label)
fig, (ax1, ax2) = plt.subplots(ncols=2)
title = 'Increase in mean and std Fortune 500 company %s from 1955 to 2005'
stds1 = group_by_year.std().profit.values
stds2 = group_by_year.std().revenue.values
plot_with_std(x, y1.values, stds1, ax1, title % 'profits', 'Profit (millions)')
plot_with_std(x, y2.values, stds2, ax2, title % 'revenues', 'Revenue (millions)')
fig.set_size_inches(14, 4)
fig.tight_layout()

```


    
![png](output_5_0.png)
    



    
![png](output_5_1.png)
    



    
![png](output_5_2.png)
    



    
![png](output_5_3.png)
    


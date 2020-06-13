---
layout: post
title: Visdom可视化Pytorch训练过程
categories: Blog
description: Visdom可视化Pytorch训练过程
keywords: 可视化, Visdom, Pytorch
---

> 使用visdom可视化pytorch训练过程。

​	

## visdom

Visdom是支持torch和Numpy实时数据可视化工具。Support by [feakbooksearch](https://github.com/facebookresearch/visdom) 。

可视化界面如下：

![](https://gitee.com/misite_J/blog-img/raw/master/img/visdom.png)

​	

## Preparation

- 安装：`pip install visdom`
- 启动：`python -m visdom.server`
  - 浏览器进入http://localhost:8097



​	

## Practice

- utils.py

  ```python
  import numpy as np
  from visdom import Visdom
  
  
  class VisdomLinePlotter(object):
      """Plots to Visdom"""
      def __init__(self, env_name='main'):
          self.viz = Visdom()
          self.env = env_name
          self.plots = {}
  
      def plot(self, var_name, mode_name, title_name, x, y):
          if var_name not in self.plots:
              self.plots[var_name] = self.viz.line(X=np.array([x]), Y=np.array([y]), env=self.env, opts=dict(
                  legend=[mode_name],  # 每条曲线指定名字
                  title=title_name,
                  xlabel='Batchs',  # 横坐标名称
                  ylabel=var_name
              ))
          else:
              self.viz.line(X=np.array([x]), Y=np.array([y]), env=self.env, win=self.plots[var_name], name=mode_name, update='append')
  ```

  **Notes:**

  - Visdom默认环境为“main”，如果要同时或者先后训练不同的网络模型，可以为每个模型创建一个环境，分别显示它们的可视化结果，如`viz = Visdom(env = 'demo') `
  - `var_name`: 变量名称 (e.g. `loss`, `acc`)
  - `mode_name`: mode name (e.g. `train`, `val`)
  - `title_name`: 图例标题 (e.g. `Classification Accuracy`)
  - `update`: Use `'append'` to append data, `'replace'` to use new data, or `'remove'` to remove the trace specified by `name`. 

- train.py

  ```python
  import utils
   
  if __name__ == "__main__":
  	global plotter
  	plotter = utils.VisdomLinePlotter(env_name='Tutorial Plots')
   
  def train():
      ......
      plotter.plot('reward', 'train', 'Rewards', train_batch_num, reward.avg)
      
  def val():
      ......
      plotter.plot('reward', 'val', 'Rewards', val_batch_num, reward.avg)
      plotter.plot('acc', 'val', 'Accurarys', val_batch_num, acc.avg)
  ```

- `main`(main.py)和`train`(train.py)不在同一文件时全局变量(`global`)的设置

  - 问题定义：global定义的全局变量只能在同一个文件中使用，在工程中需要跨文件调用全局变量。

  1. 定义一个全局变量管理文件 globalvar.py

     ```
     def _init():
         global _global_dict
         _global_dict = {}
     
     def set_value(key,value):
         """ 定义一个全局变量 """
         _global_dict[key] = value
     
     def get_value(key,defValue=None):
     　　""" 获得一个全局变量,不存在则返回默认值 """
         try:
             return _global_dict[key]
         except KeyError:
             return defValue
     ```

     

  2. `import globalvar`

     ```python
     import utils
     import globalvar as gl
     
     def visual(self, env_name):
         gl._init()
     
         plotter = utils.VisdomLinePlotter(env_name=env_name)
         gl.set_value(env_name, plotter)
     
         return gl.get_value(env_name)
         
     def train():
     	......
     	plotter = visual(env_name='Tutorial Plots')
     	plotter.plot('reward', 'train', 'Rewards', train_batch_num, reward.avg)	
     ```

     



​	

## Remote  Server (未测试)

连接ssh时，将服务器的8097端口重定向到自己机器上来：

```
ssh -L 18097:127.0.0.1:8097 username@remote_server_ip
```

其中：18097:127.0.0.1代表自己机器上的18097号端口，8097是服务器上visdom使用的端口。

在服务器上使用8097端口正常启动visdom：

```
python -m visdom.server
```

在本地浏览器中输入地址：

```
127.0.0.1:18097
```

​	



## More details

- [github/visdom](https://github.com/facebookresearch/visdom)
- [github/visdom_tutorial](https://github.com/noagarcia/visdom-tutorial)
- [使用visdom可视化pytorch训练过程](http://cnblogs.com/walker-lin/p/12036328.html)


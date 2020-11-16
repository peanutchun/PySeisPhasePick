# **PySeisPhasePick** 配置及使用教程
> Peanut是我啊，构造物理课题组，SYSU

**一款简单的辅助拾取震相程序**

- 基于Jupyterlab平台下的Obspy与Pqolot模块
- 仅支持`.sac`地震数据格式文件
- 仅在科研室内测使用
- 迭代版本3.0



## 一、PySeisPhasePick 运行配置
以Windows平台为例
- 需要下载的软件
    - Anaconda/Miniconda 科学计算环境
    - NodeJs JS环境
- 使用到的工具
    - conda
    - pip
    - jupyter lab

### 环境配置
**Anaconda/Miniconda：**https://docs.conda.io/en/latest/miniconda.html

这次演示使用Miniconda

<img src="figure\miniconda下载页面.png"  style="zoom:50%;" />

### 环境配置
**NodeJs：**https://nodejs.org/en/download/

<img src="figure\nodejs下载页面.png"  style="zoom:50%;" />

### 工具配置
**conda虚拟环境**

- 打开方式，Windows搜索框，搜索conda，以管理员身份运行即可
- 可选操作：Miniconda换源
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

- 查看已经创建的虚拟环境
```
conda env list
```

- 创建虚拟环境, 这里的Python版本指定为Python3.7
```
conda create -n seisphase Python=3.7
```

- 激活虚拟环境
```
conda activate seisphase
```


### 工具配置
**使用conda安装必备软件包**
- 打开conda，激活虚拟环境，输入以下命令

    ```
    # 安装代码补全
    conda install pyreadline
    # 安装jupyter
    conda install jupyterlab
    ```
- 使用方法
    - 打开conda
    - 激活虚拟环境
    - 运行`jupyter lab`

### 工具配置
**pip 包管理工具**

- pip换源：
- 在`C:\Users\muzim\`目录下，新建`pip`文件夹，新建`pip.ini`文件，输入一下内容
    ```
    [global]
    timeout = 10
    index-url =  http://mirrors.aliyun.com/pypi/simple/
    extra-index-url= http://pypi.douban.com/simple/ 
    [install]
    use-mirrors = true  
    trusted-host=
        mirrors.aliyun.com
        pypi.douban.com
    ```


### 工具配置
**使用pip安装必备软件包**
- 使用方法
    - 打开conda
    - 激活虚拟环境
- 安装

    ```
    pip install obspy matplotlib numpy pandas scipy bqplot
    ```


### 工具配置
**安装JupyterLab插件**
- 运行`Jupyter lab`
- 在左侧插件栏找到Warning，点击Enable
- 安装插件
    - 必须：`[@jupyter-widgets/jupyterlab-manager](https://github.com/jupyter-widgets/ipywidgets)`
    - 必须：`[bqplot](https://github.com/bloomberg/bqplot.git)`
    - `[ipyevents](https://github.com/mwcraig/ipyevents)`
    - `[@aquirdturtle/collapsible_headings](https://github.com/aquirdTurtle/Collapsible_Headings)`

## PySeisPhasePick 使用教程
- 下载V3.0_PySeisPhasePick.ipynb文件：https://github.com/peanutchun/PySeisPhasePick/tree/master/ 保存在JupyterLab的默认启动位置
- 准备/复制地震数据
- 修改必要参数，默认地震数据放在同一地震事件的文件夹中
    - SeisFolders：包含地震数据的总文件夹
    - OutputFolder：输出地震数据的总文件夹，这里需要和SeisFolders位置一致/暂时无法修复
    - OriginKey：要读取震相初至的字段，默认是t1
    - TargetKey：要写入震相初至的字段，默认是a
    - TargetKeyStr, TargetKeyStrValue：要写入震相初至的类型的字段与值，默认是ka与P
- 修改台站名称排序方式
```
# 如12.17.23.35.21.85.1.Z，其中1是台站号，用split函数以.分割台站名，提取倒数地2个作为台站的排序方式
# 如果上述修改方式不起作用，请直接注释这行代码
values = sorted(items, key=lambda x: int(x.split(".")[-2]))
```

- 点击Kernel中Run all

#### 界面介绍
- 顶部窗口，可点击折叠
<img src="figure\选择窗口.gif"  style="zoom:50%;" />
- 选择地震事件，可任意切换
- 选择台站，可任意切换
- 选择模式)
    - Auto Loop：点击`Previous & Next`可自动在Event/Station列表中循环切换，但不会自动跳出
    - Auto Plot：功能同上述相同，但会在输出文件夹内生成包含当前算法及震相初至位置的对比标示图
    - Manual：不会进行任何操作

#### 界面介绍
- 中间窗口，显示原始数据，点击可折叠
<img src="figure\展示数据.gif"  style="zoom:50%;" />
- 展示原始地震数据，滑动条及数字框可调整X轴相对位置
- Previous & Next： 切换选择的事件/台站，并即时更新左侧事件波形
- Reset：重置中部数据窗口及底部算法窗口的X轴索引
- Ignore：未实现
- Mode：设置当前的切换模式是事件/台站
- Apply：读取当前所标定的震相初至位置，并写入到输出目录中去
  

#### 界面介绍
<img src="figure\调整算法.gif"  style="zoom:50%;" />
- 底部窗口，5种初至拾取算法：https://docs.obspy.org/master/tutorial/code_snippets/trigger_tutorial.html
- 可实时调整相关参数
- Trigger参数方法未事件，但不会影响算法结果

#### 功能
- 鼠标拖动滑动条，实时更新原始数据波形图 及 算法数据波形
- 鼠标左键在原始数据窗口中拖动，可细致查看对应X轴区间算法数据波形
- 鼠标在算法窗口中移动，可实时选择震相初至位置，点击锁定/取消锁定当前位置
- 实时日志查看，可调整日志级别：INFO WARNING ERROR

#### 使用流程
- 选定模式
- 根据算法结果标定震相初至
- 点击Apply

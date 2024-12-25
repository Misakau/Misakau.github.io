---
title: 'Install D4RL'
date: 2024-12-16
permalink: /posts/2024/12/install_d4rl/
tags:
  - env
  - offline RL
  - personal notes
---

每次安装D4RL都会踩一堆坑，然后上网搜一堆零散的攻略，所以我自己整理一下整个过程，以后直接来查就好了。

基本环境
------
conda创建虚拟环境后，从[Pytorch官网](https://pytorch.org/get-started/previous-versions/)下载需要的版本。
其他的包缺啥装啥就行。conda和pip记得换源，不然可能会很慢。

**conda 换源**
```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
```
**pip 换源**
```bash
conda activate <env_name>
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
安装MuJoCo210
---
一般安装的都是MuJoCo210版本，首先去[Github](https://github.com/google-deepmind/mujoco/releases?page=4)上下载对应的压缩包（例如[mujoco210-linux-x86_64.tar.gz](https://github.com/google-deepmind/mujoco/releases/download/2.1.0/mujoco210-linux-x86_64.tar.gz)），并且解压（或者移动）到`~/.mujoco/mujoco210`文件夹。

**解压命令**
```bash
tar -zxvf <filename.tar.gz>
```
**设置环境变量**

然后在`~.bashrc`中把它添加到`LD_LIBRARY_PATH`里，如果已有别的版本的mujoco记得屏蔽掉：
```bash
export LD_LIBRARY_PATH=/home/wangxun/.mujoco/mujoco210/bin:$LD_LIBRARY_PATH
```
最后安装2.1.x的`mujoco-py`即可，例如：`mujoco-py==2.1.2.14`

安装D4RL
---
首先把D4RL和mjrl都从Github上克隆下来

**克隆命令**
```bash
git clone git@github.com:Farama-Foundation/D4RL.git
git clone git@github.com:aravindr93/mjrl.git
```

然后进入虚拟环境，进入`mjrl`文件夹，用`pip`安装mjrl

**安装命令**
```bash
cd /your_path_to_mjrl
pip install -e .
```

然后进入D4RL文件夹，注释掉`setup.py`中的mjrl链接
```diff
@@ -68,6 +68,6 @@
         "termcolor",  # adept_envs dependency
         "click",  # adept_envs dependency
         "dm_control>=1.0.3",
-        "mjrl @ git+https://github.com/aravindr93/mjrl@master#egg=mjrl",
+        #"mjrl @ git+https://github.com/aravindr93/mjrl@master#egg=mjrl",
     ],
 )
```
然后用同样的方法安装D4RL
```bash
cd /your_path_to_D4RL
pip install -e .
```
完成后测试是否能够成功导入`d4rl`包。

### 可能会有一些报错，需要注意下面几个确认点：
**确认点1**

`/usr/lib/nvidia`是否添加到环境变量，如果没有，执行下面的命令：
```bash
export LD_LIBRARY_PATH=/usr/lib/nvidia:$LD_LIBRARY_PATH
```
**确认点2**

`Cython`版本是否兼容，如果不兼容，安装以下版本：
```bash
pip install Cython==3.0.0a10
```
**确认点3**

是否安装`X11`开发工具包，如果未安装，通过以下命令安装：
```bash
sudo apt-get update
sudo apt-get install libx11-dev
```
**确认点4**

是否安装`GL/glew`开发工具包，如果未安装，通过以下命令安装：
```bash
sudo apt-get update
sudo apt-get install libglew-dev
```
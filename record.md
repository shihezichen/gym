- 安装Python编辑器

  ```shell
  # pyCharm
  # VSCode
  ```

- Anaconda，安装python解释器环境，  ： 

  ```shell
  # 下载地址：
  https://www.anaconda.com/products/individual#Downloads
  ```

- conda切换国内源

  ```shell
  # 添加清华源
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge 
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
  # 搜索时显示通道地址
  conda config --set show_channel_urls yes
  # 以上命令会自动生成 %USERPROFILE%\.condarc 文件
  
  # 或者，自行手工新建 %USERPROFILE%\.condarc 文件
  notepad %USERPROFILE%\.condarc
  # 文件内容如下
  channels:
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
    - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    - defaults
  ssl_verify: true
  ```

- pip切换国内源

  ```shell
  # 新建 %HOMEPATH%\pip 目录
  mkdir %HOMEPATH%\pip
  cd /d  %HOMEPATH%\pip
  # 在其下新建一个pip.ini文件
  notepad pip.ini
  
  # 文件内容如下：
  [global]
  index-url = https://pypi.tuna.tsinghua.edu.cn/simple
  [install]
  trusted-host = https://pypi.tuna.tsinghua.edu.cn 
  ```

- 创建python rl 环境：

  ```shell
  # python: 3.6.8
  # anaconda: 4.9.2
  # gym: 0.9.1
  # mjpro: 131
  # mujoco_py: 0.5.7
  
  
  # 创建 python 虚拟环境
  conda create -n env_rl  python=3.6.8 anaconda
  # 激活环境
  conda activate env_rl
  # 升级pip
  pip install -U pip
  # 安装 ffmpeg
  conda install ffmpeg
  pip install ffmpy
  # 在环境中安装 gym
  pip install gym==0.9.1
  # 安装gym对atari游戏的支持
  pip install --no-index -f https://github.com/Kojoley/atari-py/releases atari_py
  ```

- 初识gym

  - Gym简介

    - OpenAI Gym是一个用来开发和比较强化学习算法的工具包， 是一个强化学习的Python库。
    - Gym的核心是使用Env对象作为统一的环境接口， 其中Env对象包含如下3个核心方法：
      - `reset(self)`: 重置环境状态， 返回环境的当前状态
      - `step(self, action)`: 推进给一个时间步， 返回的信息是 state，reward，done，info； 其中done用来表示游戏是否结束， info是关于游戏的附加信息
      - `render(self, mode='human', close=False)`: 重绘环境

  - Gym运行Breakout游戏

    - Breakout游戏:  弹球游戏，一个左右移动的挡板，反弹小球。

    - 代码：

      ```python
      import gym
      import time
      
      # 声明环境
      env = gym.make('Breakout-v0')
      # 环境初始化
      env.reset()
      
      # 对环境迭代1000次
      for _ in range(1000):
          env.render()
          # 随机选择一个动作
          env.step(env.action_space.sample())
          time.sleep(0.01)
      ```

  - Gym运行CartPole游戏

    - CartPole: 倒立杆，左右移动使得杆保持平衡

    - 代码

      ```python
      import gym
      env = gym.make("CartPole-v1") # 创建游戏环境
      observation = env.reset() # 游戏回到初始状态
      for _ in range(1000):
          env.render() # 显示当前时间戳的游戏画面
          action = env.action_space.sample() # 随机生成一个动作
          # 与环境交互，返回新的状态，奖励，是否结束标志，其他信息
          observation, reward, done, info = env.step(action)
          if done:  #游戏回合结束，复位状态
              observation = env.reset()
      env.close()
      ```

  - Gym运行Hero-ram游戏

    - 代码

      ```python
      import gym
      env = gym.make('Hero-ram-v0')
      for i_episode in range(10):
          observation = env.reset()
          for t in range(1000):
              env.render()
              action = env.action_space.sample()
              observation, reward, done, info = env.step(action)
              if done:
                  print("Episode finished after {} timesteps".format(t+1))
                  break
      env.close()
      ```

      

- 

#### 附录： 安装完整 gym(含Box2D， MuJoCo， Baselines)

- 安装swig

  ```shell
  # 下载地址， 选择最新版本
  http://www.swig.org/download.html
  # 解压缩zip包， 并把解压缩路径添加到 PATH 环境变量中
  ```

- 安装 atari_py

  ```shell
  # 下载地址, 选择 atari_py-1.2.2-cp36-cp36m-win_amd64.whl 下载（win 64, python3.6）
  https://github.com/Kojoley/atari-py/releases
  # 在下载文件所在目录， 离线安装whl文件
  pip install  atari_py-1.2.2-cp36-cp36m-win_amd64.whl
  ```

- 安装Box2D

  ```shell
  # 进入如下网址， 搜索 PyBox2D ， 选择 Box2D‑2.3.10‑cp36‑cp36m‑win_amd64.whl 下载
  https://www.lfd.uci.edu/~gohlke/pythonlibs/
  # 在下载文件所在目录， 离线安装whl文件
  pip install  Box2D‑2.3.10‑cp36‑cp36m‑win_amd64.whl
  ```

- 安装MuJoCo， mujoco-py

  ```shell
  # 到MuJoCo官网填写Trail信息， 申请试用 30 days
  # 备注： 点击页面 Win64 超链接，下载一个exe文件，运行得到Computer id，把id回填到网页， 然后提交申请， 在邮箱中会收到mjkey.txt文件和LICENSE.txt文件
  https://www.roboti.us/license.html
  # 新建 %USERPROFILE%\mujoco 文件夹
  mkdir %USERPROFILE%\.mujoco
  
  # 去MuJoCo官网,下载mujoco, 选择 mjpro131（备注：不要选择mujoco200，不支持windows）
  https://www.roboti.us/index.html
  https://www.roboti.us/download/mjpro131_win64.zip
  # 把zip文件解压到上面创建的  %USERPROFILE%\.mujoco 目录下， 形如 %USERPROFILE%\.mujoco\mjpro131
  
  # 把邮箱中的mjkey.txt和LICENSE.txt文件放入到 %USERPROFILE%\.mujoco\mjpro131\bin 下
  
  # 新建两个环境变量
  MUJOCO_PY_MJPRO_PATH = %USERPROFILE%\.mujoco
  MUJOCO_PY_MJKEY_PATH = %USERPROFILE%\.mujoco\mjpro131\bin
  # 把如下路径加入到 PATH 环境变量
   %USERPROFILE%\.mujoco
   %USERPROFILE%\.mujoco\mjpro131
   %USERPROFILE%\.mujoco\mjpro131\bin
  
  # 测试Mujoco：
  # 运行 %USERPROFILE%\.mujoco\mjpro131\bin\simulate.exe， 如果弹出界面， 把 %USERPROFILE%\.mujoco\mjpro131\model\humanoid.xml 拖进去， 有模型运行则安装成功:
  cd /d %USERPROFILE%\.mujoco\mjpro131\bin
  simulate.exe  ../model/humanoid.xml
  
  # 安装mjpro131对应的mujoco-py
  pip install mujoco_py==0.5.7
  ```

- 安装baselines

  ```shell
  #下载
  https://github.com/openai/baselines.git
  # 解压缩，安装
  cd baselines-master
  python setup.py install
  ```

  
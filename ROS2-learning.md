# 基础层
<details>
<summary></summary>

可这样理解：
- 工作空间：总工程目录
- 功能包：工程中的模块
- 节点：模块中真正运行的程序

## 终端操作指南

<details>
<summary></summary>

    按向上箭头可直接键入上一条命令。
    按tab键可对命令进行补全。
- 查看当前的所有话题  
```ros2 topic list```
- 持续查看某话题的信息  
```ros2 topic echo /节点名/话题名```
- 录制一段状态  
```ros2 bag record /节点名/话题名```
- 复现一段状态  
```ros2 bag play 录制保存文件名称```

</details>

## 创建工作空间

<details>
<summary></summary>

工作空间可以理解为ROS2工程的总目录。它不是一个单独的程序，而是一个用于统一管理多个功能包、统一构建、统一加载环境的开发目录。工作空间本质上就是一个文件夹。

- 创建工作空间  
``` mkdir -p 工作空间名/src```
- 进入工作空间  
```cd 工作空间名```
- 构建工作空间（要在工作空间目录下，未报错即为成功）  
```colcon build```
- 加载工作空间环境（要在工作空间目录下，仅对当前终端会话有效）  
```source install/setup.bash```

一个典型的ROS2工作空间至少会包含：

- src/：存放源代码包的文件夹（重要）
- build/：构建过程中产生的中间文件
- install/：安装后的可执行文件与环境脚本
- log/：构建日志

</details>

## 创建功能包

<details>
<summary></summary>

        功能包的创建需进入代码空间src。

功能包是ROS2中组织代码的基本单位。一个包里可以包含：

- 节点程序
- 配置文件
- 启动文件
- 接口定义
- 测试代码
- 依赖声明

### C++功能包

- 创建C++功能包  
```ros2 pkg create --build-type ament_cmake 功能包名 [选项]```  
    - C++功能包中必定存在两个文件：
        - package.xml

        包含功能包的版权描述，和各种被依赖的声明。

        - CMakerLists.txt 

        包含编译规则，设置使用CMake语法。


### Python功能包
- 创建Python功能包  
```ros2 pkg create --build-type ament_python 功能包名 [选项]```
    - Python功能包中必定存在两个文件：
        - package.xml

        包含功能包的版权描述，和各种被依赖的声明。

        - setup.py

        一些版权信息和“entry_points”配置的程序入口。

---

        编译需进入工作空间。

- 构建工作空间  
```colcon build```  
- 加载工作空间环境（否则无法查到自己所构建的包）  
```source install/local_setup.bash```
- 查看包是否被识别  
```ros2 pkg list | grep lq_pkg1```


### 与工作空间的关系

- 一个工作空间里可以有多个功能包。
- 一个功能包里可以有一个或多个节点。
- 节点运行前，通常要先完成包的构建，再source工作空间环境。

</details>

## 创建节点

<details>
<summary></summary>

ROS2中每一个节点也是只负责一个单独的模块化的功能（比如一个节点负责控制车轮转动，一个节点负责从激光雷达获取数据、一个节点负责处理激光雷达的数据、一个节点负责定位等等）。也就是说，节点是一种模块化的程序。

一个典型的 ROS 2 节点通常包含以下要素：

- 节点名称（Node Name）：唯一标识该节点（在同一命名空间下）。
- 发布者（Publisher）：向某个话题发送消息。
- 订阅者（Subscriber）：从某个话题接收消息。
- 服务端（Service Server）：响应其他节点的服务请求。
- 客户端（Service Client）：向其他节点发起服务请求。
- 参数（Parameters）：可动态配置的变量（如速度上限、采样频率等）。
- 定时器（Timer）：周期性执行某些操作（如每秒发布一次数据）。
提示

### 代码实现流程
- 编程接口初始化
- 创建节点并初始化
- 实现节点功能
- 销毁节点并关闭接口  

#### 面向过程：

```python

#!/usr/bin/env python3 
# -*- coding: utf-8 -*-

"""
@作者: 古月居(www.guyuehome.com)
@说明: ROS2节点示例-发布“Hello World”日志信息, 使用面向过程的实现方式
"""

import rclpy                                   # ROS2 Python接口库
from rclpy.node import Node                    # ROS2 节点类
import time

def main(args=None):                           # ROS2节点主入口main函数
    rclpy.init(args=args)                      # ROS2 Python接口初始化
    node = Node("node_helloworld")             # 创建ROS2节点对象并进行初始化

    while rclpy.ok():                          # ROS2系统是否正常运行
        node.get_logger().info("Hello World")  # ROS2日志输出
        time.sleep(0.5)                        # 休眠控制循环时间

    node.destroy_node()                        # 销毁节点对象    
    rclpy.shutdown()                           # 关闭ROS2 Python接口

```

完成代码的编写后需要设置功能包的编译选项，让系统知道Python程序的入口，打开功能包的setup.py文件，加入如下入口点的配置：

```python

entry_points={
        'console_scripts': [
         'node_helloworld       = learning_node.node_helloworld:main',
        ],

```

————————>虽然实现简单，但是对于稍微复杂一点的机器人系统，就很难做到模块化编码。

#### 面向对象：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
@作者: 古月居(www.guyuehome.com)
@说明: ROS2节点示例-发布“Hello World”日志信息, 使用面向对象的实现方式
"""

import rclpy                                     # ROS2 Python接口库
from rclpy.node import Node                      # ROS2 节点类
import time

"""
创建一个HelloWorld节点, 初始化时输出“hello world”日志
"""
class HelloWorldNode(Node):
    def __init__(self, name):
        super().__init__(name)                     # ROS2节点父类初始化
        while rclpy.ok():                          # ROS2系统是否正常运行
            self.get_logger().info("Hello World")  # ROS2日志输出
            time.sleep(0.5)                        # 休眠控制循环时间

def main(args=None):                               # ROS2节点主入口main函数
    rclpy.init(args=args)                          # ROS2 Python接口初始化
    node = HelloWorldNode("node_helloworld_class") # 创建ROS2节点对象并进行初始化
    rclpy.spin(node)                               # 循环等待ROS2退出
    node.destroy_node()                            # 销毁节点对象
    rclpy.shutdown()                               # 关闭ROS2 Python接口

```
完成代码的编写后需要设置功能包的编译选项，让系统知道Python程序的入口，打开功能包的setup.py文件，加入如下入口点的配置：

```python

entry_points={
        'console_scripts': [
         'node_helloworld       = learning_node.node_helloworld:main',
         'node_helloworld_class = learning_node.node_helloworld_class:main',
        ],

```
- 运行节点
```ros2 run 功能包名 节点名```
- 查看当前运行的所有节点  
```ros2 node list```
- 查看节点信息  
```ros2 node info /节点名```

</details>

## 节点的交互

<details>
<summary></summary>

ROS2有一共四种通信方式：
- 话题-topics
- 服务-services
- 动作-Action
- 参数-parameters

### 话题通信

<details>
<summary></summary>

    - 适合连续发送数据
    - 是异步通信
    - 一般用于传感器数据、状态流等持续信息交换
在 ROS 2 中，话题（Topic）是节点之间进行单向、异步消息通信的重要机制。

话题数据传输的特性是从一个节点到另外一个节点，发送数据的对象称之为发布者，接收数据的对象称之为订阅者，每一个话题都需要有一个名字，传输的数据也需要有固定的数据类型。

- 发布者负责向某个话题发送消息；
- 订阅者负责从某个话题接收消息；
- 只要话题名称一致且消息类型匹配，节点之间就可以完成通信。

#### 特点

<details>
<summary></summary>

- 异步通信：发送方和接收方无需同步阻塞等待；
- 一对多 / 多对多：一个发布者可对应多个订阅者，一个话题也可被多个发布者使用；
- 解耦合：节点之间不需要直接知道对方程序细节，只需约定好话题名和消息类型。

（异步表示发布者发布消息后，订阅者并不一定立即接收到消息。即不是同步通信。）

</details>

#### Python发布者/订阅者程序的基本构成

<details>
<summary></summary>

- 导入 rclpy；
- 导入 Node 基类；
- 导入消息类型，例如 std_msgs.msg.String；
- 定义节点类；
- 在节点中创建 publisher 或 subscription；
- 编写回调函数；
- 在 main() 中初始化、创建节点并执行 spin()。

</details>

#### 实践

<details>
<summary></summary>

##### 1. 创建实验功能包

进入工作空间 src 目录，创建 Python 功能包：  

```ros2 pkg create learn_topic --build-type ament_python --dependencies rclpy std_msgs```  

（这里依赖多了一个 std_msgs，std_msgs 是 ROS 2 常用标准消息包。接下来的部分要使用 String 消息类型。）

##### 2. 编写发布者节点

回调函数：用于处理订阅消息、服务请求、定时器触发等事件的核心机制。ROS2使用基于 rclcpp（C++）或 rclpy（Python）的异步执行模型，通过回调函数响应各种事件。主要有订阅者（Subscriber）回调，定时器（Timer）回调，服务（Service）回调。

```python

import rclpy                                     # ROS2 Python接口库
from rclpy.node import Node                      # ROS2 节点类
from std_msgs.msg import String                  # 字符串消息类型

"""
创建一个发布者节点
"""
class PublisherNode(Node):
    def __init__(self, node_name:str, topic_name: str):
        super().__init__(node_name)
        
        # 由于节点时继承自Node类，在Node类中已经实现了通过create_publisher方法来创建一个发布者 
        # msg_type表示传递的消息类型，topic表示话题通信的名称，对于发布者和订阅者要统一，qos_profile设置为10即可，不用深究
        self.publisher_ = self.create_publisher(msg_type=String,topic=topic_name,qos_profile=10) # 创建发布者对象

        # 创建一个定时器，表示每0.5秒执行一次timer_callback这个回调函数，也就是我们要实现的发布者要发布的消息都在回调函数中实现
        self.timer_ = self.create_timer(timer_period_sec=0.5, callback = self.timer_callback)

        self.i = 0

    def timer_callback(self):                                     # 创建定时器周期执行的回调函数
        msg = String()                                            # 创建一个String类型的消息对象
        msg.data = f'Hello ROS 2: [{self.i}]'                     # 填充消息对象中的消息数据
        self.publisher_.publish(msg)                               # 发布话题消息
        self.get_logger().info('已发布消息: "%s"' % msg.data)     # 输出日志信息，提示已经完成话题发布
        self.i += 1

def main(args = None):
    rclpy.init(args=args)
    pub_node = PublisherNode(node_name='learn_topic',topic_name='topic_demo')
    rclpy.spin(pub_node) # 启动节点
    pub_node.destroy_node()
    rclpy.shutdown()

```

##### 3. 编写订阅者节点

订阅者通过 create_subscription() 订阅话题，并在回调函数中处理接收到的消息。

```python

import rclpy                      # ROS2 Python接口库
from rclpy.node   import Node     # ROS2 节点类
from std_msgs.msg import String   # ROS2标准定义的String消息

"""
创建一个订阅者节点
"""
class SubscriberNode(Node):
    def __init__(self, node_name:str, topic_name: str):
        super().__init__(node_name)                             # ROS2节点父类初始化
        self.sub = self.create_subscription(
            String, topic_name, self.listener_callback, 10) # 创建订阅者对象（消息类型、话题名、订阅者回调函数、队列长度）

    def listener_callback(self, msg: String):                      # 创建回调函数，执行收到话题消息后对数据的处理  
        self.get_logger().info(f'订阅到的数据：{msg.data}') # 输出日志信息，提示订阅收到的话题消息

def main(args=None):
    rclpy.init(args=args)
    sub_node = SubscriberNode(node_name='sub',topic_name='learn_topic')
    rclpy.spin(sub_node) # 启动节点
    sub_node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()

```

##### 4.修改 setup.py

```python

entry_points={
    'console_scripts': [
        'publisher = learn_topic.node_pub:main',
        'subscriber = learn_topic.node_sub:main',
    ],
},

```

##### 5. 编译功能包

返回工作空间根目录，编译并加载环境：  
```colcon build```  
```source install/setup.bash```

##### 6.运行发布者与订阅者节点

- 运行发布者节点
    - 打开第一个终端：  
```cd lq_ws/```  
```source install/setup.bash```  
```ros2 run learn_topic publisher```
- 运行订阅者节点
    - 第一个终端不关闭，打开第二个终端：  
```cd lq_ws/```  
```source install/setup.bash```  
```ros2 run learn_topic subscriber```

</details>

#### 验证

<details>
<summary></summary>

- 查看当前话题列表  
```ros2 topic list```

- 查看话题消息内容  
```ros2 topic echo /learn_topic # 注意替换为自己的消息名称```  

将持续显示发布者发送的消息内容。

- 查看话题信息  
```ros2 topic info /learn_topic```

可查看该话题的消息类型以及发布者/订阅者数量。

</details>

</details>

### 服务通信

<details>
<summary></summary>

    - 适合一次请求、一次返回
    - 更像函数调用
    - 一般用于命令触发、状态查询、参数计算等任务

在 ROS 2 中，服务（Service）是一种同步式请求—响应通信机制。它适合处理"调用一次，返回一次结果"的任务。

例如：请求当前两个数的和、请求机器人执行一次固定操作、请求返回当前某项状态信息。

服务端（Service Server）：负责接收请求并返回响应；
客户端（Client）：负责发送请求并等待结果。

与话题通信不同，服务通信不是持续广播，而是：

- 客户端发起请求；
- 服务端接收请求并处理；
- 服务端返回结果；
- 客户端获得响应。

示例：

- 客户端发送两个整数 a=2, b=3；
- 服务端执行加法运算；
- 服务端返回结果 sum=5；
- 客户端打印结果。

####  服务通信的基本构成

<details>
<summary></summary>

一个 ROS 2 Python 服务系统通常包括以下内容：

- 导入 rclpy 与 Node；
- 导入服务接口类型，例如 example_interfaces.srv.AddTwoInts；
- 在服务端节点中调用 create_service()；
- 在客户端节点中调用 create_client()；
- 服务端编写请求处理回调函数；
- 客户端构造请求并发送；
- 运行节点并验证服务响应。

</details>

#### 成功条件

<details>
<summary></summary>

- 客户端和服务端的服务名一致；
- 客户端和服务端的服务类型一致；
- 服务端已经处于运行状态；
- 客户端能够成功连接服务。

</details>

#### 实践

<details>
<summary></summary>

##### 1.创建实验功能包

```ros2 pkg create --build-type ament_python learn_service --dependencies rclpy example_interfaces```

这里使用 example_interfaces 中的 AddTwoInts 服务接口。

该服务接口的客户端的数据是两个 int64 的数，分别为 a 和 b，服务端的数据是 int64 的 sum，结构如下：

    int64 a
    int64 b
    ---
    int64 sum

##### 2.编写客户端节点

在功能包下新建文件 service_client.py：

```python

import rclpy                                  # ROS2 Python接口库
from rclpy.node   import Node                 # ROS2 节点类
from example_interfaces.srv import AddTwoInts                # 自定义的服务接口

class AddClient(Node):
    def __init__(self, node_name, service_name):
        super().__init__(node_name)
        self.client_ = self.create_client(AddTwoInts,service_name) # 创建服务客户端对象（服务接口类型，服务名）
        while not self.client_.wait_for_service(timeout_sec=1.0):# 循环等待服务器端成功启动
            self.get_logger().warn('service not available, waiting again...')
        
        self.request_ = AddTwoInts.Request() # 创建服务请求的数据对象

    def send_request(self, a, b):   # 创建一个发送服务请求的函数
        self.request_.a = a
        self.request_.b = b
        self.future = self.client_.call_async(self.request_) # 发送请求数据
        return self.future

def main(args = None):
    rclpy.init(args=args)
    client_node = AddClient('service_adder_client',service_name='add_two_ints') # 创建ROS2节点对象并进行初始化
    
    a = 10
    b = 20
    future = client_node.send_request(a, b)  # 发送服务请求,比如说求解10+20

    rclpy.spin_until_future_complete(client_node,future) # 阻塞当前线程，直到某个 Future 对象完成（即收到服务响应、动作结果等）。
    
    if future.result() is not None: 
        response = future.result() # future.result() 获取服务端返回的结果
        client_node.get_logger().info(f'第一个加数为:{a},第二个加数为{b},加和的结果为: {response.sum}')  # response.sum是返回的数据的变量名是sum
    else:
        client_node.get_logger().error('服务请求失败')
    
    client_node.destroy_node()
    rclpy.shutdown()

```

##### 3.编写服务端节点

在功能包下新建文件 service_server.py：

```python

import rclpy                                     # ROS2 Python接口库
from rclpy.node   import Node                    # ROS2 节点类
from example_interfaces.srv import AddTwoInts    # 自定义的服务接口

class adderServer(Node):
    def __init__(self, node_name, service_name):
        super().__init__(node_name)     
        self.server_ = self.create_service(AddTwoInts,service_name, self.adder_callback) # 创建服务器对象（接口类型、服务名、服务器回调函数）

    def adder_callback(self, request:AddTwoInts.Request, response:AddTwoInts.Response): # 创建回调函数，执行收到请求后对数据的处理
        response.sum = request.a + request.b # 完成加法求和计算，将结果放到反馈的数据中
        self.get_logger().info(f'收到请求，计算{request.a} + {request.b}')
        return response # 反馈应答信息
    
def main(args=None):                             # ROS2节点主入口main函数
    rclpy.init(args=args)                        # ROS2 Python接口初始化
    server_node = adderServer("service_adder_server",service_name='add_two_ints')   # 创建ROS2节点对象并进行初始化
    rclpy.spin(server_node)                             # 循环等待ROS2退出
    server_node.destroy_node()                          # 销毁节点对象
    rclpy.shutdown()                             # 关闭ROS2 Python接口

```

##### 4.修改 setup.py

```python

entry_points={
    'console_scripts': [
        'service_adder_client  = learn_service.service_client:main',
        'service_adder_server  = learn_service.service_server:main',
    ],
},

```

##### 5.编译功能包

返回工作空间根目录：

```colcon build```  
```source install/setup.bash```

##### 6.运行服务端与客户端节点

- 运行服务端节点
    - 打开第一个终端，到工作空间根目录：  
```source install/setup.bash```  
```ros2 run learn_service service_adder_server```

服务端启动后会一直运行，等待客户端请求。

- 运行客户端节点
    - 打开第二个终端，到工作空间根目录：  
```source install/setup.bash```  
```ros2 run learn_service service_adder_client```

</details>

#### 验证

<details>
<summary></summary>

- 查看当前服务列表  
```ros2 service list```

- 查看服务类型  
```ros2 service type /add_two_ints```

- 直接通过命令行调用服务（本质是通过命令行模拟了一个客户端）:  
```
# 格式 ros2 service call 服务名 数据接口 "{字典形式表示的请求数据}"
ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{a: 4, b: 5}"
```

</details>

</details>

### 自定义接口

在ROS2中，接口（Interface）是节点之间通信的"数据契约"。它定义了数据的结构和类型，确保不同节点之间能够正确理解和处理数据。

ROS2提供了三种主要接口类型：

- 消息（msg）：
    - 用于话题通信
    - 单向、持续的数据流
- 服务（srv）
    - 用于服务通信
    - 请求-响应模式
- 动作（action）
    - 用于动作通信
    - 可反馈的长时间任务

#### 用途

ROS 2 内置了许多标准接口（如 std_msgs、sensor_msgs 等），但在实际开发中，我们常常需要定义自己的数据结构：

- 描述特定的业务数据（如学生信息、机器人状态等）
- 组合多种数据类型形成复合结构
- 定义特定服务请求和响应格式
- 实现跨语言、跨平台的数据交互

---

    典型应用场景：

    自定义机器人传感器数据格式
    定义特定的控制指令结构
    封装复杂的状态信息
    创建项目专用的服务接口

#### 语言无关性

ROS 2 接口的一个重要特性是语言无关性：

- 接口定义使用简单的文本格式（.msg、.srv 文件）
- 编译时自动生成各语言的代码（Python、C++ 等）
- 不同语言编写的节点可以无缝通信

#### 接口包与功能包的关系

在 ROS 2 中，自定义接口通常放在独立的接口包中：

- 接口包：专门用于定义接口（msg、srv、action），不包含节点代码

- 功能包：包含实际的节点实现，依赖接口包

---

    接口包 → 编译生成 → Python/C++代码 → 功能包调用

这种分离设计的好处：

- 复用性：一个接口包可以被多个功能包使用
- 解耦：接口定义与实现分离，便于维护
- 协作：团队成员可以各自开发功能包，共享接口定义

无论使用 Python 还是 C++，只要引用相同的接口包，就能正确解析数据结构。

#### 自定义消息创建与使用



</details>

</details>
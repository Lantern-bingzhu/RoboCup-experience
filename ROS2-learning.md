# 终端操作指南
<details>
<summary></summary>

    按向上箭头可直接键入上一条命令。
    按tab键可对命令进行补全。
- 查看当前运行的所有节点  
```ros2 node list```
- 查看节点信息  
```ros2 node info /节点名```
- 查看当前的所有话题  
```ros2 topic list```
- 持续查看某话题的信息  
```ros2 topic echo /节点名/话题名```
- 录制一段状态 
```ros2 bag record /节点名/话题名```
- 复现一段状态  
```ros2 bag play 录制保存文件名称```
- 创建工作空间  
``` mkdir -p 文件夹名/src```

</details>

# 创建功能包
<details>
<summary></summary>

        功能包的创建需进入代码空间src。

## C++功能包

- 创建C++功能包  
```ros2 pkg create --build-type ament_cmake learning_pkg_c```  
    - C++功能包中必定存在两个文件：
        - package.xml

        包含功能包的版权描述，和各种被依赖的声明。

        - CMakerLists.txt 

        包含编译规则，设置使用CMake语法。


## Python功能包
- 创建Python功能包  
```ros2 pkg create --build-type ament_python learning_pkg_python```
    - Python功能包中必定存在两个文件：
        - package.xml

        包含功能包的版权描述，和各种被依赖的声明。

        - setup.py

        一些版权信息和“entry_points”配置的程序入口。

---

        编译需进入工作空间。

- 编译工作空间所有功能包  
```colcon build```  
```source install/local_setup.bash```

</details>

# 创建节点

<details>
<summary></summary>

## 代码实现流程
- 编程接口初始化
- 创建节点并初始化
- 实现节点功能
- 销毁节点并关闭接口  

### 面向过程：

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

### 面向对象：

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

</details>
## 第零步：OpenGL的功能

当电脑需要显示一个图形时，需要通过CPU将发出指令给GPU，而GPU负责将转化后的图形数据输出到屏幕上。CPU想将场景数据传输到GPU，就需要调用图形API。

OpenGL就是一个针对图形开发的API，提供了多种的接口，应用程序通过它来访问和控制其所在运行设备的图形子系统，OpenGL再通过底层的图形硬件来执行预期的指令。它的主要功能就是绘制图像从简单的线条到复杂的三维景象。OpenGL是视频行业领域中用于处理2D/3D图形的最为广泛接纳的API，在此基础上，为了用于计算机视觉技术的研究，从而催生了各种计算机平台上的应用功能以及设备上的许多应用程序。其是独立于视窗操作系统以及操作系统平台，可以进行多种不同邻域的开发和内容创作，简而言之，其帮助研发人员能够实现PC、工作站、超级计算机以及各种工控机等硬件设备上实现高性能、对于视觉要求极高的高视觉图形处理软件的开发。

## 第一步：导入库

### 库的种类

- gl是核心库，是常用的OpenGL库，其中包含的函数名的前缀以gl开头，定义了OpenGL使用的函数和常量声明，包括了最基本的3D函数。

- glu是实用库，是OpenGL实用库使用的函数和常量声明，包含有43个函数，函数名的前缀为glu，是对核心库的封装，可以使用较简单的函数实现一些复杂的功能，如纹理、坐标和基本形状等。它属于OpenGL标准的一部分。

- glut是OpenGL实用工具库，主要用于实现跨平台的窗口界面，可以在窗口上实现字体和图像。这个头文件自动包含GL库和GLU库。

- glfw和glut类似，是轻量级的跨平台工具库，提供了渲染物体所需的最低限度的接口。主要用于管理窗口、读取输入和处理事件。

- glew是跨平台的C++扩展库，它能自动识别平台所支持的最高级的OpenGL特性，也就是说，只要包含一个glew.h头文件，就能使用gl,glu,glext,wgl,glx的全部函数，并且Glew适配几乎所有流行的操作系统和显卡。

- glad与glew作用相同，可以看作它的升级版。通常来说glad和glfw配合使用。

### 库的安装

1. 下载库：[glfw的下载地址](https://www.glfw.org/download.html)以及[glad的下载地址](https://glad.dav1d.de/) 

   *在这里使用 OpenGL 3.3 以及 Visual Studio 2022 进行开发*

2. 下载后将glad文件夹下的include文件夹复制到工程目录下，并且也将glfw下include文件夹下的两个文件夹复制到刚刚的include文件夹下；

3. 工程目录下新建libs文件夹，将glfw文件夹下的lib-vc2022下的文件复制到新建的lib文件夹下

4. 将glad下的src文件夹里的glad.c也复制到工程目录下

5. 在项目源文件中添加现有项，将glad.c文件添加进去

6. 在项目设置 c/c++ 下的 常规 中添加附加包含目录 `include` (相当于就是将刚刚创建的include文件夹加入项目)

7. 在项目设置 连接器 中添加附加库目录 `libs`

8. 在项目设置 连接器 下的 输入 中的 附加依赖项 里添加 `glfw3.lib`

### 库的使用

以上述方法安装好库之后，在项目的cpp文件中加入`#include"glad/glad.h"`以及`#include"GLFW/glfw3.h"`即可正常使用库

## 第二步：初步使用

### 初始化 glfw

首先我们需要调用 `glfwInit()`来初始化 glfw，再调用`glfwWindowHint()`来配置 glfw

#### glfwInit()

注意：在调用绝大部分 glfw 函数前，都需要保证 glfw 被初始化过，也就是说在程序开始都需要保证调用过 `glfwInit()`

#### glfwWindowHint(int, int)

该函数的第一个参数是一个 glfw 定义的枚举类型的变量，表示需要调整的选项的名称；第二个参数是一个 int 类型的值，表示调整的值。所有的枚举类型和对应的值以及含义可以在[这篇文档](https://www.glfw.org/docs/latest/window.html#window_hints)中找到。

#### 例子

```c++
glfwInit(); // 初始化
glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3); // 设置大版本号为3
glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3); // 设置小版本号为3（也就是将要使用的OpenGL版本号为3.3）
glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE); // 设置使用核心模式
```

### 创建一个 OpenGL 的窗口

窗口是指屏幕上的某一个窗口，可放大放小和移动关闭。窗口在GLFW中被定义为`GLFWwindow`类，通过调用 `glfwCreateWindow()` 可以进行创建，再通过调用 `glfwMakeContextCurrent()` 将其设置为当前线程的主上下文。再通过调用 ` glViewport()` 设置 Viewport 将窗口显示出来。

#### glfwCreateWindow(int, int, string, GLFWmonitor* , GLFWwindow* )

该函数的前两个参数是窗口的宽和高。第三个参数是这个窗口的标题。后面两个参数暂时先都认为是`NULL`。它会返回一个指向`GLFWwindow`对象的指针，如果不能成功创建这个对象返回值就是`NULL`。

#### glfwMakeContextCurrent(GLFWwindow* )

该函数的参数指向一个窗口，功能是将这个窗口的上下文设置为当前线程的主上下文

#### 例子

```c++
GLFWwindow* window = glfwCreateWindow(1080, 720, "hello world", NULL, NULL); // 创建一个1080*720的名为hello world的窗口
glfwMakeContextCurrent(window); // 设置为当前线程的上下文
```



### 持续显示

刚刚的代码运行出来后只是一闪而过，我们如果想让他持续显示，就需要用到一个循环来不断显示。也叫循环渲染，能在我们让GLFW退出前一直保持窗口的运行。

首先使用`glfwWindowShouldClose()`检查是否被要求退出，否则使用`glfwSwapBuffers()`来继续绘制并输出。

#### glfwWindowShouldClose(GLFWwindow* )

该函数的参数指向一个窗口，功能是将检查这个窗口是否被关闭，若被关闭，则返回`1` 

#### glfwPollEvents()

该函数的作用是交换颜色缓冲（它是一个储存着GLFW窗口每一个像素颜色值的大缓冲），并会绘制下一帧。

#### 例子

```c++
while (!glfwWindowShouldClose(window)) { // 检查是否关闭
	glfwPollEvents(); // 否则继续绘制下一帧并显示
}
```

### 结束释放

结束的时候需要释放之前的资源（类似c++的delete），通过`glfwTerminate()`完成
#### 例子

```c++
glfwTerminate(); // 释放资源
```

### 最终效果

运行一下，就能看到一个稳定的白色窗口啦~

#### 完整代码

```c++
#include <iostream>
#include"glad/glad.h"
#include"GLFW/glfw3.h"
using namespace std;
int main() {
	glfwInit(); // 初始化
	glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3); // 设置大版本号为3
	glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3); // 设置小版本号为3（也就是将要使用的OpenGL版本号为3.3）
	glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE); // 设置使用核心模式
	GLFWwindow* window = glfwCreateWindow(1080, 720, "hello world", NULL, NULL); // 创建一个1080*720的名为hello world的窗口
	glfwMakeContextCurrent(window); // 设置为当前线程的上下文
	while (!glfwWindowShouldClose(window)) { // 检查是否关闭
		glfwPollEvents(); // 否则继续绘制下一帧并显示
	}
	glfwTerminate(); // 释放资源
	return 0;
```

# 


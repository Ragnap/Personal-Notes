## 第零步：OpenGL的功能

当电脑需要显示一个图形时，需要通过CPU将发出指令给GPU，而GPU负责将转化后的图形数据输出到屏幕上。CPU想将场景数据传输到GPU，就需要调用图形API。

OpenGL就是一个针对图形开发的API，提供了多种的接口，应用程序通过它来访问和控制其所在运行设备的图形子系统，OpenGL再通过底层的图形硬件来执行预期的指令。它的主要功能就是绘制图像从简单的线条到复杂的三维景象。OpenGL是视频行业领域中用于处理2D/3D图形的最为广泛接纳的API，在此基础上，为了用于计算机视觉技术的研究，从而催生了各种计算机平台上的应用功能以及设备上的许多应用程序。其是独立于视窗操作系统以及操作系统平台，可以进行多种不同邻域的开发和内容创作，简而言之，其帮助研发人员能够实现PC、工作站、超级计算机以及各种工控机等硬件设备上实现高性能、对于视觉要求极高的高视觉图形处理软件的开发。

## 第一步：导入库

### 库的种类

一个完整的、便于使用的OpenGL包含两个部分：**3D函数库**（gl/glu < glew < glad）和**窗口管理库**(glut < glfw)

- gl是核心库，是常用的OpenGL库，其中包含的函数名的前缀以gl开头，定义了OpenGL使用的函数和常量声明，包括了最基本的3D函数。
- glu是实用库，是OpenGL实用库使用的函数和常量声明，包含有43个函数，函数名的前缀为glu，是对核心库的封装，可以使用较简单的函数实现一些复杂的功能，如纹理、坐标和基本形状等。它属于OpenGL标准的一部分。
- glew是跨平台的C++扩展库，它能自动识别平台所支持的最高级的OpenGL特性，也就是说，只要包含一个glew.h头文件，就能使用gl,glu,glext,wgl,glx的全部函数，并且Glew适配几乎所有流行的操作系统和显卡。
- glad与glew作用相同，可以看作它的升级版。
- glut是OpenGL实用工具库，主要用于实现跨平台的窗口界面，是**基本的窗口界面**，是**独立于gl和glu的**，可以在窗口上实现字体和图像。glut的头文件自动包含gl库和glu库。
- glfw和glut类似，是轻量级的跨平台工具库，是专门针对OpenGL的C语言库，提供了渲染物体所需的**最低限度的接口**。主要用于管理窗口、读取输入和处理事件。

### 库的安装

*在这里使用 OpenGL 3.3 以及 Visual Studio 2022 进行开发*

1. 下载库：[glfw的下载地址](https://www.glfw.org/download.html)中选择 pre-compiled binaries中对应版本的下载链接，以及[glad的下载地址](https://glad.dav1d.de/)中gl版本选择 `Version 3.3` (3.3是glfw支持的最低版本)，profile选择 `core` ,勾选下方的`Generate a loader`,其他选项保持默认

2. 下载解压后将glad文件夹下的include文件夹复制到工程目录下，并且也将glfw下include文件夹下的两个文件夹复制到刚刚的include文件夹下；

3. 工程目录下新建libs文件夹，将glfw文件夹下的lib-vc2022下的文件复制到新建的lib文件夹下

4. 将glad下的src文件夹里的glad.c也复制到工程目录下

5. 在项目源文件中添加现有项，将glad.c文件添加进去

6. 在项目设置 c/c++ 下的 常规 中添加附加包含目录 `include` (相当于就是将刚刚创建的include文件夹加入项目)

7. 在项目设置 连接器 中添加附加库目录 `libs`

8. 在项目设置 连接器 下的 输入 中的 附加依赖项 里添加 `glfw3.lib`

### 库的使用

以上述方法安装好库之后，在项目的cpp文件中加入`#include"GLFW/glfw3.h"`以及`#include"glad/glad.h"`即可正常使用库

## 第二步：初步使用

### 初始化 glfw

首先我们需要调用 `glfwInit()`来初始化 glfw，再调用`glfwWindowHint()`来配置 glfw

#### glfwInit()

注意：在调用绝大部分 glfw 函数前，都需要保证 glfw 被初始化过，也就是说在程序开始都需要保证调用过 `glfwInit()`，如果发生错误将返回`false`

#### glfwWindowHint(int, int)

该函数的第一个参数是一个 glfw 定义的枚举类型的变量，表示需要调整的选项的名称；第二个参数是一个 int 类型的值，表示调整的值。所有的枚举类型和对应的值以及含义可以在[这篇文档](https://www.glfw.org/docs/latest/window.html#window_hints)中找到。

#### 例子

```c++
if(!glfwInit()) // 初始化glfw
	return -1;
glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3); // 设置大版本号为3
glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3); // 设置小版本号为3（也就是将要使用的OpenGL版本号为3.3）
glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE); // 设置使用核心模式
```

### 创建窗口

窗口是指屏幕上的某一个窗口，可放大放小和移动关闭。窗口在GLFW中被定义为`GLFWwindow`类，通过调用 `glfwCreateWindow()` 可以进行创建，再通过调用 `glfwMakeContextCurrent()` 将其设置为当前线程的主上下文。再通过调用 ` glViewport()` 设置 Viewport 将窗口显示出来。

#### glfwCreateWindow(int, int, string, GLFWmonitor\* , GLFWwindow\* )

该函数的前两个参数是窗口的宽和高。第三个参数是这个窗口的标题。后面两个参数暂时先都认为是`NULL`。它会返回一个指向`GLFWwindow`对象的指针，如果不能成功创建这个对象返回值就是`NULL`。

#### glfwMakeContextCurrent(GLFWwindow\* )

该函数的参数指向一个窗口，功能是将这个窗口的上下文设置为当前线程的主上下文

#### 例子

```c++
GLFWwindow* window = glfwCreateWindow(1080, 720, "hello world", NULL, NULL); // 创建一个1080*720的名为hello world的窗口
glfwMakeContextCurrent(window); // 设置为当前线程的上下文
```

### 初始化 glad

因为 glfw 的主要功能是创建并管理窗口和 OpenGL 上下文，所以还需要使用 glad 来使用其他的 OpenGL 函数。只需要使用`gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)`即可

#### gladLoadGLLoader(GLADloadproc)

给GLAD传入了用来加载系统相关的OpenGL函数指针地址的函数。在这里我们直接使用`gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)`即可，`glfwGetProcAddress`是GLFW根据当前编译的系统来定义，这样就能载入所有的OpenGL函数。在不能成功初始化的情况下会返回`false`

#### 例子

```c++
if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)) {
    return -1;
}
```

### 设置视口

在我们开始渲染之前还有一件重要的事情要做，我们必须指定视口来让OpenGL根据窗口大小显示数据和确定坐标。也就是通过`glViewport()`来设置

#### glViewport(int, int, int, int)

该函数前两个参数控制窗口左下角的位置（也就是绘制时坐标的原点位置）。第三个和第四个参数控制渲染窗口的宽度和高度。都是以像素为单位。

#### 例子

 ```c++
 glViewport(0, 0, 800, 600);
 ```

### 持续显示

刚刚的代码运行出来后只是一闪而过，我们如果想让他持续显示，就需要用到一个循环来不断显示。也叫**循环渲染**，能在我们让 glfw 退出前一直保持窗口的运行。

首先使用`glfwWindowShouldClose()`检查是否被要求退出，否则执行渲染，在渲染结束后使用`glfwSwapBuffers()`交换颜色缓存输出刚刚渲染的结果，再通过`glfwPollEvents()`来检测是否触发了事件并更新窗口状态。

#### glfwWindowShouldClose(GLFWwindow\* )

该函数的参数指向一个窗口，功能是将检查这个窗口是否被关闭，若被关闭，则返回`1` 

#### glfwSwapBuffers()

该函数的作用是交换颜色缓冲（它是一个储存着 glfw 窗口每一个像素颜色值的大缓冲），并会绘制下一帧到屏幕上。

#### glfwPollEvents()

该函数的作用是检测是否触发了事件（键盘输入、鼠标移动等等）并更新窗口状态，同时调用事件对应的回调函数。

#### 例子

```c++
while (!glfwWindowShouldClose(window)) { // 检查是否关闭
    
    glfwSwapBuffers(window); // 更新缓冲
	glfwPollEvents(); // 继续绘制下一帧并显示
}
```

### 清空屏幕

在上一步的循环渲染中，如果不进行特殊设置，是会在上一次渲染的结果之上继续的，所以我们需要清空一次屏幕（就像Windows控制台应用里常常使用到的`system("cls")`语句一样）。

首先使用`glClearColor()`来设置屏幕背景颜色，再通过`glClear()`来讲屏幕清空并重新设置为指定的颜色值。

#### glClearColor(float,float,float,float)

该函数是状态设置函数，是用来设置刷新颜色缓冲区时所用的颜色缓冲位，也就是下面会讲到的`GL_COLOR_BUFFER_BIT`，参数分别对应RGBA，但是需要注意的是这里参数的范围是 (0,1) ,也就是说要将RGBA值除255得到的小数作为参数，即可设置为需要的颜色。

#### glClear(GLbitfield mask)

该函数的作用是根据参数对应的缓冲标志位来清除指定的缓冲区，但是注意**glClear会忽略alpha值**。参数的类型有`GL_COLOR_BUFFER_BIT`，`GL_DEPTH_BUFFER_BIT`和`GL_STENCIL_BUFFER_BIT` ，同时设置时使用`|`连接多个标志位。但我们在这里只先使用`GL_COLOR_BUFFER_BIT`来设置颜色缓冲的值。当调用glClear函数，清除颜色缓冲之后，整个颜色缓冲都会被填充为glClearColor里所设置的颜色。

#### 例子

```c++
glClearColor(1.0f, 1.0f, 1.0f, 1.0f);// 对应黑色RGB值(255,255,255)转换后的值
glClear(GL_COLOR_BUFFER_BIT);
```

### 结束释放

结束的时候需要释放之前的资源（类似c++的delete），通过`glfwTerminate()`完成
#### 例子

```c++
glfwTerminate(); // 释放资源
```

### 运行效果

运行一下，就能看到一个稳定的~~智慧的~~蓝色窗口叫hello world啦~

![image-20220917004420571](https://s2.loli.net/2022/09/17/TUhKkX69dCMO1JP.png)

### 完整代码

```c++
#include"glad/glad.h"
#include"GLFW/glfw3.h"
#include <iostream>
using namespace std;
int main() {
	// 初始化glfw
	if (!glfwInit()) {
		cout << "Failed to initialize GLFW" << endl;
		return -1;
	}
	glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
	glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
	glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
	
	// 创建窗口
	GLFWwindow* window = glfwCreateWindow(800, 600, "hello world", NULL, NULL);
	if (window == nullptr) {
		cout << "Failed to create window" << endl;
		return -1;
	}
	glfwMakeContextCurrent(window);
	
	// 初始化glad
	if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress)) {
		cout << "Failed to initialize GLAD" << endl;
		return -1;
	}

	// 设置视口
	glViewport(0, 0, 800, 600);

	// 循环渲染
	while (!glfwWindowShouldClose(window)) { // 检查是否关闭
		// 清空窗口
		glClearColor(0.5573f, 0.7480f, 0.8745f, 1.0f);
		glClear(GL_COLOR_BUFFER_BIT);

		//刷新缓冲并检测事件
		glfwSwapBuffers(window);
		glfwPollEvents();

	}

	//清空资源
	glfwTerminate(); 
	return 0;
}
```


### 碎碎念

很奇怪的是，课程进行到这里，老师接下来开始让我们使用 OpenGL 来实现直线的扫描转换，但是在搜索了很久的相关资料后，基本上所有代码都是通过直接使用`opengl32.lib`中包含的最基本的openGL1.1的函数来进行Windows平台上的设置。而按照目前这样的设置来的话是通过使用glad来加载合适的openGL函数库，按照上面的设置包含了glad的库的话`opengl32.lib`中的部分函数就不再可用了，比如说`glVertex2f(GLfloat x, GLfloat y)`这个函数就在使用glad后寄了，取而代之的是`glVertexAttrib2f(GLuint index, GLfloat x, GLfloat y)` 。

后来在[这篇文章](https://www.cxyzjd.com/article/u013412391/108690963)里看到说，“GLAD本质上是一个[OpenGL Loading Library](https://www.khronos.org/opengl/wiki/OpenGL_Loading_Library)库”，可以用来在运行时读取OpenGL的函数地址，包括核心也包括扩展。对于想要访问高于1.1的功能，使用OpenGL Loading Library都是必须的。也就是说如果你想调用OpenGL高于1.1版本的函数，你就不得不在每次都使用OpenGL扩展机制，因为`opengl32.dll`并不提供他们的入口。所以glad的意义就是更加方便的使用除了`opengl32.dll`包含的那些最核心的函数以外的OpenGL的完整函数。

再后来仔细想想，`opengl32.dll`实际上是只用于Windows平台下的库（都是DLL了），而glad是一个跨平台的库，这两个对同一功能的实现和封装有所不同也是完全有可能的。~~*仔细一想发现就这问题还折磨我两三天*~~

再再后来，重新阅读了教程里因为没看懂被我跳过的一部分**“核心模式与立即渲染模式”** 噢才发现是这样。也就是说`opengl32.dll`里使用的是**固定渲染管线**（也就是**立即渲染模式**），而从OpenGL3.2后因为效率太低而废弃了，转而推荐使用OpenGL的核心模式进行开发。这种模式更为灵活、效率更高、更有助于深入理解图形编程。此处只需要知道glad在核心模式下完全移除了旧的特性。~~*看不懂没关系，后面还会再提及相关概念的。*~~



## 第三步：点动成线


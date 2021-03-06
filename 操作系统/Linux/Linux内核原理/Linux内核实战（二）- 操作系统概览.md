os就像一个软件外包，内核就相当于这家外包公司老板。
接下来请假设你就是这个老板，方便理解os如何协调各种资源帮客户做事。

后文中
- 用户指os的用户
- 客户指外包公司的客户
# 1 硬件概述
## 1.1 鼠标和键盘
计算机的输入设备。

用户想要告诉计算机应该做什么，都是通过这两个设备。

一家外包公司如何知道客户需求？
需配备销售、售前等角色，专门负责**和客户对接**，把客户需求拿回来，统称这些人为客户对接员。
## 1.2 屏幕
即显示器，计算机的输出设备，将计算机处理用户请求后的结果反馈给客户。

显示器上面显示的东西由显卡控制。
显示器和显卡都有“坐标”概念，即何图像在何坐标，都是定义好再画。
本来在某坐标画了一个鼠标箭头，当接到鼠标移动的事件之后，你应该按相同的方向，按照一定比例（鼠标灵敏度），在屏幕的某个坐标再画一个鼠标箭头。

当客户给你提了需求，不管你做还是不做，最终做成什么样，你都需要给客户反馈，所以你要配备交付人员，将做好的需求展示给他们。

## 1.3 输入设备驱动
客户对接员。

有时新插上一个鼠标，会弹窗通知你安装驱动，这就是os这家外包公司给你配备的对接人员。当客户告诉对接员需求时，对于os，输入设备会发送一个中断。

客户肯定希望外包公司把正在做的事情都停下来服务它。所以，这个时候客户发送的需求就被称为中断事件（Interrupt Event）。

## 1.4 输出设备驱动
交付人员。

显卡会有显卡驱动，在os中称为输出设备驱动。

# 2 从点击程序，窥探OS全貌

有了客户对接员和交付人员，外包公司就可以处理用户“在桌面上点击QQ图标”的事件了。

首先，鼠标双击会触发一个中断，这相当于客户告知客户对接员“有了新需求，需要处理一下”。你会事先把处理这种问题的方法教给客户对接员。在操作系统里面就是调用中断处理函数。操作系统发现双击的是一个图标，就明白了用户的原始诉求，准备运行QQ和别人聊天。

你会发现，运行QQ是一件大事，因为将来的一段时间，用户要一直和QQ进行交互。

这就相当于你们公司接了一个大单，而不是处理零星的客户需求，这个时候应该单独立项。一旦立了项，以后与这个项目有关的事情，都由这个项目组来处理。

## 2.1文件管理系统

立项可不能随便立，一定要有一个项目执行计划书，说明这个项目打算怎么做，一步一步如何执行，遇到什么情况应该怎么办等等。

换句话说，对QQ这个程序来说，它能做哪些事情，每个事情怎么做，先做啥后做啥，都已经作为程序逻辑写在程序里面，并且编译成为二进制了。这个程序就相当于项目执行计划书。

电脑上的程序有很多，它们都以二进制文件的形式保存在硬盘上。硬盘是个物理设备，要按照规定格式化成为文件系统，才能存放这些程序。文件系统需要一个系统进行统一管理，称为文件管理子系统（File Management Subsystem）。

## 2.2 进程与程序

对于你们公司，项目立得多了，项目执行计划书也会很多，同样需要有个统一保存文件的档案库，而且需要有序地管理起来。

当你从资料库里面拿到这个项目执行计划书，接下来就需要开始执行这个项目了。项目执行计划书是静态的，项目的执行是动态的。

同理，当操作系统拿到QQ的二进制执行文件的时候，就可以运行这个文件了。

- QQ的二进制文件是静态的，称为程序（Program），
- 运行起来的QQ，是不断进行的，称为进程（Process）。

## 2.3 系统调用

你会发现，一个项目要想顺畅进行，需要用到公司的各种资源，比如说盖个公章、开个证明、申请个会议室、打印个材料等等。

这里有个两难的权衡

- 资源有限，甚至是涉及机密的，不能由项目组滥取滥用
- 效率，咱是一个私营企业，保证项目申请资源的时候只跑一次，这样才能比较高效。

为了平衡这一点，一方面涉及核心权限的资源，还是应该被公司严格把控，审批了才能用；

另外一方面，为了提高效率，最好有个统一的办事大厅，明文列出提供哪些服务，谁需要可以来申请，然后就会有回应。

在操作系统中，也有同样的问题。

例如多个进程都要往打印机上打印文件，如果随便乱打印进程，就会出现同样一张纸，第一行是A进程输出的文字，第二行是B进程输出的文字，全乱套了。所以，打印机的直接操作是放在操作系统内核里面的，进程不能随便操作。但是操作系统也提供一个办事大厅，也就是系统调用（System Call）。

系统调用也能列出来提供哪些接口可以调用，进程有需要的时候就可以去调用。

这其中，立项是办事大厅提供的关键服务之一。同样，任何一个程序要想运行起来，就需要调用系统调用，创建进程。

## 2.4 进程管理系统

一旦项目正式立项，就要开始执行，就要成立项目组，将开发人员分配到这个项目组，按照项目执行计划书一步一步执行。

为了管理这个项目，我们还需要一个项目经理、一套项目管理流程、一个项目管理系统，例如程序员比较熟悉的Jira。如果项目多，可能一个开发人员需要同时执行多个项目，这就要考验项目经理的调度能力了。

在操作系统中，进程的执行也需要分配CPU进行执行，也就是按照程序里面的二进制代码一行一行地执行。

于是，为了管理进程，我们还需要一个进程管理子系统（Process Management Subsystem）。如果运行的进程很多，则一个CPU会并发运行多个进程，也就需要CPU的调度能力了。

## 2.5 内存管理系统

每个项目都有自己的私密资料，这些资料不能被其他项目组看到。这些资料主要是项目在执行的过程中，产生的很多中间成果，例如架构图、流程图。

执行过程中，难免要在白板上或者本子上写写画画，如果不同项目的办公空间不隔离，一方面，项目的私密性不能得到保证，A项目的细节，B项目也能看到；另一方面，项目之间会相互干扰，A项目组的人刚在白板上画了一个架构图，出去上个厕所，结果B项目组的人就给擦了。

如果把不同的项目组分配到不同的会议室，就解决了这个问题。当然会议室是有限的，需要有人管理和分配，并且需要一个会议室管理系统。

在操作系统中，不同的进程有不同的内存空间，但是整个电脑内存就这么点儿，所以需要统一的管理和分配，这就需要内存管理子系统（Memory Management Subsystem）。

如果想直观地了解QQ如何使用CPU和内存，可以打开任务管理器，你就能看到QQ这个进程耗费的CPU和内存。

项目执行的时候，有了一定的成果，就要给客户演示。例如客户说要做个应用，我们做出来了要给客户看看，如果客户说哪里需要改，可以根据客户的需求再改，这就需要交付人员了。

QQ启动之后，有一部分代码会在显示器上画一个对话框，并且将键盘的焦点放在了输入框里面。CPU根据这些指令，就会告知显卡驱动程序，将这个对话框画出来。

于是使用QQ的用户就会很开心地发现，他能和别人开始聊天了。

当用户通过键盘噼里啪啦打字的时候，键盘也是输入设备，也会触发中断，通知相应的输入设备驱动程序。

我们假设用户输入了一个“a”。这就像客户提出了新的需求给客户对接员。客户对接员收到需求后，因为是对接这个项目的，因而就回来报告，客户提新需求了，项目组需要处理一下。项目执行计划书里面一般都会有当遇到何种需求应该怎么做的规定，项目组就按这个规定做了，然后让交付人员再去客户那里演示就行了。

对于QQ来讲，由于键盘闪啊闪的焦点在QQ这个对话框上，因而操作系统知道，这个事件是给这个进程的。QQ的代码里面肯定有遇到这种事件如何处理的代码，就会执行。一般是记录下客户的输入，并且告知显卡驱动程序，在那个地方画一个“a”。显卡画完了，客户看到了，就觉得自己的输入成功了。

当用户输入完毕之后，回车一下，还是会通过键盘驱动程序告诉操作系统，操作系统还是会找到QQ，QQ会将用户的输入发送到网络上。QQ进程是不能直接发送网络包的，需要调用系统调用，内核使用网卡驱动程序进行发送。

这就像客户对接员接到一个需求，但是这个需求需要和其他公司沟通，这就需要依靠公司的对外合作部，对外合作部在办事大厅有专门的窗口，非常方便。
![](https://img-blog.csdnimg.cn/2021011219120620.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTg5NTEw,size_1,color_FFFFFF,t_70)


# 总结

到这里，一个外包公司大部分的职能部门都凑齐了。你可以对应着下图的操作系统内核体系结构，回顾一下它们是如何组成一家公司的。

QQ的运行过程，只是一个简单的比喻。在后面的章节中，我会展开讲述每个部分是怎么工作的，最后我会再将这个过程串起来，这样你就能了解操作系统的全貌了。

### 操作系统内核体系结构图
![](https://img-blog.csdnimg.cn/20210112191152444.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzNTg5NTEw,size_1,color_FFFFFF,t_70)

参考
- 趣谈Linux操作系统
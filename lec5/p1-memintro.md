---
marp: true
theme: default
paginate: true
_paginate: false
header: ''
footer: ''
backgroundColor: white
---

<!-- theme: gaia -->
<!-- _class: lead -->

# 第五讲 物理内存管理
## 第一节 地址空间
<br>
<br>

向勇 陈渝 李国良 任炬 

<br>
<br>

2025年春季

[课程幻灯片列表](https://www.yuque.com/xyong-9fuoz/qczol5/glemuu?)

---

**提纲**

### 1. 计算机的存储层次
2. 地址和地址空间
3. 虚拟存储的作用

---

##### 物理地址、逻辑地址、虚拟地址

- **物理地址**(PA, Physical Address) ：用于内存芯片级的单元寻址，与处理器和CPU连接的**地址总线**相对应。 
- **逻辑地址**(LA, Logical Address) ：在程序的编译和链接阶段生成，表示程序中的地址偏移，它在载入内存之前使用。
- **虚拟地址**(VA，Virtual Address)：操作系统在程序加载过程中，将逻辑地址调整或映射到适当的虚拟地址空间


---


##### 虚拟地址转换为物理地址
- **段式管理**:
   - **有**段式内存管理：虚拟地址通过**分段**转换为物理地址
   - 有段式内存管理时，虚拟地址也称为**线性地址**(LA，Linear Address)

- **页式管理**:
   - **有**页式内存管理：虚拟地址通过**分页**转换为物理地址

- **段页式管理**:
    - 虚拟地址先通过**分段**，再通过**分页**转换为物理地址
<!--

   - **没有**段式内存管理：虚拟地址与物理地址相同
   - **没有**页式内存管理的情况下，虚拟地址和物理地址相同

- **逻辑地址**(LA, Logical Address) ：**CPU执行机器指令**时，用来指定一个操作数或者是一条指令的地址。也是用户编程时使用的地址。

- **线性地址(linear address)或也叫虚拟地址(virtual address)**：跟逻辑地址类似，它也是一个不真实的地址。
- 逻辑地址 + 段式管理 > 虚拟地址(线性地址)
    - 在**没有**段式内存管理的情况下，逻辑地址与虚拟地址相同
- 虚拟地址 + 页式管理 > 物理地址
    - 在**没有**页式内存管理的情况下，虚拟地址和物理地址相同
-->
<!-- - 逻辑地址指CPU在**段式**内存管理转换前的地址；
# - 线性地址指CPU在**页式**内存管理转换前的地址。

#---

##### 逻辑地址与物理地址的关系
-->


---

##### 计算机的存储层次结构

![w:800](figs/computer.png)

---

##### 计算机的存储多层结构
![w:700](figs/mem-layers.png)

比较新的各种存储介质的访问速度参数：
* [金字塔层次结构](https://www.cnblogs.com/binarylei/p/12588928.html)：越靠近CPU速度越快，容量越小，价格越贵

---

##### 操作系统对内存资源的抽象
![w:950](figs/os-mem-mgr.png)

<!--
[操作系统内核的特征](https://learningos.github.io/os-lectures/lec1/p2-whatisos.html#9)：并发、共享、虚拟、异步
-->

---

##### 内存管理

- 操作系统中的**内存管理方式**
  - 重定位(relocation)
  - 分段(segmentation)
  - 分页(paging)
  - 虚拟存储(virtual memory/storage)
- **操作系统的内存管理高度依赖硬件**
  - 与计算机存储架构紧耦合
  - MMU (内存管理单元): 处理CPU存储访问请求的硬件

---
**提纲**

1. 计算机的存储层次
### 2. 地址和地址空间
3. 虚拟存储的作用

---

##### 地址空间的定义


- 物理地址空间：物理内存的地址空间
  - 起始地址$0$，直到 $MAX_{phy}$
- 虚拟地址空间：虚拟内存的地址空间
  - 起始地址$0$，直到 $MAX_{virt}$
- 逻辑地址空间：程序执行的地址空间
  - 起始地址$0$， 直到 $MAX_{prog}$

三种地址空间的**视角不同**

---

##### 逻辑地址生成
![w:950](figs/create-logic-addr.png)

---

##### 地址生成时机

- 编译时
  - 假设起始地址**已知**
  - 如果起始地址改变，必须重新编译
- 加载时
  - 如编译时起始地址**未知**，编译器需生成可重定位的代码 (relocatable code) 
  - 加载时，位置可不固定，生成绝对（虚拟）地址
- 执行时
  - 执行时代码不可修改
  - 需**地址转换(映射)硬件**支持

---

##### 地址生成过程
- CPU
  <!-- ALU：需要**逻辑地址**的内存内容-->
  - MMU：进行虚拟地址和物理地址的**转换**
  - CPU控制逻辑：给总线发送**物理地址**请求
- 内存
  - 发送**物理地址**的内容给CPU
  - 接收CPU数据到物理地址
- 操作系统
  - 建立虚拟地址VA和物理地址PA的映射

---

##### 地址检查
![bg w:950](figs/addr-check-exp.png)

---

**提纲**

1. 计算机的存储层次
2. 地址和地址空间
### 3. 虚拟存储的作用

---

##### 外存的缓存

虚拟内存可作为外存的缓存

- **常用数据**放在物理内存中
- **不常用数据**放在外存 
- 运行的程序**直接用虚存地址**，不用关注具体放在物理内存还是外存

![bg right:49% 95%](figs/os-mem-mgr.png)

---

##### 简化应用编译和加载运行

每个运行程序具有**独立的地址空间**，而不管代码和数据在物理内存的实际存放，从而简化：
- 编译的执行程序**链接**
- 操作系统的执行程序**加载**
- **共享**：动态链接库、共享内存 
- 内存分配：物理不连续，**虚拟连续**
![bg right:43% 100%](figs/os-mem-mgr.png)

---

##### 保护数据

虚拟内存可保护数据
- **独立的地址空间**使得区分不同进程各自内存变得容易
- 地址转换机制可以进行可读/可写/可执行的检查
- 地址转换机制可以进行特权级检查
![bg right:49% 100%](figs/os-mem-mgr.png)
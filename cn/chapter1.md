# 概述

## 1.1  介绍
本文档详细介绍了OpenMAX集成层（IL）的API。由Khronos Group提出， 用于音视频以及图像编解码在嵌入式/移动设备上的底层接口的开放标准。主要目标是将编解码器进行系统抽象，使之适配与各种操作系统和软件平台。

### 1.1.1 关于Khronos Group
The Khronos Group is a member-funded industry consortium focused on the creation of open standard APIs to enable the authoring and playback of dynamic media on a wide variety of platforms and devices. All Khronos members may contribute to the development of Khronos API specifications, may vote at various stages before public deployment, and may accelerate the delivery of their multimedia platforms and applications through early access to specification drafts and conformance tests. The Khronos Group is responsible for open APIs such as OpenGL ES, OpenML, and OpenVG.

Khronos Group是一个行业联盟成员资助重点的API使创作，在多种平台和设备的动态媒体播放开放标准的建立。所有Khronos成员可能有助于Khronos API规范的发展，可能会投票前公共部署的各个阶段，并可以通过早期访问规范的草稿和一致性测试加速多媒体平台和应用的交付。Khronos Group负责如OpenGL ES API开放，开放多媒体程序库，和OpenVG。

### 1.1.2 OpenMAX简史
The OpenMAX set of APIs was originally conceived as a method of enabling portability of codecs and media applications throughout the mobile device landscape. Brought into the Khronos Group in mid-2004 by a handful of key mobile hardware companies, OpenMAX has gained the contributions of companies and institutions stretching the breadth of the multimedia field. As such, OpenMAX stands to unify the industry in taking steps toward media codec portability. Stepping beyond mobile platforms, the general nature of the OpenMAX IL API makes it applicable to all media platforms.

API集的OpenMAX原先是一种使编解码器和媒体在移动设备应用程序的可移植性景观的方法。进入Khronos集团在2004年由少数关键的移动硬件公司，OpenMAX取得贡献的公司和机构延伸的多媒体领域的广度。因此，OpenMAX站统一采取措施向媒体编解码器的可移植性的行业。跨越移动平台，对OpenMAX IL的一般性质的API使得它适用于所有媒体平台。

##1.2  OpenMAX集成层
The OpenMAX IL API strives to give media codecs portability across an array of platforms. The interface abstracts the hardware and software architecture in the system. Each codec and relevant transform is encapsulated in a component interface. The OpenMAX IL API allows the user to load, control, connect, and unload the individual components. This flexible core architecture allows the Integration Layer to easily implement almost any media use case and mesh with existing graph-based media frameworks.

OpenMAX IL的API力求给媒体编解码器在一系列的平台的可移植性。接口抽象了系统的硬件和软件体系结构。每个编解码器和相关的变换封装在一个组件接口。OpenMAX IL的API允许用户负荷，控制，连接，和卸载单独的组件。这种灵活的核心架构允许集成层轻松地实现几乎所有的媒体使用情况和网格与现有的基于图形的媒体框架。

###1.2.1 主要特性和优点
The OpenMAX IL API gives applications and media frameworks the ability to interface with multimedia codecs and supporting components (i.e., sources and sinks) in a unified manner. The codecs themselves may be any combination of hardware or software and are completely transparent to the user. Without a standardized interface of this nature, codec vendors must write to proprietary or closed interfaces to integrate into mobile devices. In this case, the portability of the codec is minimal at best, costing many development-years of effort in re-tooling these solutions between systems. Thus, the IL incorporates a specialized arsenal of features, honed to combat the problem of portability among many vastly different media systems. Such features include:

OpenMAX IL的API为应用程序和多媒体框架的能力和多媒体编解码器的接口和支持组件（即，源和汇）在一个统一的方式。可能是自己的编解码器的硬件或软件的任意组合，对用户是完全透明的。如果没有这种标准化的接口，编解码器的厂商必须写入专有或封闭的接口，以整合到移动设备。在这种情况下，可编解码器的可移植性是最小的，成本的许多发展多年的努力，在重新设计这些解决方案之间的系统。因此，IL将专业阿森纳的特点，磨练战斗的便携性问题的许多不同的媒体系统之间。这些特点包括：

- 灵活的基于组件的API Core
- 可以轻松添加新的编解码器
- Coverage of targeted domains (audio, video, and imaging) while remaining easily extensible by both the Khronos Group and individual vendors目标域（音频，视频，覆盖范围和成像）而其余的易扩展由Khronos集团和个体商贩
- Capable of being implemented as either static or dynamic libraries能够被实现为静态或动态库
- Retention of key features and configuration options needed by parent software (such as media frameworks)保留关键特征与配置选项needed by父母软件（such as媒体框架）
- Ease of communication between the client and the codecs and between codecs themselves在客户端与编解码器和之间的通信编解码器之间自己缓解

### 1.2.2 设计理念
As previously stated, the key focus of the OpenMAX IL API is portability of media codecs. The diversity of existing devices and media implementation solutions necessitates that the OpenMAX IL target the higher level of the media software stack as
the key initial user. For most operating systems, this means an existing media framework. Thus, much of the OpenMAX IL API is defined by requirements generated by the needs of media frameworks. Similarly, the IL is designed to allow the media framework layer to be as lightweight as possible. The result is an interface that is easily pluggable into most software stacks across operating system and framework boundaries. Likewise, several features of media frameworks were perceived to be handled at higher levels and not included in the API. Among these is the issue of file handling, which, if desired, may
be easily added to the IL structure outside of the standard. The design of the API also strove to accommodate as many system architectures as possible. The resulting design uses highly asynchronous communications, which allows processing to take place in another thread, on multiple processing elements, or on specialized hardware. In addition, the ability of hardware-accelerated codecs to communicate directly with one another via tunneling affords implementation architectures even greater flexibility and efficiency.

如前所述，对OpenMAX IL的重点API是媒体编解码器的可移植性。多样性的现有设备和媒体解决方案必须实现OpenMAX IL的目标媒体软件栈的较高水平
关键初始用户。对于大多数操作系统，这意味着现有的媒体框架。因此，大部分的OpenMAX IL的API是由媒体框架的需求而产生的需求定义。同样，IL的设计允许媒体框架层尽可能轻。其结果是一个接口，容易嵌入到大多数软件栈跨操作系统和框架的边界。同样，媒体框架的几个功能被认为是在更高的水平，而不是在API中处理。其中之一是文件处理的问题，如果需要，可以
容易添加到IL以外的标准结构。API的设计也力求尽可能容纳尽可能多的系统架构。由此产生的设计采用高度异步通信，它允许处理发生在另一个线程，在多个处理元素，或在专门的硬件。此外，硬件加速解码能力与另一个直接通过隧道提供了更大的灵活性和效率，实现结构。

###1.2.3 软件的景观
In most systems, a user-level media framework already exists. The OpenMAX IL API is designed to easily fit below these frameworks with little to no overhead between the interfaces. In most cases, the media framework provided by the operating system can be replaced with a thin layer that simply translates the API. Figure 1-1 illustrates the software landscape for the OpenMAX IL API.

在大多数系统中，用户级媒体框架已经存在。OpenMAX IL的API的设计非常适合在这些框架几乎没有开销之间的接口。在大多数情况下，操作系统提供的媒体框架可以用一个简单的API层来替换。图1-1显示了OpenMAX IL的API软件景观。

![](img/1_1.png)

**Figure 1-1. OpenMAX IL API Software Landscape**

To remove possible reader confusion, the OpenMAX standard also defines a set of Development Layer (DL) primitives on which codecs can be built. The DL primitives and their full relationship to the IL are specified in other OpenMAX specification
documents.

为了消除可能的读者混淆，OpenMAX标准还定义了一套开发层（DL）原语，编解码器可建。DL元素及其对IL完全的关系在其他OpenMAX规范规定
文件.

###1.2.4 利益相关者
A few categories of stakeholders represent the broad array of companies participating in the production of multimedia solutions, each with their own interest in the IL API.

一些类别的利益相关者代表了广泛的参与多媒体解决方案的生产企业，每个都有自己的兴趣在IL的API。

####1.2.4.1  芯片厂商
Silicon vendors (SV) are responsible for delivering a representative set of OpenMAX IL components that are specific to the vendor’s platform. The vendors are anticipated to also supply components that are representative of the capabilities of their platforms.

芯片厂商（SV）负责提供一组有代表性的OpenMAX IL成分是特定于供应商的平台。预计供应商还将提供代表其平台能力的组件。

####1.2.4.2  独立软件供应商
Independent software vendors (ISV) are anticipated to deliver additional differentiated OpenMAX IL components that may or may not be specific to a given silicon vendor’s platform.

独立软件供应商（ISV）预计将提供额外的分化OpenMAX IL的组件，可能会或可能不会具体到一个给定的硅供应商的平台。

####1.2.4.3  操作系统厂商
Operating System Vendors (OSV) are anticipated to deliver software multimedia framework and standard reference OpenMAX IL components that enable integration of the representative silicon vendor’s components and ISV components. The OSV is responsible for conformance testing of the standard reference OpenMAX components.

操作系统厂商（OSV）预计交付软件的多媒体框架和标准OpenMAX IL组件，使代表硅供应商的组件和软件组件的集成。OSV负责标准参考OpenMAX组件一致性测试。

####1.2.4.4  原始设备制造商
Original Equipment Manufacturers (OEM) are anticipated to modify and optimize the integration of OpenMAX components provided by SVs, ISVs, and OSVs to their specific product architectures to enable delivery of OpenMAX integrated multimedia devices. OEMs may also develop and integrate their own proprietary OpenMAX components.

原始设备制造商（OEM）预计修改和OpenMAX成分的SVS，ISV提供的优化整合，以其独特的产品架构和OSVs使OpenMAX集成多媒体设备交货。OEM可以开发和整合自己的专有OpenMAX组件。

###1.2.5 接口
The OpenMAX IL API is a component-based media API that consists of two main segments: the core API and the component API.

OpenMAX IL的API是一个基于组件的多媒体API，主要分为两部分：核心接口(core API)和组件接口(componet API)。

####1.2.5.1  核心
The OpenMAX IL core is used for dynamically loading and unloading components and for facilitating component communication. Once loaded, the API allows the user to communicate directly with the component, which eliminates any overhead for high commands. Similarly, the core allows a user to establish a communication tunnel between two components. Once established, the core API is no longer used and communications flow directly between components.

OpenMAX IL的核心是用于动态加载和卸载的组件和组件之间通信的便利。一旦加载，API允许用户直接与组件进行通信，从而消除了高命令的任何开销。同样，核心允许用户在两个组件之间建立通信通道。一旦建立，核心API不再使用和通信流组件之间的直接。

####1.2.5.2  组件
In the OpenMAX Integration Layer, components represent individual blocks of functionality. Components can be sources, sinks, codecs, filters, splitters, mixers, or any other data operator. Depending on the implementation, a component could possibly represent a piece of hardware, a software codec, another processor, or a combination thereof.

在OpenMAX集成层、组件代表独立的功能块。组件可以是源、汇、编解码器、滤波器、功分器、混频器、或任何其他数据操作。根据实现，一个组件可能代表一个硬件，一个软件编解码器，另一个处理器，或其组合。

The individual parameters of a component can be set or retrieved through a set of associated data structures, enumerations, and interfaces. The parameters include data relevant to the component’s operation (i.e., codec options) or the actual execution state of the component.

Buffer status, errors, and other time-sensitive data are relayed to the application via a set of callback functions. These are set via the normal parameter facilities and allow the API to expose more of the asynchronous nature of system architectures.

一个组件的各个参数可以设置或检索，通过一组相关的数据结构和接口，枚举。参数包括与组件操作相关的数据（即编解码器选项）或组件的实际执行状态。

Data communication to and from a component is conducted through interfaces called ports. Ports represent both the connection for components to the data stream and the buffers needed to maintain the connection. Users may send data to components through input ports or receive data through output ports. Similarly, a communication tunnel between two components can be established by connecting the output port of one component to a similarly formatted input port of another component.

数据通信和从一个组件是通过接口称为端口。端口代表组件与数据流的连接以及维护连接所需的缓冲区。用户可以通过输入端口发送数据到组件，或者通过输出端口接收数据。类似地，通过将一个组件的输出端口连接到另一个组件的类似格式化的输入端口，可以建立两个组件之间的通信通道。

##1.3  Definitions
When this specification discusses requirements and features of the OpenMAX IL API, specific words are used to convey their necessity in an implementation. Table 1-1 shows a list of these words.

当这个规范讨论要求和OpenMAX IL的功能API，具体的词来表达他们的需要的实现。表1-1列出了这些话。


| Word | Definition |
| ------------- | --------- |
| May  | The stated functionality is an optional requirement for an implementation of the OpenMAX IL API. Optional features are not required by the specification but may have conformance requirements if they are implemented. This is an optional feature as in “The component may have vendor specific extensions.” |
| Shall | The stated functionality is a requirement for an implementation of the OpenMAX IL API. If a component fails to meet a shall statement, it is not considered to conform to this specification. Shall is always used as a requirement, as in “The component designers shall produce good documentation.” |
| Should | The stated functionality is not a requirement for an implementation of the OpenMAX IL API but is recommended or is a good practice. Should is usually used as follows: “The component should begin processing buffers immediately after it transitions to the OMX_StateExecuting state.” While this is good practice, there may be a valid reason to delay processing buffers, such as not having input data available. |
| Will | The stated functionality is not a requirement for an implementation of the OpenMAX IL API. Will is usually used when referring to a third party, as in “the application framework will correctly handle errors.” |

**Table 1-1. Definitions of Commonly Used Words**

##1.4  作者
The following individuals, listed alphabetically by company, contributed to the OpenMAX Integration Layer Application Programming Interface Specification.

OpenMAX IL API协议由下列人员按公司字母表排列顺序完成。

-  Gordon Grigor (ATI)
-  Andrew Rostaing (Beatnik)
-  Chris Grigg (Beatnik)
-  Russell Tillitt (Beatnik)
-  Roger Nixon (Broadcom)
-  Brian Murray (Freescale)
-  Norbert Schwagmann (Infineon)
-  Mark Kokes (Nokia)
-  Samu Kaajas (Nokia)
-  Yeshwant Muthusamy (Nokia)
-  Jim Van Welzen (NVIDIA)
-  David Siorpaes (STMicroelectronics)
-  Diego Melpignano (STMicroelectronics)
-  Giulio Urlini (STMicroelectronics)
-  Kevin Butchart (Symbian)
-  Viviana Dudau (Symbian)
-  David Newman (Texas Instruments)
-  Leo Estevez (Texas Instruments)
-  Richard Baker (Texas Instruments)

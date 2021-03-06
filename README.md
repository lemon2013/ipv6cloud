# IPv6Cloud
基于IPv6的动态多层次云安全查询技术研究
## 简介
本章将简要地说明软件操作手册(以下简称本手册)的目的、范围以及名词定义
### 手册目的
本手册的目的在于告诉基于IPv6的云存储系统(以下简称本系统)的使用者本系统提供了哪些功能，以及如何正确地、有效地来使用这些功能。
### 手册范围
本手册首先简要地介绍本系统结构以及软件环境，然后说明本系统为使用者提供的各项功能及其详细的操作步骤。关于系统详细软硬件部署方面的内容，请参考本系统的《系统部署手册》
(详细见[配置基于IPv6的单节点Ceph](https://lemon2013.github.io/2016/11/06/配置基于IPv6的Ceph/)
，[Ceph对象存储(rgw)的IPv6环境配置](https://lemon2013.github.io/2016/11/09/Ceph对象存储-rgw-IPv6环境配置/)
，[ceph-rest-api的IPv6环境配置]( https://lemon2013.github.io/2017/12/26/ceph-rest-api/)
)。
本手册的使用者包括：Ceph运维人员、云盘用户、系统维护人员等。

本手册各章节内容安排如下：

第一章 简介：简单说明本手册的目的、范围和名词定义。

第二章 系统概述: 简单说明本系统的结构及其执行环境。

第三章 功能概述：简单说明本系统的功能。

第四章 操作说明: 逐一说明各项功能及其详细的操作步骤。其中“第三章 功能概述”与“第四章 操作说明”为本手册之重点，希望使用者能深入了解。
### 名词定义
OSD: Ceph集群中的节点。

MONMAP: Ceph集群中所有监控节点的信息。

OSDMAP: Ceph集群中所有OSD的信息，所有OSD节点的改变如进程退出，节点的加入和退出或者节点权重的变化都会反映到这张Map上。

PGMAP: 由 Monitor 维护的所有 PG 的状态。

Pool: Ceph存储数据时的逻辑分区。

## 系统简述
在本项目中，我们将云安全技术与网络安全技术相结合起来，在IPv6的环境下实现了一个动态多层次的云安全查询平台。本项目采用Java 语言利用Eclipse 集成开发环境开发一个平台，然后利用Tomcat7.0 搭建云计算平台的门户网站，该平台采用的服务器是IBM 服务器X3650M4 E5-2650v2，其操作系统为Cent0S 7.0。本系统主机配置：CPU4个，内存8G，磁盘160G。
## 功能概述
系统总共分为Ceph集群监控，配置管理，存储管理，共享文件等功能。

（1）Ceph集群监控：Ceph集群监控分为控制面板和OSD面板。控制面板实现了健康状态、存储容量分布状态、读写速度状态、对象数量状态、Pools信息分布状态、节点数量、存储节点数量、PGMAP等监控功能；OSD面板实现了所有OSD的详情监控以及OSD信息的搜索。

（2）配置管理：实现了用户信息的新增、删除、修改及查找功能；能够查看对应用户的最大桶数量、已建桶数量、文件数、已用量、使用状态、最大文件数及存储容量等信息。

（3）存储管理：对象存储模块实现了桶信息的新增、删除、查找功能；能够查看对应桶的桶名、创建时间、拥有者及拥有者ID信息。内容管理模块实现了文件信息的上传、下载、删除、查找及共享功能；能够查看对应文件的所在桶名、文件名、文件大小、更新时间、存储类型、拥有者等信息。

（4）共享文件：安全检索模块实现了密文领域的多关键词排名查询。基于加密文件以及对应的加密关键词信息构建安全索引树，用户在查询时通过关键词切分以及陷门生成算法生成陷门，服务器根据陷门以及索引树通过深度优先算法返回用户最相关的k个文件，并提供文件下载功能。安全存储模块实现了文本关键字提取功能、关键字的加密功能、文件加解密功能。
## 操作说明
### 功能模块1
集群监控：集群监控分为控制面板和OSD面板，能够监控各种状态信息。
#### 控制面板
功能描述：监控健康状态、监控节点数量、监控存储节点数量、监控PGMAP，可以从各个板块看到对应的监控信息。
操作说明及示例：

1）点开菜单栏集群监控->控制面板，可查看各状态信息
![首页](http://oqo3t056d.bkt.clouddn.com/cephindex.png)
2）移动鼠标至MONMAP可查看监控节点的IP地址。

3）移动鼠标至OSDMAP可查看OSD的IP地址。

4）移动鼠标至容量分布图显示实时的存储容量分布状态。

5）移动鼠标至节点查看当前具体时间和读写速度。

6）移动鼠标至节点查看当前具体时间及对象数。

7）移动鼠标至不同节点查看各Pool的数量分布状态。

8）移动鼠标至节点查看具体的时间和已经使用的容量。

#### OSD面板
![首页](http://oqo3t056d.bkt.clouddn.com/osdindex.png)
（1）功能描述：对所有OSD的详情监控，选择特定OSD可监控特定OSD使用量状态、主机地址、主机名，并能监控各OSD上的PG数量、Variance、Storage、Latency的状态信息.
操作说明及示例：

1）下拉OSD列表，点击选择OSD。

2）查看使用量。

3）移动鼠标至节点查看具体时间及PG数目。

4）移动鼠标至节点查看OSD上的Variance信息。

5）移动鼠标至黑色菱形节点查看具体时间及可用存储大小，移动鼠标至蓝色圆形节点查看具体时间及已用存储信息。

6）移动鼠标至节点查看具体时间及延时信息。

（2）信息搜索
功能描述：搜索OSD的ID，查看OSD的信息。
操作说明：在搜索框输入OSD的ID可查看对应OSD的WEIGHT、REWEIGHT、SIZE、USE、AVAIL、VAR等信息。
操作示例：
![首页](http://oqo3t056d.bkt.clouddn.com/osdinfo.png)
### 功能模块2
配置管理：配置管理包括用户管理，能够对用户信息实现增删查改操作，并能查看用户的各种信息。
#### 用户管理
![首页](http://oqo3t056d.bkt.clouddn.com/userindex.png)
（1）新增用户
功能描述：新增用户信息
操作说明：点击界面上的“新增”按钮，弹出信息框，填写完整用户名ID、用户名、邮箱，选择是否停用、是否配额，再点击信息框右下角的“新增”按钮即新增完成；点击信息框右下角的“关闭”按钮即取消新增操作。
操作示例：

（2）修改用户信息：
功能描述：对特定用户的个人信息进行修改。
操作说明：鼠标勾选要修改的用户，点击界面上的“编辑”按钮，弹出信息框，填写完整对应信息，再点击信息框右下角的“修改”按钮即完成用户信息修改，点击信息框右下角的“关闭”按钮即取消修改操作。


（3）删除用户
功能描述：删除特定用户的信息。
操作说明：鼠标勾选要删除的用户，点击界面上的“删除”按钮即完成用户的删除。
操作示例：

（4）用户信息查找
功能描述：搜索用户，查看特定用户的具体信息。
操作说明：在界面上的搜索框内输入关键词，比如用户ID、用户名等信息，即可搜索到相应的用户信息。
操作示例：

### 功能模块3
存储管理：存储管理包括对象存储和内容管理。对象存储能够实现对桶信息的增删查操作；内容管理能够实现对文件的上传/下载、删除和查找操作。
#### 对象存储
![首页](http://oqo3t056d.bkt.clouddn.com/bucketindex.png)
（1）新增桶信息
功能描述：增加新的桶。
操作说明：点击界面上“新增”按钮，弹出信息框，在信息框内填写新增桶名再点击信息框右下角的“新增”按钮即完成新增桶，点击信息框右下角的“关闭”按钮即取消新增操作。
操作示例：

（2）删除桶信息
功能描述：删除特定桶的信息。
操作说明：鼠标勾选要删除的桶，点击界面上的“删除”按钮即完成删除。

（3）桶信息查找
功能描述：搜索桶，查看特定桶的具体信息。
操作说明：在界面上的搜索框内输入关键词，比如桶名、创建时间等信息，即可搜索到对应的桶信息。
操作示例：
#### 内容管理
![首页](http://oqo3t056d.bkt.clouddn.com/objindex.png)
（1）上传文件
功能描述：上传本地文件至服务器。
操作说明：点击界面上的“上传”按钮，弹出信息框，可在本地文件夹选择要上传的文件并输入桶名，再点击信息框右下角的“上传”按钮即完成文件上传，点击信息框右下角的“关闭”按钮即取消文件上传。
操作示例：

（2）下载文件
功能描述：从服务器下载文件至本地。
操作说明：鼠标勾选要下载的文件，点击界面上的“下载”按钮即可下载文件至本地。
操作示例：

（3）删除文件
功能描述：删除服务器端的特定文件。
操作说明：鼠标勾选要删除的文件，点击界面上的“删除”按钮即完成删除。
操作示例：

（4）查找文件
功能描述：从服务器端搜索文件。
操作说明：在界面上的搜索框内输入关键词，比如桶名、文件名或者部分文件名等信息，即能搜索到对应文件的信息。
操作示例：

### 功能模块4
共享文件：该模块包括安全检索和和安全存储。安全检索能够实现密文领域的多关键词排名查询；安全存储能够实现关键词提取、关键词加密和文件加解密。
#### 安全检索
![首页](http://oqo3t056d.bkt.clouddn.com/tbmsm.png)

（1）密文领域的多关键词排名查询
功能描述：输入关键词，系统根据加密算法将关键词加密，并在密文领域进行top-k查询，将与查询关键词最相关的k个文件返回。
操作说明：在界面上的搜索框内输入搜索关键词，下拉旁边的选择框选择返回最相关文件的数量，再点击界面上的“搜索”按钮即显示查询结果。
操作示例：
#### 安全存储
![首页](http://oqo3t056d.bkt.clouddn.com/tbs.png)

（1）文本关键词提取
功能描述：对特定文本内容提取选定数量的关键词。
操作说明：下拉选择框选择关键词提取个数，在文本输入框输入一段内容，点击“提取”按钮，即能显示系统根据算法提取的最相关的关键词。
操作示例：

（2）关键词加密
功能描述：对关键词进行加密，并保证同一用户在不同时刻加密同一关键词的结果不一样，且不同用户对同一个关键字加密结果也不一样，并判断密文关键词是否相等。
操作说明：在输入框内分别输入关键词内容，并分别点击“用户1加密”按钮和“用户2加密”按钮，即能显示两者加密结果，再点击“判断是否相等”按钮即能判断密文关键词是否相等。
操作示例：

（3）文件加解密
功能描述：直接输入要加密的文本内容或选择本地文件，系统根据加密算法对其进行加密。
操作说明1：在加密部分直接输入要加密的文本，点击“加密”按钮，即完成加密并显示加密结果。在解密部分直接输入密文文本，点击“解密”按钮，即完成解密并显示解密结果。
操作示例1：

操作说明2：在加密部分点击“选择文件”按钮在本地文件夹选择要加密的文件，点击“文件加密”按钮完成加密，系统显示“下载”按钮，点击“下载”按钮即可下载密文文件。在解密部分，点击“选择文件”按钮在本地文件夹选择要解密的密文文件，点击“文件解密”按钮完成解密，系统显示“下载”按钮，点击“下载”按钮即可下载解密文件。
操作示例2：




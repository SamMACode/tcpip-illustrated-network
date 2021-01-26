# DNS：域名系统

> 域名系统（`DNS`）是一种用于`TCP/IP`应用程序的分布式数据库，它提供主机名字和`IP`地址之间的转换及有关电子邮件的选路信息。这里提到的分布式是指在`Internet`上的单个站点不能拥有所有的信息。

从应用的角度来看，对`DNS`的访问是 通过一个地址解析器（`resolver`）来完成的，在`Unix`主机中，该解析器通过两个库函数`gethostbyname(3)`和`gethostbyaddress(3)`来完成访问，解析器通过一个或多个名字服务器来完成这种相互转换。 

`DNS`的名字空间和`Unix`的文件系统相似，也具有层次结构。每个结点有一个至多`63`个字符长的标识，这颗树的树根是没有任何标识的特殊结点。以点"."结尾的域名称为绝对域名或完全合格的域名`FQDN（Full Qualified Domain Name）`，例如`sun.tuc.noao.edu`。

![dns-header-data](./reference-media/dns-header-data.png)	


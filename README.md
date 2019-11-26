![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 在SQL中将SSIS用于多个SQL实例
#### Using SSIS for multiple SQL Instances
**发布-日期: 2016年09月14日 (评论)**



## Contents

- [中文](#中文)
- [English](#English)
- [XML Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
在此示例中，我们有一个带有SQL Server 2014的通用数据库服务器。这有6个实例。
DEFAULT
SQLSHARE01
SQLSHARE02
SQLSHARE03
SQLSHARE04
SQLSHARE05

现在根据SQL 2014，你仍然无法在多实例SQL数据库服务器上安装SSIS。在大多数情况下，你将在实例中至少有一些数据库需要SSIS服务来处理它的ETL自动化。
好，那么你在这种情况下做什么？
使用单个SSIS安装来驱动所有其他实例的SSIS进程没有问题，但是如何管理每个MSDB中的SSIS包？
答案（现在和以前的SQL版本一样的话）是编辑SSIS的配置文件。在我的例子中，它位于这里：



## English
In this example we have a generic database server with SQL Server 2014. There are 6 instances.

DEFAULT
SQLSHARE01
SQLSHARE02
SQLSHARE03
SQLSHARE04
SQLSHARE05
Now; according to SQL 2014; you are still unable to install SSIS on a Multi-Instance SQL Database Server. Ok this is all well and good, but most of the time you’ll have least some databases across the instances which requires the SSIS Service to handle it’s ETL automations.
Ok; so what do you do in this case?
It’s no problem to have a single SSIS install which drives the SSIS processes across all other instances, but how do you manage the SSIS packages in each MSDB?
The classic answer to this ( which is still current today as it was in former SQL versions ) is to edit the config file for SSIS. In my case it’s located here:


The actual file name is called this: MsDtsSrvr.ini You’ll notice it’s basically an XML file with extension .ini

In any event; here’s what you’ll need to edit:

D:Program FilesMicrosoft SQL Server120DTSBinn
![#](images/use-multiple-ssis-instances-in-sql-server-a.png?raw=true "#")



---
## Logic
```XML
<?xml version="1.0" encoding="utf-8"?>
<DtsServiceConfiguration xmlns:xsd="<a href="http://www.w3.org/2001/XMLSchema"">http://www.w3.org/2001/XMLSchema"</a> xmlns:xsi="<a href="http://www.w3.org/2001/XMLSchema-instance">">http://www.w3.org/2001/XMLSchema-instance"></a>
<StopExecutingPackagesOnShutdown>true</StopExecutingPackagesOnShutdown>
<TopLevelFolders>
<Folder xsi:type="SqlServerFolder">
<Name>MSDB</Name>
<ServerName>.</ServerName>
</Folder>
<Folder xsi:type="FileSystemFolder">
<Name>File System</Name>
<StorePath>..Packages</StorePath>
</Folder>
</TopLevelFolders>
</DtsServiceConfiguration>


```

So what do you need to edit exactly? Just take the following lines… Copy & Paste, and edit them for each instance you want to manage.

那么你需要准确编辑什么？只需复制并粘贴以下行，并为要管理的每个实例编辑它们。

```XML
<Folder xsi:type="SqlServerFolder">
 
<Name>MSDB MyInstanceName</Name>
<ServerName>. MyInstanceName</ServerName>
 
</Folder>

```
As you can see I just used MSDB SQLSHARE01, …02, …03 etc. This perfectly denotes where to find the MSDB database packages in the other instances.


正如你所看到的，我只使用了MSDB SQLSHARE01，... 02，... 03等。这完美地表示了在其他实例中查找MSDB数据库包的位置。

Then simply paste it back into the original script like the following: ( I gave additional edits to represent every instance in my server ).


然后将其简单地粘贴回原始脚本，如下所示：我提供了额外的编辑来表示我服务器中的每个实例。


![#](images/use-multiple-ssis-instances-in-sql-server-b.png?raw=true "#")

Once you do this; you’ll be able to manage each package set from each database instance. This is still classified as a work around by the way. It’s not the end-all-be-all solution until Microsoft makes each instance capable have carrying it’s own SSIS Service independent of the other.
You’ll see the folders for each MSDB under the b-tree folder structure under the SSIS portion of Management Studio.
Anyway; hope this is helpful.

一旦你这样做了，你将能够管理每个数据库实例中的每个包集。顺便提一下，这仍然被归类为一种解决方案。在Microsoft中使每个实例都能够独立承载自己的SSIS服务之前，这并不是最终解决方案。 你将在Management Studio的SSIS部分下的b-tree文件夹结构下看到每个MSDB的文件夹。希望可以帮到你。


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")


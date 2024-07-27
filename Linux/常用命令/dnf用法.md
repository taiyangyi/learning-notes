## 1. 命令简介

`dnf` 是新一代的RPM软件包管理器，与 `yum` 包管理器相比，在用户体验、内存占用、依赖分析、运行速度等多方面得到了较好的提升。

`dnf` 首次出现在Fedora18(注：中文名 `费多拉`，Fedora对于用户而言，是一套功能完备、更新快速的免费操作系统)这个发行版中，在Fedora22发行版本中正式取代 `yum` 成为其默认的包管理器。

`dnf` 未默认在 `RHEL` 或 `CentOS 7` 系统中安装，如果使用，可以单独安装。

> 安装步骤

```
# 安装epel-release依赖
yum install epel-release
# 安装dnf包
yum install dnf
```

## 2. 语法格式

```
dnf [options] COMMAND
```

## 3. 选项说明

### 3.1 主要命令列表

```
alias                     列出或新建命令别名
autoremove                删除所有原先因为依赖关系安装的不需要的软件包
check                     在包数据库中寻找问题
check-update              检查是否有软件包升级
clean                     删除已缓存的数据
deplist                   列出软件包的依赖关系和提供这些软件包的源
distro-sync               同步已经安装的软件包到最新可用版本
downgrade                 降级包
group                     显示或使用组信息
help                      显示一个有帮助的用法信息
history                   显示或使用事务历史
info                      显示关于软件包或软件包组的详细信息
install                   向系统中安装一个或多个软件包
list                      列出一个或一组软件包
makecache                 创建元数据缓存
mark                      在已安装的软件包中标记或者取消标记由用户安装的软件包。
module                    与模块交互。
provides                  查找提供指定内容的软件包
reinstall                 重装一个包
remove                    从系统中移除一个或多个软件包
repolist                  显示已配置的软件仓库
repoquery                 搜索匹配关键字的软件包
repository-packages       对指定仓库中的所有软件包运行命令
search                    在软件包详细信息中搜索指定字符串
shell                     运行一个非交互式的 DNF shell
swap                      运行交互式的 DNF mod 以删除或安装 spec
updateinfo                显示软件包的参考建议
upgrade                   升级系统中的一个或多个软件包
upgrade-minimal           升级，但只有“最新”的软件包已修复可能影响你的系统的问题
```

### 3.2 插件命令列表

```
builddep                  Install build dependencies for package or spec file
changelog                 查看软件包的改变日志数据
config-manager            管理 dnf 配置选项和软件仓库
copr                      与 Copr 仓库交互
debug-dump                转储已安装的 RPM 软件包信息至文件
debug-restore             恢复调试用转储文件中的软件包记录
debuginfo-install         安装调试信息软件包
download                  下载软件包至当前目录
needs-restarting          判断所升级的二进制文件是否需要重启
playground                与 Playground 仓库交互。
repoclosure               显示仓库中未被解决的依赖关系的列表
repodiff                  列出两组仓库中的不同
repograph                 以点线图方式输出完整的软件包依赖关系图
repomanage                管理 RPM 软件包目录
reposync                  下载远程仓库中的全部软件包
```

### 3.3 可选参数

```
  -c [config file], --config [config file] 配置文件位置
  -q, --quiet           静默执行
  -v, --verbose         详尽执行
  --version             显示 DNF 版本并推出
  --installroot [path]  设置目标根目录
  --nodocs              不要安装文档
  --noplugins           禁用所有插件
  --enableplugin [plugin] 启用指定名称的插件
  --disableplugin [plugin] 禁用指定名称的插件
  --releasever RELEASEVER 覆盖在配置文件和仓库文件中 $releasever 的值
  --setopt SETOPTS      设置任意配置和仓库选项
  --skip-broken         通过跳过软件包来解决依赖问题
  -h, --help, --help-cmd 显示命令帮助
  --allowerasing        允许解决依赖关系时删除已安装软件包
  -b, --best            在事务中尝试最佳软件包版本。
  --nobest              不用把事务限制在最佳选择
  -C, --cacheonly       完全从系统缓存运行，不升级缓存
  -R [minutes], --randomwait [minutes] 最大命令等待时间
  -d [debug level], --debuglevel [debug level] 调试输出级别
  --debugsolver         转储详细解决结果至文件
  --showduplicates      在 list/search 命令下，显示仓库里重复的条目
  -e ERRORLEVEL, --errorlevel ERRORLEVEL 错误输出级别
  --obsoletes           对升级启用 dnf 的过期处理逻辑，或对 info、list 和 repoquery 显示软件包过期的功能
  --rpmverbosity [debug level name] rpm调试输出等级
  -y, --assumeyes       全部问题自动应答为是
  --assumeno            全部问题自动应答为否
  --enablerepo [repo]   启用其他存储库。列出选项。支持 glob，可以多次指定。
  --disablerepo [repo]  禁用存储库。列出选项。支持 glob，可以多次指定。
  --repo [repo], --repoid [repo] 启用指定 id 或 glob 的仓库，可以指定多次
  --enable              使用 config-manager 命令启用 repos (自动保存)
  --disable             使用 config-manager 命令禁用 repos (自动保存)
  -x [package], --exclude [package], --excludepkgs [package] 用全名或通配符排除软件包
  --disableexcludes [repo], --disableexcludepkgs [repo] 禁用 excludepkgs
  --repofrompath [repo,path] 要使用的附加存储库的标签和路径（与 baseurl 中相同的路径），可以多次指定。
  --noautoremove        禁用删除不再被使用的依赖软件包
  --nogpgcheck          禁用 gpg 签名检查 (如果 RPM 策略允许)
  --color COLOR         配置是否使用颜色
  --refresh             在运行命令之前将元数据标记为过期。
  -4                    仅解析 IPv4 地址
  -6                    仅解析 IPv6 地址
  --destdir DESTDIR, --downloaddir DESTDIR 设置软件包要复制到的目录
  --downloadonly        仅下载软件包
  --comment COMMENT     为事务添加一个注释
  --bugfix              在更新中包括与 bug 修复有关的软件包
  --enhancement         在更新中包括与功能增强有关的软件包。
  --newpackage          在更新中包括与新软件包有关的软件包
  --security            在更新中包括与安全有关的软件包
  --advisory ADVISORY, --advisories ADVISORY 在更新中包括修复指定公告所必须的软件包
  --bz BUGZILLA, --bzs BUGZILLA 在更新中包括修复给定 BZ 所必须的软件包
  --cve CVES, --cves CVES 在更新中包括修复给定 CVE 所必须的软件包
  --sec-severity {Critical,Important,Moderate,Low}, --secseverity {Critical,Important,Moderate,Low} 在更新中包括匹配给定安全等级的安全相关的软件包
  --forcearch ARCH      强制使用一个架构
```

## 4. 示例说明

### 4.1 查看命令

#### 查看 `dnf` 包管理器的版本

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf --version
4.2.23
  已安装： dnf-0:4.2.23-4.el8.noarch 在 2021年01月08日 星期五 07时08分18秒
  构建    ：CentOS Buildsys <bugs@centos.org> 在 2020年08月04日 星期二 18时52分03秒

  已安装： rpm-0:4.14.3-4.el8.x86_64 在 2021年01月08日 星期五 07时07分57秒
  构建    ：CentOS Buildsys <bugs@centos.org> 在 2020年07月21日 星期二 17时36分08秒
```

#### 查看可用的dnf软件库

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf repolist
```

#### 查看可用和不可用的dnf软件库

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf repolist all
```
#### `dnf search` 软件包名称

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf search wget
```

#### 列出已安装和可安装软件包列表 `@`开头的为已安装状态

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf list
```

#### 列出已安装软件包列表

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf list installed
```

#### 列出可安装软件包列表

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf list available
```

#### 查看软件包详情

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf info wget
Repository epel is listed more than once in the configuration
上次元数据过期检查：0:44:48 前，执行于 2021年11月22日 星期一 22时50分11秒。
已安装的软件包
名称         : wget
版本         : 1.19.5
发布         : 10.el8
架构         : x86_64
大小         : 2.8 M
源           : wget-1.19.5-10.el8.src.rpm
仓库         : @System
来自仓库     : AppStream
概况         : A utility for retrieving files using the HTTP or FTP protocols
URL          : http://www.gnu.org/software/wget/
协议         : GPLv3+
描述         : GNU Wget is a file retrieval utility which can use either the HTTP or
             : FTP protocols. Wget features include the ability to work in the
             : background while you are logged out, recursive retrieval of
             : directories, file name wildcard matching, remote file timestamp
             : storage and comparison, use of Rest with FTP servers and Range with
             : HTTP servers to retrieve files over slow or unstable connections,
             : support for Proxy servers, and configurability.
```

#### 查看可更新软件包列表

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf check-update
```

#### 查看dnf执行历史

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf history
```

### 安装命令

#### `-y` 安装时全部选是 `-q` 不显示安装过程

```
# dnf install -y 软件包名称
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf install -y wget
```

#### 重新安装特定软件包

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf reinstall wget
```

### 升级命令

#### 升级所有软件包

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf update
或
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf upgrade
```

#### 升级指定软件包

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf update -y wget
```

#### 升级所有软件包至最新稳定发行版

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf distro-sync
```

#### 回滚软件包版本

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf downgrade wget
```


### 卸载命令

#### 卸载指定软件包

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf remove -y wget
```

#### 卸载无用孤立的软件包

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf autoremove
```

#### 清理缓存的无用软件包

```
[root@iZbp1d8rn0652ia3bzzmioZ ~]# dnf clean all
```
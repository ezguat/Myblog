# Linux自动化执行任务
&ensp;&ensp;有的时候我们会需要一个自动化运行的命令，比如定时启动抓包、爬虫软件等等，亦或是返回每时每刻的数据。
这些时候我们都需要一个能够自动地、并且稳定执行的命令。<br>

&ensp;&ensp;在这里我们推荐的时候是Linux自带的一个软件 cron 有了它就能够自动化执行命令 <br>

## 1.首先查看一下cron运行情况，一般情况下是默认运行的

```scss
/etc/init.d/cron status
```
## 2.确定cron运行正常后，进入编辑文件编辑自动化命令(默认使用的是Ubuntu)
```scss
vim /etc/crontab
```
## 3.打开文件后，一般都会有官方的介绍，比如下面的是ubuntu下cron配置文件，前面的*号都是代表的分钟/小时/日期/星期几，然后后面紧跟着的是执行命令的用户，最后就是需要执行的是什么命令
```scss
m h dom mon dow user commandd
*/1 * * * * test namp -sP 192.168.1.100-120

"*" #代表整个时间段
"/" #为特别单位，表示为“每”如“0/15”表示每隔15分钟执行一次,“0”表示为从“0”分开始, “3/20”表示表示每隔20分钟执行一次，“3”表示从第3分钟开始执行

http://manpages.ubuntu.com/manpages/precise/man5/crontab.5.html (这是Ubuntu官方的cron详细配置)
```
## 4.配置所需要的命令后，要对crontab重启命令，就可以自动执行了

```scss
/etc/init.d/cron restart
```

# fetch_sth.exe 使用说明

在 `GNSS` 数据处理和应用中，经常要下载如广播星历、精密星历、精密钟差等各类文件。
目前，已有许多 `GNSS` 数据下载工具。比较常用的有 `GAMIT` 的 `sh_get_orbit` 和
 `sh_get_nav` 脚本、`RTKLIB` 的 `rtkget` 工具、课题组常用的的点下载工具。
 但这些工具有如下缺点
1. `sh_get_orbit` 和 `sh_get_nav` 只能运行于 `Linux` 环境；
2. `rtkget` 只能运行于 `Windows` 环境，且必须手动操作，无法自动下载，无法定时任务下载；
3. 点下载工具只能运行于 `Windows` 环境，且必须手动操作，依赖迅雷等下载工具，无法定时任务下载；

`fetch_sth.exe` 是一个星历下载工具，有如下功能
1.	跨平台，支持 `Windows`, `Linux`, `Mac`等多平台下载
2.	可从不同分析中心(`cddis`, `whu`, `garner`)下载广播星历、精密星历（`IGS`、`IGU`、`IGR`）
3.	可批量下载各种星历
4.	可定时根据下载时刻下载指定日期或当天星历
5.	支持一些表文件下载，如太阳表、月亮表、ATX天线文件等

### 工具介绍
*用法*

        fetch_sth --task_name=igu|igr|igs|brdc [--year=YEAR] [--doy=DOY] 
                    [--session=00|06|12|18] [--ftp=cddis|whu|garner]
                    [--outdir=DIR] 
*选项与参数*
- --task_name 
  
  需下载的任务，可选参数为 `igu`, `igr`, `igs`, `brdc`, 该命令也支持表文件下载，但尚未调试，包括 `clk`, `clk_30s`, `svs_exclude.dat`, `pmu.bull_f`, `pmu.bull_a`, `rcvant.dat`, `antmod.dat`, `ut1.usno`, `pole.usno`, `leap.sec`, `soltab`, `luntab`, `nutabl`, `igs14.atx` 均可做为任务参数。

- --year
  
  下载任务需提供年份

- --doy
  
  下载任务需提供年积日

- --session
  
  下载IGU时，需指定下载时段。若不指定，默认为0时时段

- --ftp
  
  从哪个数据中心下载，支持 `whu`, `cddis`, `garner`. 表文件只能从 `garner` 下载。
- --outdir
  
  下载的文件保存在哪里。默认为当前目录
## 用法示例
可使用的任务名有如下几个
1.	下载2019年100天的广播星历
```
    fetch_sth.exe --task_name=brdc --year=2019 --doy=100
```
2.	下载2019年100天的6点时段的超快速星历
```
    fetch_sth.exe --task_name=igu --year=2019 --doy=100 --session=06
```
3.	下载2019年100天的快速星历至E:\path目录
```
    fetch_sth.exe --task_name=igr --year=2019 --doy=100 --outdir=E:\path
```
4.	从CDDIS数据中心下载2019年100天的最终精密星历
```
    fetch_sth.exe --task_name=igs --year=2019 --doy=100 --outdir=E:\path --ftp=cddis
```
5. 下载 30s 精密钟差文件
```
    fetch_sth.exe --task_name=clk_30s --year=2019 --doy=100
```
5.	每天北京时间10点下载前一天18时的超快星历，并调用gzip.exe自动解压
编辑bat脚本，写入下列内容，并将脚本添加到Windows定时任务。

```
@echo off
set BIN_DIR=E:\scripts\
set DATA_POOL=E:\testdata
set idir=%DATA_POOL%\igs
set unzip=%BIN_DIR%\gzip.exe
set FTP=whu

%BIN_DIR%\fetch_sth.exe --task_name=igu --session=18 --ftp=%FTP% --outdir=%idir%
```

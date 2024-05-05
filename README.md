# 特别声明：
*该页面的逻辑及思路来自作者[houzisbw](https://github.com/houzisbw)的vue选座项目,github地址[点击](https://github.com/houzisbw/MeiTuanCinemaSmartChoose)。本人只针对uni-app做了样式及逻辑适配。感谢原作者[houzisbw](https://github.com/houzisbw), 如有侵权, 请举报。*


## 使用说明

* 1.下载`示例工程`，解压
* 2.复制static目录下的`selected.png` 、`bought.png`、`selected.png`三个图标文件到你自己项目的static目录下
* 3.复制pages/seat目录下的`seat.vue`到你自己的项目中作为选座页。
* 4.`pages.json`增加选座页并设置条件编译关闭H5的导航栏。
* 5.采用你自己的真实座位数据，删除`import { seatData } from '@/utils/seat-data.js'`假数据的引入

    //假数据说明：SeatCode座位编号，RowNum-行号，ColumnNum-纵号，YCoord-Y坐标，XCoord-X坐标，Status-状态。采用真实数据的话自行修改相关字段。

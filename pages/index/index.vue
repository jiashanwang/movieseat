<template>
	<view class="w-100">
		<view class="bg-f1 h-100vh">
			<!-- 	<view class="pt-f left-0 w-100 p-0-32 bg-white z1000" :style="'height: 132rpx;top:0'">
				<view>
					<view class="fz-34 fw-b pt-20">
						{{show_name}}
					</view>
					<view class="mt-10 fz-28 color-666">
						{{show_time}} {{show_version}}
					</view>
				</view>
			</view> -->
			<view style="padding-top: 40rpx; box-sizing:  border-box;">
				<!-- <view class="dp-f jc-c ai-c mb-20 fz-28" v-if="showTis"> -->
				<view class="dp-f jc-c ai-c mb-20 fz-28">
					<!-- <view class="dp-f jc-c ai-c m-0-10">
						<image :style="'width:'+seatSize+'px;height:'+seatSize+'px'" src="/static/images/unselected.png"
							mode="aspectFit"></image><span class="ml-10">可选</span>
					</view> -->
					<view class="dp-f jc-c ai-c m-0-10">
						<image :style="'width:'+seatSize+'px;height:'+seatSize+'px'" src="../../static/imgs/bought.png"
							mode="aspectFit"></image><span class="ml-10">不可选</span>
					</view>
					<view class="dp-f jc-c ai-c m-0-10">
						<image :style="'width:'+seatSize+'px;height:'+seatSize+'px'" src="../../static/imgs/selected.png"
							mode="aspectFit"></image><span class="ml-10">选中</span>
					</view>
				</view>

				<view class="dp-f jc-c ai-c mb-20 fz-28 youxuan">
					<view v-for="(item,index) in priceGrouping" :key="index" class="dp-f jc-c ai-c m-0-10">
						<image :style="'width:'+seatSize+'px;height:'+seatSize+'px'" :src="item.icon" mode="aspectFit">
						</image><span class="ml-10">{{item.value}}</span>
					</view>
				</view>

			</view>

			<movable-area :style="'height:'+(seatRow*40+350)+'rpx;width: 100vw;top:'+(rpxNum*132)+'px;margin-top: 8vw;'"
				class="pt-f left-0">
				<movable-view :style="'width: 104vw;height:'+(seatRow*40+350)+'rpx;'" :inertia="true" :scale="true"
					:scale-min="0.95" :scale-max="2" direction="all" @change="onMove" @scale="onScale">
					<view class="Stage dp-f jc-c ai-c fz-22 color-333">
						<image src="https://images.laiyipiao.fun/userfiles/fileupload/202311/1730131189419610112.png"
							mode="widthFix"></image>
						<p>{{screens}}</p>
						<!-- <p>3号临境音厅</p> -->
					</view>
					<!-- <view style="width: 100rpx;height: 30rpx;" class="m-0-a mt-48 dp-f jc-c ai-c fz-20 color-999 b-1 br-5"></view> -->
					<view class="pt-f pa-v-2 b-d-1"
						:style="'height:'+seatRow*(20+seatSize*pxNum)+'rpx;top:165rpx;width:0'"></view>
					<view v-for="(item,index) in seatArray" :key="index" class="dp-f jc-c mt-20"
						:style="'width:'+boxWidth+'px;height:'+seatSize+'px'">
						<view v-for="(seat,col) in item" :key="col" class="dp-ib"
							:style="'width:'+seatSize+'px;height:'+seatSize+'px'" @click="handleChooseSeat(index,col)">

							<!-- <image v-if="seat.type===0" class="w-100 h-100" :src="../../static/imgs/unselected.png"
								mode="aspectFit"></image> -->
							<!-- 原来逻辑begin -->
							<image v-if="seat.type===0" class="w-100 h-100" :src="seat.icon" mode="aspectFit"></image>
							<image v-else-if="seat.type===1  " class="w-100 h-100"
								src="../../static/imgs/selected.png" mode="aspectFit">

							</image>
							<image v-else-if="seat.type===2" class="w-100 h-100" src="../../static/imgs/bought.png"
								mode="aspectFit"></image>	
							<!-- 原来逻辑end -->
						</view>
					</view>
					<!-- mArr -->
					<view class="pt-f bg-line br-15 over-h"
						:style="'left: '+(10-moveX/scale)+'px;top:109rpx;width:30rpx;'">
						<view class="w-100 dp-f ai-c jc-c fz-22 color-fff"
							:style="'height:'+seatSize+'px;margin-top:20rpx;'" v-for="(m,mindex) in mArr" :key="mindex">
							{{m}}
						</view>
						<view :style="'height: 20rpx;'"></view>
					</view>
				</movable-view>
			</movable-area>



			<view class="pt-f bottom-bar left-0 dp-f fd-cr z1000">

				<view class="bg-white p-all-32">
					<view class=" mb-20">
						<view>
							<view class="fz-34 fw-b pt-20">
								{{show_name}}
							</view>
							<view class="mt-10 fz-28 color-666">
								{{time_type}} 	
							</view>
						</view>
					</view>
					<!-- <view class="dp-f ai-c jc-c fz-28 color-333 mb-20" v-if="SelectNum===0"> -->
					<!-- 推荐选座 -->
					<!-- <view style="width: 106rpx;height: 60rpx;" class="b-1 br-5 dp-f ai-c jc-c fz-28 ml-20" v-for="(n,numindex) in 4"
						 :key="n" @click="smartChoose(numindex+1)">
							{{numindex+1}}人 {{optItem.RowNum+'排'+optItem.ColumnNum+'座'}}
						</view> -->
					<!-- </view> -->
					<view class="dp-f ai-c fw-w fz-28 color-333 mb-20" v-if="SelectNum>0">
						<text>已选</text>
						<view class="p-all-10 b-1 br-5 dp-f ai-c jc-c fz-28 ml-20" v-for="(optItem,optindex) in optArr"
							:key="optindex">
							<!-- {{optItem.RowNum+'排'+optItem.ColumnNum+'座'}} -->
							{{optItem.name}}
					<!-- 		<span style="font-size: 22rpx;color: red;" v-if="userInfoCard.centerNumberType">
								￥{{spunYuan(optItem.discountPriceXt)}}</span>
							<span style="font-size: 22rpx;color: red;" v-else>
								￥{{spunYuan(optItem.practicalPriceXt)}}</span> -->
						</view>
					</view>
					<!-- <view style="width: 686rpx;height: 90rpx;" :class="noClick == true?'bg-red-1':'bg-unbtn'"
						class="dp-f jc-c ai-c br-10 fz-34 color-fff btns" @click="noMultipleClicks(buySeat)"> -->
					<view style="width: 686rpx;height: 90rpx;" 
						class="dp-f jc-c ai-c br-10 fz-34 color-fff btns" @click="noMultipleClicks(buySeat)">
						<!-- {{SelectNum>0?('￥ '+aPrice+' 确认座位'):'请选座位'}} -->
						<!-- {{SelectNum>0?('确认座位'):'请选座位'}} -->
						{{SelectNum>0?('￥ '+aPrice+'  确认座位'):'请选座位'}}
					</view>

				</view>
				<!-- 		<view class="dp-f jc-c ai-c mb-20 fz-28" v-if="showTis">
					<view class="dp-f jc-c ai-c m-0-10">
						<image :style="'width:'+seatSize+'px;height:'+seatSize+'px'" src="/static/unselected.png" mode="aspectFit"></image><span
						 class="ml-10">可选</span>
					</view>
					<view class="dp-f jc-c ai-c m-0-10">
						<image :style="'width:'+seatSize+'px;height:'+seatSize+'px'" src="/static/bought.png" mode="aspectFit"></image><span
						 class="ml-10">不可选</span>
					</view>
					<view class="dp-f jc-c ai-c m-0-10">
						<image :style="'width:'+seatSize+'px;height:'+seatSize+'px'" src="/static/selected.png" mode="aspectFit"></image><span
						 class="ml-10">选中</span>
					</view>
				</view> -->


			</view>
		</view>
	</view>
</template>
<script>
	export default {
		// 页面配置
		  navigationBarTitleText: '自定义导航栏',
		  navigationStyle: 'custom',
		data() {
			return {
				priceGrouping: [], //分组

				noClick: true, //重复提交事件
				zyflag: false, //是否存在情侣座
				scaleMin: 1, //h5端为解决1无法缩小问题，设为0.95
				boxWidth: 400, //屏幕宽度px
				space: ' ', //空格
				seatArray: [], //影院座位的二维数组,-1为非座位，0为未购座位，1为已选座位(绿色),2为已购座位(红色),一维行，二维列
				seatRow: 0, //影院座位行数
				seatCol: 0, //影院座位列数
				seatSize: 0, //座位尺寸
				SelectNum: 0, //选择座位数
				moveX: 0, //水平移动偏移量
				scale: 1, //放大倍数
				minRow: 0, //从第几行开始排座位
				minCol: 0, //从第几列开始排座位
				showTis: true, //显示选座提示
				seatList: [], //接口获取的原始位置
				meList: [], //自定義值
				mArr: [], //排数提示
				optArr: [], //选中的座位数组。
				isWXAPP: false,
				cinemaSchedules_id: "",
				tmplIds: "", //模板ID
				show_name: '',
				show_version: '',
				show_time: '',
				time_type:"",
				screens: '',
				//座位限制
				seatCount: 4,
				seatCountInit: 4,
				ids: '',
				aPrice: 0.0,
				wxSamllConfig: {},
				userInfo: {},
				userInfoCard: {},
				brandName:"",// 品牌影城
				openid:"",// 用户openid;
				cinemaId:"",// 电影院ID
				movieId:"",// 电影ID
				detailImg:"",// 电影海报
			};
		},
		computed: {
			// aPrice() {
			// 	return this.SelectNum * 36
			// },
			rpxNum() {
				return this.boxWidth / 750
			},
			pxNum() {
				return 750 / this.boxWidth
			},
		},
		onLoad(options) {
			this.cinemaSchedules_id = options.schedulingid;
			this.show_name = options.name;
			this.time_type = options.timeType;
			this.screens = options.hallName;
			this.brandName = options.cinemaName;
			this.openid = options.openid;
			this.cinemaId = options.cinemaId;
			this.movieId = options.movieId;
			this.detailImg = options.detailImg;
			
			// this.cinemaSchedules_id = "48771A4540EA6AF9685112173F8EBAB5"
			// this.path = options.path;
			uni.showLoading({
				title: "加载中..."
			})
			//获取座位
			this.getseatSelec();
		},
		onShow() {
		},
		methods: {
			/**
			 * 分转元
			 * @param {Object} val
			 */
			spunYuan(val) {
				if (val > 0) {
					//金额转换 分->元 保留2位小数 并每隔3位用逗号分开 1,234.56
					var str = (val / 100).toFixed(2) + '';
					var intSum = str.substring(0, str.indexOf(".")).replace(/\B(?=(?:\d{3})+$)/g, ','); //取到整数部分
					var dot = str.substring(str.length, str.indexOf(".")) //取到小数部分搜索
					var ret = intSum + dot;
					return ret;
				}
				return val;
			},
			//获取 yyyy-mm-dd
			getDateStr() {
				var nowDate = new Date();
				var year = nowDate.getFullYear();
				var month = nowDate.getMonth() + 1 < 10 ? "0" + (nowDate.getMonth() + 1) :
					nowDate.getMonth() + 1;
				var day = nowDate.getDate() < 10 ? "0" + nowDate.getDate() : nowDate
					.getDate();
				var dateStr = year + "-" + month + "-" + day;
				return dateStr;
			},
			//
			getStrTime(date) {
				date = date.substring(0, 19);
				date = date.replace(/-/g, '/'); //必须把日期'-'转为'/'
				var timestamp = new Date(date).getTime();
				return timestamp;
			},
			/**
			 * 匹配
			 */
			matching(obj, key) {
				let icon = "../../static/imgs/unselected1.png";
				for (let i = 0; i < this.priceGrouping.length; i++) {
					if (this.priceGrouping[i].key == key) {
						icon = this.priceGrouping[i].icon;
					}
				}
				if (this.priceGrouping.length == 1) {
					//0：普通座位，1：情侣首座(左座)，2：情侣第二座(右座)
					if (obj.flag == 1) {
						icon = "../../static/images/z.png";
						this.zyflag = true;
					}
					if (obj.flag == 2) {
						icon = "../../static/images/y.png";
						this.zyflag = true;
					}
				}
				return icon;
			},
			
			/**
			 * 获取座位
			 */
			getseatSelec() {
				// 获取电影院
				let that = this;
				uni.request({
					url: 'https://www.chishunpay.com/leka/movies/findSeatList',
					// url: 'http://127.0.0.1:4000/leka/movies/findSeatList',
					data: {
						schedulingId:that.cinemaSchedules_id,
					},
					method: "POST",
					success: (resp) => {
						uni.hideLoading();
						let result = resp.data;
						if (result.code == 200) {
							let res = result.data;
							let arrList = res.data.cinemaSeatList;
							if (arrList <= 0) {
								uni.showToast({
									title: '该电影暂无位置可选,请选择其他场次',
									icon: 'none',
									duration: 1500,
								})
								return
							}
							// let priceGrouping = res.data.totalPriceList;
							let priceGrouping = res.data.totalPriceList.sort((a, b) => a - b);
							this.priceGrouping = [];
							//循环遍历
							for (var i=0;i<priceGrouping.length;i++) {
								let obj = {
									key: priceGrouping[i],
									value: this.spunYuan(priceGrouping[i]) + "元",
									icon: "../../static/imgs/selected" + i + ".png"
								}
								this.priceGrouping.push(obj);
							}
							console.log('priceGrouping==')
							console.log(this.priceGrouping)
							let list = [];
							for (let i = 0; i < arrList.length; i++) {
								let pobj = arrList[i];
								let obj = {
									"icon": this.matching(pobj, pobj.totalPrice),
									"YCoord": pobj.row,
									"XCoord": pobj.column,
									"SeatCode": pobj.ext_id,
									"Status": pobj.status == 0 ? 0 : 1,  // 座位状态:1：可售,0：不可售
									"exId": pobj.ext_id,
									"flag": pobj.flag,
									"RowNum": pobj.row,
									"ColumnNum": pobj.column,
									"type": -1,
									'practicalPriceXt': pobj.practicalPriceXt,
									'discountPriceXt': pobj.discountPriceXt,
									'price': pobj.totalPrice,
									'name': pobj.name,
								};
								// console.log(obj);
								list.push(obj);
							}
							//情侣座
							if (this.zyflag) {
								//
								this.priceGrouping.push({
									key: "",
									value: "情侣座左",
									icon: "/static/images/z.png"
								});
								//
								this.priceGrouping.push({
									key: "",
									value: "情侣座右",
									icon: "/static/images/y.png"
								});
							}
							
							this.meList = list;
							let arr = list;
							let row = 0
							let col = 0
							let minCol = parseInt(arr[0].XCoord)
							let minRow = parseInt(arr[0].YCoord)
							for (let i of arr) {
								minRow = parseInt(i.YCoord) < minRow ? parseInt(i.YCoord) : minRow
								minCol = parseInt(i.XCoord) < minCol ? parseInt(i.XCoord) : minCol
								row = parseInt(i.YCoord) > row ? parseInt(i.YCoord) : row
								col = parseInt(i.XCoord) > col ? parseInt(i.XCoord) : col
							}
							this.seatList = arr
							this.seatRow = row - minRow + 1
							this.seatCol = col - minCol + 3
							this.minRow = minRow
							this.minCol = minCol - 1
							this.initSeatArray()
						};
					},
					fail: (err) => {},
					complete: () => {
						uni.hideLoading();
					}
				});
			},

			initData: function() {
				// let arr = seatData
				console.log(arr)
				//假数据说明：SeatCode座位编号，RowNum-行号，ColumnNum-纵号，YCoord-Y坐标，XCoord-X坐标，Status-状态
				let row = 0
				let col = 0
				let minCol = parseInt(arr[0].XCoord)
				let minRow = parseInt(arr[0].YCoord)
				for (let i of arr) {
					minRow = parseInt(i.YCoord) < minRow ? parseInt(i.YCoord) : minRow
					minCol = parseInt(i.XCoord) < minCol ? parseInt(i.XCoord) : minCol
					row = parseInt(i.YCoord) > row ? parseInt(i.YCoord) : row
					col = parseInt(i.XCoord) > col ? parseInt(i.XCoord) : col
				}
				// this.seatList = arr

				this.seatRow = row - minRow + 1
				console.log(this.seatRow)
				console.log(row)
				this.seatCol = col - minCol + 3
				this.minRow = minRow
				this.minCol = minCol - 1
				this.initSeatArray()
			},
			groupBy: function(arr, name) {
				const groups = {}
				arr.forEach(function(f) {
					const group = JSON.stringify(f[name])
					groups[group] = groups[group] || []
					groups[group].push(f)
				})
				return Object.keys(groups).map(function(group) {
					return groups[group]
				})
			},
			//初始座位数组
			initSeatArray: function() {
				// const newArr = this.groupBy(this.meList, 'RowNum');
				// console.log(newArr);
				// 0 [] 1 [] 2 []
				let seatArray = Array(this.seatRow).fill(0).map(() => Array(this.seatCol).fill({
					type: -1,
					SeatCode: '',
					RowNum: '',
					ColumnNum: '',
					price: '',
					flag: '',
				}));
				this.seatArray = seatArray
				this.seatSize = this.boxWidth > 0 ?
					parseInt(parseInt(this.boxWidth, 10) / (this.seatCol + 1), 10) :
					parseInt(parseInt(414, 10) / (this.seatCol + 1), 10)
				this.initNonSeatPlace();
			},
			//初始化是座位的地方
			initNonSeatPlace: function() {
				let seat = this.seatList.slice()
				let arr = this.seatArray.slice()
				// console.log(seat,'----------------------------------------------')
				for (let num in seat) {
					// let status = 2 //-1为非座位，0为未购座位，1为已选座位(绿色),2为已购座位(红色)
					// if (seat[num].Status === 0) {
					// 	status = 0
					// } else if (seat[num].Status === -1) {
					// 	status = -1
					// }
					let status = 2 //-1为非座位，0不可选购，1为可选购,
					if (seat[num].Status === 0) {
						status = 2
					}else if (seat[num].Status === 1){
						status = 0
					} else if (seat[num].Status === -1) {
						status = -1
					}
					// arr[parseInt(seat[num].column) - this.minRow][parseInt(seat[num].row) - this.minCol] = {
					arr[parseInt(seat[num].YCoord) - this.minRow][parseInt(seat[num].XCoord) - this.minCol] = {
						type: status,
						SeatCode: seat[num].SeatCode,
						RowNum: seat[num].RowNum,
						ColumnNum: seat[num].ColumnNum,
						price: seat[num].price,
						practicalPriceXt: seat[num].practicalPriceXt,
						discountPriceXt: seat[num].discountPriceXt,
						flag: seat[num].flag,
						icon: seat[num].icon,
						"name": seat[num].name
					}
					console.log("arr==")
					console.log(arr)
				}
				this.seatArray = arr.slice();

				//----------------------------------------------------
				let mArr = []
				let count = 0;
				for (let i in arr) {
					count++;
					let m = ''
					for (let n of arr[i]) {
						if (n.SeatCode) {
							m = count;
							break;
						}
					}
					if (m) {
						mArr.push(m)
					} else {
						mArr.push('')
					}
				}
				this.mArr = mArr
			},
			//放大缩小事件
			onScale: function(e) {
				this.showTis = false
				// this.moveX=-e.detail.x
				let w = this.boxWidth * 0.5
				let s = 1 - e.detail.scale
				this.moveX = w * s
				this.scale = e.detail.scale
				if (s > 0 || s === 0) {
					this.showTis = true
				}
			},
			//移动事件
			onMove: function(e) {
				// console.log(e)
				this.showTis = false
				this.moveX = e.detail.x
			},
			//重置座位
			resetSeat: function() {
				this.SelectNum = 0
				this.optArr = []
				//将所有已选座位的值变为0
				let oldArray = this.seatArray.slice();
				for (let i = 0; i < this.seatRow; i++) {
					for (let j = 0; j < this.seatCol; j++) {
						if (oldArray[i][j].type === 1) {
							oldArray[i][j].type = 0
						}
					}
				}
				this.seatArray = oldArray;
			},
			//选定且购买座位
			buySeat: function() {
				let that = this;
				if (this.SelectNum === 0) {
					return
				}
				let oldArray = [];
				for (let i = 0; i < this.seatRow; i++) {
					for (let j = 0; j < this.seatCol; j++) {
						if (this.seatArray[i][j].type === 1) {
							oldArray.push(this.seatArray[i][j].SeatCode)
						}
					}
				}

				let grOut = this.groupBy(that.optArr, "price");
				let kCount = 0;
				for (let key in grOut) {
					kCount++;
				}
				if (kCount > 1) {
					uni.showToast({
						title: '不能同时选择多个价格座位',
						icon: 'none',
						duration: 1000
					});
					// that.noClick = true;
					return
				}
				uni.showModal({
					title: '提示',
					content: '确认已选座位么?',
					success: function(res) {
						if (res.confirm) {
							that.saveFilmTicket()
						} else if (res.cancel) {
							//取消
							// that.noClick = true;
						}
					}
				});

			},
			/**
			 * 按钮提交事件
			 * @param {Object} methods
			 * @param {Object} info
			 */
			noMultipleClicks(methods, info) {
				// methods是点击后需要执行的函数， info是函数需要传的参数
				let that = this;
				if (that.optArr.length <= 0) {
					uni.showToast({
						title: '请选择座位',
						icon: 'none',
						duration: 1000
					});
					return;
				}
				methods();

				// if (that.noClick) {
					// 第一次点击
					// that.noClick = false;
					// if ((info && info !== '') || info == 0) {
					// 	// info是执行函数需要传的参数
					// 	methods(info);
					// } else {
					// 	methods();
					// }
					// setTimeout(() => {
					// 	that.noClick = true;
					// }, 1500)
				// } else {
				// 	//  这里是重复点击的判断
				// 	// console.log("重复点击");
				// }
			},
			/**
			 * 提交电影票
			 */
			saveFilmTicket() {
				let that = this
				let data = {}
				//-------------------座位计算金额
				let seatId = "";
				let bayNumber = "";
				let price = 0.0;
				let practicalPrice = 0.0;
				let discountPrice = 0.0;
				that.optArr.forEach(item => {
					seatId += item.SeatCode + ","
					// let numName = item.RowNum + '排' + item.ColumnNum + '座';
					let numName = item.name;
					bayNumber += numName + ","
					price += parseFloat(item.price);
					// practicalPrice += parseFloat(item.practicalPriceXt);
					// discountPrice += parseFloat(item.discountPriceXt);
				})
				//座位IDS
				seatId = seatId.substring(0, seatId.lastIndexOf(','));
				//座位名称IDS
				bayNumber = bayNumber.substring(0, bayNumber.lastIndexOf(','));

				//座位ID
				data.seatId = seatId;
				//座位名称
				data.bayNumber = bayNumber;
				//原价
				data.totalPrice = this.spunYuan(price);
				data.brandName = this.brandName;
				data.movieName = this.show_name;
				data.openid = this.openid;
				data.cinemaId = this.cinemaId;
				data.movieId = this.movieId;
				data.movieSchedulingId = this.cinemaSchedules_id;
				data.detailImg = this.detailImg;
				//非会员价格
				// data.integralPrice = this.spunYuan(practicalPrice);
				// //会员价
				// data.purchasePrice = this.spunYuan(discountPrice);
				uni.navigateTo({
				    url:`/pages/movieOrder/movieOrder?data=`+ encodeURIComponent(JSON.stringify(data))
				})
				// console.log("提交DATA", data);

				//存储-放入参数
				// that.$http({
				// 	url: 'saveParameter',
				// 	header: {
				// 		'token': token,
				// 		'content-type': 'application/json'
				// 	},
				// 	data: data,
				// }).then(res => {
				// 	uni.hideLoading();
				// 	console.log(res);
				// 	if (res.code == 200) {
				// 		//跳转页面
				// 		uni.navigateTo({
				// 			url: "/pages/seatSelection/orderPay?state=" + data.key
				// 		})
				// 	}
				// });

			},
			//处理座位选择逻辑
			handleChooseSeat: function(row, col) {
				//获取当前的座位
				let seat = this.seatArray[row][col];
				//获取座位类型
				let seatValue = seat.type;
				//获取所有的座位
				let newArray = this.seatArray;
				// console.log(newArray)
				//如果是已购座位，直接返回
				if (seatValue === 2 || seatValue === -1) return

				//情侣座-未选
				if (seat.flag == 1 || seat.flag == 2) {
					let col2 = col;
					let seat2 = {};
					if (seat.flag == 1) {
						col2 = col + 1;
					}
					if (seat.flag == 2) {
						col2 = col - 1;
					}
					//如果是已选座位点击后变未选 （已选择）
					if (seatValue === 0) {
						//单场次卡不能选
						if (this.seatCountInit == 1) {
							uni.showToast({
								title: '情侣座不能分开兑换，请更换场次',
								icon: 'none',
								duration: 1000
							});
							return;
						}
						//
						if (this.SelectNum > (this.seatCount - 2)) {
							uni.showToast({
								title: '可选座位数超出',
								icon: 'none',
								duration: 1000
							});
							return;
						}
					}
					//添加
					this.handleChooseSeatTwo(row, col);
					//添加
					this.handleChooseSeatTwo(row, col2);
				} else {
					//普通座位
					this.handleChooseSeatTwo(row, col);
				}
			},
			/**
			 * 处理选座逻辑
			 * @param {Object} row
			 * @param {Object} col
			 */
			handleChooseSeatTwo: function(row, col) { 
				//获取当前的座位
				let seat = this.seatArray[row][col];
				//获取座位类型
				let seatValue = seat.type;
				//获取所有的座位
				let newArray = this.seatArray;
				// console.log(newArray)
				//如果是已购座位，直接返回
				if (seatValue === 2 || seatValue === -1) return
				//如果是已选座位点击后变未选 （已选择）
				if (seatValue === 1) {
					//状态调整成未选中
					newArray[row][col].type = 0;
					//选中座位减一
					this.SelectNum--
					//处理清除
					this.getOptArr(newArray[row][col], 0);
				} else if (seatValue === 0) {
					if (this.SelectNum > (this.seatCount - 1)) {
						uni.showToast({
							title: '座位数量超出',
							icon: 'error',
							duration: 1000
						});
						return;
					}
					//状态调整成已选中
					newArray[row][col].type = 1;
					//选中座位加一
					this.SelectNum++
					//处理选中
					this.getOptArr(newArray[row][col], 1)
				}
				//必须整体更新二维数组，Vue无法检测到数组某一项更新,必须slice复制一个数组才行
				this.seatArray = newArray.slice();
			},

			/**
			 * 处理已选择
			 * @param {Object} item 座位
			 * @param {Object} type 1进行添加 0进行清除
			 */
			getOptArr: function(item, type) {
				//选中座位
				let optArr = this.optArr
				//添加工作
				if (type === 1) {
					optArr.push(item);
				} else if (type === 0) {
					let arr = []
					//清除原有的已选择的座位
					optArr.forEach(v => {
						if (v.SeatCode != item.SeatCode) {
							arr.push(v)
						}
					})
					optArr = arr
				}
				//重新赋值座位
				this.optArr = optArr.slice();
				let sumPrice = 0;
				for (let key in this.optArr) {
					//非会员折扣价
					// let price = this.optArr[key].practicalPriceXt;
					// //会员价
					// if (this.userInfoCard.centerNumberType) {
					// 	price = this.optArr[key].discountPriceXt;
					// }
					let price = this.optArr[key].price;
					// console.log(this.optArr[key]);
					sumPrice += parseInt(price);
				}
				this.aPrice = this.spunYuan(sumPrice);
			},
		}
	}
</script>
<style>
	.uni-nav-bar {
	  display: none;
	}
</style>

<style lang="scss" scoped>
	.p-all-10 {
		padding: 10rpx;
	}

	.ml-10 {
		margin-left: 10rpx;
	}

	.m-0-10 {
		margin: 0 10rpx;
	}

	.bg-unbtn {
		// background: linear-gradient(51deg, #0C75DF 0%, #02BBEF 100%);
		background: linear-gradient(51deg, #C0C0C0 0%, #C8C7CC 100%) !important;
	}

	.bg-red-1 {
		background-color: #dd2f14 important !important;
		// background: linear-gradient(51deg, #0C75DF 0%, #02BBEF 100%) !important;
	}

	.br-10 {
		border-radius: 10rpx;
	}

	.ml-20 {
		margin-left: 20rpx;
	}

	.mb-20 {
		margin-bottom: 20rpx;
	}

	.p-all-32 {
		padding: 32rpx;
	}

	.fd-cr {
		flex-direction: column-reverse;
		/* 主轴方向从下到上,默认从左到右 */
	}

	.bottom-bar {
		bottom: var(--window-bottom);
	}

	.btns {
		width: 345px;
		height: 50px;
		// background: linear-gradient(51deg, #0C75DF 0%, #02BBEF 100%);
		background-color: #dd2f14;
		border-radius: 25px;
	}

	.color-fff {
		color: #fff
	}

	.br-15 {
		border-radius: 15rpx;
	}
	.youxuan{
		margin-top:40rpx;
	}

	.over-h {
		overflow: hidden;
	}

	.dp-ib {
		display: inline-block;
	}

	.mt-20 {
		margin-top: 20rpx;
	}

	.pa-v-2 {
		/* 定位垂直对齐 */
		left: 50%;
		transform: translateX(-50%)
	}

	.b-d-1 {
		border: 1px dashed #e5e5e5;
	}

	.w-100 {
		width: 100%;
	}

	.h-100 {
		height: 100%;
	}

	.bg-f1 {
		background-color: #f1f1f1;
	}

	.h-100vh {
		// height: 100vh;
		height:80vh;
	}

	.pt-f {
		position: fixed;
	}

	.left-0 {
		left: 0;
	}

	.p-0-32 {
		padding: 0 32rpx;
	}

	.pt-20 {
		padding-top: 20rpx;
	}

	.bg-white {
		background-color: #fff;
	}

	.z1000 {
		z-index: 1000;
	}

	.fz-34 {
		font-size: 34rpx;
	}

	.fw-b {
		font-weight: bold;
	}

	.mt-10 {
		margin-top: 10rpx;
	}

	.fz-28 {
		font-size: 28rpx;
	}

	.color-666 {
		color: #666666
	}

	.dp-f {
		display: flex;
	}

	.jc-c {
		justify-content: center;
	}

	.ai-c {
		align-items: center;
	}

	.fz-22 {
		font-size: 22rpx;
	}

	.color-333 {
		color: #333333
	}

	.m-0-a {
		margin: 0 auto;
	}

	.mt-48 {
		margin-top: 48rpx;
	}

	.fz-20 {
		font-size: 20rpx;
	}

	.color-999 {
		color: #999999
	}

	.b-1 {
		border: 1px solid #CCCCCC;
	}

	.br-5 {
		border-radius: 5rpx;
	}

	.Stage {
		// background-color: #FFFFFF;
		display: flex;
		flex-direction: column;

		// width: 380rpx;
		// height: 34rpx;
		// transform: perspective(34rpx) rotateX(-10deg);
		// margin: 0 auto;
		p {
			margin-top: -20rpx;
		}

		image {}
	}

	.bg-line {
		background-color: rgba(0, 0, 0, 0.3);
	}

	.sel-seat {
		background: url('/static/selected.png') center center no-repeat;
		background-size: 100% 100%;
	}

	.unsel-seat {
		background: url('/static/unselected.png') center center no-repeat;
		background-size: 100% 100%;
	}

	.bought-seat {
		background: url('/static/bought.png') center center no-repeat;
		background-size: 100% 100%;
	}
</style>
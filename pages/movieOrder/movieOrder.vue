<template>
	<view class="orderWrap">
		<view class="seatInfo">
			<view class="name">座位信息</view>
			<view class="itemWrap">
				<view class="leftPart">影院名称：</view>
				<view class="rightPart">{{cinemaName}}</view>
			</view>
			<view class="itemWrap">
				<view class="leftPart">选购电影：</view>
				<view class="rightPart">{{movieName}}</view>
			</view>
			<view class="itemWrap">
				<view class="leftPart">选购座位：</view>
				<view class="rightPart">{{seatList}}</view>
			</view>
			<view class="itemWrap">
				<view class="leftPart">座位价格：</view>
				<view class="rightPart price">¥ {{seatTotalPrice}}</view>
			</view>
		</view>
		<view class="orderInfo">
			<view class="name">短信通知<text class="notify">出票成功后系统自动发送通知短信</text></view>
			<view class="itemWrapPhone">
				<view class="leftPartPhone">手机号码：</view>
				<view class="rightPartPhone">
					<input type="text" v-model="phone" placeholder="请输入手机号码" @input="checkPhone" confirm-type="next"
						class="phone-input" />
				</view>
			</view>
			<text v-if="phoneError" class="error-text">{{ phoneErrorMsg }}</text>
		</view>
		<view class="orderInfo">
			<view class="name">订单信息</view>
			<view class="itemWrap">
				<view class="leftPart">选购数量：</view>
				<view class="rightPart seatNum">{{seatNum}} <text class="guanying">个观影位</text></view>
			</view>
			<view class="itemWrap">
				<view class="leftPart">合计：</view>
				<view class="rightPart price">{{seatTotalPrice}} <text class="jifen">积分</text></view>
			</view>
		</view>
		<view class="orderDesc">
			<view class="name">订单信息</view>
			<view class="title">观影须知:</view>
			<view class="stepDesc"><text class="xuhao">1.</text>请提前到达影院现场，找到自助取票机，打印纸质电影票，完成取票，凭打印好的纸质电影票，检票入场观影。</view>
			<view class="stepDesc"><text class="xuhao">2.</text>这是特惠出票，下单后需3-30分钟左右出票时间，如现场自助取票机无法打印电影票，请联系影院前台工作人员。
			</view>
			<view class="stepDesc"><text class="xuhao">3.</text>无座位时，接受系统调座(所选座位被占座时，系统自动调至最佳观影座位)</view>
			<view class="stepDesc"><text class="xuhao">4.</text>电影票暂时不支持退改签，请知晓，出票相关疑问，请联系在线客服。</view>
			<view class="stepDesc"><text class="xuhao">5.</text>如果有开票需要，请保管好电影票根，尽量在观影当天联系影城工作人员进行开具。</view>
		</view>
		<view class="payBtn" @click="exchange">立即兑换</view>
	</view>
</template>

<script>
	import {
		Base64
	} from 'js-base64';
	export default {
		data() {
			return {
				cinemaName: "",
				movieName: "",
				seatList: "",
				seatTotalPrice: "",
				seatNum: 1,
				seatId: "", // 座位code
				openid: "", //用户openid
				phone: '',
				phoneError: false,
				phoneErrorMsg: '',
				cinemaId:"",// 电影院ID
				movieId:"",// 电影ID
				movieSchedulingId:"",// 电影排期ID
				detailImg:"",// 电影海报
				purchasePrice:0,//
				integralPrice:0,//
			}
		},
		onLoad: function(option) {
			if (option.data) {
				const queryData = JSON.parse(decodeURIComponent(option.data));
				this.movieSchedulingId = queryData.movieSchedulingId;
				this.cinemaName = queryData.brandName;
				this.movieName = queryData.movieName;
				this.seatId = queryData.seatId;
				this.cinemaId = queryData.cinemaId;
				this.movieId = queryData.movieId;
				this.seatTotalPrice = queryData.totalPrice;
				let seatList = queryData.bayNumber;
				this.seatList = seatList;
				this.openid = queryData.openid;
				this.detailImg = queryData.detailImg;
				// this.integralPrice = queryData.integralPrice;
				// this.purchasePrice = queryData.purchasePrice;
				if (seatList.indexOf(",") == -1) {
					this.seatNum = 1;
				} else {
					let aar = seatList.split(',');
					this.seatNum = seatList.split(',').length;
				}
			}
		},
		methods: {
			checkPhone() {
				const phoneRegex = /^1[3456789]\d{9}$/;
				if (!phoneRegex.test(this.phone)) {
					this.phoneError = true;
					this.phoneErrorMsg = '请输入正确的手机号码';
				} else {
					this.phoneError = false;
					this.phoneErrorMsg = '';
				}
			},
			// 将字符串转换为base64编码
			stringToBase64(str) {
				return btoa(unescape(encodeURIComponent(str)));
			},
			// 将base64编码转换为字符串
			base64ToString(base64) {
				return decodeURIComponent(escape(atob(base64)));
			},
			// 立即兑换
			exchange() {
				if (!this.phone.trim()){
					uni.showToast({
						title: "手机号码不能为空!",
						icon: 'none'
					});
					return ;
				}else if (this.phoneError){
					uni.showToast({
						title: "请输入正确的手机号码!",
						icon: 'none'
					});
					return ;
				};
				let that = this;
				let params = {
					openid: this.openid,
					productName: "电影票",
					buyNo: 1,
					// integralPrice: parseFloat(this.seatTotalPrice), // 总支付金额
					integralPrice: 1, // 假数据
					serviceCharge: 0, // 服务费
					remark: "",
					pid: 39,
					specsId: 144, // 无用，占位符 在product_specs 中定义的
					productType: 11, // 产品分类表
					extendData: {
						memberPhone:this.phone.trim(),// 手机号码
						movieCinemaId:this.cinemaId,// 电影院ID
						movieFilmId:this.movieId,// 电影ID
						movieSchedulingId:this.movieSchedulingId,// 电影排期ID
						seatId:this.seatId,// 座位IDS,座位ID逗号隔开
						detail_img:this.detailImg,// 商品图标
						integralPrice:52,// 
						purchasePrice:50,// 
					}, //扩展数据
					payModel: 1, // 支付方式 仅支持积分支付
					// integralAmount: parseFloat(this.seatTotalPrice), // 积分支付金额
					integralAmount: 1, // 积分支付金额  假数据
					wxAmount: 0, // 微信支付金额
					cardDealerHasCharge: 1, // 卡商是否收取服务费 1收取，0 不收取
					cardDealerServiceCharge: 0.05, // 卡商收取服务费的点数
				};
				uni.showLoading({
					title: "加载中..."
				});
				uni.request({
					url: 'https://www.chishunpay.com/leka/order/createExchangeOrder',
					// url: 'http://127.0.0.1:4000/leka/order/createExchangeOrder',
					data: {
						...params
					},
					method: "POST",
					success: (resp) => {
						uni.hideLoading();
						let result = resp.data;
						if (result.code == 200) {
							let uoid = result.data.uoid;
							let outtradeno = result.data.outtradeno;
							let baseUrl = "https://www.chishunpay.com/html/lekah5?outtradeno=" + outtradeno +
								"&path=paymentCompleted";
							let backurl = Base64.encode(baseUrl);
							let url = "https://mpay.ltsw.cn/payment/#/?uoid=" + uoid + "&backurl=" + backurl;
							window.location.href = url;
						}
					},
					fail: (err) => {},
					complete: () => {
						uni.hideLoading();
					}
				});
			},
		},
	}
</script>

<style>

</style>
<style lang="scss" scoped>
	/* App.vue 或其他页面的 <style> 标签中 */
	page {
		background-color: #f1f1f1;
		/* 设置为你想要的颜色 */
	}

	.uni-page {}

	.orderWrap {
		background-color: #f1f1f1;
	}

	.seatInfo,
	.orderInfo,
	.orderDesc {
		margin: 20rpx;
		padding: 20rpx;
		background-color: #ffffff;
		border-radius: 8rpx;
	}

	.name {
		font-size: 32rpx;
		border-bottom: 1rpx solid #eeeeee;
		padding-bottom: 10rpx;
		font-weight: bold;
		color: #545454;
	}

	.itemWrap {
		display: flex;
		font-size: 28rpx;
		color: #959595;
		margin-top: 20rpx;
	}

	.itemWrapPhone {
		display: flex;
		justify-content: flex-start;
		font-size: 30rpx;
		color: #959595;
		margin-top: 20rpx;
	}


	.orderDesc {
		font-size: 24rpx;
		color: #959595;
	}

	.orderDesc .title {
		font-size: 28rpx;
		margin-top: 10rpx;
	}

	.stepDesc {
		margin-top: 10rpx;
		line-height: 36rpx;
	}

	.leftPart {
		width: 160rpx;
		text-align: left;

	}

	.rightPart {
		flex-grow: 1;
		text-align: right;
	}

	.price {
		color: #ff0000;
	}

	.seatNum {
		color: green;
	}

	.jifen {
		font-size: 22rpx;
		margin-left: 10rpx;
	}

	.guanying {
		margin-left: 20rpx;
		color: #959595;
		font-size: 22rpx;
	}

	.xuhao {
		font-size: 32rpx;
	}

	.payBtn {
		position: fixed;
		bottom: 30rpx;
		left: 24rpx;
		right: 24rpx;
		height: 140rpx;
		text-align: center;
		color: #ffffff;
		height: 80rpx;
		line-height: 80rpx;
		background: linear-gradient(to right, #fdb208, #f97b04);
		letter-spacing: 5rpx;
		border-radius: 40rpx;
		font-size: 34rpx;
		font-size: 32rpx;
	}

	.error-text {
		color: red;
		margin-top: 5px;
		font-size:24rpx;
	}

	.rightPartPhone {
		line-height: 60rpx;
		height: 60rpx;
		border: 1rpx solid #cccccc;
		border-radius: 10rpx;
		flex-grow: 1;
		text-align: left;
	}
	.rightPartPhone input{
		height:60rpx;
		line-height: 60rpx;
		padding-left:20rpx;
		color:#cccccc;
		font-size:24rpx;
	}

	.leftPartPhone {
		width: 160rpx;
		text-align: left;
		line-height: 60rpx;
		height: 60rpx;
	}
	.notify{
		color:#eb8013;
		font-size:20rpx;
		font-weight: normal;
		padding-left:20rpx;
	}
</style>
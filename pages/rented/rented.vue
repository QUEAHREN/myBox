<template>
	<view>
		<view v-if="!hasOrders" class="noorder">
			没有正在进行的订单
			<image src="../../static/empty.png" class="empty"></image>
		</view>
		<view v-for="(item, index) in rents" class="cardbox" v-else>
			<fridge-card :fridgeId="item.Fridge_ID" :boxId="item.Box_ID"></fridge-card>
		</view>
	</view>
</template>

<script>
	let _self;
	const BASE_URL = "https://layznana.xyz/"
	// const BASE_URL = "http://1.15.231.43:8080/";
	export default {
		data() {
			return {
				hasOrders: false,
				openid: "",
				rents: []
			}
		},
		methods: {
			setStor(key, data) {
				uni.setStorage({
					key: key,
					data: data,
					success: function() {
						console.log('success');
					}
				});
			},
		},
		created() {
			_self = this;
		},
		onShow() {
			uni.getStorage({
				key: 'isLogin',
				success: function(res) {
					if (res.data != "true") {
						_self.isLogin = false;
					} else {
						_self.setStor("isLogin", "true");
						_self.isLogin = true;
						uni.request({
							url: BASE_URL + "query/box?User_ID=" + _self.username,
							success: (res) => {
								console.log(res.data)
								console.log(res.data.length)
								if (res.data.length != 0) {
									_self.hasOrders = true;
									_self.rents = res.data;
								} 
								else {
									_self.hasOrders = false;
								}
							},
							fail: () => {
								_self.hasOrders = false;
							}
						})
					}
				},
				fail: function(res) {
					_self.isLogin = false;
				}
			})
			console.log(_self.isLogin)
		},
		onLoad() {
			_self.rents = []
			uni.getStorage({
				key: 'username',
				success: function(res) {
					_self.username = res.data
				},
				fail: function(res) {
					_self.isLogin = false;
				}
			})
			uni.getStorage({
				key: 'isLogin',
				success: function(res) {
					if (res.data != "true") {
						_self.isLogin = false;
					} else {
						_self.setStor("isLogin", "true");
						_self.isLogin = true;
						uni.request({
							url: BASE_URL + "query/box?User_ID=" + _self.username,
							success: (res) => {
								console.log(res.data)
								console.log(res.data.length)
								if (res.data.length != 0) {
									_self.hasOrders = true;
									_self.rents = res.data;
								} 
								else {
									_self.hasOrders = false;
								}
							},
							fail: () => {
								_self.hasOrders = false;
							}
						})
					}
				},
				fail: function(res) {
					_self.isLogin = false;
				}
			})
		}
	}
</script>

<style>
	.noorder {
		font-size: 50rpx;
		color: #B2B2B2;
		margin-top: 50rpx;
		display: flex;
		align-items: center;
		flex-direction: column;
		justify-content: center;
	}

	.empty {
		margin-top: 40rpx;
		width: 200rpx;
		height: 200rpx;
	}

	.cardbox {
		margin-top: 20rpx;
	}
</style>

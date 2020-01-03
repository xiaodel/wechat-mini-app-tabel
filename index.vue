<template>
	<view class="statistics">

		<view class="cont" @touchstart="showBtnBox = false" @touchcancel="showBtnBox = true" @touchend="showBtnBox = true">
			<view class="table_box">
				<view v-if="dataList.length == 0 && status=='noMore'">
						暂无数据
				</view>
				<view class="table_list" v-if="dataList.length != 0">
					<scroll-view class="scroll-view" scroll-x="true">
						<view class="div-table">
							<view class="thead">
								<view class="th">
									<view class="td">租客</view>
									<view class="td">类型</view>
									<view class="td">成交日期</view>
									<view class="td">租期</view>
									<view class="td">月租金</view>
									<view class="td">房源地址</view>
								</view>
							</view>
							<view class="tbody">
								<view class="th" v-for="(item,key) in dataList" :key="key">
									<view class="td">{{item.buyerName || ''}}-{{item.phone || ''}}</view>
									<view class="td">{{item.typeName || '-'}}</view>
									<view class="td">{{item.tradeDate || '-'}}</view>
									<view class="td">{{item.rentDate || '-'}}个月</view>
									<view class="td">{{item.price || '0'}}</view>
									<view class="td">{{item.address || '-'}}</view>
								</view>
							</view>
						</view>
					</scroll-view>
				</view>
				

			</view>
		</view>
		<view :class="{'btn_box': true, 'showBtn': showBtnBox, 'removeBtnBox': !showBtnBox}">
			<button @click="addNew" class="button">新增成交记录</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showBtnBox: true,
				dataList: [],
				status: 'loading',
				hasNextPage: false,
				pageNum: 1,
				pageSize: 20,
			}
		},
		onShow(){
			if(Object.keys(this.filter).length != 0){
				this.dataList = [];
				this.pageNum = 1;
				this.initData();
			}
		},
		methods: {
			initData(){
				this.status = 'loading';
				let res = {"message":"success","code":200,"success":true,"data":{"pageNum":1,"pageSize":20,"size":5,"startRow":1,"endRow":5,"total":"5","pages":1,"list":[{"id":"1212636946376155137","buyerName":"123","phone":"14523432432","tradeDate":"2020-01-02","price":1200.0,"houseNumber":"1323","rentDate":24,"type":0,"postName":"锦江区","name":"成都远洋太古里","address":"四川省成都市锦江区中纱帽街8号","typeName":"自己登记"},{"id":"1212637372706185217","buyerName":"李白","phone":"15405154545","tradeDate":"2020-01-02","price":1200.0,"houseNumber":"1栋1单元1702","rentDate":24,"type":0,"postName":"锦江区","name":"成都远洋太古里","address":"四川省成都市锦江区中纱帽街8号","typeName":"自己登记"},{"id":"1212647513434349569","buyerName":"丽水","phone":"15520693232","tradeDate":"2020-01-02","price":1200.0,"houseNumber":"1栋2-1202","rentDate":3,"type":0,"postName":"武侯区","name":"武侯区人民政府","address":"四川省成都市武侯区武侯祠大街264号","typeName":"自己登记"},{"id":"1212648144106676226","buyerName":"2","phone":"14532324324","tradeDate":"2020-01-02","price":1222.0,"houseNumber":"123123","rentDate":24,"type":0,"postName":"武侯区","name":"武侯区人民政府","address":"四川省成都市武侯区武侯祠大街264号","typeName":"自己登记"},{"id":"1212648629077270529","buyerName":"吧发布了","phone":"15858555655","tradeDate":"2020-01-02","price":0.0,"houseNumber":"不过答案","rentDate":24,"type":0,"postName":"锦江区","name":"汇融创客广场D座","address":"四川省成都市锦江区锦华路三段88汇融创客广场","typeName":"自己登记"}],"prePage":0,"nextPage":0,"isFirstPage":true,"isLastPage":true,"hasPreviousPage":false,"hasNextPage":false,"navigatePages":8,"navigatepageNums":[1],"navigateFirstPage":1,"navigateLastPage":1,"firstPage":1,"lastPage":1}}
				this.dataList = this.dataList.concat(res.list);
				this.hasNextPage = res.hasNextPage;
				if(res.hasNextPage){
					this.status = 'more';
				}else{
					this.status = 'noMore';
				}
			},
			addNew(){
				uni.navigateTo({
					url: './add'
				})
			},
			bindFilter(data){
				this.filter = data;
				this.dataList = [];
				this.pageNum = 1;
				this.initData();
			}
		},
		onReachBottom() {
			if(this.hasNextPage){
				this.pageNum++;
				this.initData();
			}
		}

	}
</script>

<style lang="scss" scoped>
	.statistics {

		.cont{
			box-sizing: border-box;
			min-height: 100vh;
			padding-top: 100rpx;
			padding-bottom: 160rpx;
			.table_box{
				.table_list{
					.scroll-view{
						display: inline-block;
						.div-table{
							display: inline-block;
							width: 1600rpx;
							border-left: 2rpx solid #f5f5f5;
							border-top: 2rpx solid #f5f5f5;

							.thead{
								color:white;
								background: #6699FF;
								font-size: 30rpx;
								display: grid;

								.td{
									background: #6699FF;
									/*text-align: center;*/
								}
							}
							.tbody{
								font-size: 28rpx;
								display: grid;
								.th{
									&:nth-child(odd){
										background-color: #fff;
									}
									&:nth-child(even){
										background-color: #dff0d8;
									}
								}
							}
							.th{
								display: inline-grid;
								grid-template-columns: 340rpx repeat(4, 180rpx) 600rpx;
								justify-content: space-around;
								justify-items: stretch;
								grid-template-rows: auto;
								grid-auto-flow: column;
								/*grid-gap: 20rpx 20rpx;*/

								.td{
									border-right: 2rpx solid #f5f5f5;
									border-bottom: 2rpx solid #f5f5f5;
									padding: 15rpx 10rpx;
									justify-items: stretch;
									align-items: center;
									overflow: hidden;
									text-overflow: ellipsis;
									white-space: nowrap;
								}
							}
						}
					}
				}
			}
		}
		.btn_box{
			position: fixed;
			z-index: 100;
			bottom: 0;
			left:0;
			width: 100%;
			height: 150rpx;
			background: #fff;
			padding: 20rpx 30rpx;
			box-sizing: border-box;

			&.showBtn{
				transform: translateY(0rpx);
				transition: all ease .45s;
			}
			&.removeBtnBox{
				transform: translateY(150rpx);
				transition: all ease .45s;
			}

			.button{
				width: 100%;
				height: 90rpx;
				line-height: 90rpx;
				background: #ED3843;
				color: #fff;
				font-size: 32rpx;
				border-radius: 8rpx;
			}
		}
	}

</style>

<template>
	<mescroll-uni @init="mescrollInit" :up="upOption" :down="downOption" @down="downCallback" @up="upCallback">
    <view class="cu-bar bg-white search" :style="[{top:CustomBar + 'px'}]">
			<picker v-if="queryList.length>1" mode="selector" :range="queryList" range-key="queryName" :value="queryIndex" @change="queryChange" style="padding-left: 20upx;">
				<view><image style="width: 20upx;height: 33upx;" src="../../static/center/to.png"></image></view>
			</picker>

                              			<view v-if="queryIndex==0" class="search-form round">
				<text class="cuIcon-search"></text>
				<input v-model="searchForm.shangpinmingcheng" type="text" placeholder="商品名称" ></input>
			</view>
                  			<view v-if="queryIndex==1" class="search-form round">
				<text class="cuIcon-search"></text>
				<input v-model="searchForm.shangpinfenlei" type="text" placeholder="商品分类" ></input>
			</view>
                  			<view v-if="queryIndex==2" class="search-form round">
				<text class="cuIcon-search"></text>
				<input v-model="searchForm.pinpai" type="text" placeholder="品牌" ></input>
			</view>
                                                                              			<view v-if="queryIndex==3" class="search-form round">
				<text class="cuIcon-search"></text>
				<input v-model="searchForm.dianpuming" type="text" placeholder="店铺名" ></input>
			</view>
                                                                                                
			<view class="action">
				<button @tap="search" :style="{width:'auto',borderRadius:'8rpx',height:'80rpx',fontSize:'28rpx',color:'#fff',backgroundColor:btnColor[0],borderColor:btnColor[0]}" class="cu-btn shadow-blur round">搜索</button>
			</view>
		</view>

		<view :style="2 != 1 ? 'display: flex;' : ''">
                                                            <scroll-view scroll-y="true" class="tabView" :style='{"padding":"8rpx 0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0","borderColor":"rgba(0,0,0,1)","backgroundColor":"rgba(255, 255, 255, 1)","borderRadius":"0","borderWidth":"0","width":"140rpx","borderStyle":"solid","height":"100%"}'>
        <view style="display: flex;flex-direction: column;">
          <view :style='categoryName===item.shangpinfenlei?sactiveItem:sitem' class="tab" v-for="(item,index) in categoryList" :key="index" :class="categoryName===item.shangpinfenlei?'tabActive':''" @tap="categoryClick(item.shangpinfenlei)">{{item.shangpinfenlei}}</view>
        </view>
      </scroll-view>
                                                                                                                                                                                                
            <!-- 样式1 -->
            <!-- 样式2 -->
            <!-- 样式3 -->
            <view class="list-box4" :style='{"padding":"24rpx","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0","borderColor":"red","backgroundColor":"rgba(255, 255, 255, 1)","borderRadius":"20rpx","borderWidth":"0","width":"100%","borderStyle":"solid","height":"auto"}' style="display:flex;flex-wrap:wrap;box-sizing: border-box;align-content: flex-start;">
        <block v-for="(product,index) in list" :key="index">
        <view v-if="index%6==0" @tap="onDetailTap(product)" class="list-item" :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0 0 40rpx 0","borderColor":"red","backgroundColor":"rgba(6, 6, 6, 0.27)","borderRadius":"20rpx","borderWidth":"0","width":"100%","borderStyle":"solid","height":"320rpx"}'>
                                                                                                                                  <image :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0 0 8rpx 0","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","borderRadius":"20rpx","borderWidth":"0","width":"100%","borderStyle":"solid","height":"320rpx"}' mode="aspectFill" :src="product.tupian?baseUrl+product.tupian.split(',')[0]:''"></image>
                                                                                                                                                                                                                                                                                                  <view class="list-item-body" style="display: flex;flex-wrap: wrap;">
            <view :style='{"padding":"24rpx","boxShadow":"0 0 0px rgba(255,0,0,0)","margin":"-80rpx 0 0 0","borderColor":"red","backgroundColor":"rgba(0, 0, 0, 0.45)","color":"rgba(255, 255, 255, 1)","textAlign":"left","borderRadius":"0 0 20rpx 20rpx","borderWidth":"0","width":"100%","lineHeight":"36rpx","fontSize":"28rpx","borderStyle":"solid"}' class="title">{{product.shangpinmingcheng}}</view>
                                                                                                                                                                                                                                                                                                                                                                                    <view :style='{"padding":"0 24rpx","boxShadow":"0 0 0px rgba(255,0,0,0)","margin":"-60rpx 0 8rpx 0","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","color":"rgba(255, 255, 255, 1)","textAlign":"right","borderRadius":"0","borderWidth":"0","width":"100%","lineHeight":"36rpx","fontSize":"28rpx","borderStyle":"solid"}' class="price">￥{{product.price}}</view>
                                                                                  </view>
                                                                                                                                                                                                                                                                                                                                          <view style="display: flex;justify-content: space-between;">
            <text v-if="userid && isAuth('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="userid && isAuth('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
          </view>
        </view>
        <view v-if="index%6==1" @tap="onDetailTap(product)" :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0 2% 20rpx 0","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","borderRadius":"20rpx","borderWidth":"0","width":"48%","borderStyle":"solid","height":"auto"}'>
                                                                                                                                  <image :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","borderRadius":"20rpx","borderWidth":"0","width":"100%","borderStyle":"solid","height":"200rpx"}' mode="aspectFill" :src="product.tupian?baseUrl+product.tupian.split(',')[0]:''"></image>
                                                                                                                                                                                                                                                          <view style="display: flex;justify-content: space-between;">
            <text v-if="userid && isAuth('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="userid && isAuth('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
          </view>
        </view>
        <view v-if="index%6==2" @tap="onDetailTap(product)" :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0 0 20rpx 2%","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","borderRadius":"0","borderWidth":"0","width":"48%","borderStyle":"solid","height":"auto"}'>
                                                                                                                                  <image :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","borderRadius":"20rpx","borderWidth":"0","width":"100%","borderStyle":"solid","height":"200rpx"}' mode="aspectFill" :src="product.tupian?baseUrl+product.tupian.split(',')[0]:''"></image>
                                                                                                                                                                                                                                                          <view style="display: flex;justify-content: space-between;">
            <text v-if="userid && isAuth('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="userid && isAuth('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
          </view>
        </view>
        <view v-if="index%6==3" @tap="onDetailTap(product)" class="list-item" :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0 0 40rpx 0","borderColor":"red","backgroundColor":"rgba(0, 206, 209, 0)","borderRadius":"0","borderWidth":"0","width":"100%","borderStyle":"solid","height":"320rpx"}'>
                                                                                                                                  <image :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0 0 8rpx 0","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","borderRadius":"20rpx","borderWidth":"0","width":"100%","borderStyle":"solid","height":"320rpx"}' mode="aspectFill" :src="product.tupian?baseUrl+product.tupian.split(',')[0]:''"></image>
                                                                                                                                                                                                                                                                                                  <view class="list-item-body" style="display: flex;flex-wrap: wrap;">
            <view :style='{"padding":" 24rpx","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"-80rpx 0 0 ","borderColor":"red","backgroundColor":"rgba(0, 0, 0, 0.47)","color":"rgba(255, 255, 255, 1)","textAlign":"left","borderRadius":"0 0 20rpx 20rpx","borderWidth":"0","width":"100%","lineHeight":"36rpx","fontSize":"28rpx","borderStyle":"solid"}' class="title">{{product.shangpinmingcheng}}</view>
                                                                                                                                                                                                                                                                                                                                                                                    <view :style='{"padding":"0 24rpx","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"-60rpx 0 8rpx 0","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","color":"rgba(255, 255, 255, 1)","textAlign":"right","borderRadius":"0","borderWidth":"0","width":"100%","lineHeight":"36rpx","fontSize":"28rpx","borderStyle":"solid"}' class="price">￥{{product.price}}</view>
                                                                                  </view>
                                                                                                                                                                                                                                                                                                                                          <view style="display: flex;justify-content: space-between;">
            <text v-if="userid && isAuth('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="userid && isAuth('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
          </view>
        </view>
        <view v-if="index%6==4" @tap="onDetailTap(product)" :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0 2% 0 0","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","borderRadius":"0","borderWidth":"0","width":"48%","borderStyle":"solid","height":"auto"}'>
                                                                                                                                  <image :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","borderRadius":"20rpx","borderWidth":"0","width":"100%","borderStyle":"solid","height":"200rpx"}' mode="aspectFill" :src="product.tupian?baseUrl+product.tupian.split(',')[0]:''"></image>
                                                                                                                                                                                                                                                          <view style="display: flex;justify-content: space-between;">
            <text v-if="userid && isAuth('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="userid && isAuth('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
          </view>
        </view>
        <view v-if="index%6==5" @tap="onDetailTap(product)" :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0 0 0 2%","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","borderRadius":"0","borderWidth":"0","width":"48%","borderStyle":"solid","height":"auto"}'>
                                                                                                                                  <image :style='{"padding":"0","boxShadow":"0 0 12rpx rgba(255,0,0,0)","margin":"0","borderColor":"red","backgroundColor":"rgba(255,0,0,0)","borderRadius":"20rpx","borderWidth":"0","width":"100%","borderStyle":"solid","height":"200rpx"}' mode="aspectFill" :src="product.tupian?baseUrl+product.tupian.split(',')[0]:''"></image>
                                                                                                                                                                                                                                                          <view style="display: flex;justify-content: space-between;">
            <text v-if="userid && isAuth('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','修改')" class="cuIcon-edit" @click.stop="onUpdateTap(product.id)">修改</text>
            <text v-if="userid && isAuth('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
            <text v-if="!userid && isAuthFront('chongwuyongpin','删除')" class="cuIcon-delete" @click.stop="onDeleteTap(product.id)">删除</text>
          </view>
        </view>
        </block>
      </view>
            <!-- 样式4 -->
            <!-- 样式5 -->
            
                                                                                                                                                                                                                                            		</view>

		<button :style='{"padding":"0","boxShadow":"0 0 16rpx rgba(0,0,0,0) inset","margin":"0","backgroundColor":"rgba(255, 105, 18, 1)","borderColor":"rgba(255, 105, 18, 1)","borderRadius":"80rpx","color":"#fff","borderWidth":"1","width":"300rpx","fontSize":"28rpx","borderStyle":"solid","height":"80rpx"}' v-if="userid && isAuth('chongwuyongpin','新增')" class="add-btn" @click="onAddTap()">新增</button>
		<button :style='{"padding":"0","boxShadow":"0 0 16rpx rgba(0,0,0,0) inset","margin":"0","backgroundColor":"rgba(255, 105, 18, 1)","borderColor":"rgba(255, 105, 18, 1)","borderRadius":"80rpx","color":"#fff","borderWidth":"1","width":"300rpx","fontSize":"28rpx","borderStyle":"solid","height":"80rpx"}' v-if="!userid && isAuthFront('chongwuyongpin','新增')" class="add-btn" @click="onAddTap()">新增</button>

	</mescroll-uni>
</template>

<script>
	export default {
		data() {
			return {
				btnColor: ['#409eff','#67c23a','#909399','#e6a23c','#f56c6c','#356c6c','#351c6c','#f093a9','#a7c23a','#104eff','#10441f','#a21233','#503319'],
				queryList:[
					{
						queryName:"商品名称",
					},
					{
						queryName:"商品分类",
					},
					{
						queryName:"品牌",
					},
					{
						queryName:"店铺名",
					},
				],
				sactiveItem: {"padding":"0","boxShadow":"0 0 0px rgba(0,0,0,.3)","margin":"0 0 10rpx","borderColor":"rgba(0,0,0,1)","backgroundColor":"rgba(255, 99, 8, 1)","color":"#fff","borderRadius":"0","borderWidth":"0","width":"auto","lineHeight":"80rpx","fontSize":"28rpx","borderStyle":"solid"},
				sitem: {"padding":"0","boxShadow":"0 0 0px rgba(0,0,0,.3)","margin":"0 0 10rpx","borderColor":"rgba(0,0,0,1)","backgroundColor":"rgba(225, 225, 225, 1)","color":"rgba(255, 255, 255, 1)","borderRadius":"0","borderWidth":"0","width":"140rpx","lineHeight":"80rpx","fontSize":"28rpx","borderStyle":"solid"},
				queryIndex: 0,
				list: [],
                userid: '',
				mescroll: null, //mescroll实例对象
				downOption: {
					auto: false //是否在初始化后,自动执行下拉回调callback; 默认true
				},
				upOption: {
					noMoreSize: 5, //如果列表已无数据,可设置列表的总数量要大于半页才显示无更多数据;避免列表数据过少(比如只有一条数据),显示无更多数据会不好看; 默认5
					textNoMore: '~ 没有更多了 ~',
				},
				hasNext: true,
				searchForm:{},
				categoryList:[],
				categoryName :'全部',
				CustomBar: '0'
			};
		},
		computed: {
			baseUrl() {
				return this.$base.url;
			}
		},
		async onShow() {
			this.btnColor = this.btnColor.sort(()=> {
                                return (0.5-Math.random());
                        });
            let res = {};
            if(this.userid) {
                res = await this.$api.page('shangpinfenlei', {page:1,limit:100});
            } else {
                res = await this.$api.list('shangpinfenlei', {page:1,limit:100});
            }
			res.data.list.splice(0,0,{id:0,shangpinfenlei:'全部'})
			this.categoryList = res.data.list;
			this.hasNext = true
			// 重新加载数据
			if (this.mescroll) this.mescroll.resetUpScroll()
		},
		onLoad(options) {
            if(options.userid) {
                this.userid=options.userid;
            } else {
                this.userid = "";
            }
			this.hasNext = true
			// 重新加载数据
			if (this.mescroll) this.mescroll.resetUpScroll()
		},
		methods: {
			//查询条件切换
			queryChange(e) {
				this.queryIndex=e.detail.value;
				this.searchForm.shangpinmingcheng="";
				this.searchForm.shangpinfenlei="";
				this.searchForm.pinpai="";
				this.searchForm.dianpuming="";
			},
			//类别搜索
			categoryClick(categoryName){
				this.categoryName = categoryName;
				this.mescroll.resetUpScroll();
			},
			// mescroll组件初始化的回调,可获取到mescroll对象
			mescrollInit(mescroll) {
				this.mescroll = mescroll;
			},
			/*下拉刷新的回调 */
			downCallback(mescroll) {
				this.hasNext = true
				// 重置分页参数页数为1
				mescroll.resetUpScroll()
			},
			/*上拉加载的回调: mescroll携带page的参数, 其中num:当前页 从1开始, size:每页数据条数,默认10 */
			async upCallback(mescroll) {
				let params = {
					page: mescroll.num,
					limit: mescroll.size
				}

				if(this.categoryName!='全部'){
					params.shangpinfenlei = '%' + this.categoryName + '%'
				}

				if(this.searchForm.shangpinmingcheng){
					params['shangpinmingcheng'] = '%' + this.searchForm.shangpinmingcheng + '%'
				}
				if(this.searchForm.shangpinfenlei){
					params['shangpinfenlei'] = '%' + this.searchForm.shangpinfenlei + '%'
				}
				if(this.searchForm.pinpai){
					params['pinpai'] = '%' + this.searchForm.pinpai + '%'
				}
				if(this.searchForm.dianpuming){
					params['dianpuming'] = '%' + this.searchForm.dianpuming + '%'
				}



                let res = {}
                if(this.userid) {
                    res = await this.$api.page(`chongwuyongpin`, params);
                } else {
                    res = await this.$api.list(`chongwuyongpin`, params);
                }
				// 如果是第一页数据置空
				if (mescroll.num == 1) this.list = [];
				this.list = this.list.concat(res.data.list);
				if (res.data.list.length == 0) this.hasNext = false;
				mescroll.endSuccess(mescroll.size, this.hasNext);
			},
			// 详情
			onDetailTap(item) {
                uni.setStorageSync("useridTag",this.userid);
				this.$utils.jump(`./detail?id=${item.id}&userid=`+this.userid)
			},
			// 修改
			onUpdateTap(id){
                uni.setStorageSync("useridTag",this.userid);
				this.$utils.jump(`./add-or-update?id=${id}`)
			},
			// 添加
			onAddTap(){
                uni.setStorageSync("useridTag",this.userid);
				this.$utils.jump(`./add-or-update`)
			},
			onDeleteTap(id){
				var _this = this;
				uni.showModal({
					title: '提示',
					content: '是否确认删除',
					success: async function(res) {
						if (res.confirm) {
							await _this.$api.del('chongwuyongpin', JSON.stringify([id]));
							_this.hasNext = true
							// 重置分页参数页数为1
							_this.mescroll.resetUpScroll()
						}
					}
				});
			},
			// 搜索
			async search(){
				this.mescroll.num = 1
				let searchForm = {
					page: this.mescroll.num,
					limit: this.mescroll.size
				}
				if(this.searchForm.shangpinmingcheng){
					searchForm['shangpinmingcheng'] = '%' + this.searchForm.shangpinmingcheng + '%'
				}
				if(this.searchForm.shangpinfenlei){
					searchForm['shangpinfenlei'] = '%' + this.searchForm.shangpinfenlei + '%'
				}
				if(this.searchForm.pinpai){
					searchForm['pinpai'] = '%' + this.searchForm.pinpai + '%'
				}
				if(this.searchForm.dianpuming){
					searchForm['dianpuming'] = '%' + this.searchForm.dianpuming + '%'
				}
                let res = {};
                if(this.userid) {
                    res = await this.$api.page(`chongwuyongpin`, searchForm);
                } else {
                    res = await this.$api.list(`chongwuyongpin`, searchForm);
                }
				// 如果是第一页数据置空
				if (this.mescroll.num == 1) this.list = [];
				this.list = this.list.concat(res.data.list);
				if (res.data.list.length == 0) this.hasNext = false;
				this.mescroll.endSuccess(this.mescroll.size, this.hasNext);
			}
		}
	};
</script>

<style>
	/* product */
	page {
		background: #EEEEEE;
	}

	view {
		font-size: 28upx;
	}

	.tabView {
		display: flex;
		align-items: center;
		justify-content: flex-start;
		background: #ffffff;
		height: 60upx;
		margin-bottom: 20upx;
		white-space: nowrap;
		box-shadow: 0px 1px 14px 0px rgba(38, 38, 35, 0.07);
		margin-top: 6upx;
	}
	.tab {
		text-align: center;
		display: inline-block;
		margin: 0 0 10rpx;
		padding: 0;
		width: 140rpx;
		line-height: 80rpx;
		color: rgba(255, 255, 255, 1);
		font-size: 28rpx;
		border-radius: 0;
		border-width: 0;
		border-style: solid;
		border-color: rgba(0,0,0,1);
		background-color: rgba(225, 225, 225, 1);
		box-shadow: 0 0 0px rgba(0,0,0,.3);
	}
	.tabActive{
		margin: 0 0 10rpx;
		padding: 0;
		width: auto;
		line-height: 80rpx;
		color: #fff;
		font-size: 28rpx;
		border-radius: 0;
		border-width: 0;
		border-style: solid;
		border-color: rgba(0,0,0,1);
		background-color: rgba(255, 99, 8, 1);
		box-shadow: 0 0 0px rgba(0,0,0,.3);
	}
	.tab:last-of-type {
		border: none;
	}

	.uni-product-list {
		display: flex;
		width: 100%;
		flex-wrap: wrap;
		flex-direction: row;
		margin-top: 0upx;
		// justify-content: space-around;
	}

	.uni-product {
		padding: 10upx;
		margin: 10upx;
		display: flex;
		flex-direction: column;
		background: #FFFFFF;
	}

	.image-view {
		height: 330upx;
		width: 330upx;
		margin: 12upx 0;
	}

	.uni-product-image {
		height: 330upx;
		width: 330upx;
	}

	.uni-product-title {
		width: 300upx;
		word-break: break-all;
		display: -webkit-box;
		overflow: hidden;
		line-height: 1.5;
		text-overflow: ellipsis;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 2;
	}

	.uni-product-price {
		margin-top: 10upx;
		font-size: 28upx;
		line-height: 1.5;
		position: relative;
	}

	.uni-product-price-original {
		color: #e80080;
	}

	.uni-product-price-favour {
		color: #888888;
		text-decoration: line-through;
		margin-left: 10upx;
	}

	.uni-product-tip {
		position: absolute;
		right: 10upx;
		background-color: #ff3333;
		color: #ffffff;
		padding: 0 10upx;
		border-radius: 5upx;
	}

	uni-image > div, uni-image > img {
		width: 100%;
		height: 140px;
		object-fit: cover;
	}

	.add-btn {
		position: fixed;
		left: 30upx;
		right: 30upx;
		// #ifndef MP
		bottom: 106upx;
		// #endif
		// #ifdef MP-WEIXIN
		bottom: 16upx;
		// #endif
		z-index: 95;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 690upx;
		height: 80upx;
		font-size: 32upx;
		color: #fff;
		background-color: red;
		border-radius: 10upx;
		box-shadow: 1px 2px 5px rgba(219, 63, 96, 0.4);
	}

	.list {
		padding: 20upx 20upx 20upx;
	}

	.listm {
		background: #fff;
		border-radius: 15upx;
		box-shadow: 0 0 2upx rgba(0, 0, 0, 0.1);
		margin-bottom: 20upx;
		padding: 30upx;
	}

	.listmpic {
		border-radius: 10upx;
		width: 166upx;
		margin-right: 20upx;
	}

	.listmr {
		// width: 460upx;
		display: inline-block;
		flex: 1;
		display: flex;
		justify-content: space-between;
		flex-direction: column;
	}

  .news-box6 .dian::before {
    content: "";
    display: block;
    width: 8px;
    height: 8px;
    background-color: red;
    position: absolute;
    top: -4px;
    left: 50%;
    transform: translateX(-50%);
    border-radius: 100%;
    z-index: 1;
  }

  .hide1 {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 1;
    line-clamp: 1;
    -webkit-box-orient: vertical;
  }

  .hide2 {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    line-clamp: 2;
    -webkit-box-orient: vertical;
  }

  .hide4 {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 4;
    line-clamp: 4;
    -webkit-box-orient: vertical;
  }
</style>

<template>
	<view class="home">
        <scroll-view scroll-x class="navscroll">
            <view class="item" v-for="(item,index) in navArr" :key="item.id" :class="navIndex == index ? 'color' : ''" @click="isColor(index,item.id)">{{item.classname}}</view>
        </scroll-view>
		<view class="content">
            <view class="row" v-for="val in newsArr" :key="val.id">
                <newsBox @click.native="goDetail(val)" :item="val"></newsBox>
            </view>
        </view>
        <view class="nodata" v-if="!newsArr.length">
            <!-- mode 图片裁剪、缩放的模式  widthFix 宽度不变，高度自动变化，保持原图宽高比不变 -->
            <image class="img" src="../../static/null.png" mode="widthFix"></image>
        </view>
        <view class="loading" v-if="newsArr.length">
            <view v-show="loading==1">数据加载中...</view>
            <view v-show="loading==2">没有更多了~~</view>
        </view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navArr: [],
                newsArr: [],
                navIndex: 0,
                currentPage: 1,
                loading: 0          // 0默认  1加载中    2没有更多了
			}
		},
		onLoad() {
            // 初始化导航条数据
            this.getNavData()
            // 初始化新闻列表数据
            this.getNewsData()
		},
        // 监听用户的下拉动作,一般是下拉刷新
        onPullDownRefresh() {
            console.log(1111)
        },
        // 页面滚动到底部
        onReachBottom() {
            // 没有数据之后,让请求结束,不再发送请求
            if(this.loading = 2) {
                return
            }
            // 当页面触底之后,页码加一,然后再重新获取数据
            this.currentPage++;
            this.loading = 1
            this.getNewsData()
        },
		methods: {
            // 点击导航栏触发不同的效果
            isColor(e,id) {
                // 点击导航栏切换背景色
                this.navIndex = e
                // 页面滚动到底部触发加载更多之后,再点击导航栏就会没有效果，是因为其他导航栏中的内容已经被加载出来了
                // 解决:点击导航栏的时候,把页码重新赋值到1
                this.currentPage = 1;
                // 页面滚动到底部触发加载更多之后,再点击导航栏就会没有效果，是因为其他导航栏中的内容已经被加载出来了
                // 解决: 点击导航栏时，把原来新闻列表中的内容清空
                this.newsArr = []
                // 点击导航栏跳转不同的新闻列表
                console.log(id)
                // 切换导航栏时把loading换成恢复成默认状态
                this.loading = 0
                // 把点击导航栏的下标传递到新闻列表数据中
                this.getNewsData(id)
            },
            // 跳转到新闻详情页
            goDetail(id) {
                console.log(id)
                uni.navigateTo({
                    url:`/pages/Detail/Detail?cid=${id.classid}&id=${id.id}`
                })
            },
            // 获取导航列表的数据
            getNavData() {
                uni.request({
                    url:"https://ku.qingnian8.com/dataApi/news/navlist.php",
                    success:res=> {
                        this.navArr = res.data
                    }
                })
            },
            // 获取新闻列表数据
            getNewsData(id) {
                uni.request({
                    url: "https://ku.qingnian8.com/dataApi/news/newslist.php",
                    data:{
                        // id为接收到的导航栏下标
                        cid: id,
                        // 页码
                        page: this.currentPage
                        
                    },
                    success: res=> {
                        console.log(res)
                        // 判断,数据加载完成显示没有更多了
                        if(res.data.length == 0) {
                            this.loading = 2
                        }
                        // 页面触底之后,旧新闻要拼接新的新闻
                        // [...this.newsArr,...res.data] ES6写法,数组的展开方式,旧数据拼接新数据
                        this.newsArr = [...this.newsArr,...res.data]
                        // this.newsArr = res.data
                    }
                })
            }
		}
	}
</script>

<style lang="scss" scoped>
.navscroll {
    height: 100rpx;
    background: #f7f8fa;
    white-space: nowrap;
    line-height: 100rpx;
    position: fixed;
    // 这里设置导航条固定顶部,h5中会被覆盖,为避免这个情况,top值设置为var(--window-top)。被覆盖的原因是：h5在浏览器上被模拟成一个小程序，然后top：0的值是从最顶部开始计算的，所以导航条会被覆盖，而小程序不会出现这种问题，因为小程序top为0的值是从标题处开始计算的
    // --window-top吸顶效果
    top: var(--window-top);
    left: 0;
    z-index: 10;
    // 深度穿透,修改h5 中导航出现滑动条的问题
    // 小程序端本身就没有滑动条
    /deep/ ::-webkit-scrollbar {
        width:  4px !important;
        height:  1px !important;
        overflow:  auto !important;
        background: transparent !important;
        -webkit-appearance: auto !important;
        display: block;
    }
    .item {
        height: 70rpx;
        font-size: 40rpx;
        display: inline-block;
        line-height: 70rpx;
        padding: 0 28rpx;
        margin: 0 10rpx;
        color: #333333;
        border-radius: 20rpx;
        &.color {
            background-color: rgb(66,185,131);
            color: #FFFFFF;
        }
    }
}
.content {
    padding: 30rpx;
    padding-top: 130rpx;
    .row {
        border-bottom: 1px solid #efefef;
        padding: 15rpx 0;
    }
}
.nodata {
    display: flex;
    justify-content: center;
    .img {
        width: 250rpx;
    }
}
.loading {
    text-align: center;
    color: #999;
    font-size: 28rpx;
    line-height: 2em;
}
</style>
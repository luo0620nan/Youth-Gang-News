<template>
    <view class="user">
        <view class="top" @click="openTran">
            <image src="../../static/logo.png" mode=""></image>
            <view class="text">浏览历史</view>
        </view>
        <view class="content">
            <view class="row" v-for="item in listArr" :key="item.id">
                <newsBox :item="item" @click.native="goDetail(item)"></newsBox>
            </view>
        </view>
        <view class="nodata" v-show="!listArr.length">
            <!-- mode 图片裁剪、缩放的模式  widthFix 宽度不变，高度自动变化，保持原图宽高比不变 -->
            <image class="img" src="../../static/null.png" mode="widthFix"></image>
            <view class="text">暂无浏览记录</view>
        </view>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                listArr: []
            };
        },
        onLoad() {
            this.getData()
        },
        onShow() {
            this.getData()
        },
        methods: {
            // 打开动画测试页面
            openTran() {
               uni.navigateTo({
                   url: '../text/text'
               }) 
            },
            // 打开新闻详情页
            goDetail(item) {
                uni.navigateTo({
                    url:`/pages/Detail/Detail?cid=${item.classid}&id=${item.id}`
                })
            },
            getData() {
                let history = uni.getStorageSync('historyArr') || []
                console.log(history)
                this.listArr = history
            },
        }
    }
</script>

<style lang="scss">
.user {
    .top {
        padding: 50rpx;
        background-color: #F8F8F8;
        color: #555;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        image {
            width: 150rpx;
            height: 150rpx;
        }
        .text {
            font-size: 38rpx;
            padding-top: 20rpx;
        }
    }
    .content {
        padding: 30rpx;
        .row {
            border-bottom: 1px solid #efefef;
            padding: 15rpx 0;
        }
    }
    .nodata{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        image { 
            width: 200rpx;
            height: auto;
        }
        .text {
            font-size: 28rpx;
            color: #999;
            padding: 20rpx 0;
        }
    }
}
</style>

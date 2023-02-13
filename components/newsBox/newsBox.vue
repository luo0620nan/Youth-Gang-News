<template>
    <view class="newContent">
        <view class="pic">
            <image :src="item.picurl" mode="aspectFill"></image>
        </view>
        <view class="text">
            <view class="title">{{item.title}}</view>
            <view class="info" v-show="!item.looktime">
                <text>{{item.author}}</text>
                <text>{{item.hits}}浏览</text>
            </view>
            <view class="info" v-show="item.looktime">
                <text>{{item.looktime}}</text>
            </view>
        </view>
    </view>
    
</template>

<script>
    export default {
        name:"newsBox",
        props: {
            item: {
                type: Object,
                default() {
                    return {
                        title: '组件内的默认标题',
                        author: "张三",
                        hits: 668,
                        picurl: "../../static/logo.png",
                        looktime: ''
                    }
                }
            }
        },
        data() {
            return {
                
            };
        }
    }
</script>

<style lang="scss">
.newContent {
    display: flex;
    .pic {
        width: 230rpx;
        height: 160rpx;
        image {
            width: 100%;
            height: 100%;
        }
    }
    .text {
        flex: 1;
        padding-left: 20rpx;
        .title {
            font-size: 36rpx;
            color: #333333;
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            line-clamp: 2;
            -webkit-box-orient: vertical;
        }
        .info {
            font-size: 26rpx;
            color: #999;
            padding-top: 20rpx;
            text {
                padding-right: 36rpx;
            }
        }
    }
}
</style>

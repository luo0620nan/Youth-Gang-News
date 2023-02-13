<template>
    <view>
        <view class="detail">
            <view class="title">{{detail.title}}</view>
            <view class="info">
                <view class="author">编辑：{{detail.author}}</view>
                <view class="time">发布日期：{{detail.posttime}}</view>
            </view>
            <view class="content">
                <rich-text :nodes="detail.content"></rich-text>
            </view>
            <view class="description">
                声明：本站的内容均采集与腾讯新闻，如果侵权请联系管理（513894357@qq.com）进行整改删除，本站进行了内容采集不代表本站及作者观点，若有侵犯请及时联系管理员，谢谢您的支持。
            </view>
        </view>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                options: null,
                detail: []
            };
        },
        onLoad(e) {
            this.options = e
            this.getDetail()
        },
        methods:{
            getDetail() {
                const that = this
                uni.request({
                    url: 'https://ku.qingnian8.com/dataApi/news/detail.php',
                    data:that.options,
                    success(res) {
                        // 根据正则表达式查找所有的图片并改变尺寸
                        res.data.content = res.data.content.replace(/<img/gi,'<img style="max-width:100%"')
                        let time = that.getLocalTime(res.data.posttime)
                        res.data.posttime = time
                        that.detail = res.data
                        uni.setNavigationBarTitle({
                            title:that.detail.title
                        })
                        that.savehistory(that.detail)
                    }
                })
            },
            savehistory(e) {
                let historyArr = uni.getStorageSync('historyArr') || []
                console.log(123,historyArr)
                let item = {
                    id: e.id,
                    cid: e.classid,
                    picurl: e.picurl,
                    title: e.title,
                    looktime: this.getLocalTime(parseInt(new Date().getTime() /1000) + '')
                }
                // 浏览记录去重
                let index = historyArr.findIndex(i => {
                    return i.id == this.detail.id
                })
                // 如果重复的话就删除数组里重复的东西,然后再重新添加
                if(index>=0) {
                    historyArr.splice(index,1)
                }
                historyArr.unshift(item)
                uni.setStorageSync('historyArr',historyArr)
            },
            getLocalTime(nS) {
                let time = new Date(nS * 1000)
                let y = time.getFullYear();
                y = y > 9 ? y : '0' + y;
                let m = time.getMonth() + 1;
                m = m > 9 ? m : '0' + m;
                let d = time.getDate();
                d = d > 9 ? d : '0' + d
                let t = time.getHours();
                t = t > 9 ? t : '0' + t
                let s = time.getSeconds();
                s = s > 9 ? s : '0' + s
                let mi = time.getMilliseconds();
                mi = mi > 9 ? mi : '0' + mi
                let time_data = y + '-' + m + '-' + d +' '+ t + ':'+s+':'+mi
                return time_data
            }
        }
    }
</script>

<style lang="scss">
.detail {
    padding: 30rpx;
    .title {
        font-size: 46rpx;
        color: #333;
    }
    .info {
        background: #F6F6F6;
        padding: 20rpx;
        font-size: 22rpx;
        color: #666;
        display: flex;
        justify-content: space-between;
        margin: 40rpx 0;
    }
    .content {
        font-size: 24rpx;
        padding-bottom: 50rpx;
    }
    .description {
        background: #fef0f0;
        font-size: 26rpx;
        padding: 20rpx;
        color: #F89898;
        line-height: 1.8rem;
    }
}
</style>

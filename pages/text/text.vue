<template>
    <view class="content">
        <uni-transition ref="ani" customClass="custom-transition" modeClass="fade" :duration="0" :show="show"></uni-transition>
        <button type="primary" @click="run">Execute Animation</button>
    </view>
</template>

<script>
    export default {
        data() {
            return {
                show: true,
                styles: {
                    'width': '100px',
                    'height': '100px',
                    'backgroundColor': 'red'
                }
            };
        },
        onReady() {
            this.$refs.ani.init({
                // 动画过渡持续时间
                duration: 1000,
                // 定义动画的效果 默认值'linear'
                // linear  动画从头到尾的速度是相同的
                timingFunction: 'linear',
                transformOrigin: '50% 50%',
                // 动画延迟时间
                delay: 500
            })
        },
        methods: {
            run() {
                this.$refs.ani.step({
                    // x轴偏移位置
                    translateX: '100px',
                    // rotate: '360'
                })
                this.$refs.ani.step({
                    translateX: '0px',
                    // deg的范围,-180~180,从原点顺时针旋转一个deg角度
                    rotate: '0'
                },
                {
                    // 动画以低速开始
                    timingFunction: 'ease-in',
                    // 动画持续时间
                    duration: 200
                })
                this.$refs.ani.run(()=> {
                    console.log('动画支持完毕')
                })
            }
        }
    }
</script>

<style lang="scss">
.content /deep/ .custom-transition {
    width: 100px;
    height: 100px;
    background-color: red;
}
</style>

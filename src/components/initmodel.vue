<template>
    <div class="initmodel">
        <div class="initmodel-title">为项目制定步骤规划命脉</div>
        <img class="close" v-if="imgUrl" :src="imgUrl+'shut-down.png'"/>
        <i-tabs :current=" tabss " @change="changeBar">
            <i-tab key="tab1" title="默认模板"></i-tab>
            <i-tab key="tab2" title="自定义模板"></i-tab>
        </i-tabs>
        <div>
            <model v-if="!showFirst" @submitMy="submit" :initNum="initNum"></model>
            <my-model v-else @submitMy="submit"></my-model>
        </div>
    </div>
</template>
<script>
import model from '@/components/model'
import myModel from '@/components/my-model'
export default {
    props:["states","initNum"],
    data(){
        return {
            imgUrl: this.GLOBAL.localImg,
            tabss:'tab1',
            showFirst:false,
        }
    },
    components:{
        model,
        myModel
    },
    methods:{
        changeBar(){
            if(this.tabss == 'tab2')
                this.tabss = 'tab1'
            else
                this.tabss = 'tab2'
            console.log(this.tabss)
            this.showFirst = !this.showFirst
        },
        submit(msg,num){
            this.$emit('submitList',msg,num)
        }
    }
}
</script>
<style lang="scss" scoped>
.initmodel{
    padding: 10rpx;
    position: relative;
    .close{
        position: absolute;
        right: 10rpx;
        top: 10rpx;
        height: 40rpx;
        width: 40rpx;
    }
    &-title{
        padding: 15rpx 15rpx 15rpx 0;
        margin-left: 15rpx;
        margin-bottom: 15rpx;
        border-bottom: 1rpx dashed black;
        font-size: 35rpx;
        font-weight: 900;
        width: 72%;
        text-align: center
    }
    &-bar{
        display: flex;
        width: 100%;
        align-items: center;
        margin-top: 20rpx;
        div{
            width: 50%;
            text-align: center;
            font-size: 28rpx;
            line-height: 50rpx;
            color:#80848f;
            height: 50rpx;
        }
        .select{
            border-bottom: 4rpx solid #87CEFF;
            color: black
        }
    }
}
</style>

<template>
    <div class="work">
        <div class="work-title">创建任务</div>
        <div class="work-content">
            <i-cell-group>
                <i-cell title="项目名称">
                    <i-icon slot="icon" type="activity" size="26"/>
                    <p slot="footer">{{pname}}</p>
                </i-cell>
                <i-cell title="状态">
                    <i-icon slot="icon" type="createtask" size="26"/>
                    <p slot="footer">{{sname}}</p>
                </i-cell>
                <i-cell title="任务名称">
                    <i-icon slot="icon" type="accessory" size="26"/>
                    <input slot="footer" v-model="workName" placeholder="请输入任务名称"/>
                </i-cell>
                <picker
                    mode="date"
                    :value="date"
                    start="2019-01-01"
                    end="2023-12-31"
                    @change="bindDateChange"
                >
                    <i-cell title="设置截止时间" class="aaa">
                        <view class="inner"></view>
                        <view slot="footer" class="picker">{{date == "" ? '请选择' : date}}</view>
                        <i-icon slot="icon" type="clock" size="26"/>
                    </i-cell>
                </picker>
                <i-cell title="备注">
                    <i-icon slot="icon" type="barrage" size="26"/>
                </i-cell>
                <i-cell>
                    <textarea v-show="!showSide" v-model="remark" class="add" placeholder="请填写项目备注"></textarea> 
                </i-cell>
                <i-cell title="参与者">
                    <p slot="footer">请添加项目参与者</p>
                    <i-icon slot="icon" type="addressbook" size="26"/>
                </i-cell>
                <i-cell>
                    <div class="avater">
                        <div class="avater-area" v-for="element in part" :key="element.userName" >
                            <img class="addpng" :src="element.avatar" @click="show"/>
                            <span>{{element.userName}}</span>
                        </div>
                        <img class="addpng" v-if="imgUrl" :src="imgUrl+'add.png'" @click="show"/>
                    </div>
                </i-cell>
            </i-cell-group>
        </div>
        <div class="work-bnt">
            <i-button class="aaa" @click="handleClick" type="info" shape="circle" size="large">发 布 任 务</i-button>
        </div>
        <div class="toast" v-if="showSide">
            <div class="toast-back change-color" @click="hide"></div>
            <div class="toast-menu animation-up">
                <invite :part="part" @submitList="submitList"></invite>
            </div>
        </div>
    </div>
</template>
<script>
import invite from '@/components/invite'
import {createWork} from '@/utils/API'
import {showToast, showModal} from '@/utils/wxFunc'
export default {
    data(){
        return{
            imgUrl: this.GLOBAL.localImg,
            showSide: true,
            workName:'',
            remark:'',
            part:'',
            date:'',
            sname:'',
            sid:'',
            pname:'',
            pid:''
        }
    },
    components:{
        invite
    },
    onLoad(options){
        let {sname,sid,pname,pid} = options;
        this.sname = sname
        this.sid =  sid;
        this.pid = pid;
        this.pname = pname;
    },
    methods:{
        hide(){
        this.showSide = false
        },
        show() {
        this.showSide = true;
        },
        aa(){
            console.log('ppp')
        },
        submitList(e){
            this.part = e
        },
        async handleClick(){
            let participant = [];
            if(this.workName == "" || this.date == "" || this.part.length == 0){
                throw new Error('fail')
                console.log("请登录 ")
            }
            this.part.forEach((element)=>{
                participant.push(element.openid)
            })
            let params = {
                stateid:this.sid,
                workname:this.workName,
                endtime:this.date + " 00:00:00",
                remark:this.remark,
                participant:participant
            }
            console.log(params)
            await createWork(params).catch((err)=>{
                console.log(err);
                throw new Error('fail')
            })
            showToast('任务创建成功！')
        },
        bindDateChange(e){
           this.date = e.mp.detail.value
        }
    }
}
</script>
<style lang="scss" scoped>
.work{
    padding: 30rpx 20rpx;
    &-title{
        font-size: 42rpx;
        font-weight: 900;
        text-align: center
    }
    &-content{
        padding: 0rpx 25rpx;
        margin-top: 60rpx;
    }
    &-bnt{
        margin-top: 80rpx;
    }
}
.text{
    text-align: left;
    padding: 0 30rpx;
    padding-left: 60rpx;
}
.add{
    height: 100rpx;
    padding: 0 30rpx;
    font-size: 30rpx;
}
.addpng{
    height: 90rpx;
    width: 90rpx;
    margin-right: 15rpx;
    border-radius: 50%;
}
.avater{
    padding: 0 10rpx;
    display: flex;
    flex-wrap: wrap;
    &-area{
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-right: 10rpx;
        margin-bottom: 10rpx;
    }
}
.toast{
    width: 100vw;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    background: rgba(0,0,0,0.4);
    z-index: 999;
}
.toast{
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 999;
  &-menu{
    position: fixed;
    width: 75vw;
    background: white;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: white;
    border-radius: 15rpx;
  }
  &-back{
    position: fixed;
    width: 100vw;
    height: 100vh;
    background: black;
    opacity:0.6;
    z-index: -1
  }
}
.change-color {
  opacity: 0.7;
  animation: change-to-black 0.1s linear;
}
@keyframes change-to-black {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 0.7;
  }
}

.animation-up {
  opacity: 1;
  animation: bottom-toast-up 0.1s linear;
}

@keyframes bottom-toast-up {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
.aaa{
    position: relative;
}
.inner{
    height: 76rpx;
    width: 100%;
    right: 0;
    position: absolute;
    bottom: 0;
    z-index: 999;
}
</style>

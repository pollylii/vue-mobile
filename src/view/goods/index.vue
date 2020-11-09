<template>
  <div class="goods">
	<div class="container" id="container">
        <div class="top-bg">
            <div class="f-l">
                <p class="num">522</p>
                <p style="color: #FFFFFF;line-height: 20px;">今日获客</p>
            </div>
            <div class="f-r">
                <p class="r"><van-icon name="search" size="24px" @click="showPopup"/></p>
                <van-popup v-model="show" position="top" :style="{ height: '60%' }" >
                    <div class="mar-15">
                        <div style="font-size: 16px;font-family: PingFang-SC-Heavy;line-height: 22px;">今日拓客</div>
                        <div style="margin: 11px 19px 30px 0;">
                            <div class="color-z">停留时间</div>
                            <div class="flex">
                                <p class="s-t"><van-field v-model="starttime" clearable placeholder="---" @focus="start" /></p>
                                <p class="s-t"><van-field v-model="endtime" clearable placeholder="---" @focus="end" /></p>
                                <p class="s-t">
                                    <img height="18px" width="16px" src="@/asserts/images/icon_rili.svg">
                                </p>
                            </div>
                            <div class="color-z">MAC码</div>
                            <p class="s-t"><van-field v-model="value" placeholder="请输入MAC码" /></p>
                            <div class="flex">
                                <div class="flex-1 color-z">
                                    <p style="margin-left: 10px;">状态</p>
                                    <p class="s-t">
                                        <van-field readonly clickable :value="value" placeholder="" @click="showPicker = true"/>
                                    </p>
                                </div>
                                <div class="flex-1 color-z">
                                    <p>访问次数</p>
                                    <p class="s-t">
                                        <van-field readonly clickable :value="value1" placeholder="" @click="showPicker1 = true"/>
                                    </p>
                                </div>
                            </div>
                        </div>
                        <div style="text-align: center;margin-bottom: 20px" >
                            <van-button type="default" size="small" class="ref-btn search-btn" @click="show = false">重置</van-button>
                            <van-button type="default" size="small" class="conf-btn search-btn" @click="show = false">确定</van-button>
                        </div>
                    </div>
                </van-popup>
                <van-button type="default" size="mini" class="ads"  to="info">投放广告</van-button>
            </div>
            <div class="inster-tag flex">
                <p class="flex-1 mar-tb-19" style="padding-left:20px;">
                    <span class="mar-r-4">时间</span>
                    <img height="5px" width="8px" src="@/asserts/images/xiala02.svg">
                </p>
                <p class="flex-1 mar-tb-19">
                    <span class="mar-r-4">访问</span>
                    <img height="5px" width="8px" src="@/asserts/images/xiala02.svg">
                </p>
                <p class="flex-1 mar-tb-19">
                    <span class="mar-r-4">距离</span>
                    <img height="5px" width="8px" src="@/asserts/images/xiala02.svg">
                </p>
                <p class="flex-1 mar-tb-19" style="padding-right:20px;">
                    <span class="mar-r-4">停留时长</span>
                    <img height="5px" width="8px" src="@/asserts/images/xiala02.svg">
                </p>
            </div>
        </div>
        <div class="list">
            <div class="bg-list" v-for="i in 10" :key="i">
                <div class="flex top-c">
                    <div><p class="title-left">{{i%2===0?'回流':'新增'}}</p></div>
                    <div class="mes-right">
                        <p class="d-t">14:3c:3c:77:55:ba<span style="color: #2CBCFF;font-size: 12px;">【MAC】</span></p>
                        <p class="t-t">2020/11/5 21:40</p>
                    </div>
                </div>
                <div class="flex bottom-c">
                    <div class="flex-1">
                        <p class="color-z">手机</p>
                        <p>华为</p>
                    </div>
                    <div class="flex-1">
                        <p class="color-z">访问次数</p>
                        <p>2</p>
                    </div>
                    <div class="flex-1">
                        <p class="color-z">距离/m</p>
                        <p>46.4</p>
                    </div>
                    <div class="flex-1">
                        <p class="color-z">平均停留/s</p>
                        <p>1953</p>
                    </div>
                </div>
            </div>
        </div>

        <van-popup v-model="showPicker" round position="bottom">
            <van-picker show-toolbar :columns="columns" @cancel="showPicker = false" @confirm="onConfirm"/>
        </van-popup>
        <van-popup v-model="showPicker1" round position="bottom">
            <van-picker show-toolbar :columns="columns1" @cancel="showPicker1 = false" @confirm="onConfirm1"/>
        </van-popup>

        <!-- 开始时间控件 -->
        <van-popup v-model="startTimeShow" position="bottom">
            <van-datetime-picker
                v-model="currentDate"
                type="datetime"
                :min-date="minDate"
                :max-date="maxDate"
                @confirm="confirm"
                @cancel="cancel"
                :formatter="formatter"/>
        </van-popup>
        <!-- 结束时间控件 -->
        <van-popup v-model="endTimeShow" position="bottom">
            <van-datetime-picker
                v-model="currentDate1"
                type="datetime"
                :min-date="minDate"
                :max-date="maxDate"
                @confirm="confirm1"
                @cancel="cancel1"
                :formatter="formatter"/>
        </van-popup>

    </div>
  </div>
</template>

<script>
import { Field, Icon, Button, Popup, DatetimePicker, Picker} from 'vant';
import LoanCell from '@/components/LoanCell.vue';
import axios from 'axios'

export default {
  components: {
	  [Field.name]: Field,
	  [Icon.name]: Icon,
	  [Button.name]: Button,
	  [Popup.name]: Popup,
	  [DatetimePicker.name]: DatetimePicker,
	  [Picker.name]: Picker,
	  'apo-cell': LoanCell
  },
  data() {
/*      var url = 'https://test.soeasy666.com/api/probe/probeData/page'
      var data = {
          "current": 1,
          "map": {},
          "model": {
          },
          "order": "desc",
          "size": 100,
          "sort": "createTime"
      }
      axios.post(url,data,{headers:{
              tenant:"MDAwMA==",
              token:"Bearer test",
              'Content-Type':'application/json'
          }}).then(res=>{
              console.log(res)
      })*/
    return {
        show: false,
        startTimeShow: false, //开始时间弹窗
        endTimeShow: false, //结束时间弹窗
        minHour: 10,
        maxHour: 20,
        minDate: new Date(),
        maxDate: new Date(2020, 11, 31),
        currentDate: new Date(), //开始标准时间
        currentDate1: new Date(), //结束标准时间
        starttime: "", //开始时间
        starttime1: "", //开始时间时间戳
        endtime: "", //结束时间
        endtime1: "", //结束时间时间戳
        value: '全状态',
        showPicker: false,
        showPicker1: false,
        columns: ['新增', '常来', '不常来', '回流'],
        columns1:['1次', '2次', '3次', '4次', '5次', '6次', '7次', '8次', '9次'],
        value1: 0,
        value2: 'a',
    };
  },

  methods: {
      onConfirm(value) {
          this.value = value;
          this.showPicker = false;
      },
      onConfirm1(value) {
          this.value1 = value;
          this.showPicker1 = false;
      },
      showPopup() {
          this.show = true;
      },
      // 选择开始时间
      start() {
          this.startTimeShow = true;
      },
      // 选择结束时间
      end() {
          this.endTimeShow = true;
      },
      // 点击确定
      confirm() {
          this.startTimeShow = false;
          this.starttime =
              this.currentDate.getFullYear() +
              "-" +
              (Number(this.currentDate.getMonth()) + 1) +
              "-" +
              this.currentDate.getDate() +
              "- " +
              this.currentDate.getHours() +
              ":" +
              this.currentDate.getMinutes();
          this.starttime1 = new Date(this.currentDate).getTime() / 1000;
      },
      // 点击取消
      cancel() {
          this.startTimeShow = false;
      },
      confirm1() {
          this.endTimeShow = false;
          this.endtime =
              this.currentDate1.getFullYear() +
              "-" +
              (Number(this.currentDate1.getMonth()) + 1) +
              "-" +
              this.currentDate1.getDate() +
              "- " +
              this.currentDate1.getHours() +
              ":" +
              this.currentDate1.getMinutes();
          this.endtime1 = new Date(this.currentDate1).getTime() / 1000;
      },
      cancel1() {
          this.endTimeShow = false;
      },
      // 处理控件显示的时间格式
      formatter(type, value) {
          // 格式化选择器日期
          if (type === "year") {
              return `${value}年`;
          } else if (type === "month") {
              return `${value}月`;
          } else if (type === "day") {
              return `${value}日`;
          } else if (type === "hour") {
              return `${value}时`;
          } else if (type === "minute") {
              return `${value}分`;
          }
          return value;
      },
  }
};
</script>
<style scoped>
    .container{
        background: #F4F4F4;
        height: 1000px;
        width: 100%;
        font-size: 14px;
    }
    .mar-15{
        margin: 11px 0 11px 15px;
    }
    .mar-tb-19{
        margin: 19px 0;
    }
    .mar-r-4{
        margin-right: 4px;
    }
    .flex{
        display: flex;
    }
    .flex-1{
        flex: 1;
        margin: auto;
        left: 0;
        right: 0;
    }
    .f-l{
        float: left;
        margin: 20px 0 0 32px;
    }
    .f-r{
        margin: 6px 19px 0 0;
        float: right;
    }
    .top-bg{
        width: 100%;
        height: 156px;
        background-image: url("../../asserts/images/bg_1x.png");
    }
    .num{
        font-family: DIN-Bold;
        font-size: 36px;
        color: #FFFFFF;
        letter-spacing: 0;
        text-align: center;
        line-height: 44px;
    }
    .r{
        color: #ffffff;
        margin-left: 60px;
    }
    .ads{
        opacity: 0.6;
        background: rgb(44,188,255);
        text-align: center;
        width: 88px;
        height: 34px;
        border: 1px solid #FFFFFF;
        font-size: 14px;
        color: #FFFFFF;
        letter-spacing: 0;
    }
    .inster-tag{
        margin: auto;
        left: 0;
        right: 0;
        top: 104px;
        width: 92%;
        height: 60px;
        box-shadow: #989A9C;
        border-radius: 5px;
        background: #ffffff;
        line-height: 22px;
        position: absolute;
        font-size: 16px;
        color: #3F3F3F;
    }
    .list{
        margin-top: 18px;
        height: 100%;
        background: #F4F4F4;
    }
    .imgClass{
        height: 40px;
        width: 40px;
        border-radius: 50%;
    }
    .d-t{
        height: 20px;
        font-size: 14px;
        color: #000000;
        letter-spacing: 0;
    }
    .t-t{
        height: 17px;
        font-size: 12px;
        color: #999999;
        letter-spacing: 0;
        text-align: left;
    }
    .color-z{
        color: #999999;
    }
    .bg-list{
        text-align: center;
        background: #ffffff;
        margin: 15px;
        padding: 15px;
        box-sizing: border-box;
        border-radius: 5px;
        height: 143px;
        line-height: 30px;
    }
    .search-btn{
        font-size: 18px;
        height: 44px;
        width: 135px;
        border-radius: 5px;
        border-radius: 5px;
        margin-right: 15px;
        letter-spacing: 0;
    }
    .ref-btn{
        color: #666666;
        background: #E4E4E4;
    }
    .conf-btn{
        color: #FFFFFF;
        background: #2CBCFF;
    }
    .title-left{
        font-size: 16px;
        color: #FFFFFF;
        letter-spacing: 0;
        height: 40px;
        line-height: 40px;
        width: 40px;
        border-radius: 50%;
        background-image: linear-gradient(0deg, #5C8CFF 0%, #54C3FF 100%);
    }
    .mes-right{
        margin-left: 10px;
    }
    .top-c{
        height: 40px;
        margin-bottom: 22px;
    }
    .bottom-c{
        height: 48px;
        margin-bottom: 15px;
        line-height: 24px;
    }
    /deep/ .van-cell{
        padding: 0;
    }
    .s-t{
        height: 20px;
        line-height: 20px;
        padding: 15px 0 30px;
    }
</style>

<template>
<div class="popover">
    <el-popover
        placement="bottom-start"
        popper-class="monthDays_popper"
        :popper-class="monthDaysOptions.popperClass"
        v-model="visible"
        @show="openDialog">
        <el-card class="box-card">
           <div slot="header" class="box-header">
                 <div class="el-icon-arrow-left" @click="upMonth"></div>
                 <div @click="showMonthes">{{nowMonth}}</div>
                 <div class="el-icon-arrow-right" @click="nextMonth"></div>
            </div>
            <div v-if="isShowMonthes" class="show">
                <div v-for="(month,index) in monthes" :key="index" class="monthes">
                    <span v-bind:class="{ active : month === nowMonth }"
                    @click="changNowMonth(month)">{{month}}</span>
                </div>
            </div>
             <div v-if="isShowDays" class="show">
                <div v-for="(day,index) in days" :key="index" class="days">
                    <span v-bind:class="{ active : day == nowDay }"
                    @click="changNowDay(day)">{{day}}</span>
                </div>
            </div>
        </el-card>
        
        <el-input 
            class="mouth-days-value"
            slot="reference"
            prefix-icon="el-icon-date"
            v-model="monthDaysValue"
            :placeholder="monthDaysOptions.placeholder"
            :size="monthDaysOptions.size"
            :clearable="monthDaysOptions.clearable"
            :disabled="monthDaysOptions.disabled"
            :readonly="monthDaysOptions.readonly"
            @clear="clearClick"
        >
        </el-input>
    </el-popover>
</div>
</template>

<script>
import dataSelect from './dataSelect.json'
export default {
  props: {
    value: String,
    monthDaysOptions: {
        type: Object,
        default() {
            return {
                placeholder: '选择日期',
                size: 'large',
				clearable: true,
				disabled: false,
				readonly: false,
				popperClass: ''
            }
        }
    }
  },
  data(){
      return{
          monthDaysValue:'',
          monthes:[],
          days:[],
          nowMonth:'',
          nowDay:'',
          nowMonthArab:'',
          monthesArab:[],
          isShowMonthes:false,
          isShowDays:true,
          visible:false
      }
  },
  watch:{
      nowMonth(){
         let feature = '大';
         for (let index = 0; index < dataSelect.month.length; index++) {
            if(dataSelect.month[index].option == this.nowMonth){
                feature = dataSelect.month[index].feature;
                break;
            }
        }
         for (let index = 0; index < dataSelect.days.length; index++) {
            if(dataSelect.days[index].feature == feature){
                this.days = dataSelect.days[index].selectValues;
                break;
            }
        }
      }
  },
  created () {
    this.monthDaysValue = this.value
  },
  mounted(){
    this.init()
  },
  methods:{
    clearClick () {
        this.monthDaysValue = ''
        this.nowMonth = ''
        this.nowDay = ''
        this.$emit('getValue',this.monthDaysValue)
    },
    openDialog () {
        if (this.monthDaysOptions.disabled || this.monthDaysOptions.readonly) return this.visible = false
        if (this.monthDaysValue) {
            const dataArr = this.monthDaysValue.split('-')
            if (dataArr[0][0] === '0') {
                dataArr[0] = dataArr[0].substr(1)
            }
            if (dataArr[1][0] === '0') {
                dataArr[1] = dataArr[1].substr(1);
            }
            this.nowDay = dataArr[1]
            dataSelect.month.forEach(item => {
                if (dataArr[0] === item.arab) {
                    this.nowMonth = item.option
                    this.nowMonthArab = dataArr[0]
                }
            })
        } else {
            this.init()
        }
    },
    init () {
        let date = new Date();
        this.nowMonth = date.getMonth() + 1;
        this.nowDay = date.getDate();
        this.getNowMonth();
        if (this.monthDaysValue) {
            const dataArr = this.monthDaysValue.split('-')
            this.nowDay = dataArr[1]
            dataSelect.month.forEach(item => {
                if (dataArr[0] === item.arab) {
                    this.nowMonth = item.option
                }
            })
        }
    },
    getmonthesArab(){
        this.monthes = [];
        for (let index = 0; index < dataSelect.month.length; index++) {
          this.monthes.push(dataSelect.month[index].option);
        }
    },
    getNowMonth(){
        for (let index = 0; index < dataSelect.month.length; index++) {
            if(dataSelect.month[index].arab == this.nowMonth){
                this.nowMonth = dataSelect.month[index].option;
                this.nowMonthArab = dataSelect.month[index].arab;
                break;
            }
        }
    },
    showMonthes(){
        this.isShowMonthes = !this.isShowMonthes;
        this.isShowDays = !this.isShowDays;
        this.getmonthesArab();
    },
    upMonth(){
        for (let index = 0; index < dataSelect.month.length; index++) {
            if(dataSelect.month[index].arab == this.nowMonthArab){
                if(index === 0 ){
                    index = dataSelect.month.length;
                }
                this.nowMonth = dataSelect.month[index-1].option;
                this.nowMonthArab = dataSelect.month[index-1].arab;
                break;
            }
        }
    },
    nextMonth(){
        for (let index = 0; index < dataSelect.month.length; index++) {
            if(dataSelect.month[index].arab == this.nowMonthArab){
                if(index === dataSelect.month.length-1){
                    index = -1;
                }
                this.nowMonth = dataSelect.month[index+1].option;
                this.nowMonthArab = dataSelect.month[index+1].arab;
                break;
            }
        }
    },
    changNowMonth(month){
        this.isShowMonthes = !this.isShowMonthes;
        this.isShowDays = !this.isShowDays;
        for (let index = 0; index < dataSelect.month.length; index++) {
            if(dataSelect.month[index].option === month){
                this.nowMonth = dataSelect.month[index].option;
                this.nowMonthArab = dataSelect.month[index].arab;
                break;
            }
        }
    },
    changNowDay(day){
        this.nowDay = day;
        let nowMon1,nowDay1;
        if(this.nowMonthArab > 0 && this.nowMonthArab < 10){
            nowMon1 = '0'+ this.nowMonthArab;
        }else{
            nowMon1 = this.nowMonthArab;
        }
         if(this.nowDay > 0 && this.nowDay < 10){
            nowDay1 = '0'+ this.nowDay;
        }else{
            nowDay1 = this.nowDay;
        }
        this.monthDaysValue = nowMon1 +'-' + nowDay1;
        this.visible = false;
        this.$emit('getValue',this.monthDaysValue);
    }
  }
}
</script>
<style>
 .el-popover{
    padding:0;
  }
</style>
<style  scoped>
  .box-card {
    width: 320px;
  }
  .mouth-days-value {
      width: 220px;
  }
  .box-header{
      text-align: center;
      display: flex;
      justify-content: space-between;
      cursor: pointer;
  }
  .show{
      display: flex;
      flex-wrap: wrap;
      cursor: pointer;
  }
  .monthes{
      width: 25%;
      text-align: center;
      margin-top: 4%;
      margin-bottom: 4%;
  }
  .days{
      width: 13%;
      text-align: center;
      margin-top: 2%;
      margin-bottom: 2%;
      margin-left: 1%;
      cursor: pointer;
  }
  .days span {
      display: block;
  }
  .active{
      color:#409EFF;
  }
</style>>

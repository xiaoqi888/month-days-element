<template>
<div class="popover">
    <el-popover
        placement="bottom"
        v-model="visible">
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
            placeholder="请选择日期"
            suffix-icon="el-icon-date"
            v-model="monthDaysValue">
        </el-input>
        
    </el-popover>
</div>
</template>

<script>
import dataSelect from './dataSelect.json'
export default {
  props: {
    value: String
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
  mounted(){
    this.monthDaysValue = this.value
    let date = new Date();
    this.nowMonth = date.getMonth() + 1;
    this.nowDay = date.getDate();
    this.getNowMonth();
  },
  methods:{
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
    width: 300px
  }
  .mouth-days-value {
      width: 200px;
  }
  .box-header{
      text-align: center;
      display: flex;
      justify-content: space-between;
  }
  .show{
      display: flex;
      flex-wrap: wrap;
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
  }
  .active{
      color:#409EFF;
  }
</style>>

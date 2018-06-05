<template>
    <div class="zss-date-picker">
        <input class="zss-date-input" type="text" readonly v-model="currentValue" @focus="inputFocus" />
        <transition name="fade">
        <div class="zss-picker-model" @click.self="modelClick" v-show="isShow">
            <div class="zss-panel">
                <!-- header -->
                <div class="zss-panel-header">
                    <div class="zss-date-year">
                        <div class="zss-date-prev zss-text-hover" @click="prevYear">＜</div>
                        <div class="zss-date-text zss-text-hover">{{firstDate[0]}}</div>
                        <div class="zss-date-next zss-text-hover" @click="nextYear">＞</div>
                    </div>
                    <div class="zss-date-month">
                        <div class="zss-date-prev zss-text-hover" @click="prevMonth">＜</div>
                        <div class="zss-date-text zss-text-hover">{{firstDate[1]}}</div>
                        <div class="zss-date-next zss-text-hover" @click="nextMonth">＞</div>
                    </div>
                </div>
                <!-- /header -->
                
                <div class="zss-panel-body">
                    <div class="zss-date-week">
                        <div class="zss-week" v-for="item in weekArray">{{item}}</div>
                    </div>
                    <div class="zss-date" :class="{'zss-date-now':isNow(item),'zss-month-now':isMonth(item)}" v-for="item in dateArray" @click="clickDate(item)">{{item[2]}}</div>
                </div>                
            </div>
        </div>
        </transition>
    </div>
</template>
<script>
import momnet from 'moment'
export default {
    props:{
        value:{
            type:String,
            default:'1970-01-01'
        }
    },
    data(){
        return {
            currentValue:'', //当前值
            zIndex:1,
            isShow:false,
            weekArray:['日','一','二','三','四','五','六'],
            currentDate:[], //当前日期
            dateArray:[],  //当月日期列表
            firstDate:[]  //当月1号
        }
    },
    mounted(){
        this.initData();
    },
    methods:{
        //初始化值
        initData(){
            this.currentValue = this.value;
            this.currentDate = this.value.split('-');
            this.firstDate = momnet(this.currentDate.join('-')).startOf('month').format('YYYY-MM-DD').split('-');
            this.getDateArray();
        },
        //获取当前页面的最大Index
        getIndex(){
            let array = [...document.all];
            console.log(array);
        },
        //获取当前显示日期列表
        getDateArray(){
            let day = momnet(this.firstDate.join('-')).day(); //1号是周几
            let list = []
            for(var i = 0; i < 42; i++){
                let date = null;
                if(day == 0){
                    date = momnet(this.firstDate.join('-')).add((i-7),'days');
                }else{
                    date = momnet(this.firstDate.join('-')).add((i-day),'days');
                }
                list.push(date.format('YYYY-MM-DD').split('-'));
            }
            this.dateArray = list;
        },
        //获取焦点
        inputFocus(){
           this.initData();
           this.isShow = true;
        },
        //点击遮罩层
        modelClick(){
            this.isShow = false;
        },
        //获取当前日期
        clickDate(item){
            let date = item.join('-');
            this.currentValue = date;
            this.$emit('input',date);
            this.isShow = false;
        },
        //判断是否是当天
        isNow(item){
            if(this.firstDate[1] != this.currentDate[1]){
              return false;
            }
            if(item[0] != this.currentDate[0]){
                return false;
            }
            if(item[1] != this.currentDate[1]){
                return false;
            }
            if(item[2] != this.currentDate[2]){
                return false;
            }
            return true;
        },
        //判断是否当月
        isMonth(item){
            if(item[0] != this.firstDate[0]){
                return false;
            }
            if(item[1] != this.firstDate[1]){
                return false;
            }
            return true;
        },
        //点击前一个月
        prevMonth(){
            let prevMonthDate = momnet(this.firstDate.join('-')).startOf('month').add(-1,'month');
            this.firstDate = prevMonthDate.format('YYYY-MM-DD').split('-');
            this.getDateArray();
        },
        //点击下一个月
        nextMonth(){
            let nextMonthDate = momnet(this.firstDate.join('-')).startOf('month').add(1,'month');
            this.firstDate = nextMonthDate.format('YYYY-MM-DD').split('-');
            this.getDateArray();
        },
        //点击前一年
        prevYear(){
            let prevYearDate = momnet(this.firstDate.join('-')).startOf('month').add(-1,'year');
            this.firstDate = prevYearDate.format('YYYY-MM-DD').split('-');
            this.getDateArray();
        },
        //点击下一年
        nextYear(){
            let nextYearDate = momnet(this.firstDate.join('-')).startOf('month').add(1,'year');
            this.firstDate = nextYearDate.format('YYYY-MM-DD').split('-');
            this.getDateArray();
        }
    }
}
</script>
<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: all .3s
}
.fade-enter, .fade-leave-to {
  opacity: 0;
  transform: scale(1.2);
}
.zss-date-picker{
    display: inline-block;
    box-sizing: border-box;
    width: 400px;
    height: 40px;
    border: 1px solid #E6E6E6;
    font-family: Arial, Helvetica, sans-serif;
}
.zss-date-picker >.zss-date-input{
    width: 100%;
    height: 100%;
    border: 0;
    box-sizing: border-box;
    padding: 0 20px;
    outline: none;
    font-size: 16px;
    color: #414141;
}
.zss-date-picker > .zss-picker-model{
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    background: rgba(0,0,0,.5);
}
.zss-date-picker > .zss-picker-model > .zss-panel{
    margin: 0 auto;
    width: 550px;
    height: 440px;
    background: #FFF;
    padding: 10px;
    box-shadow: 0 10px 20px rgba(0,0,0,.2);
}
.zss-panel > .zss-panel-header{
    display: flex;
    height: 60px;
    line-height: 60px;
    border-bottom:1px solid #E6E6E6;
    background: #EEE;
}
.zss-date-year{
    background: #FFF;
    flex: 1;
    height: 60px;
    display: flex;
}
.zss-date-month{
    background: #FFF;
    flex: 1;
    height: 60px;
    display: flex;
    margin-left:1px;
}
.zss-date-prev{
    flex: 1;
    cursor: pointer;
}
.zss-date-text{
    flex: 1;
    cursor: pointer;
}
.zss-date-next{
    flex: 1;
    cursor: pointer;
}
.zss-text-hover{
    transition: color .3s;
}
.zss-text-hover:hover{
    color: #25b864;
}
.zss-panel-body{
   padding:20px 30px;
   overflow: hidden;
}
.zss-date-week{
    display: flex;
}
.zss-week{
    width: 70px;
    height: 30px;
}
.zss-date{
  float: left;
  width: 70px;
  height: 50px;
  text-align: center;
  line-height: 50px;
  cursor: pointer;
  transition: all .3s;
  color: #E6E6E6;
}
.zss-month-now{
    color: #414141;
}
.zss-month-now:hover{
    color: #25b864;
}
.zss-date-now{
    background: #25b864;
    color: #FFF;
}
.zss-date-now:hover{
    color: #FFF;
}
</style>



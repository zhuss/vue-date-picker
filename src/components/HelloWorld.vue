<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>This is datePicker compoment</h2>
    <div class="demo-box">
      <date-picker v-model="date"></date-picker>
    </div>
    <div class="demo-box">
      <button class="button" type="button" @click="submitClick">确定</button>
    </div>
    <div class="demo-box">
      <p>当前选日期:{{date}}</p>
    </div>
  </div>
</template>

<script>
import DatePicker from './DatePicker'
export default {
  name: 'HelloWorld',
  components:{
    DatePicker
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      date:'2018-06-05'
    }
  },
  watch:{
    date(val){
      console.log(val);
    }
  },
  methods:{
    submitClick(){
      console.log(this.date);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello > .demo-box{
    margin: 20px 0;
}
.hello  .button{
  background: #25b864;
  padding: 0 40px;
  height: 40px;
  font-size: 16px;
  border: none;
  cursor: pointer;
  color: #FFF;
}
</style>

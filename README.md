# vue-date

> A Vue.js project

## 如何在项目中使用方法

``` javascript
//npm / yarn
yarn add https://github.com/zhuss/vue-date-picker.git

//javascript
import DatePicker from 'vue-date/src/components/DatePicker.vue'

export default {
	components:{ 
		DatePicker
	},
	data(){
		return {
			date:'2018-06-05'
		}
	}
}

// template
<date-picker v-model="date"></date-picker>

```


## Build Setup

``` bash
# install dependencies
# npm install
yarn

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

[案例地址:https://zhuss.github.io/vue-date-picker/dist/#/](https://zhuss.github.io/vue-date-picker/dist/#/)

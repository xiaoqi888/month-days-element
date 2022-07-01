# month-days-element

> A vue component that selects the month and day based on element

## Install

```bash
npm install month-days-element
```

## Usage

```bash
<template>
	<MonthDays v-model="monthDaysValue" :monthDaysOptions="monthDaysOptions" @getValue="getValue" />
</template>
<script>
import MonthDays from 'month-days-element'
export default {
	components: {
	  MonthDays
	},
	data () {
		return {
			monthDaysValue: '',
			monthDaysOptions: {
				placeholder: '请选择', // 占位内容，默认为选择日期
				size: 'small', // 输入框尺寸 large, small, mini 默认large
				clearable: true, // 是否显示清除按钮，默认true
				disabled: false, // 禁用，默认false
				readonly: false, // 只读，默认false
				popperClass: '', // 弹出框类名
			}
		}
	},
	methods:{
		getValue (val) {
			this.monthDaysValue = val
		}
	}
}
</script>
```


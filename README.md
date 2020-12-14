# month-days-element

> A vue component that selects the month and day based on element

## Install

```bash
npm install month-days-element
```

## Usage

```bash
<template>
	<MonthDays v-model="monthDaysValue" @getValue="getValue" />
</template>
<script>
import MonthDays from 'month-days-element'
export default {
	components: {
	  MonthDays
	},
	data () {
		return {
			monthDaysValue: ''
		}
	},
	getValue (val) {
		this.monthDaysValue = val
	}
}
</script>
```


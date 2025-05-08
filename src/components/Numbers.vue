<script setup lang="ts">
import { computed, ref, watch } from 'vue';

const limit = ref(100);
const randomNumbersFromLimit = computed(() => {
	const numbers = [];
	for (let i = 1; i <= limit.value; i++) {
		numbers.push(i);
	}
	return numbers.sort(() => Math.random() - 0.5)
})
const nums = ref([] as HTMLElement[])
const hoveredNumber = ref(0)

watch(hoveredNumber, () => {
	if (hoveredNumber.value !== 0) {
		for (let i = 0; i < nums.value.length; i++) {
			const num = Number(nums.value[i].textContent?.trim());
			if (hoveredNumber.value % num === 0) {
				nums.value[i].classList.add('active')
			} else {
				nums.value[i].classList.remove('active')
			}
		}
	}
	else {
		nums.value.forEach(elem => elem.classList.remove('active'))
	}
})

function reset() {
	hoveredNumber.value = 0;
}
</script>

<template>
	<div>
		<input type="number" v-model="limit" /><br /><br />
		<div class="number" v-for="num in randomNumbersFromLimit" :key="num"
			@mouseover="hoveredNumber = Number(num)" @mouseout="reset" ref="nums">
			{{ num }}
		</div>
	</div>
</template>

<style>
.number {
	display: inline-block;
	padding: 5px;
	background-color: lightgrey;
	margin: 5px;
}

.active {
	background-color: red;
}
</style>

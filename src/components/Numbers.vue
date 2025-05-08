<script setup lang="ts">
import { computed, ref, watch } from 'vue';

const limit = ref(100); // Added ref to updated the state of the random numbers displayed when user changes input
const randomNumbersFromLimit = computed(() => { // Created a computed so the numbers will only change if the limit changes
	const numbers = [];
	for (let i = 1; i <= limit.value; i++) {
		numbers.push(i);
	}
	return numbers.sort(() => Math.random() - 0.5)
})
const nums = ref([] as HTMLElement[]) // Created a reference for a list of each number element generated in the component for loop
const hoveredNumber = ref(0) // Created a hoveredNumber state to update number elements when a number is hovered

watch(hoveredNumber, () => { // Added a watch function to the hovered number to iterate over the other number elements and change its classes to active if they are divisors of the hovered number
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

function reset() { // Resets the hovered number to 0 so all the elements gets the active class removed on the watch function
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

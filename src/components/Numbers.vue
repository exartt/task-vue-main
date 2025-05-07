<script setup lang="ts">
import {computed, ref} from "vue";

// numbers array removed, not needed

let limit = ref<number>(100); // it was made reactive.

// transforming function into computed to optimize n
const n = computed<number[]>(() => {
  const forceRule = Math.min(100, Math.max(1, limit.value));
	let numbers: number[] = [];
	for(let i = 1; i <= forceRule; i++) { numbers.push(i); } // improving the way the array is populated.

	return numbers.sort(() => Math.random() - 0.5);
})

// restriction from 1 to 100 to enforce the rule.
function validateLimit() {
  if (limit.value < 1) limit.value = 1
  if (limit.value > 100) limit.value = 100
}

function hov(number: number): void {
  const nums = document.querySelectorAll('.number');

  for(let i = 0; i < nums.length; i++)
  {
    // this parse was added to prevent NaN.
    const num = parseInt(nums[i].textContent.trim());
    if(number % num === 0)
    {
      nums[i].classList.add('active')
      console.log('divisor', num)
    }
  }
}

function reset(): void {
	const nums = document.querySelectorAll('.number');
	nums.forEach(num => num.classList.remove('active'))
}
</script>

<template>
	<div>
    <!-- restriction from 1 to 100 to enforce the rule. -->
		<input type="number" v-model="limit" max="100" min="1" @change="validateLimit()" /><br /><br />
		<div class="number"
			:id="'number-'+number"
			v-for="number in n"
			:key="number"
			@mouseover="hov(number)"
			@mouseout="reset"
		>
			{{ number }}
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

<script setup>
	import { computed } from 'vue';
	
	const props = defineProps({
		data: Object,
		modelValue: String,
	});

	const emit = defineEmits(["update:modelValue"]);

	const localValue = computed({
		get() {
			return props.modelValue;
		},

		set(newValue) {
			emit("update:modelValue", newValue);
		}
	})

</script>

<template>

	<input
		type="radio"
		:id="data.id"
		:name="data.name"
		:value="data.value"
		v-model="localValue"
	>

	<label :for="data.id">
		<span>
			<slot>Fallback text</slot>
		</span>
	</label>
</template>

<style scoped>
	span {
		text-transform: capitalize;
	}

	label, input[type="radio"] {
		cursor: pointer;
	}

	input[type="radio"] {
		display: none;
	}

	label {
		padding: 0.7rem 2rem;
		background-color: #E8EFF4;

		color: #004E89;
		border: 1px solid #004E89;
		border-radius: 1rem;
	}

	input[type="radio"]:checked + label {
		background-color: #002962;
		color: white;
	}
</style>
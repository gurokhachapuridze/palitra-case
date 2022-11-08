<template>
	<MultiSelect class="mb-2" :options="list1" />
	<MultiSelect :options="list2" />
</template>

<script>
	import MultiSelect from './components/MultiSelect.vue';

	export default {
		name: 'App',
		components: {
			MultiSelect,
		},
		setup() {
			const list1 = ['list', 'of', 'options'];
			const list2 = ['პირველი', 'მეორე', 'მესამე'];

			return {
				value: null,
				list1,
        list2,
			};
		},
	};
</script>

<style>
	.mb-2 {
		margin-bottom: 20px;
	}
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}
</style>

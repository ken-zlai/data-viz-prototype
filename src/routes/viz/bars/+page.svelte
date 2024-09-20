<script lang="ts">
	import { onMount } from 'svelte';
	import * as echarts from 'echarts';

	const categories = ['Null Values'];

	const oldData = [0.1];

	const currentData = [0.9];

	const option = {
		title: {
			text: 'Null Rate - GroupBy 1, Feature 10, 01-04-2024'
		},
		tooltip: {
			trigger: 'axis'
		},
		legend: {
			data: ['Test', 'Production']
		},
		xAxis: {
			type: 'category',
			data: categories
		},
		yAxis: {
			type: 'value'
		},
		series: [
			{
				name: 'Test',
				type: 'bar',
				data: oldData,
				itemStyle: {
					color: 'gray'
				}
			},
			{
				name: 'Production',
				type: 'bar',
				data: currentData,
				itemStyle: {
					color: 'blue'
				}
			}
		]
	};

	onMount(() => {
		const chartDom = document.getElementById('barChart');
		const myChart = echarts.init(chartDom);
		myChart.setOption(option);
	});
</script>

<div id="barChart" style="height: 400px; width: 100%;"></div>

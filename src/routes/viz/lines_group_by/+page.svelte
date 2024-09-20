<script lang="ts">
	import { onMount } from 'svelte';
	import * as echarts from 'echarts';

	const xAxis = [
		'01-01-2024',
		'01-02-2024',
		'01-03-2024',
		'01-04-2024',
		'01-05-2024',
		'01-06-2024',
		'01-07-2024'
	];

	const yAxis = [
		'Feature 1',
		'Feature 2',
		'Feature 3',
		'Feature 4',
		'Feature 5',
		'Feature 6',
		'Feature 7',
		'Feature 8',
		'Feature 9',
		'Feature 10'
	];

	const spikes = {
		'Feature 10': ['01-03-2024', '01-04-2024']
	};

	function generateLineData(xAxis, featureName) {
		return xAxis.map((date) => {
			if (spikes[featureName]?.includes(date)) {
				return Math.random() * 0.3 + 0.7; // random number between 0.7 and 1
			} else {
				return Math.random() * 0.2; // random number between 0 and 0.2
			}
		});
	}

	const series = yAxis.map((feature) => ({
		name: feature,
		type: 'line',
		data: generateLineData(xAxis, feature)
	}));

	const option = {
		title: {
			text: 'GroupBy 1'
		},
		tooltip: {
			trigger: 'axis'
		},
		xAxis: {
			type: 'category',
			data: xAxis
		},
		yAxis: {
			type: 'value',
			max: 1
		},
		series: series
	};

	onMount(() => {
		const chartDom = document.getElementById('lineChart');
		const myChart = echarts.init(chartDom);
		myChart.setOption(option);
	});
</script>

<div id="lineChart" style="height: 400px; width: 100%;"></div>

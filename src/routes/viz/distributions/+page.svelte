<script lang="ts">
	import { onMount } from 'svelte';
	import * as echarts from 'echarts';

	// Function to generate bell-shaped curve data
	function generateBellCurveData(mean, deviation, count, scale = 1) {
		const data = [];
		for (let i = -count; i <= count; i++) {
			const x = scale * i + mean;
			const y =
				Math.exp(-Math.pow(i, 2) / (2 * deviation * deviation)) /
				(deviation * Math.sqrt(2 * Math.PI));
			data.push([x, y]);
		}
		return data;
	}

	const mean1 = 50; // Mean of the first curve
	const deviation1 = 10; // Deviation of the first curve
	const mean2 = 65; // Mean of the second curve (shifted)
	const deviation2 = 8; // Deviation of the second curve (narrower)

	const curve1 = generateBellCurveData(mean1, deviation1, 50, 1);
	const curve2 = generateBellCurveData(mean2, deviation2, 50, 1);

	const option = {
		title: {
			text: 'Distribution - GroupBy[x], Feature [x], [date]',
			left: 'center'
		},
		legend: {
			// Try 'horizontal'
			orient: 'vertical',
			right: 10,
			top: 'center'
		},
		xAxis: {
			type: 'value'
		},
		yAxis: {
			type: 'value'
		},
		series: [
			{
				name: 'Original',
				type: 'line',
				data: curve1,
				areaStyle: { opacity: 0.8, color: 'gray' },
				itemStyle: { color: 'gray' }, // Ensure legend color matches

				lineStyle: { opacity: 0 },
				smooth: true,
				showSymbol: false
			},
			{
				name: 'Drifted',
				type: 'line',
				data: curve2,
				areaStyle: { opacity: 0.8, color: 'blue' },
				itemStyle: { color: 'blue' }, // Ensure legend color matches

				lineStyle: { opacity: 0 },
				smooth: true,
				showSymbol: false
			}
		]
	};

	onMount(() => {
		const chartDom = document.getElementById('driftChart');
		const myChart = echarts.init(chartDom);
		myChart.setOption(option);
	});
</script>

<div id="driftChart" style="height: 400px; width: 100%;"></div>

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

	const xAxisAnomolies = ['01-03-2024', '01-04-2024'];
	const yAxisAnomolies = ['Feature 10'];

	function generateData(xAxis, yAxis, xAxisAnomolies, yAxisAnomolies) {
		const data = [];

		for (let i = 0; i < xAxis.length; i++) {
			for (let j = 0; j < yAxis.length; j++) {
				let z;
				if (xAxisAnomolies.includes(xAxis[i]) && yAxisAnomolies.includes(yAxis[j])) {
					z = Math.random() * 0.3 + 0.7; // random number between 0.7 and 1
				} else {
					z = Math.random() * 0.2; // random number between 0 and 0.2
				}
				data.push([i, j, z]);
			}
		}

		return data;
	}

	const data = generateData(xAxis, yAxis, xAxisAnomolies, yAxisAnomolies);

	const option = {
		xAxis: {
			type: 'category',
			data: xAxis
		},
		yAxis: {
			type: 'category',
			data: yAxis,
			name: 'GroupBy 1',
			nameLocation: 'middle',
			nameRotate: 90,
			nameGap: 70, // Adjust the gap between the y-axis labels and the name
			nameTextStyle: {
				fontSize: 18 // Optional: Adjust the font size
			}
		},
		series: [
			{
				name: 'Heatmap',
				type: 'heatmap',
				data: data
			}
		],
		visualMap: {
			min: 0,
			max: 1,
			orient: 'vertical',
			right: '0',
			inRange: {
				color: ['green', 'yellow']
			}
		},
		grid: {
			left: '100px'
		}
	};

	onMount(() => {
		var chartDom = document.getElementById('heatmap');
		var myChart = echarts.init(chartDom);
		myChart.setOption(option);
	});
</script>

<div id="heatmap" style="height: 300px; width: 100%"></div>

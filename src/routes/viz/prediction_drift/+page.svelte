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

	const xAxisAnomolies = ['01-03-2024', '01-04-2024'];

	function generateData(xAxis, xAxisAnomolies) {
		const data = [];

		for (let i = 0; i < xAxis.length; i++) {
			let z;

			if (xAxisAnomolies.includes(xAxis[i])) {
				z = Math.random() * 0.2; // random number between 0 and 0.2
			} else {
				z = Math.random() * 0.3 + 0.7; // random number between 0.7 and 1
			}
			data.push(z);
		}

		return data;
	}

	const data = generateData(xAxis, xAxisAnomolies);

	const option = {
		xAxis: {
			type: 'category',
			data: xAxis
		},
		yAxis: {
			type: 'value',
			name: 'Model Accuracy',
			nameLocation: 'middle',
			nameRotate: 90,
			nameGap: 40, // Adjust the gap between the y-axis labels and the name
			nameTextStyle: {
				fontSize: 18 // Optional: Adjust the font size
			}
		},
		series: [
			{
				data: data,
				type: 'line',
				smooth: true
			}
		]
		// visualMap: {
		// 	min: 0,
		// 	max: 1,
		// 	orient: 'vertical',
		// 	right: '0'
		// },
		// grid: {
		// 	left: '100px'
		// }
	};

	onMount(() => {
		var chartDom = document.getElementById('prediction-drift');
		var myChart = echarts.init(chartDom);
		myChart.setOption(option);
	});
</script>

<div id="prediction-drift" style="height: 300px; width: 100%"></div>

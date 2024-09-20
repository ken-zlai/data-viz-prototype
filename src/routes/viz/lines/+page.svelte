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
	const yAxisAnomolies = ['Feature 9', 'Feature 10'];

	function generateRainfallData(xAxis, xAxisAnomolies) {
		const data = [];

		for (let i = 0; i < xAxis.length; i++) {
			let z;
			if (xAxisAnomolies.includes(xAxis[i])) {
				z = Math.random() * 0.3 + 0.7; // random number between 0.7 and 1
			} else {
				z = Math.random() * 0.2; // random number between 0 and 0.2
			}
			data.push(z);
		}

		return data;
	}

	function generateFlowData(xAxis, xAxisAnomolies, yAxisAnomolies, feature) {
		const data = [];

		for (let i = 0; i < xAxis.length; i++) {
			let z;
			if (xAxisAnomolies.includes(xAxis[i]) && yAxisAnomolies.includes(feature)) {
				z = Math.random() * 0.3 + 0.7; // random number between 0.7 and 1
			} else {
				z = Math.random() * 0.2; // random number between 0 and 0.2
			}
			data.push(z);
		}

		return data;
	}

	const featuresSeries = yAxis.map((feature) => {
		return {
			name: feature,
			type: 'line',
			lineStyle: {
				width: 1
			},
			emphasis: {
				focus: 'series'
			},
			data: generateFlowData(xAxis, xAxisAnomolies, yAxisAnomolies, feature)
		};
	});

	const customTextMap = yAxis.reduce((acc, feature) => {
		acc[feature] = feature + ' drift';
		return acc;
	}, {});

	const option = {
		title: {
			text: 'Prediction Drift vs Feature Drift',
			left: 'center'
		},
		grid: {
			bottom: 80
		},
		toolbox: {
			feature: {
				dataZoom: {
					yAxisIndex: 'none'
				},
				restore: {},
				saveAsImage: {}
			}
		},
		tooltip: {
			trigger: 'axis',
			confine: 'true',

			formatter: function (params) {
				let tooltipText = params[0].axisValueLabel + '<br/>'; // Display the x-axis value (date)
				params.reverse().forEach(function (param) {
					let seriesName = param.seriesName;
					let customText = customTextMap[seriesName] || 'Prediction Drift';
					tooltipText += param.marker + ' ' + customText + ': ' + param.value + '<br/>';
				});
				return tooltipText;
			}
		},
		legend: {
			data: ['Prediction Drift'],
			left: 10
		},

		xAxis: [
			{
				type: 'category',
				boundaryGap: false,
				axisLine: { onZero: false },
				data: xAxis
			}
		],
		yAxis: [
			{
				name: 'Feature Drift',
				type: 'value'
			},
			{
				name: 'Prediction Drift',
				nameLocation: 'start',
				alignTicks: true,
				type: 'value',
				inverse: true
			}
		],
		series: [
			...featuresSeries,
			{
				name: 'Prediction Drift',
				type: 'line',
				yAxisIndex: 1,
				areaStyle: {},
				lineStyle: {
					width: 1
				},
				emphasis: {
					focus: 'series'
				},
				markArea: {
					silent: true,
					itemStyle: {
						opacity: 0.3
					},
					data: [
						[
							{
								xAxis: '2009/9/10\n7:00'
							},
							{
								xAxis: '2009/9/20\n7:00'
							}
						]
					]
				},
				data: generateRainfallData(xAxis, xAxisAnomolies)
			}
		]
	};

	onMount(() => {
		const chartDom = document.getElementById('prediction-drift');
		const myChart = echarts.init(chartDom);
		myChart.setOption(option);
	});
</script>

<div id="prediction-drift" style="height: 600px; width: 100%"></div>

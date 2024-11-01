<script>
	import { scaleLinear } from 'd3-scale';

	const points = [
		{ year: 1990, birthrate: 16.7 },
		{ year: 1995, birthrate: 14.6 },
		{ year: 2000, birthrate: 14.4 },
		{ year: 2005, birthrate: 14 },
		{ year: 2010, birthrate: 13 },
		{ year: 2015, birthrate: 12.4 }
	];

	const xTicks = [1990, 1995, 2000, 2005, 2010, 2015];
	const yTicks = [0, 5, 10, 15, 20];
	const padding = { top: 20, right: 15, bottom: 20, left: 25 };

	let width = 500;
	let height = 200;
	let birthrate1990 = points[0].birthrate; // Bind input to the birthrate of 1990

	function formatMobile(tick) {
		return "'" + tick.toString().slice(-2);
	}

	// Update points when birthrate1990 changes
	$: points[0].birthrate = birthrate1990;

	let xScale = scaleLinear()
		.domain([0, xTicks.length])
		.range([padding.left, width - padding.right]);

	let yScale = scaleLinear()
		.domain([0, Math.max(...yTicks)])
		.range([height - padding.bottom, padding.top]);

	let innerWidth = width - (padding.left + padding.right);
	let barWidth = innerWidth / xTicks.length;
</script>

<h2>US birthrate by year</h2>

<!-- Input field to change birthrate for the year 1990 -->
<label for="birthrate1990">Birthrate (1990): </label>
<input type="number" id="birthrate1990" bind:value={birthrate1990} min="0" max="20" step="0.1" />

<div class="chart">
	<svg width="100%" height={height}>
		<!-- y axis -->
		<g class="axis y-axis">
			{#each yTicks as tick}
				<g class="tick tick-{tick}" transform="translate(0, {yScale(tick)})">
					<line x2="100%" />
					<text y="-4">{tick} {tick === 20 ? ' per 1,000 population' : ''}</text>
				</g>
			{/each}
		</g>

		<!-- x axis -->
		<g class="axis x-axis">
			{#each points as point, i}
				<g class="tick" transform="translate({xScale(i)}, {height})">
					<text x={barWidth / 2} y="-4">{width > 380 ? point.year : formatMobile(point.year)}</text>
				</g>
			{/each}
		</g>

		<!-- Bars -->
		<g class="bars">
			{#each points as point, i}
				<rect
					x={xScale(i) + 2}
					y={yScale(point.birthrate)}
					width={barWidth - 4}
					height={yScale(0) - yScale(point.birthrate)}
				/>
			{/each}
		</g>
	</svg>
</div>

<style>
	h2 {
		text-align: center;
	}

	.chart {
		width: 100%;
		max-width: 500px;
		margin: 0 auto;
	}

	svg {
		position: relative;
		width: 100%;
		height: 200px;
	}

	.tick {
		font-family: Helvetica, Arial;
		font-size: 0.725em;
		font-weight: 200;
	}

	.tick line {
		stroke: #e2e2e2;
		stroke-dasharray: 2;
	}

	.tick text {
		fill: #ccc;
		text-anchor: start;
	}

	.tick.tick-0 line {
		stroke-dasharray: 0;
	}

	.x-axis .tick text {
		text-anchor: middle;
	}

	.bars rect {
		fill: #a11;
		stroke: none;
		opacity: 0.65;
	}
</style>

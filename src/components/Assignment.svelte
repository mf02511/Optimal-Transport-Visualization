<script>
	import * as d3 from 'd3';
	const width = 700;
	const height = 500;
	const gridSize = 50;
	const radSize = gridSize/3;

	const xGridData = [];
	const yGridData = [];

	for (let i = 1; i < width / gridSize; i++) {
	xGridData.push([[i*gridSize, height],[i*gridSize, 0]])
	}

	for (let i = 1; i < height / gridSize; i++) {
	yGridData.push([[0, i*gridSize], [width, i*gridSize]])
	}

	function xp(x) {return gridSize * x};
	function yp(y) {return height - (gridSize * y)};

	function dist(x, y) {
		return Math.pow(Math.pow(Math.abs(x[0] - y[0]),2) + Math.pow(Math.abs(x[1] - y[1]),2),0.5);
	}

	$: line = d3.line()
	  .x(d => d[0])
	  .y(d => d[1]);

	$: eqRed = [[7, 8, 1], [7, 2, 0]];
	$: eqBlue = [[4, 5, -1], [10, 5, -1]];

	let totalCost = 8.48;
	let costArr = [4.24, 4.24];
	let textY = 50;
	$: i = -1;

	$: highlighted = 0;
	
</script>



<div
	class='assignment-demo'
>
	<p>
  		simple assignment problem
	</p>
	<svg
	  width = {width}
	  height = {height}
	  class='main-plot'
	  viewbox='0 0 {width} {height}'	
	>
	  <defs>
	    <marker
	      id="arrow"
	      viewBox="0 0 10 10"
	      refX="5"
	      refY="5"
	      markerWidth="6"
	      markerHeight="6"
	      fill="#82675e"
	      orient="auto-start-reverse">
	      <path 
	      	d="M 0 0 L 10 5 L 0 10 z" 
	      	stroke="#82675e"
	      />
	    </marker>
	  </defs>

	  <g>
	    {#each xGridData as l}
	      <path
	      	class='grid'
	        d={line(l)}
	      >
	      </path>
	    {/each}
	  </g>
	  <g>
	    {#each yGridData as l}
	      <path
	      	class='grid'
	        d={line(l)}
	      >
	      </path>
	    {/each}
	  </g>
	  {#each eqRed as data}
		<circle
			class = {data[2] === 1 ? 'highlight':'static'}
			cx={xp(data[0])}
		    cy={yp(data[1])}
		    fill='#da7454'
		    r={radSize}
		/>
	  {/each}
	  {#each eqBlue as data}
		  <circle
		  	class = 'static'
		    cx={xp(data[0])}
		    cy={yp(data[1])}
		    fill='#7685c0'
		    r={radSize}
		    on:click={(event) => {
		    	if (data[2] < 0) {
		    		data[2] = highlighted;
		    		eqRed[highlighted][2] = 0;
		    		highlighted++;
		    		if (highlighted < eqRed.length) {
			    		eqRed[highlighted][2] = 1;
			    	}
			    	i++;
			    	let d = costArr[i];
			    	d3.select('#cost')
			    		.append('text')
			    		.attr('x', '100')
			    		.attr('y', textY + 0.5 * textY)
			    		.style('font-size', '24px')
			    		.text('+' + d)
			    	textY += 0.5 * textY;

			    	d3.select('svg')
			    		.append('path')
			    		.attr('d', line([[xp(data[0]), yp(data[1])], [xp(eqRed[data[2]][0]), yp(eqRed[data[2]][1])]]))
			    		.attr('stroke', '#82675e')
			    		.attr('stroke-dasharray', '5,5')
			    		.attr('stroke-width', '2px')
			    		.attr('marker-start', 'url(#arrow)')
			    	return costArr;
		    	} else {
		    		return;
		    	}
		    }}
		  />
	  {/each}
	</svg>

	<svg
		width=200
		height={height}
		class='cost'
		id='cost'
	>
		<text
			x=15
			y=40
			font-weight='600'
			font-size='30px'
		>
			cost:
		</text>

		{#if i === costArr.length - 1}
			<text
				x=20
				y={textY + 0.2 * textY}
				font-weight='600'
			>
				____________________
			</text>

			<text
				x=25
				y={textY + 0.5 * textY}
				font-weight='600'
				font-size='24px'
			>
				total:&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp{totalCost}
			</text>
		{/if}
	</svg>
</div>

<style>
	.assignment-demo {
		margin: auto;
		width: 70%;
	}
	
	text {
		padding: 0;
	}

	.main-plot {
	  	height: auto;
	  	background-color: rgba(235, 235, 235, 0.5);
	  	display: inline;
	  	margin: 20px;
	  	border: #d9bdb2 solid 4px;
	}
	.cost {
		background-color: rgba(235, 235, 235, 0.4);
		display:inline;
		margin: 20px;
		margin-top: 0px;
	}

	.static {
	}

	.highlight {
		stroke: rgba(255, 241, 84, 0.8);
		stroke-width: 4px;

	}
	.grid {
		stroke: #b5b3b3;
	}
</style>
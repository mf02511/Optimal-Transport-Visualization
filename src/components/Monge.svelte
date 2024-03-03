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
		return parseFloat(Math.pow(Math.pow(Math.abs(x[0] - y[0]),2) + Math.pow(Math.abs(x[1] - y[1]),2),0.5).toFixed(2));
	}

	var sym = d3.symbol().type(d3.symbolTriangle).size(500);

	$: N = 0;

	$: line = d3.line()
	  .x(d => d[0])
	  .y(d => d[1]);

	let monRed;
	let monBlue;
	let totalCostM;
	let costArrM;	
	let textYM;
	let iM;
	let highlightedM;
	let optM;

	$: if (N===0) {
		monRed = [[7, 8, radSize * 3, 1], [7, 2, radSize * 1, 0]];
		monBlue = [[4, 5, radSize * 1, -1], [10, 5, radSize * 3 -1]];
		totalCostM = 0;
		costArrM = [];
		textYM = 50;
		iM = -1;
		highlightedM = 0;
		optM = 8.48;
	} else {
		monRed = [[7, 8, radSize * 2, 1], [4, 3, radSize * 2, 0], [12, 2, radSize * 0.5, 0]];
		monBlue = [[10, 6, radSize* 2.5, -1], [9, 3, radSize * 2, -1]];
		totalCostM = 0;
		costArrM = [];
		textYM = 50;
		iM = -1;
		highlightedM = 0;
		optM = 10.89;
	}

	
</script>



<div
	class='assignment-demo'
>
	<p
		class='instruction'
		style='color: #3b2923; font-size: 24px; font-family: "Roboto Condensed", sans-serif'
	>
		WORK IN PROGRESS (Please don't touch this plot)
	</p>
	<svg
		width = {1000}
		height = {50}
		class='buttons'
	>
  		<g>
  			<circle
  				class='prevM'
  				cx='225'
  				cy='25'
  				r='20'
  				fill='lightgrey'
  				stroke='rgba(235, 235, 235, 0.7)'
  				stroke-width='3px'
  				on:click={(event) => {
  					if (N === 0) {
  						return;
  					} else {
  						N--;
  						d3.select('.buttons')
  							.selectAll('.prevM')
  							.attr('fill', 'lightgrey')
  							.attr('stroke', 'rgba(235, 235, 235, 0.7)');

  						d3.select('.buttons')
  							.selectAll('.prev-textM')
  							.attr('fill', 'rgba(235, 235, 235, 0.7)');

  						d3.select('.buttons')
  							.selectAll('.nextM')
  							.attr('fill', 'rgba(235, 235, 235, 0.7)')
  							.attr('stroke', '#d9bdb2');

						d3.select('.buttons')
							.selectAll('.next-textM')
							.attr('fill', '#d9bdb2');
  					}
  				}}
  			/>
  			<text
  				class='prev-textM'
  				x='211.5'
  				y='33'
  				font-size='23px'
  				fill='rgba(235, 235, 235, 0.7)'
  			>
  				&#x25C0;
  			</text>
  			<rect
  				x='300'
  				y='2.5'
  				height='45'
  				width='100'
  				rx='15'
  				stroke='#d9bdb2'
  				stroke-width='3px'
  				fill='rgba(235, 235, 235, 0.7)'
  				on:click={(event) => {
				for (let i = 0; i < monRed.length; i++) {
					if (i === 0) {
						monRed[i][3] = 1;
					} else {
						monRed[i][3] = 0;
					}
				}

				for (let i = 0; i < monBlue.length; i++) {
						monBlue[i][3] = -1;
				}
				highlightedM = 0;
				iM = -1;
				totalCostM = 0;
				costArrM = [];
				textYM = 50;
				d3.select('#cost-monge')
					.selectAll('.dists')
					.remove();
				d3.select('#main-plot-monge')
					.selectAll('.lines')
					.remove();
			}}
  			/>
  			<text
  				x='323'
  				y='32'
  			>
  				reset
  			</text>
  			<circle
  				class='nextM'
  				cx='475'
  				cy='25'
  				r='20'
  				fill='rgba(235, 235, 235, 0.7)'
  				stroke='#d9bdb2'
  				stroke-width='3px'
  				on:click={(event) => {
  					if (N === 1) {
  						return;
  					} else {
  						N++;
  					}
  					d3.select('.buttons')
  							.selectAll('.nextM')
  							.attr('fill', 'lightgrey')
  							.attr('stroke', 'rgba(235, 235, 235, 0.7)');

					d3.select('.buttons')
						.selectAll('.next-textM')
						.attr('fill', 'rgba(235, 235, 235, 0.7)');

					d3.select('.buttons')
  							.selectAll('.prevM')
  							.attr('fill', 'rgba(235, 235, 235, 0.7)')
  							.attr('stroke', '#d9bdb2');

					d3.select('.buttons')
						.selectAll('.prev-textM')
						.attr('fill', '#d9bdb2');

  				}}
  			/>
  			<text
  				class='next-textM'
  				x='467.5'
  				y='33'
  				font-size='23px'
  				fill='#d9bdb2'
  			>
  				&#x25B6;
  			</text>
  		</g>
	</svg>
	<svg
	  width = {width}
	  height = {height}
	  class='main-plot'
	  id='main-plot-monge'
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
	  {#each monRed as data}
		<circle
			class = {data[3] === 1 ? 'highlight':'static'}
			cx={xp(data[0])}
		    cy={yp(data[1])}
		    fill='#da7454'
		    r={data[2]}
		/>
	  {/each}
	  {#each monBlue as data}
		  <circle
		  	class = 'static'
		    cx={xp(data[0])}
		    cy={yp(data[1])}
		    fill='#7685c0'
		    r={data[2]}
		    on:click={(event) => {
		    	iM++;
		    	if (data[3] < 0) {
		    		data[3] = highlightedM;
		    		monRed[highlightedM][3] = 0;
		    		highlightedM++;
		    		if (highlightedM < monRed.length) {
			    		monRed[highlightedM][3] = 1;
			    	}
			    	let d = dist([monRed[data[3]][0], monRed[data[3]][1]], [data[0], data[1]]);
			    	totalCostM += d;
			    	costArrM.push(d);
			    	d3.select('#cost-monge')
			    		.append('text')
			    		.attr('class', 'dists')
			    		.attr('fill', '#3b2923')
			    		.attr('x', '100')
			    		.attr('y', textYM + 30)
			    		.style('font-size', '24px')
			    		.text('+' + d)
			    	textYM += 30;

			    	d3.select('#main-plot-monge')
			    		.append('path')
			    		.attr('class', 'lines')
			    		.attr('d', line([[xp(data[0]), yp(data[1])], [xp(monRed[data[3]][0]), yp(monRed[data[3]][1])]]))
			    		.attr('stroke', '#82675e')
			    		.attr('stroke-dasharray', '5,5')
			    		.attr('stroke-width', '2px')
			    		.attr('marker-start', 'url(#arrow)')
			    	return costArrM;
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
		id='cost-monge'
	>
		<text
			x=15
			y=40
			font-weight='600'
			font-size='30px'
		>
			cost:
		</text>

		{#if iM === monRed.length - 1}
			<text
				x=20
				y={textYM + 20}
				font-weight='600'
			>
				____________________
			</text>

			<text
				x=23
				y={textYM + 50}
				font-weight='600'
				font-size='24px'
			>
				total:&nbsp&nbsp&nbsp&nbsp&nbsp{totalCostM}
			</text>

			<text
				x=15
				y=475
				font-size='30px'
				font-weight='600'
				max-width='90%'
				fill='#bf7f65'
			>
				{#if Math.abs(optM - totalCostM) < 0.01}
					optMimal!!
				{:else}
					not optMimal...
				{/if}
			</text>
		{/if}
	</svg>
</div>

<style>
	.assignment-demo {
		margin: auto;
		width: 70%;
		font-family: "Roboto Condensed", sans-serif;
	}
	
	text {
		padding: 0;
	}

	.buttons {
		display: block;
		margin: auto;
		fill: #3b2923;
		font-size: 28px;
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
		fill: #3b2923;
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
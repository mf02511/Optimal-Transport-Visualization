<script>
	import * as d3 from 'd3';
	const width = 700;
	const height = 500;
	const gridSize = 50;
	const radSize = gridSize/3;
	const baseArea = Math.pow(radSize,2) * Math.PI;
	// console.log(baseArea);

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
	let distArrM;
	let weightArrM;	
	let textYM;
	let iM;
	let highlightedM;
	let optM;

	function rad(size) {
		return radSize * Math.pow(size, 0.5)
	}

	function reset() {
		for (let i = 0; i < monRed.length; i++) {
			if (i === 0) {
				monRed[i][3] = 1;
			} else {
				monRed[i][3] = 0;
			}
		}

		for (let i = 0; i < monBlue.length; i++) {
				monBlue[i][3] = -1;
				monBlue[i][4] = monBlue[i][2];
		}
		highlightedM = 0;
		iM = -1;
		totalCostM = 0;
		distArrM = [];
		weightArrM = [];
		textYM = 50;
		d3.select('#cost-monge')
			.selectAll('.distsM')
			.remove();
		d3.select('#main-plot-monge')
			.selectAll('.linesM')
			.remove();
	}

	$: if (N===0) {
		monRed = [[3, 4, 1, 1], [4, 2, 1, 0], [7, 8, 2, 0], [9, 3, 1, 0]];
		monBlue = [[5, 4, 3, -1, 3], [10, 5, 2, -1, 2]];
		totalCostM = 0;
		distArrM = [];
		weightArrM = [];
		textYM = 50;
		iM = -1;
		highlightedM = 0;
		optM = 16.84;
	} else if (N===1) {
		monRed = [[2, 3, 1, 1], [5, 7, 1, 0], [8, 2, 1, 0], [10, 8, 2, 0], [11, 7, 1, 0]];
		monBlue = [[8, 6, 3, -1, 3], [7, 3, 3, -1, 3]];
		totalCostM = 0;
		distArrM = [];
		weightArrM = [];
		textYM = 50;
		iM = -1;
		highlightedM = 0;
		optM = 19.7;
	} else {
		monRed = [[1, 4, 1, 1], [4, 7, 1, 0], [7, 8, 1, 0], [8, 4, 2, 0], [13, 9, 2, 0]];
		monBlue = [[3, 9, 2, -1, 2], [9, 5, 2, -1, 2], [5, 1, 3, -1, 3]];
		totalCostM = 0;
		distArrM = [];
		weightArrM = [];
		textYM = 50;
		iM = -1;
		highlightedM = 0;
		optM = 31.16;
	}

	
</script>



<div
	class='assignment-demo'
>
	<p
		class='instruction'
		style='color: #3b2923; font-size: 24px; font-family: "Roboto Condensed", sans-serif'
	>
		WORK IN PROGRESS
	</p>
	<svg
		width = {1000}
		height = {50}
		class='buttonsM'
	>
  		<g
  			on:click={(event) => {
  					if (N === 0) {
  						return;
  					} else {
  						N--;
  						if (N === 0) {
  							d3.select('.buttonsM')
  							.selectAll('.prevM')
  							.attr('fill', 'lightgrey')
  							.attr('stroke', 'rgba(235, 235, 235, 0.7)');

	  						d3.select('.buttonsM')
	  							.selectAll('.prev-textM')
	  							.attr('fill', 'rgba(235, 235, 235, 0.7)');
  						}
  						
  						d3.select('.buttonsM')
  							.selectAll('.nextM')
  							.attr('fill', 'rgba(235, 235, 235, 0.7)')
  							.attr('stroke', '#d9bdb2');

						d3.select('.buttonsM')
							.selectAll('.next-textM')
							.attr('fill', '#d9bdb2');
						reset();
  					}
  				}}
  		>
  			<circle
  				class='prevM'
  				cx='225'
  				cy='25'
  				r='20'
  				fill='lightgrey'
  				stroke='rgba(235, 235, 235, 0.7)'
  				stroke-width='3px'
  				
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
  		</g>
		<g
  			on:click={(event) => {
					reset();
				}}
  		>	
  			<rect
  				x='300'
  				y='2.5'
  				height='45'
  				width='100'
  				rx='15'
  				stroke='#d9bdb2'
  				stroke-width='3px'
  				fill='rgba(235, 235, 235, 0.7)'
  			/>
  			<text
  				x='323'
  				y='32'
  			>
  				reset
  			</text>
  		</g>
  		<g
  			on:click={(event) => {
  					if (N === 2) {
  						return;
  					} else {
  						N++;

  						if (N === 2) {
	  						d3.select('.buttonsM')
	  							.selectAll('.nextM')
	  							.attr('fill', 'lightgrey')
	  							.attr('stroke', 'rgba(235, 235, 235, 0.7)');

							d3.select('.buttonsM')
								.selectAll('.next-textM')
								.attr('fill', 'rgba(235, 235, 235, 0.7)');
  						}
  						

						d3.select('.buttonsM')
	  							.selectAll('.prevM')
	  							.attr('fill', 'rgba(235, 235, 235, 0.7)')
	  							.attr('stroke', '#d9bdb2');

						d3.select('.buttonsM')
							.selectAll('.prev-textM')
							.attr('fill', '#d9bdb2');
						reset();
  					}
  					
  				}}
  		>
  			<circle
  				class='nextM'
  				cx='475'
  				cy='25'
  				r='20'
  				fill='rgba(235, 235, 235, 0.7)'
  				stroke='#d9bdb2'
  				stroke-width='3px'
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
	  	<g>
			<circle
				class = {data[3] === 1 ? 'highlight':'static'}
				cx={xp(data[0])}
			    cy={yp(data[1])}
			    fill='#da7454'
			    r={rad(data[2])}
			/>
			<text
				x={xp(data[0])}
				y={yp(data[1]) + 5}
				text-anchor='middle'
				fill='white'
				font-size=14px
			>
				{data[2]}
			</text>
		</g>
	  {/each}
	  {#each monBlue as data}
	  	<g
	  		on:click={(event) => {
		    	if (data[4] > 0 && monRed[highlightedM][2] <= data[4]) {
		    		data[4]-=monRed[highlightedM][2];
		    		iM++;
		    		data[3] = highlightedM;
		    		monRed[highlightedM][3] = 0;
		    		highlightedM++;
		    		if (highlightedM < monRed.length) {
			    		monRed[highlightedM][3] = 1;
			    	}
			    	let d = dist([monRed[data[3]][0], monRed[data[3]][1]], [data[0], data[1]]);
			    	totalCostM += d * monRed[data[3]][2];
			    	distArrM.push(d);
			    	weightArrM.push(monRed[data[3]][2]);
			    	d3.select('#cost-monge')
			    		.append('text')
			    		.attr('class', 'distsM')
			    		.attr('fill', '#3b2923')
			    		.attr('x', '270')
			    		.attr('y', textYM + 30)
			    		.attr("text-anchor", "end")
			    		.style('font-size', '24px')
			    		.text('+' + d.toFixed(2) + ' * ' + monRed[data[3]][2] + ' = ' + (d * monRed[data[3]][2]).toFixed(2))
			    	textYM += 30;

			    	d3.select('#main-plot-monge')
			    		.append('path')
			    		.attr('class', 'linesM')
			    		.attr('d', line([[xp(data[0]), yp(data[1])], [xp(monRed[data[3]][0]), yp(monRed[data[3]][1])]]))
			    		.attr('stroke', '#82675e')
			    		
			    		.attr('pathLength', '8')
			    		.attr('stroke-width', '2px')
			    		.attr('marker-start', 'url(#arrow)')
		    	} else {
		    		return;
		    	}
		    }}
	  	>
		  <circle
		  	class = 'static'
		    cx={xp(data[0])}
		    cy={yp(data[1])}
		    fill='#7685c0'
		    r={rad(data[2])}
		  />
		  <text
			x={xp(data[0])}
			y={yp(data[1]) + 5}
			text-anchor='middle'
			fill='white'
			font-size=14px
		  >
			{data[2]}
		  </text>
		</g>
	  {/each}
	</svg>

	<svg
		width=300
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
				_______________________________________
			</text>

			<text
				x=23
				y={textYM + 50}
				font-weight='600'
				font-size='24px'
			>
				total:&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp{totalCostM.toFixed(2)}
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
					optimal!!
				{:else}
					not optimal...
				{/if}
			</text>
		{/if}
	</svg>
</div>

<style>
	.assignment-demo {
		margin: auto;
		width: 90%;
		font-family: "Roboto Condensed", sans-serif;
	}
	
	text {
		padding: 0;
	}

	.buttonsM {
		display: block;
		margin: auto;
		fill: #3b2923;
		font-size: 28px;
	}
	.main-plot {
	  	height: auto;
	  	background-color: rgba(235, 235, 235, 0.5);
	  	display: inline;
	  	margin-right: 10px;
	  	margin-left: 200px;
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
<script>
	import * as d3 from 'd3';
	const width = 700;
	const height = 500;
	const gridSize = 50;
	const radSize = gridSize/3;
	const baseArea = Math.pow(radSize,2) * Math.PI;

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

	function midpoint(a, b) {
		let midx = (b[0] + a[0]) / 2;
		let midy = (b[1] + a[1]) / 2;
		return [midx, midy];
	}

	/*
	function arrowEndPoints(a, ar, b, br) {
		let endA = a;
		let endB = b;
		if (a[0] < b[0]) {
			if (a[1] == b[1]){
				endA[0] += ar;
				endB[0] -= br;
			}else if (a[1] < b[1]) {
				endA[0] += ar/1.414;
				endB[0] -= br/1.414;
				endA[1] += ar/1.414;
				endB[1] -= br/1.414;
			} else {
				endA[0] += ar/1.414;
				endB[0] -= br/1.414;
				endA[1] -= ar/1.414;
				endB[1] += br/1.414;
			}
		} else if (a[0] > b[0]) {
			if (a[1] == b[1]){
				endA[0] -= ar;
				endB[0] += br;
			}else if (a[1] < b[1]) {
				endA[0] -= ar/1.414;
				endB[0] += br/1.414;
				endA[1] += ar/1.414;
				endB[1] -= br/1.414;
			} else {
				endA[0] -= ar/1.414;
				endB[0] += br/1.414;
				endA[1] -= ar/1.414;
				endB[1] += br/1.414;
			}
		} else {
			if (a[1] < b[1]) {
				endA[1] += ar;
				endB[1] -= br;
			} else if (a[1] > b[1]) {
				endA[1] -= ar;
				endB[1] += br;
			}
		}
		return [endA, endB];
	}
	*/

	function angle(a, b) {
		let theta;
		if ((b[0] - a[0]) != 0) {
			let slope = (b[1] - a[1]) / (b[0] - a[0]);
			theta = Math.atan(slope);
		} else {
			theta = Math.PI / 2;
		}
		return theta;
	}

	function newPoint(a, ar, b, br) {
		let theta = angle(a, b)
		let xm = Math.abs(Math.cos(theta));
		let ym = Math.abs(Math.sin(theta));

		let aa = [a[0], a[1]];
		let bb = [b[0], b[1]];

		// above and to the right
		if ((bb[0] > aa[0]) && (bb[1] < aa[1])) {
			aa[0] = aa[0] + (ar * xm);
			aa[1] = aa[1] - (ar * ym);
			bb[0] = bb[0] - (br * xm);
			bb[1] = bb[1] + (br * ym);
		} 

		// above and to the left
		else if ((b[0] < a[0]) && (b[1] < a[1])) {
			aa[0] = aa[0] - (ar * xm);
			aa[1] = aa[1] - (ar * ym);
			bb[0] = bb[0] + (br * xm);
			bb[1] = bb[1] + (br * ym);
		}

		// below and to the left
		else if ((b[0] < a[0]) && (b[1] > a[1])) {
			aa[0] = aa[0] - (ar * xm);
			aa[1] = aa[1] + (ar * ym);
			bb[0] = bb[0] + (br * xm);
			bb[1] = bb[1] - (br * ym);
		}

		// below and to the right
		else if ((b[0] > a[0]) && (b[1] > a[1])) {
			aa[0] = aa[0] + (ar * xm);
			aa[1] = aa[1] + (ar * ym);
			bb[0] = bb[0] - (br * xm);
			bb[1] = bb[1] - (br * ym);
		}

		// horizontal and to the right
		else if ((b[0] > a[0]) && (b[1] == a[1])) {
			aa[0] = aa[0] + (ar * xm);
        	bb[0] = bb[0] - (br * xm);
		}

		// horizontal and to the left
		else if ((b[0] < a[0]) && (b[1] == a[1])) {
			aa[0] = aa[0] - (ar * xm);
        	bb[0] = bb[0] + (br * xm);
		}

		// vertical and to the top
		else if ((b[0] == a[0]) && (b[1] < a[1])) {
			aa[1] = aa[1] - (ar * ym);
        	bb[1] = bb[1] + (br * ym);
		}

		// vertical and to the bottom
		else if ((b[0] == a[0]) && (b[1] > a[1])) {
			aa[1] = aa[1] + (ar * ym);
        	bb[1] = bb[1] - (br * ym);
		}

		return [aa, bb];
	}

	var sym = d3.symbol().type(d3.symbolTriangle).size(500);

	$: N = 0;

	$: line = d3.line()
	  .x(d => d[0])
	  .y(d => d[1]);

	let kanRed;
	let totalWeight;
	let kanBlue;
	let transMat;
	let matId;
	let totalCostK;
	let distArrK;
	let weightArrK;	
	let textYK;
	let iK;
	let highlightedK;
	let optK;

	let failedK = 0;

	$: if (iK == totalWeight && Math.abs(optK - totalCostK) > 0.01) {
		failedK = 1;
	}

	function rad(size) {
		return radSize * Math.pow(size, 0.5)
	}

	function reset() {
		totalWeight = 0;
		for (let i = 0; i < kanRed.length; i++) {
			if (i === 0) {
				kanRed[i][3] = 1;
			} else {
				kanRed[i][3] = 0;
			}
			kanRed[i][4] = kanRed[i][2];
			totalWeight += kanRed[i][2]
		}

		for (let i = 0; i < kanBlue.length; i++) {
				kanBlue[i][3] = -1;
				kanBlue[i][4] = kanBlue[i][2];
		}

		for (let i = 0; i < transMat.length; i++) {
			for (let j = 0; j < transMat[i].length; j++) {
				transMat[i][j] = 0;
			}
		}
		highlightedK = 0;
		iK = 0;
		totalCostK = 0;
		distArrK = [];
		weightArrK = [];
		textYK = 80;
		failedK = 0;
		d3.select('#cost-kanto')
			.selectAll('.distsK')
			.remove();
		d3.select('#main-plot-kanto')
			.selectAll('.linesK')
			.remove();
		d3.select('#main-plot-kanto')
			.selectAll('.arrowText')
			.remove();
	}

	$: if (N===0) {
		kanRed = [[8, 6, 4, 1, 4], [4, 2, 1, 0, 1]];
		kanBlue = [[5, 4, 3, -1, 3, 0], [10, 5, 2, -1, 2, 1]];
		transMat = [[0, 0], [0, 0]];
		matId = [['aa', 'ab'], ['ba', 'bb']];
		totalWeight = 5;
		totalCostK = 0;
		distArrK = [];
		weightArrK = [];
		textYK = 80;
		iK = 0;
		highlightedK = 0;
		optK = 13.94;
	} else if (N===1) {
		kanRed = [[2, 9, 2, 1, 2], [4, 3, 1, 0, 1], [8, 8, 3, 0, 3], [9, 5, 2, 0, 2], [11, 7, 2, 0, 2]];
		kanBlue = [[11, 4, 4, -1, 4, 0], [7, 2, 3, -1, 3, 1], [5, 7, 3, -1, 3, 2]];
		transMat = [[0, 0, 0], [0, 0, 0], [0, 0, 0], [0, 0, 0], [0, 0, 0]];
		matId = [['aa', 'ab', 'ac'], ['ba', 'bb', 'bc'], ['ca', 'cb', 'cc'], ['da', 'db', 'dc'], ['ea', 'eb', 'ec']];
		totalWeight = 10;
		totalCostK = 0;
		distArrK = [];
		weightArrK = [];
		textYK = 80;
		iK = 0;
		highlightedK = 0;
		optK = 36.47;
	} else {
		kanRed = [[2, 3, 4, 1, 4], [7, 5, 3, 0, 3], [10, 6, 3, 0, 3]];
		kanBlue = [[4, 5, 2, -1, 2, 0], [5, 2, 1, -1, 1, 1], [6, 9, 2, -1, 2, 2], [9, 8, 3, -1, 3, 3], [13, 5, 2, -1, 2, 4]];
		transMat = [[0, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0]];
		matId = [['aa', 'ab', 'ac', 'ad', 'ae'], ['ba', 'bb', 'bc', 'bd', 'be'], ['ca', 'cb', 'cc', 'cd', 'ce']];
		totalWeight = 10;
		totalCostK = 0;
		distArrK = [];
		weightArrK = [];
		textYK = 80;
		iK = 0;
		highlightedK = 0;
		optK = 35.93;
	}

	
</script>



<div
	class='assignment-demo'
>
	<p
		class='instruction'
		style='color: #3b2923; font-size: 20px; font-family: "Roboto Condensed", sans-serif'
	>
		&nbsp;&nbsp;&nbsp; map the highlighted <span style='color: #da7454'>red</span> point to a <span style='color: #7685c0'>blue</span> point:
	</p>
	<svg
		width = {1000}
		height = {50}
		class='buttonBar'
	>
  		<g
  			class='buttonsK'
  			on:click={(event) => {
  					if (N === 0) {
  						return;
  					} else {
  						N--;
  						if (N === 0) {
  							d3.selectAll('.buttonsK')
  							.selectAll('.prevK')
  							.attr('fill', 'lightgrey')
  							.attr('stroke', 'rgba(235, 235, 235, 0.7)');

	  						d3.selectAll('.buttonsK')
	  							.selectAll('.prev-textK')
	  							.attr('fill', 'rgba(235, 235, 235, 0.7)');
  						}
  						
  						d3.selectAll('.buttonsK')
  							.selectAll('.nextK')
  							.attr('fill', 'rgba(235, 235, 235, 0.7)')
  							.attr('stroke', '#d9bdb2');

						d3.selectAll('.buttonsK')
							.selectAll('.next-textK')
							.attr('fill', '#d9bdb2');
						reset();
  					}
  				}}
  		>
  			<circle
  				class='prevK'
  				cx='225'
  				cy='25'
  				r='20'
  				fill='lightgrey'
  				stroke='rgba(235, 235, 235, 0.7)'
  				stroke-width='3px'
  				
  			/>
  			<text
  				class='prev-textK'
  				x='211.5'
  				y='33'
  				font-size='23px'
  				fill='rgba(235, 235, 235, 0.7)'
  			>
  				&#x25C0;
  			</text>
  		</g>
		<g
			class='buttonsK'
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
  				class={failedK === 1 ? 'highlight':'static'}
  			/>
  			<text
  				x='323'
  				y='32'
  			>
  				reset
  			</text>
  		</g>
  		<g
  			class='buttonsK'
  			on:click={(event) => {
  					if (N === 2) {
  						return;
  					} else {
  						N++;

  						if (N === 2) {
	  						d3.selectAll('.buttonsK')
	  							.selectAll('.nextK')
	  							.attr('fill', 'lightgrey')
	  							.attr('stroke', 'rgba(235, 235, 235, 0.7)');

							d3.selectAll('.buttonsK')
								.selectAll('.next-textK')
								.attr('fill', 'rgba(235, 235, 235, 0.7)');
  						}
  						

						d3.selectAll('.buttonsK')
	  							.selectAll('.prevK')
	  							.attr('fill', 'rgba(235, 235, 235, 0.7)')
	  							.attr('stroke', '#d9bdb2');

						d3.selectAll('.buttonsK')
							.selectAll('.prev-textK')
							.attr('fill', '#d9bdb2');
						reset();
  					}
  					
  				}}
  		>
  			<circle
  				class={(iK == totalWeight && N < 2 && failedK === 0) ? 'highlight nextK':'static nextK'}
  				cx='475'
  				cy='25'
  				r='20'
  				fill='rgba(235, 235, 235, 0.7)'
  				stroke='#d9bdb2'
  				stroke-width='3px'

  			/>
  			<text
  				class='next-textK'
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
	  id='main-plot-kanto'
	  viewbox='0 0 {width} {height}'	
	>
	  <defs>
	    <marker
	      id="arrowK"
	      viewBox="0 0 10 10"
	      refX="5"
	      refY="5"
	      markerWidth="6"
	      markerHeight="6"
	      fill='rgba(130, 103, 94)'
	      orient="auto-start-reverse">
	      <path 
	      	d="M 0 0 L 10 5 L 0 10 z" 
	      	stroke="rgba(130, 103, 94)"
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
	  {#each kanRed as data}
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
				y={yp(data[1]) + 8}
				text-anchor='middle'
				fill='white'
				font-size=24
				font-weight=600
			>
				{data[2]}
			</text>
		</g>
	  {/each}
	  {#each kanBlue as data}
	  	<g
	  		class='bluePoint'
	  		on:click={(event) => {
		    	if (data[4] > 0) {
		    		data[4]--;
		    		iK++;
		    		data[3] = highlightedK;
		    		transMat[data[3]][data[5]]++;
		    		let d = dist([kanRed[data[3]][0], kanRed[data[3]][1]], [data[0], data[1]]);
			    	totalCostK += d;
			    	distArrK.push(d);
			    	weightArrK.push(kanRed[data[3]][2]);
			    	d3.select('#cost-kanto')
			    		.append('text')
			    		.attr('class', 'distsK')
			    		.attr('fill', '#3b2923')
			    		.attr('x', '270')
			    		.attr('y', textYK + 30)
			    		.attr("text-anchor", "end")
			    		.style('font-size', '24px')
			    		.text('+' + d.toFixed(2) + ' * 1 = ' + d.toFixed(2))
			    	textYK += 30;

			    	d3.select('#main-plot-kanto')
			    		.append('path')
			    		.attr('class', 'linesK')
			    		.attr('d', line(newPoint([xp(data[0]), yp(data[1])], rad(data[2]), [xp(kanRed[data[3]][0]), yp(kanRed[data[3]][1])], rad(kanRed[data[3]][2]))))
			    		.attr('stroke', 'rgba(130, 103, 94, 0.5)')
			    		.attr('stroke-dasharray', '5,5')
			    		.attr('stroke-width', '2px')
			    		.attr('marker-start', 'url(#arrowK)')

			    	let mid = midpoint([data[0], data[1]],[kanRed[data[3]][0], kanRed[data[3]][1]]);

			    	let textId = matId[data[3]][data[5]];
			    	if (transMat[data[3]][data[5]] == 1) {
			    		d3.select('#main-plot-kanto')
			    		.append('text')
			    		.attr('class', 'arrowText')
			    		.attr('id', textId)
			    		.attr('x', xp(mid[0]) - 5)
			    		.attr('y', yp(mid[1]) + 8)
			    		.attr('fill', '#3b2923')
			    		.attr('text-anchor', 'middle')
			    		
			    		.attr('style', 'font-size: 24px; font-weight: 600')
			    		.text(transMat[data[3]][data[5]])
			    	} else {
			    		d3.select('#main-plot-kanto')
			    		.select('#'+textId)
			    		.text(transMat[data[3]][data[5]])
			    	}
			    	

		    		kanRed[highlightedK][4]--;
		    		if (kanRed[highlightedK][4] === 0) {
		    			kanRed[highlightedK][3] = 0;
		    			highlightedK++;
		    			if (highlightedK < kanRed.length) {
				    		kanRed[highlightedK][3] = 1;
				    	}
		    		}
			    	
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
			y={yp(data[1]) + 8}
			text-anchor='middle'
			fill='white'
			font-size=24
			font-weight=600
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
		id='cost-kanto'
	>
		<text
			x=15
			y=40
			font-weight='600'
			font-size='30px'
		>
			cost:
		</text>
		<text
			x=15
			y=70
			font-weight='400'
			font-size='24px'
		>
			&sum;(distance(i,j) * mass(i,j))=
		</text>

		{#if iK == totalWeight}
			<text
				x=20
				y={textYK + 20}
				font-weight='600'
			>
				_______________________________________
			</text>

			<text
				x=23
				y={textYK + 50}
				font-weight='600'
				font-size='24px'
			>
				total:&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp{totalCostK.toFixed(2)}
			</text>

			<text
				x=15
				y=475
				font-size='30px'
				font-weight='600'
				max-width='90%'
				fill='#bf7f65'
			>
				{#if Math.abs(optK - totalCostK) < 0.01}
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

	.buttonBar {
		display: block;
		margin: auto;
		fill: #3b2923;
		font-size: 28px;
	}
	.buttonsK:hover {
		cursor: pointer;
	}
	.bluePoint:hover {
		cursor: pointer;
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
		stroke-width: 8px;
		animation: blinking 0.8s linear infinite alternate-reverse;

	}
	.grid {
		stroke: #b5b3b3;
	}

	@keyframes blinking {
	  from { stroke-opacity: 1; }
	  to { stroke-opacity: 0.1; }
	}
</style>
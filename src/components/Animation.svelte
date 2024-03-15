<script>
import * as d3 from 'd3';
$: line = d3.line()
          .x(d => d[0])
          .y(d => d[1]);

let factories = [[270, 0], [200, 250]];
let stores = [[1000, 0], [1180, 150], [1020, 350]];

let play = 0;

</script>

<svg
	class='animateButtonBand'
	width='100%'
	height='100'
>
	<g
		class={play===0 ? 'animateButton': 'disabled'}
		on:click={(event)=>{
			play=1;
		}}
	>
		<rect
			class={play===0 ? 'animateButtonRect': 'disabledRect'}
			x='700'
			y='51%'
			height='45'
			width='150'
			rx='15'
			
		/>
		<text
			class={play===0 ? 'animateButtonText': 'disabledText'}
			x='717'
			y='80%'
			text-anchor='left'
			style='font-family: "Roboto Condensed", sans-serif; font-size: 28px'
		>
			transport!!
		</text>
	</g>

	<g
		class={play===1 ? 'resetAnimate':'disabled'}
		on:click={(event)=>{
			play=0;
		}}
	>
		<circle
			class={play===1 ? 'resetAnimate':'disabledCircle'}
			cx='890'
			cy='75%'
			r='20'
			fill='rgba(235, 235, 235, 0.7)'
  			stroke='#d9bdb2'
  			stroke-width='3px'
  		/>
		<text
			class={play===1 ? 'resetAnimateText':'disabledText'}
			x='890'
			y='82%'
			text-anchor='middle'
			style='font-size: 30px'
		>
			&#8635;
		</text>
	</g>
</svg>
<svg
	class='animation'
	width='100%'
	height='600px'
>
	
	<image
		href='background.svg'
		x='120'
		y='10'
		style='width: 1200'
	/>

	{#each factories as fact}
		<image 
			class='factory'
			href='factory.svg' 
			x='{fact[0]}'
			y='{fact[1]}'
		/>
	{/each}
	{#each stores as store}
		<image 
			class='store'
			href='store.svg' 
			x='{store[0]}'
			y='{store[1]}'
		/>
	{/each}

	<g
		class='boxesA'
	>
		<image
			class={play===1 ? 'boxAA':'box'}
			href='box.svg'
			x=430
			y=80
		/>

		<image
			class={play===1 ? 'boxAB':'box'}
			href='box.svg'
			x=400
			y=120
		/>
		<image
			class={play===1 ? 'boxAC':'box'}
			href='box.svg'
			x=460
			y=120
		/>
	</g>

	<g
		class='boxesB'
	>

		<image
			class={play===1 ? 'boxBA':'box'}
			href='box.svg'
			x=330
			y=400
		/>
		<image
			class={play===1 ? 'boxBB':'box'}
			href='box.svg'
			x=390
			y=400
		/>
	</g>

	<g
		class='slotsA'
	>
		<rect
			x=870
			y=80
			width=50
			height=50
			fill=none
			stroke='#82675e'
			stroke-width='2px'
			stroke-dasharray='3,3'
		/>
		<rect
			x=930
			y=80
			width=50
			height=50
			fill=none
			stroke='#82675e'
			stroke-width='2px'
			stroke-dasharray='3,3'
		/>
	</g>

	<g
		class='slotsB'
	>
		<rect
			x=1050
			y=220
			width=50
			height=50
			fill=none
			stroke='#82675e'
			stroke-width='2px'
			stroke-dasharray='3,3'
		/>
		<rect
			x=1110
			y=220
			width=50
			height=50
			fill=none
			stroke='#82675e'
			stroke-width='2px'
			stroke-dasharray='3,3'
		/>
	</g>

	<g
		class='slotsC'
	>
		<rect
			x=950
			y=420
			width=50
			height=50
			fill=none
			stroke='#82675e'
			stroke-width='2px'
			stroke-dasharray='3,3'
		/>
		
	</g>

	<image
		class={play===1 ? 'truckAA': 'truck'}
		href='truck.svg'
		x='520'
		y='100'
	/>
	<image
		class={play===1 ? 'truckAB': 'truck'}
		href='truck.svg'
		x='520'
		y='120'
	/>
	<image
		class={play===1 ? 'truckBB': 'truck'}
		href='truck.svg'
		x='450'
		y='380'
	/>
	<image
		class={play===1 ? 'truckBC': 'truck'}
		href='truck.svg'
		x='410'
		y='400'
	/>

	<g
		class={play===1 ? 'arrowAA': 'arrow'}
	>
		<path
			d={line([[390, 120], [865, 100]])}
			stroke='#82675e'
	        stroke-dasharray='5,5'
	        stroke-width='2px'
	        marker-end='url(#arrow)'
		/>
		<text
			x='700'
			y='100'
			text-anchor='middle'
			fill='#82675e'
			font-size='28px'
			font-weight='600'
		>
			2
		</text>
	</g>
	<g
		class={play===1 ? 'arrowAB': 'arrow'}
	>
		<path
			d={line([[390, 120], [1130, 220]])}
			stroke='#82675e'
	        stroke-dasharray='5,5'
	        stroke-width='2px'
	        marker-end='url(#arrow)'
		/>
		<text
			x='700'
			y='155'
			text-anchor='middle'
			fill='#82675e'
			font-size='28px'
			font-weight='600'
		>
			1
		</text>
	</g>
	<g
		class={play===1 ? 'arrowBB': 'arrow'}
	>
	<path
		d={line([[320, 370], [1045, 245]])}
		stroke='#82675e'
        stroke-dasharray='5,5'
        stroke-width='2px'
        marker-end='url(#arrow)'
	/>
	<text
		x='700'
		y='300'
		text-anchor='middle'
		fill='#82675e'
		font-size='28px'
		font-weight='600'
	>
		1
	</text>
	</g>
	<g
		class={play===1 ? 'arrowBC': 'arrow'}
	>
		<path
			d={line([[320, 370], [940, 445]])}
			stroke='#82675e'
	        stroke-dasharray='5,5'
	        stroke-width='2px'
	        marker-end='url(#arrow)'
		/>
		<text
			x='700'
			y='410'
			text-anchor='middle'
			fill='#82675e'
			font-size='28px'
			font-weight='600'
		>
			1
		</text>
	</g>
</svg>
	
<style>
	.animation {
		margin-left: 0px;
	}

	text {
		font-family: "Roboto Condensed", sans-serif; 
	}
	.factory {
		width: 120px;
	}
	.store {
		width: 120px;
	}
	.truck {
		width: 100px;
		opacity: 0;
	}
	.box {
		width: 50px;
	}
	.arrow {
		opacity: 0;
	}
	.animateButtonRect {
		stroke-width: 3px;
		fill: rgba(235, 235, 235, 0.7);
		stroke: #d9bdb2;
	}
	.animateButtonText {
		fill: #3b2923;
	}
	.animateButton:hover {
		cursor: pointer;
	}
	.resetAnimate:hover {
		cursor: pointer;
	}
	.resetAnimateText {
		fill: #3b2923;
	}
	.disabledCircle {
		fill: lightgrey;
		stroke: rgba(235, 235, 235, 0.7);
	}
	.disabledText {
		fill: rgba(235, 235, 235, 0.7);
	}
	.disabledRect {
		fill: lightgrey;
		stroke: rgba(235, 235, 235, 0.7);
		stroke-width: 3px;
	}

	.boxAA {
		width: 50px;
		transform: translate(500px, 0px);
		animation-name: moveAA;
		animation-duration: 4s;
		animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
	}

	.boxAB {
		width: 50px;
		transform: translate(470px, -40px);
		animation-name: moveAB;
		animation-duration: 4s;
		animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
	}
	.boxAC {
		width: 50px;
		transform: translate(650px, 100px);
		animation-name: moveAC;
		animation-duration: 8s;
		animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
	}
	.boxBB {
		width: 50px;
		transform: translate(660px, -180px);
		animation-name: moveBB;
		animation-duration: 12s;
		animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
	}
	.boxBA {
		width: 50px;
		transform: translate(620px, 20px);
		animation-name: moveBA;
		animation-duration: 16s;
		animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
	}
	@keyframes moveAA {
    	0% {
      		transform: translate(0px, 0px);
    	}
    	100% {
      		transform: translate(500px, 0px);
    	}
    }
    @keyframes moveAB {
    	0% {
      		transform: translate(0px, 0px);
    	}
    	100% {
      		transform: translate(470px, -40px);
    	}
    }
    @keyframes moveAC {
    	0% {
      		transform: translate(0px, 0px);
    	}
    	50% {
      		transform: translate(0px, 0px);
    	}
    	100% {
      		transform: translate(650px, 100px);
    	}
    }
    @keyframes moveBB {
    	0% {
      		transform: translate(0px, 0px);
    	}
    	66% {
      		transform: translate(0px, 0px);
    	}
    	100% {
      		transform: translate(660px, -180px);
    	}
    }
    @keyframes moveBA {
    	0% {
      		transform: translate(0px, 0px);
    	}
    	75% {
      		transform: translate(0px, 0px);
    	}
    	100% {
      		transform: translate(620px, 20px);
    	}
    }

    .truckAA {
    	width: 80px;
    	opacity: 0;
    	animation-name: moveTruckAA;
		animation-duration: 4s;
		animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
    }
    @keyframes moveTruckAA {
    	0% {
    		opacity: 1;
      		transform: translate(0px, 0px);
    	}
    	85% {
    		opacity: 1;
    	}
    	100% {
    		opacity: 0;
      		transform: translate(500px, 0px);
    	}
    }

    .truckAB {
    	width: 80px;
    	opacity: 0;
    	animation-name: moveTruckAB;
		animation-duration: 8s;
		animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
    }

    @keyframes moveTruckAB {
    	0% {
    		opacity: 0;
      		transform: translate(0px, 0px);
    	}
    	49% {
    		opacity: 0;
    	}
    	50% {
    		opacity: 1;
      		transform: translate(0px, 0px);
    	}
    	90% {
    		opacity: 1;
    	}
    	100% {
    		opacity: 0;
      		transform: translate(650px, 100px);
    	}
    }
    .truckBB {
    	width: 80px;
    	opacity: 0;
    	animation-name: moveTruckBB;
		animation-duration: 12s;
		animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
    }
    @keyframes moveTruckBB {
    	0% {
    		opacity: 0;
      		transform: translate(0px, 0px);
    	}
    	65% {
    		opacity: 0;
    	}
    	66% {
    		opacity: 1;
      		transform: translate(0px, 0px);
    	}
    	93% {
    		opacity: 1;
    	}
    	100% {
    		opacity: 0;
      		transform: translate(660px, -180px);
    	}
    }
    .truckBC {
    	width: 80px;
    	opacity: 0;
    	animation-name: moveTruckBC;
		animation-duration: 16s;
		animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
    }
    @keyframes moveTruckBC {
    	0% {
    		opacity: 0;
      		transform: translate(0px, 0px);
    	}
    	74% {
    		opacity: 0;
    	}
    	75% {
    		opacity: 1;
      		transform: translate(0px, 0px);
    	}
    	95% {
    		opacity: 1;
    	}
    	100% {
    		opacity: 0;
      		transform: translate(620px, 20px);
    	}
    }
    .arrowAA {
    	opacity: 1;
    	animation-name: showArrowAA;
    	animation-duration: 4s;
    	animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
    }
    @keyframes showArrowAA {
    	0% {
      		opacity: 0;
    	}
    	90% {
    		opacity: 0;
    	}
    	100% {
      		opacity: 1;
    	}
    }
    .arrowAB {
    	opacity: 1;
    	animation-name: showArrowAB;
    	animation-duration: 8s;
    	animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
    }
    @keyframes showArrowAB {
    	0% {
      		opacity: 0;
    	}
    	90% {
    		opacity: 0;
    	}
    	100% {
      		opacity: 1;
    	}
    }

    .arrowBB {
    	opacity: 1;
    	animation-name: showArrowBB;
    	animation-duration: 12s;
    	animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
    }
    @keyframes showArrowBB {
    	0% {
      		opacity: 0;
    	}
    	92% {
    		opacity: 0;
    	}
    	100% {
      		opacity: 1;
    	}
    }
    .arrowBC {
    	opacity: 1;
    	animation-name: showArrowBC;
    	animation-duration: 16s;
    	animation-iteration-count: 1;
		animation-timing-function: ease-in-out;
    }
    @keyframes showArrowBC {
    	0% {
      		opacity: 0;
    	}
    	95% {
    		opacity: 0;
    	}
    	100% {
      		opacity: 1;
    	}
    }
</style>

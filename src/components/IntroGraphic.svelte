<script>
	import * as d3 from 'd3';
	$: line = d3.line()
              .x(d => d[0])
              .y(d => d[1]);

    let factories = [[350, 0], [200, 250]];
    let stores = [[850, 0], [1150, 150], [1000, 350]];
</script>

<svg
	width='100%'
	height='600px'
	class='intro-graphic'
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
	<path
		d={line([[470, 120], [850, 100]])}
		stroke='#82675e'
        stroke-dasharray='5,5'
        stroke-width='2px'
        marker-end='url(#arrow)'
	/>
	<path
		d={line([[470, 120], [1000, 450]])}
		stroke='#82675e'
        stroke-dasharray='5,5'
        stroke-width='2px'
        marker-end='url(#arrow)'
	/>
	<path
		d={line([[320, 370], [1150, 250]])}
		stroke='#82675e'
        stroke-dasharray='5,5'
        stroke-width='2px'
        marker-end='url(#arrow)'
	/>

	<image
		class='truck'
		id='truckA'
		href='truck.svg'
		x='480'
		y='90'
	/>

	<image
		class='truck'
		id='truckB'
		href='truck.svg'
		x='480'
		y='-190'
	/>
	<image
		class='truck'
		id='truckC'
		href='truck.svg'
		x='270'
		y='360'
	/>
</svg>

<style>
	.intro-graphic {
		margin-left: 25px;
	}
	.factory {
		width: 120px;
	}
	.store {
		width: 120px;
	}
	.truck {
		width: 100px;
	}
	#truckA {
		animation-name: truckARun;
		animation-duration: 10s;
		animation-iteration-count: infinite;
		animation-timing-function: ease-in-out;
	}

	#truckB {
		animation-name: truckBRun;
		animation-duration: 10s;
		animation-iteration-count: infinite;
		animation-timing-function: ease-in-out;
	}

	#truckC {
		animation-name: truckCRun;
		animation-duration: 10s;
		animation-iteration-count: infinite;
		animation-timing-function: ease-in-out;
	}

	@keyframes truckARun {
    	0% {
      		transform: translate(0px, 0px) rotate(-3.01deg);
    	}
    	60% {
      		transform: translate(260px, -20px) rotate(-3.01deg);
    	}
    	100% {
      		transform: translate(260px, -20px) rotate(-3.01deg);
    	}
    }

    @keyframes truckBRun {
    	0% {
      		transform: translate(0px, 0px) rotate(30deg);
    	}
    	75% {
      		transform: translate(420px, 270px) rotate(30deg);
    	}
    	100% {
      		transform: translate(420px, 270px) rotate(30deg);
    	}
    }

    @keyframes truckCRun {
    	0% {
      		transform: translate(0px, 0px) rotate(-8.5deg);
    	}
    	100% {
      		transform: translate(720px, -105px) rotate(-8.5deg);
    	}
    }
  </style>


<script>
  import * as d3 from 'd3';
  import Assignment from './Assignment.svelte';
  import Monge from './Monge.svelte';
  import Kantoro from './Kantoro.svelte';

  $: line = d3.line()
              .x(d => d[0])
              .y(d => d[1]);

  $: console.log()
</script>

<main>
  <img src='truck.svg' id='title-truck'/>
  <h1>Optimal Transport Mapping</h1>
  <div id='main-body'>
    <p class='intro'>Wasserstein distance, also known as earth mover's distance, is a measure of distance between two probability distributions, which has a variety of applications in machine learning. This distance is calculated by finding the path that minimizes the total cost of moving one distribution to another, known as the <b>optimal transport map</b>. This problem was first proposed by French mathematician Gaspard Monge in 1781. <br> <br> As an analogy, if we view one distribution <i>X</i> as piles of sand and the other distribution <i>Y</i> as buckets of varying sizes, the objective is to find the most cost efficient way to fill the buckets with the sand, where the cost of each transport is equal to the distance times the weight of the sand.</p>
    <div class='intro-images'>
      <img src='factory.svg' class='factory' id='factoryA'/>
      <img src='store.svg' class='store' id='storeA'/>
      <svg
        width= '1500'
        height='450'
      >
      
        <path
          class='intro-line'
          d={line([[350, 100], [950, 90]])}
          stroke='#82675e'
          stroke-dasharray='5,5'
          stroke-width='2px'
        />

        <path
          class='intro-line'
          d={line([[350, 100], [1100, 370]])}
          stroke='#82675e'
          stroke-dasharray='5,5'
          stroke-width='2px'
        />
        <path
          class='intro-line'
          d={line([[200, 335], [1300, 180]])}
          stroke='#82675e'
          stroke-dasharray='5,5'
          stroke-width='2px'
        />
      </svg>
      <img src='factory.svg' class='factory' id='factoryB'/>
      <img src='store.svg' class='store' id='storeB'/>
      <img src='store.svg' class='store' id='storeC'/>
      <img src='truck.svg' class='truck' id='truckA'/>
      <img src='truck.svg' class='truck' id='truckB'/>
      <img src='truck.svg' class='truck' id='truckC'/>
    </div>
    <h2><i>Optimal Assignment Problem</i></h2>
    <p>Let's start by looking at a simple case, often referred to as the optimal assignment problem. Consider 2 point sets, <i>X</i> and <i>Y</i>, both with <i>n</i> points of the same weight. Due to the uniform distribution of weights, when transporting <i>X</i> to <i>Y</i>, we only need to account for the distance between points when calculating the cost. In this situation, finding the optimal transport map between <i>X</i> and <i>Y</i> is equivalent to a one-to-one assignment problem where we try to minimize the total distance the points travel. <br><br> Below, try to find the optimal assignment by mapping the red point set to the blue point set &mdash; for each highlighted red point, click on a blue point to assign it to and once all points have been assigned, check out if the transport map you created is indeed the optimal!</p>
    <br>
    
    <Assignment />

    <h2><i>Monge Problem</i></h2>
    <p>Now let's consider a more general case where the weights of all points in <i>X</i> and <i>Y</i> are not necessarily equal and do not necessarily have the same number of points, known as the Monge problem. Since the weights of all points are no longer equal, we cannot simply do a one-to-one assignment and minimize only the distance. We must now also take into account the individual weights of each point when finding the optimal map. The main constraint in this situation is mass conservation: the total mass of the points in <i>X</i> must be equal to the total mass of points in <i>Y</i>.</p>
    <br>
    <Monge />
    <br>
    <Kantoro />
  </div>
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=EB+Garamond:ital,wght@0,400..800;1,400..800&family=Instrument+Serif:ital@0;1&display=swap');

  @import url('https://fonts.googleapis.com/css2?family=Barlow+Condensed:ital,wght@0,100;0,200;0,300;0,400;0,gridSize0;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,gridSize0;1,600;1,700;1,800;1,900&family=Instrument+Serif:ital@0;1&display=swap');


  @import url('https://fonts.googleapis.com/css2?family=EB+Garamond:ital,wght@0,400..800;1,400..800&family=Instrument+Serif:ital@0;1&family=Roboto+Condensed:ital,wght@0,100..900;1,100..900&display=swap');

  :global(body) {
    background-color: #e6dad5;
  }

  main {
    font-family: "EB Garmond", serif;
    color: #3b2923;
  }

  h1 {
    font-family: "EB Garmond", serif;
    text-align: center;
    font-weight: 600;
    font-size: 40px;
    opacity: 100;
    animation-duration: 4s;
    animation-name: titleFade;
    animation-timing-function: linear;
  }

  p {
    font-size: 20px;
    text-align: left;
    margin-left: 50px;
    margin-right: 50px;
    line-height: 1.25;
  }

  h2 {
    font-family: "EB Garmond", serif;
    font-weight: 600;
    font-size: 30px;
    margin-left: 45px;
  }

  #title-truck {
    position: absolute;
    opacity: 0;
    width: 180px;
    animation: linear;
    animation-iteration-count: 1;
    animation-duration: 4s;
    animation-name: truckMove;
  }

  #main-body {
    animation-duration: 5s;
    animation-name: bodyFade;
  }

  .intro-images {
    height: 450px;
  }
  .factory {
    width: 120px;
  }
  .store {
    width: 120px;
  }
  .truck {
    width: 90px;
  }

  #factoryA {
    position: absolute;
    left: 240px;
    top: 250px;
  }
  #factoryB {
    position: absolute;
    left: 90px;
    top: 475px;
  }

  #storeA {
    position: absolute;
    left: 950px;
    top: 265px;
  }
  #storeB {
    position: absolute;
    left: 1250px;
    top: 370px;
  }
  #storeC {
    position: absolute;
    left: 1050px;
    top: 550px;
  }

  #truckA {
    position: absolute;
    left: 580px;
    top: 315px;
  }

  #truckB {
    position: absolute;
    left: 660px;
    top: 440px;
    transform: rotate(20deg);
  }

  #truckC {
    position: absolute;
    left: 360px;
    top: 525px;
    transform: rotate(-8deg);
  }

  @keyframes titleFade {
    0% {
      opacity: 0;
    }
    25% {
      opacity: 0;
    }
    50% {
      opacity: 50%;
    }
    80% {
      opacity: 100%;
    }
    100% {
      opacity: 100%;
    }
  }

  @keyframes truckMove {
    0% {
      left: 0;
      opacity: 100%;
    }
    100% {
      left: 100%;
      opacity: 100%;
    }
  }

  @keyframes bodyFade {
    0% {
      opacity: 0%;
    }
    75% {
      opacity: 0%;
    }
    100% {
      opacity: 100%;
    }
  }
</style>

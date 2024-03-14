<script>
  import * as d3 from 'd3';
  import Assignment from './Assignment.svelte';
  import Monge from './Monge.svelte';
  import Kantoro from './Kantoro.svelte';
  import Animation from './Animation.svelte';
  import IntroGraphic from './IntroGraphic.svelte';

  

  $: console.log()
</script>

<main>
  <img src='truck.svg' id='title-truck'/>
  <h1>Optimal Transport Mapping</h1>
  <div id='main-body'>
    <p class='intro'>
    Wasserstein distance, also known as Earth Mover's Distance, is a measure of distance between two probability distributions, which has a variety of applications in modern machine learning. This distance is calculated by finding the path that minimizes the total cost of transferring mass from one distribution to the other, referred to as the <b>optimal transport map</b>. This problem was first introduced by French mathematician Gaspard Monge in 1781. 
    <br><br> 
    As an analogy, suppose we view one distribution <i>X</i> as as a collection of factories from a single company and the other distribution <i>Y</i> as a collection of stores that receive shipments from the factories. Suppose we also have a cost function <i>c</i> such that <i>c(x<sub>i</sub>,y<sub>j</sub>)</i> calculates the cost of transporting one shipment of material from factory <i>x<sub>i</sub></i> to store <i>y<sub>j</sub></i>.
    Below is an illustration of one possible path of deliveries between the factories and the stores that the company could choose. However, with multiple other potential delivery paths between the factories and the stores, a dilemma arises: how should the company decide which path to choose?
    </p>
    <IntroGraphic />
    <p>
    The key is for the company to take into account factors such as the distance and amount of material being transported to calculate the cost of each delivery. Therefore, the most business-sound decision for the company would be to find the path that is the most cost-efficient based on these factors, which is exactly the premise of optimal transport mapping. 
    <br><br>
    The following series of interactive simulations introduces the concepts of optimal transport, from the very basic case to the more complex cases. 
    </p>
    <h2><i>Optimal Assignment Problem</i></h2>
    <p>
    Let's start by looking at the simplest case of optimal transport known as the optimal assignment problem. In this sitation, we consider 2 point sets, <i>X</i> and <i>Y</i>, both with <i>n</i> points of the same weight. Due to the uniform distribution of weights, when transporting <i>X</i> to <i>Y</i>, we only need to account for the Euclidean distance between the points when calculating the cost. In this situation, finding the optimal transport map between <i>X</i> and <i>Y</i> is equivalent to a one-to-one assignment problem where we try to minimize the total distance the points travel. 
    <br><br> 
    Below, try to find the optimal assignment by mapping the red point set to the blue point set &mdash; for each highlighted red point, click on a blue point to assign it to and once all points have been assigned, check out if the transport map you created is indeed the optimal!
    </p>
    <br>
    
    <Assignment />

    <h2><i>Monge Problem</i></h2>
    <p>
    Now let's consider a more general case of the previous problem where the weights of all points in <i>X</i> and <i>Y</i> are not necessarily equal and do not necessarily have the same number of points. This case of optimal transport is known as the Monge problem. Since the weights of all points are no longer equal, we cannot simply do a one-to-one assignment and minimize only the distance between the points. We must now also take into account the individual weights of each point when finding the optimal map, so now our cost is calculated as mass times distance. 
    <br><br>
    As in the previous problem, there is a requirement that the total mass of the points in <i>X</i> must be equal to the total mass of points in <i>Y</i>. However, in addition, we now also have the constraint of mass conservation: multiple points in <i>X</i> can be mapped to the same point in <i>Y</i> so long as the sum of the mass of the <i>X</i> points is equal to the mass of the <i>Y</i> point. 
    <br><br>
    Below, again try to find the optimal Monge map by clicking on a blue point to map each highlighted red point to. Note that each point has a labeled mass, and keep in mind the added constraint of mass conservation when creating the mappings. Once all points have been assigned, check out if the transport map you created is indeed the optimal!
    </p>
    <br>
    <Monge />
    <br>
    <h2><i>Kantorovich Relaxation</i></h2>
    <p>
    Notice that although the Monge problem allows the mapping of multiple points in <i>X</i> to a single point in <i>Y</i>, it only allows for a point <i>x<sub>i</sub></i> to be mapped to single other point <i>y<sub>j</sub></i>. Because of this, there sometimes may be no optimal Monge map between two point sets since the mass conservation constraint cannot be satisfied. 
    <br><br>
    Then, in the 1940s, Soviet mathematician Leonid Kantorovich proposed the idea of "relaxing" the deterministic nature of the Monge problem, referred to as Kantorovich relaxation. In particular, Kantorovich proposed that mass at a point <i>x<sub>i</sub></i> could be split up and dispatched to different locations. Note that the cost function in this situation is still calculated as mass times distance. Now, the only constraints are the following: 
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;1. the sum of the masses outgoing from a point <i>x<sub>i</sub></i> must be equal to the total mass of that point
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;2. the total amount of mass transported to a point <i>y<sub>j</sub></i> must be equal to the total mass of that point
    <br><br>
    Below, once again try to find the optimal transport map by clicking on a blue point to map each highlighted red point to. Each point has a labeled mass, and keep in mind mass splitting when creating the mappings. Once all points have been assigned, check out if the transport map you created is indeed the optimal!
    </p>
    <p>
    <b>NOTE:</b> In order to visualize mass splitting, only 1 unit of mass from a red point will be mapped during a single click to a blue point. For example, the first highlighted red point has a mass of 4 units; if you want to distribute 2 of its units to the blue point on to its left, click that blue point two times. The arrow between the two points will have a label with the amount of mass that is currently being transported between them and will update on each clicks. 
    </p>
    <Kantoro />
    <br>
    <h2>With what we've learned...</h2>
    <p style='line-height: 1.7'>
    Bringing back the factory-to-store analogy from earlier, click on the 
    <mark style='background-color: rgba(235, 235, 235, 1); border: #d9bdb2 solid 2px; border-radius: 8px; font-family: "Roboto Condensed", sans-serif; padding: 0.2em;'>&nbsp;&nbsp;transport!!&nbsp;&nbsp;</mark> 
    button below see the delivery plan that the company decided to implement based on their newly gained knowledge about optimal transport mapping!
    </p>
    <Animation />
    <p>
    Now, that we've explored the different layers of the optimal transport mapping, hopefully you have a better conceptual understanding of the problem. Note that the examples above are simple data sets to introduce the basics of the problem. In practice, finding the optimal map, and subsequently the Wasserstein distance, is computationally expensive as the size and dimension of the data increases. One ongoing challenge in modern machine learning is finding more efficient ways to calculate the optimal transport map using various methods such as neural networks and different mathematical approaches.
    </p>
    <br><br>
    <p style='text-align: center; font-size: 18px;'>
    Here are some resources if you want to learn more about the mathematical formulations for optimal transport and Wasserstein distance:
    <br>
    <a href='https://doi.org/10.48550/arXiv.1803.00567'>Computational Optimal Transport, G. Peyré and M. Cuturi (2018)</a>
    <br>
    <a href='https://www.damtp.cam.ac.uk/research/cia/files/teaching/Optimal_Transport_Notes.pdf'>Introduction to Optimal Transport, M. Thorpe (2018)</a>
    </p>
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

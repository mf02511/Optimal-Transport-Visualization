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
    Wasserstein distance is a measure of dissimilarity between two probability distributions, which has a variety of applications in modern machine learning. This distance is calculated by finding the path that minimizes the total cost of transferring mass from one distribution to the other, referred to as the <b>optimal transport map</b>. This problem was first introduced by French mathematician Gaspard Monge in 1781. 
    <br><br> 
    As an analogy, suppose we view one distribution <i>X</i> as a collection of factories from a single company and the other distribution <i>Y</i> as a collection of stores that receive shipments from the factories. Suppose we also have a cost function <i>c</i> such that <i>c(x<sub>i</sub>,y<sub>j</sub>)</i> calculates the cost of transporting one shipment of material from factory <i>x<sub>i</sub></i> to store <i>y<sub>j</sub></i>.
    An employee in charge of product distribution notices that the company's current delivery plan designed by her predecessor is wasting a lot of resources. Her goal is to rearrange the delivery plan to minimize the total cost of the shipments. With so many factories and store locations, there are numerous potential delivery paths that she can choose. She must also consider the amount products produced at each factory and the supply demand at each store. With these factors in mind, how should the employee decide which path to choose? This is exactly the premise of optimal transport mapping. 
    </p>
    <IntroGraphic />
    <p>
    The following series of interactive simulations introduces the concepts of optimal transport, from the very basic case to the more complex cases. 
    </p>
    <h2><i>Optimal Assignment Problem</i></h2>
    <p>
    Let's start by looking at the simplest case of optimal transport known as the optimal assignment problem. In this sitation, we assume that there is an equal number of <span style='color: #da7454'>factories</span> and <span style='color: #7685c0'>stores</span>. Each <span style='color: #da7454'>factory</span> produces the same amount of products and they each deliver to a single <span style='color: #7685c0'>store</span>. Due to the uniform distribution of products/supply demand, when transporting, we only need to account for the Euclidean distance between the <span style='color: #da7454'>factories</span> and <span style='color: #7685c0'>stores</span> to calculate the cost. In this case, finding the optimal transport map between <span style='color: #da7454'>factories</span> and <span style='color: #7685c0'>stores</span> is equivalent to a one-to-one assignment problem where we try to find a set of paths that would minimize the total distance traveled. 
    <br><br> 
    Below, try to find the optimal assignment by mapping the red point set (<span style='color: #da7454'>factories</span>) to the blue point set (<span style='color: #7685c0'>stores</span>) &mdash; for each highlighted red point, click on a blue point to assign it to and once all points have been assigned, check out if the transport map created is indeed the optimal!
    </p>
    <br>
    <Assignment />
    <br>
    <p>
    We can see that this is a relatively easy problem to optimize since the only factor in play is the distance! But what if the company has more store locations than factories or vice versa? Or, if each factory produces different amount of products?
    </p>
    <h2><i>Monge Problem</i></h2>
    <p>
    Now let's consider a more general case of the previous problem where the number of <span style='color: #da7454'>factories</span> and <span style='color: #7685c0'>stores</span> are not necessarily the same, and the amount of products produced at each <span style='color: #da7454'>factory</span> and required at each <span style='color: #7685c0'>stores</span> are also not always equal. This case of optimal transport is known as the Monge problem. Since number of <span style='color: #da7454'>factories</span> &ne; number of <span style='color: #7685c0'>stores</span>, we cannot simply do a one-to-one assignment and minimize only the distance between the points. We must now also take into account the individual weights of each point when finding the optimal map, so now our cost is calculated as distance times mass. 
    <br><br>
    Keep in mind the following rules: 
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;1. every <span style='color: #7685c0'>store</span> must receive the exact amount of products that it needs
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;2. each <span style='color: #da7454'>factory</span> can still only deliver to a single <span style='color: #7685c0'>store</span>
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;3. each <span style='color: #7685c0'>store</span> can receive shipments from multiple <span style='color: #da7454'>factories</span>
    <br><br>
    Below, try to find the optimal Monge map by again clicking on a blue point to map each highlighted red point to. Note that each point has a labeled mass (representing the amount of products produced/needed), and this time taking into account the above constraints. Once all points have been assigned, check out if the transport map created is indeed the optimal!
    </p>
    <br>
    <Monge />
    <br>
    <p>
    We can see that having different numbers of points with different masses makes it more challenging to obtain the optimal map. However, to implement the best possible delivery plan, we must take it one step further. What if we allow each factory to deliver to multiple store loctions?
    </p>
    <h2><i>Kantorovich Relaxation</i></h2>
    <p>
    Notice that although the Monge problem allows a single <span style='color: #7685c0'>store</span> to receive multiple shipments, but it requires that a single <span style='color: #da7454'>factory</span> only delivers to one <span style='color: #7685c0'>store</span>. Because of this, there sometimes may be no optimal Monge map between the <span style='color: #da7454'>factories</span> and <span style='color: #7685c0'>stores</span> if there are more <span style='color: #7685c0'>store</span> locations than <span style='color: #da7454'>factories</span>, or, if a <span style='color: #da7454'>factory</span> produces more than any single <span style='color: #7685c0'>store</span> needs. 
    <br><br>
    Then, in the 1940s, Soviet mathematician Leonid Kantorovich proposed the idea of "relaxing" the deterministic nature of the Monge problem, referred to as Kantorovich relaxation. In particular, Kantorovich proposed that mass at a point <i>x<sub>i</sub></i> could be split up and dispatched to different locations, meaning that the products produced at a single <span style='color: #da7454'>factory</span> can be delivered to multiple <span style='color: #7685c0'>stores</span>. Note that the cost function for each delivery in this situation is still calculated as distance times mass. Now, the rules are the following: 
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;1. every <span style='color: #7685c0'>store</span> must receive the exact amount of products that it needs
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;2. each <span style='color: #da7454'>factory</span> can deliver to multiple <span style='color: #7685c0'>stores</span>
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;3. each <span style='color: #7685c0'>store</span> can receive shipments from multiple <span style='color: #da7454'>factories</span>
    <br><br>
    Below, once again try to find the optimal transport map by clicking on a blue point to map each highlighted red point to. Once all points have been assigned, check out if the transport map you created is indeed the optimal!
    </p>
    <p>
    <b>NOTE:</b> In order to visualize mass splitting, only 1 unit product is transported by each click. For example, the first highlighted red point has a mass of 4 units; to distribute 2 units to the blue point on to its left, click that blue point two times. The arrow between the two points will have a label with the amount of mass that is currently being transported between them and will update on each click. 
    </p>
    <br>
    <Kantoro />
    <br>
    <p>
    This version of the problem above is the current formulation of the optimal transport problem today. 
    </p>
    <h2><i>With what we've learned...</i></h2>
    <p style='line-height: 1.7'>
    With her knowledge of optimal transport mapping, the employee utilizes the Python package <a href='https://pythonot.github.io/'>POT: Python Optimal Transport</a> to successfully compute a delivery plan for her company that would optimize their resources! Click on the 
    <mark style='background-color: rgba(235, 235, 235, 1); border: #d9bdb2 solid 2px; border-radius: 8px; font-family: "Roboto Condensed", sans-serif; padding: 0.2em;'>&nbsp;&nbsp;transport!!&nbsp;&nbsp;</mark> 
    button below to see a snippet of her delivery plan!
    </p>
    <br>
    <Animation />
    <p>
    Now, that we've explored the different layers of the optimal transport mapping, hopefully you have a better conceptual understanding of the problem. Note that the examples above are simple data sets to introduce the basics of the problem. As you worked through the set of problems in the last section, you might have had to attempt several times to achieve the optimal map since there are many possible combinations of paths. In practice, computational algorithms for finding the optimal transport map often becomes more and more costly as the size and dimensions of the data increases. One ongoing challenge in modern machine learning is finding more efficient ways to calculate the optimal transport map using various methods such as neural networks and different mathematical approaches.
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
    font-size: 45px;
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


</style>

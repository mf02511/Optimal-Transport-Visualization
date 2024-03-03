<script>
  import * as d3 from 'd3';
  import Assignment from './Assignment.svelte';
  import Monge from './Monge.svelte';
</script>

<main>
  <h1>Optimal Transport Mapping</h1>
  <p class='intro'></p>

  <h2></h2>
  <p></p>
  <Assignment />

  <h2></h2>
  <p></p>
  <Monge />
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
  }

  h1 {
    font-family: "EB Garmond", serif;
    text-align: center;
    font-weight: 400;
    font-size: 45px;
    color: #3b2923;
  }

  .intro {
    font-size: 18px;
    text-align: left;
    margin: 50px;
  }

</style>

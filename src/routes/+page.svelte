{#if !ready}
  <h1>XKCDGuessr</h1>
  <span>
    <button on:click={reset}>play number</button>
    <button on:click={reset}>play date</button>
  </span>
{:else}
  <h1 style="">{comic.safe_title}</h1>
  <div id = "UIContainer">
    1
    <input type="range" min=1 max={num} bind:value={guess} />
    {num}
  </div>
  <img src = {comic.img} alt={comic.transcript}>
  <button style = "margin: 10px;" on:click={reset}>guess {guess}</button>
  <p id = "score">score: {num_rounds > 0 ? Math.round(score/num_rounds) : 0}</p>
{/if}

<style>
  
  #score {
    position: absolute;
    top: 0vw;
    left: 25vw;
  }
  
  #UIContainer {
    width: 100%;
    text-align: center;
    font-size: 1vw;
  }
  
  #UIContainer input {
    width: 90%;
    background-color: #6e7b91;
  }
  
</style>

<script>
  import { onMount } from "svelte";
  
  let num = 0
  let comic = {};
  let ready = false;
  let guess = 1024;
  let score = 0;
  let num_rounds = -1;
  
  function getComic(comic="latest") {
    return fetch("https://xkcd.now.sh/?comic=" + comic)
      .then(res => res.json())
  }
  
  async function reset() {
    num_rounds++;
    
    if (num_rounds > 0) {
      score += 100*(1 + -Math.abs(comic.num-guess)/num)
    }
    
    await getComic().then(data => num = parseInt(data.num))
    await getComic(Math.floor(Math.random()*num + 1)).then(data => comic = data)
    
    ready = true;
    
  }
</script>
<script>
  import { Canvas } from "@threlte/core";
  import Scene from "./Scene.svelte";
  import { Theatre } from "@threlte/theatre";
  import { Project, Sheet } from "@threlte/theatre";
  import state from "./theatre-state.json";
  import InfoPage from "./InfoPage.svelte";
  import { onMount } from 'svelte';

  let isVisible = false;
  onMount(() => {
    isVisible = true;
  })
  let isFaded = true;
  let isIntro = false;
  function animationOver() {
    isFaded = true;
  }
  async function cssAnimationOver(e) {
    isIntro = false;
  }
</script>

<!-- "Mug" (https://skfb.ly/6TOJx) by Gjuroo is licensed under Creative Commons Attribution (http://creativecommons.org/licenses/by/4.0/). -->
{#if isIntro}
  <div
    id="canvas-container"
    class:faded={isFaded}
    on:animationend={cssAnimationOver}
  >
    <Canvas>
      <Project config={{ state }}>
        {#if !isFaded}
          <Sheet>
            <!-- pass it a store to update once animation is done, to trigger some CSS animations -->
            <Scene {animationOver} />
          </Sheet>
        {/if}
      </Project>
    </Canvas>
  </div>  
{/if}
{#if isVisible}
  <InfoPage />
{/if}

<style lang="scss">
  @keyframes slidein {
    from {
      opacity: 100%;
      backdrop-filter: blur(2px);
    }

    to {
      opacity: 0;
      backdrop-filter: blur(0px);
    }
  }
  .faded {
    animation-duration: 0.5s;    animation-fill-mode: forwards;
    animation-name: slidein;
  }
  #canvas-container {
    /* background: white; */
    /* background: repeat url(/wavy-dots.png) #aaa; */
    backdrop-filter: blur(2px);
    position: relative;
    display: flex;
    flex-direction: row;
    justify-content: center;
    /* max-height: 70vh; */
    margin: auto;
    min-height: 100vh;
    /* width: 100%; */
    /* aspect-ratio: 1/1; */
    box-shadow:
      1px 2px 2px rgba(0, 0, 0, 0.3),
      0px 4px 8px rgba(0, 0, 0, 0.3);
  }
</style>

<script>
  import Svg from "./Svg.svelte";
  let message = "Hello world!";
  let colors = ["#ebedf0", "#9be9a8", "#40c463", "#30a14e", "#216e39"];
</script>

<main>
  <h1>Typograssy GUI</h1>
  <p>
    Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn
    how to build Svelte apps.
  </p>
  <div>
    <Svg bind:message={message} bind:colors={colors} />
  </div>
  <div>
    <div>
      <input bind:value={message} />
    </div>
    {#each colors as color}
      <div>
        <input type="color" bind:value={color} />
      </div>
    {/each}
  </div>
</main>

<style>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>

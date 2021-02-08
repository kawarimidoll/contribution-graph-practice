<script>
  import Svg from "./Svg.svelte";
  let message = "Hello world!";
  let colors = ["#ebedf0", "#9be9a8", "#40c463", "#30a14e", "#216e39"];
  let bg = "#ffffff";
  let frame = "#000000";
</script>

<main>
  <h1>Typograssy GUI</h1>
  <p>
    Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn
    how to build Svelte apps.
  </p>
  <div>
    <Svg bind:message bind:colors bind:bg bind:frame />
  </div>
  <div>
    <div>
      <span>Message</span>
      <input bind:value={message} />
    </div>
    {#each colors as color, i}
      <div>
        <span>Pixel color level {i}</span>
        <input type="color" bind:value={color} />
      </div>
    {/each}
    <div><span>Frame color</span><input type="color" bind:value={frame} /></div>
    <div>
      <span>Background color</span><input type="color" bind:value={bg} />
    </div>
  </div>
</main>

<style>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>

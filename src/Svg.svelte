<script>
  import getPixelPositons from "./get-pixel-positions.js";

  const weeks = new Array(53).fill(0).map((_, i) => i);
  const days = new Array(7).fill(0).map((_, i) => i);

  const rectSize = 10;
  const rectSpan = 3;
  const rectRadius = 2;
  const rectStep = rectSize + rectSpan;
  const rectStroke = "rgba(27,31,35,0.06)";
  const rectStrokeWidth = 2;
  const getRectAttrs = (x, y, fill) => ({
    x: x * rectStep,
    y: y * rectStep,
    fill,
    width: rectSize,
    height: rectSize,
    rx: rectRadius,
    ry: rectRadius,
    stroke: rectStroke,
    "stroke-width": rectStrokeWidth,
  });

  const width = rectStep * (weeks.length + 2) - rectSpan;
  const height = rectStep * (days.length + 3) - rectSpan;

  export let message = "";
  export let colors = ["#ebedf0", "#9be9a8", "#40c463", "#30a14e", "#216e39"];
  export let bg = "#ffffff";
  export let frame = "#000000";
  export let speed = 200;

  let pixelPositons = getPixelPositons(message);
  let steps = pixelPositons.length;
  let needScroll = steps > weeks.length;
  let translateX = steps * rectStep;
  let style = `--speed:${
    speed * steps
  }ms;--steps:${steps};--translate-x: translateX(-${translateX}px)`;
  let rects = [];

  $: {
    pixelPositons = getPixelPositons(message);
    steps = pixelPositons.length;
    needScroll = steps > weeks.length;
    translateX = steps * rectStep;
    speed = 200;
    style = `--speed:${
      speed * steps
    }ms;--steps:${steps};--translate-x: translateX(-${translateX}px)`;

    const offset = needScroll ? 1 : Math.ceil((weeks.length - steps) / 2);

    const getRandomColor = () =>
      colors[Math.floor(Math.random() * (colors.length - 1)) + 1];
    const newRects = [];
    pixelPositons.forEach((line, x) => {
      days.forEach((day) => {
        if (line.includes(day)) {
          const color = getRandomColor();
          newRects.push(getRectAttrs(x + offset, day, color));
          if (needScroll && x < weeks.length - 1) {
            newRects.push(getRectAttrs(x + offset + steps, day, color));
          }
        }
      });
    });
    rects = newRects;
  }
</script>

<svg {width} {height}>
  <rect {width} {height} stroke={frame} fill={bg} />
  <g transform="translate({rectStep}, {rectStep})">
    <g>
      {#each weeks as week}
        {#each days as day}
          <rect {...getRectAttrs(week, day, colors[0])} />
        {/each}
      {/each}
    </g>
    <svg width={width - rectStep * 2} height={height - rectStep * 2} {style}>
      {#if needScroll}
        <style>
          rect {
            animation: step var(--speed) steps(var(--steps)) infinite;
          }
          @keyframes step {
            to {
              transform: var(--translate-x);
            }
          }
        </style>
      {/if}
      {#each rects as rect}
        <rect {...rect} />
      {/each}
    </svg>
  </g>

  <g transform="translate({rectStep}, {height - rectSize})">
    <text font-size={rectStep}>
      <a href="https://github.com/kawarimidoll">@kawarimidoll</a>
    </text>
  </g>
  <g transform="translate({width - rectStep * 7}, {height - rectSize * 2})">
    {#each colors as color, i}
      <rect {...getRectAttrs(i, 0, color)} />
    {/each}
  </g>
</svg>

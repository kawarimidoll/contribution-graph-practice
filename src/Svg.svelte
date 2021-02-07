<script>
  const colors = ["ebedf0", "9be9a8", "40c463", "30a14e", "216e39"];
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
    fill: `#${fill}`,
    width: rectSize,
    height: rectSize,
    rx: rectRadius,
    ry: rectRadius,
    stroke: rectStroke,
    "stroke-width": rectStrokeWidth,
  });

  const fontSize = rectSize + rectSpan;

  const width = rectStep * (weeks.length + 2) - rectSpan;
  const height = rectStep * (days.length + 2) - rectSpan + fontSize;
</script>

<svg {width} {height}>
  <rect {width} {height} stroke="#000" fill="none" />
  <g transform="translate({rectStep}, {rectStep})">
    {#each weeks as week}
      {#each days as day}
        <rect {...getRectAttrs(week, day, colors[0])} />
      {/each}
    {/each}
  </g>

  <g transform="translate({rectStep}, {height - rectSize})">
    <text font-size={fontSize}>
      <a href="https://github.com/kawarimidoll">@kawarimidoll</a>
    </text>
  </g>
  <g transform="translate({width - rectStep * 7}, {height - rectSize * 2})">
    {#each colors as color, i}
      <rect {...getRectAttrs(i, 0, color)} />
    {/each}
  </g>
</svg>

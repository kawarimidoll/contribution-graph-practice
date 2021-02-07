<script>
  export let message = "";

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

  const characters = {
    " ": "//",
    A: "123456/04/123456",
    B: "0123456/036/1245",
    C: "12345/06/15",
    D: "0123456/06/12345",
    E: "0123456/036/06",
    F: "0123456/03/0",
    G: "12345/06/13456",
    H: "0123456/3/0123456",
    I: "06/0123456/06",
    J: "5/6/012345",
    K: "0123456/23/01456",
    L: "0123456/6/6",
    M: "0123456/123/0123456",
    N: "0123456/0/123456",
    O: "12345/06/12345",
    P: "0123456/03/12",
    Q: "12345/046/123456",
    R: "0123456/03/12456",
    S: "125/036/145",
    T: "0/0123456/0",
    U: "0123456/6/0123456",
    V: "01234/56/01234",
    W: "0123456/345/0123456",
    X: "0156/234/0156",
    Y: "012/3456/012",
    Z: "056/02346/016",
  };

  const getCharacterArray = (char) => {
    const points = characters[char];
    if (!points) {
      throw new Error(`Invalid character '${char}' is detected!`);
    }
    return points
      .split("/")
      .map((p) => p.split("").map((n) => Number(n)))
      .concat([[]]);
    // add a blank line to separate each character
  };
  const pixels = message
    .split("")
    .reduce((acc, crnt) => acc.concat(getCharacterArray(crnt)), []);

  console.log(pixels);
</script>

<svg {width} {height}>
  <rect {width} {height} stroke="#000" fill="none" />
  <g transform="translate({rectStep}, {rectStep})">
    <g>
      {#each weeks as week}
        {#each days as day}
          <rect {...getRectAttrs(week, day, colors[0])} />
        {/each}
      {/each}
    </g>
    {#each pixels as line, x}
      {#each days as day}
        {#if line.includes(day)}
          <rect {...getRectAttrs(x + 1, day, colors[2])} />
        {/if}
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

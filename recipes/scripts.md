# Recipe: Code on Canvas (Script Nodes)

Generate data-driven visuals with JavaScript on the pen.dev canvas.

## Create a bar chart

Create a script file `bar-chart.js`:

```js
/**
 * @schema 2.18
 *
 * @input columns: number(min=1) = 5
 * @input gap: number(min=0) = 8
 * @input color: color = #3B82F6
 */
const cols = Math.floor(pencil.input.columns);
const gap = pencil.input.gap;
const cellW = (pencil.width - gap * (cols - 1)) / cols;

const nodes = [];
for (let c = 0; c < cols; c++) {
  const h = pencil.height * (0.3 + Math.random() * 0.7);
  nodes.push({
    type: "rectangle",
    x: c * (cellW + gap),
    y: pencil.height - h,
    width: cellW,
    height: h,
    cornerRadius: 4,
    fill: pencil.input.color,
  });
}
return nodes;
```

## Create a grid

Create `grid.js`:

```js
/**
 * @schema 2.18
 *
 * @input rows: number(min=1, max=20) = 5
 * @input cols: number(min=1, max=20) = 5
 * @input gap: number(min=0) = 4
 * @input fill: color = #E5E7EB
 * @input rounded: boolean = true
 */
const rows = Math.floor(pencil.input.rows);
const cols = Math.floor(pencil.input.cols);
const gap = pencil.input.gap;
const cellW = (pencil.width - gap * (cols - 1)) / cols;
const cellH = (pencil.height - gap * (rows - 1)) / rows;

const nodes = [];
for (let r = 0; r < rows; r++) {
  for (let c = 0; c < cols; c++) {
    nodes.push({
      type: "rectangle",
      x: c * (cellW + gap),
      y: r * (cellH + gap),
      width: cellW,
      height: cellH,
      cornerRadius: pencil.input.rounded ? 4 : 0,
      fill: pencil.input.fill,
    });
  }
}
return nodes;
```

## Use with the CLI

Ask the AI to link the script to a node:

```bash
pen --in design.pen --out design.pen --prompt "Add a 400x300 script node linked to bar-chart.js at position (50,50)"
```
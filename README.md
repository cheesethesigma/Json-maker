<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Raygun Bag JSON Creator</title>
  <style>
    body { font-family: Arial; padding: 20px; }
    .hidden { display: none; }
    .bag-section, .item-section { margin-top: 20px; }
    select, input { margin: 5px; }
  </style>
</head>
<body>
  <button id="startBtn">Start</button>

  <div id="bagContainer" class="bag-section hidden">
    <h3>Select Bags</h3>
    <div id="bags"></div>
  </div>

  <div id="itemContainer" class="item-section hidden">
    <h3>Assign Items to Bags</h3>
    <div id="items"></div>
    <button onclick="generateJSON()">Generate JSON</button>
    <pre id="output"></pre>
  </div>

  <script>
    const bagList = ["Large Blue Bag", "Medium Red Bag", "Small Green Bag"];
    let raygunsWithChildren = [];

    document.getElementById('startBtn').addEventListener('click', async () => {
      const response = await fetch("https://percthirty.github.io/Animal-Company-ID-Tracker/data.json");
      const data = await response.json();

      // Filter items with children (rayguns)
      raygunsWithChildren = data.filter(item => item.children && item.children.length > 0);

      const bagsDiv = document.getElementById('bags');
      bagList.for

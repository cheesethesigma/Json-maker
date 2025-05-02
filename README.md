<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Bag JSON Creator</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; max-width: 800px; margin: auto; }
    button, select, input { margin: 10px 0; font-size: 16px; width: 100%; max-width: 100%; }
    .hidden { display: none; }
    .bag-section, .item-section { margin-top: 20px; }
    pre { background: #f3f3f3; padding: 10px; overflow: auto; }
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
      try {
        const response = await fetch("https://percthirty.github.io/Animal-Company-ID-Tracker/data.json");
        const data = await response.json();
        ray

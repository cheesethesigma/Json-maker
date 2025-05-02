<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Bag JSON Creator</title>
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

  <div id="itemContainer"

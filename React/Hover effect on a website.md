
On a Website

1. Use HTML/CSS: Wrap the word in a `<div>` with a class like “tooltip”. Add a child element with the tooltip text and style it to appear on hover using CSS.
2. JavaScript: You can also use JavaScript to dynamically add tooltips by wrapping words in `<span>` tags and displaying hidden divs on hover.

Example HTML/CSS Code:
```
<div class="tooltip">Hover over me
  <span class="tooltiptext">This is a tooltip</span>
</div>

.tooltip {
  position: relative;
  display: inline-block;
}

.tooltip .tooltiptext {
  visibility: hidden;
  width: 120px;
  background-color: black;
  color: #fff;
  text-align: center;
  padding: 5px 0;
  border-radius: 6px;
  position: absolute;
  z-index: 1;
}

.tooltip:hover .tooltiptext {
  visibility: visible;
}
```
# AE-Expressions
A collection of usefull AE expressions

### Hide backside on 3D flip
Add this nifty Dan expression to the Opacity of the front (will be on top in the stack) layer (Copy the expression, Alt-Click the Opacity Stopwatch, then Paste, hit Enter) [source](https://forums.creativecow.net/thread/2/915001#915005):
```
if (toCompVec([0, 0, 1])[2] > 0 ) { 
  value 
} else { 
  0 
}
```

### Get sizes of layer
Get size of text layer.
```
thisComp.layer("My Text Layer").sourceRectAtTime().width;
thisComp.layer("My Text Layer").sourceRectAtTime().height;
```

### Select sibling 
Get sibling of current layer.
```
olderSibling = thisComp.layer(index - 1);
youngerSibling = thisComp.layer(index + 1);
```
### Always center ancherpoint to viewport
Add this expression to the anchorpoint of the layer you want to center 
```
s = thisLayer; // this used to point to the layer (in this case itself)
sTop = s.sourceRectAtTime().top; // spit out the top position property of the layer
sLeft = s.sourceRectAtTime().left; // spit out the left position property

sHeight = s.sourceRectAtTime().height; // spit out the height property of the layer
sWidth = s.sourceRectAtTime().width; // spit out the width property

sAnchorY = sTop + (sHeight/2); // calculate the Y-axis anchor from the top property added by half of the height of the layer
sAnchorX = sLeft + (sWidth/2); // more or less the same just for the horizontal or X-axis

[sAnchorX, sAnchorY] // actually putting all the end value to the value of the anchor point of the layer
```

### Calucalte size of path
Note: this don't include tangents. And Bodymovin is not supporting es6+.
```
function pathBounds(points) {
	let minX = points[0][0], 
		minY = points[0][1], 
		maxX = points[0][0], 
		maxY = points[0][1];
	
	points.forEach(p => {
		minX = Math.min(minX, p[0]);
		maxX = Math.max(maxX, p[0]);
		minY = Math.min(minY, p[1]);
		maxY = Math.max(maxY, p[1]);
	});

	return {
		width: maxX - minX,
		height: maxY - minY
	}
}

const test = pathBounds([...thisComp.layer("TEST").content("face").content("mouth").content("lipLower").path.points(), ...thisComp.layer("TEST").content("face").content("mouth").content("lipUpper").path.points()]);
```

### Typewriter smooth text center
TODO: Description
```
TRANSITION_DURATION = 0.5;
TIME_OFFSET = 0.25;

self = thisLayer;
sourceText = self.text.sourceText;
timeWithOffset = time + TIME_OFFSET;

x = value[0];

// If has keyframes
if (sourceText.numKeys > 0) {
  closestKeyframe = sourceText.nearestKey(timeWithOffset).index;

  // If keyframe is in future
  if (timeWithOffset < sourceText.key(closestKeyframe).time) {
    // Get to previous frame
    closestKeyframe--;
  }

  // If has close frame
  if (closestKeyframe > 0) {

    // Frame is not first
    if (closestKeyframe > 1) {

      prevKeyframeTime = sourceText.key(closestKeyframe - 1).time;
      currentKeyframeTime = sourceText.key(closestKeyframe).time;

      prevWidth = self.sourceRectAtTime(prevKeyframeTime, false).width;
      currentWidth = self.sourceRectAtTime(currentKeyframeTime,false).width;

      x = (x - ease(timeWithOffset, currentKeyframeTime, currentKeyframeTime + TRANSITION_DURATION, prevWidth, currentWidth)) / 2;
    } else {
      x = (x - self.sourceRectAtTime(timeWithOffset, false).width) / 2;
    }
  }
}

[x, value[1]]
```

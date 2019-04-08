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

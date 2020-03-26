# OpenSeadragonAnnotationPaperjsOverlay

An [OpenSeadragon](http://openseadragon.github.io) plugin that adds [Paper.js](http://paperjs.org) overlay capability.

Compatible with OpenSeadragon 2.0.0 or greater.
Compatible with Paper.js 0.9.25 or less (flicker annotations if use greater 0.9.25)

License: The BSD 3-Clause License. The software was forked from [OpenSeadragonPaperjsOverlay](https://github.com/eriksjolund/OpenSeadragonPaperjsOverlay), that also is licensed under the BSD 3-Clause License.

##Demo web page

See the [online demo](https://choikyusu.github.io/OpenSeadragonAnnotationPaperjsOverlay/annotation.html)
where some Paper.js annotations are shown on top of an OpenSeadragon window. The annotations can be drawn with the mouse.

## Introduction

To use, include the `openseadragon-paperjs-overlay.js` file after `openseadragon.js` on your web page.
   
To add Paper.js overlay capability to your OpenSeadragon Viewer, call `paperjsOverlay()` on it. 

`````javascript
    var viewer = new OpenSeadragon.Viewer(...);
    var overlay = viewer.paperjsOverlay();
`````

This will return a new object with the following methods:

* `paperjsCanvas()`: Returns Paper.js canvas that you can add elements to
* `resize()`: If your viewer changes size, you'll need to resize the Paper.js overlay by calling this method.

##Add drag support
Functionality for dragging Paper.js objects can be added by using OpenSeadragon.MouseTracker


`````javascript
    new OpenSeadragon.MouseTracker({
        element: viewer.canvas,
        pressHandler: press_handler,
        dragHandler: drag_handler,
        dragEndHandler: dragEnd_handler
    }).setTracking(true);
`````

together with these callbacks

`````javascript
var option = 'move';
var currentPath = null;
var drag_handler = function(event) {
    if (['red', 'green', 'blue', 'yellow', 'black', 'skyblue', 'pink', 'white'].indexOf(option) > -1) {
        var transformed_point = paper.view.viewToProject(new paper.Point(event.position.x, event.position.y));
        currentPath.add(transformed_point);
        paper.view.draw();
    }
};
var dragEnd_handler = function(event) {
    if (['red', 'green', 'blue', 'yellow', 'black', 'skyblue', 'pink', 'white'].indexOf(option) > -1) {
        currentPath.simplify();
        window.viewer.setMouseNavEnabled(true);
        currentPath = null;
        paper.view.draw();
	option = 'move';
    }
};
var press_handler = function(event) {
    if (['red', 'green', 'blue', 'yellow', 'black', 'skyblue', 'pink', 'white'].indexOf(option) > -1) {
        if (currentPath) {
            currentPath.selected = false;
        }
        var transformed_point = paper.view.viewToProject(new paper.Point(event.position.x, event.position.y));
        currentPath = new paper.Path({
                segments: [transformed_point],
                strokeColor: option,
                strokeWidth: 50 / viewer.viewport.getZoom()
        });
        circles.push(currentPath);
        window.viewer.setMouseNavEnabled(false);
    }
};
`````
Note: The file package.json was modified to reflect the change from Fabric.js to Paper.js.
But the file has not been tested.

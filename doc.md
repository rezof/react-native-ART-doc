# ATTENTION HERE!
STILL UNDER COLLECTING, SOME PROPERTIES I HAVEN'T CHECKED MYSELF. 

react JSX syntax doesn't allow comments inline, so remember to delete comments in demo code to avoid errors.
---

#### Elements

```js
const {
	Surface,
	Shape,
	Group,
	Text,
	Path,
	ClippingRectangle,
	LinearGradient,
	RadialGradient,
	Pattern,
	Transform
} = React.ART
```

##### Surface
Container for all other ART components.

```js
render(){
	return (
		<Surface>
			{ all other components }
		</Surface>
	)
}
```
Property | Type | Must | tag
:-:|:-:|:-:|:-:
width| string | false | width of target surface
height|string|false|height of target surface
visible |boolean|false|visible or invisible


##### Group
To combine shapes or other groups into hierarchies that can be transformed as a set.

```js
render(){
	return (
		<Surface>
			{ this.getContainer() }
			<Shape/>
		</Surface>
	)
}

getContainer = () => {
	return (
		<Group>
			<Shape>
			<Shape>
		</Group>
	)
}
```

##### Shape
Used to draw arbitrary vector shapes from Path.
Shape implements Transform as a mixin which means it has all transform methods available for moving, scaling and rotating a shape.

```js
render(){
	return (
		<Surface>
			<Shape/>
		</Surface>
	)
}
```

Property | Type | Must | tag
:-:|:-:|:-:|:-:
width | Number | false | width of Shape
height | Number | false | height of Shape
d| String | false | container of path
fill|String|false|fill style of Shape.Any color object module will be support
stroke | String |false|stroke color of paths it contains
strokeWidth | String |false|stroke width of paths it contains
strokeDash | Object | false | demo followed.
strokeCap | String | false | cap style of path end. oneOf(["butt", "round"(default), "square"])
strokeJoin | String | false | path join point style. oneOf(["miter", "round"(default), "bevel"])

```js
render(){
	return (
		<Surface>
			<Shape
				d = "..."
				fill = '#000000'
				stroke = '#FFFFFF'
				strokeWidth = 12
				// strokeDash Demo
				strokeDash = {{
					count:12, // StrokeDash array length,
					strokeDash:[
						10, 20, 20
					]
				}}
				strokeCap:"butt" // or round(default)/square
				strokeJoin:"bevel" // or miter/round(default)
			/>
		</Surface>
	)
}
```


##### Text
Text component creates a shape based on text content using native text rendering.

```js
render(){
	return (
		<Surface>
			<Text
			
				font={`13px "Helvetica Neue", "Helvetica", Arial`}
				
				/* Another way to define font property
				 * font = {{
				 * 	fontFamily:'Helvetica, Neue Helvetica, Arial',
				 * 	fontSize:23,
				 * 	fontWeight:"bold", // or "normal"
				 * 	fontStyle:"italic" // or "normal"
				 * }}
				**/
				
				fill = "#000000"
				alignment = "center"
			>
				Hello World
			</Text>
		</Surface>
	)
}
```

`ART module` in React Native supplies Text component different from sebmarkbage's `art` repo that that mix `Text` and `Font` up. So `font` property is necessary, or your app will crash. And in fact, ART makes `Text` with `Path`, so just try methods what `Shape` has.

Property | Type | Must | tag
:-:|:-:|:-:|:-:
font | String or Object | true | font name and font size for text content
fill | String | false | fill color
x | Number | false | x position 
y | Number | false | y position
alignment | String | false | oneOf(["right", "left", "center"])


#### APIs

```js
redner(){
	return (
		<Surface>
			<Shape
				d={ this.getPaths() } // You can get what this.getPaths method do in following path.move demo
			/>
		</Surface>
	)
}
```

##### Path
###### Path.move

```js
getPaths = () => {
	return	(
		new Path()
		.move(10, 20)
		
		// This means move ctx form current point to relative right 10px bottom 20px
		// for example ctx now at (20, 20) point
		// after move(10, 20) the point will change to (30, 40)
	)
}
```

###### Path.moveTo

```js
getPaths = () => {
	return	(
		new Path()
		.moveTo(10, 20)
		
		// This means move ctx to absolute coordinate (10, 20)
		// for example ctx now at (20, 20) point
		// after moveTo(10, 20) the point will change to (10, 20)
	)
}
```

###### Path.line

```js
getPaths = () => {
	return	(
		new Path()
		.line(10, 20)
		
		// This means draw a line from current position to relative right 10px bottom 20px
		// for example ctx now at (20, 20) point
		// after line(10, 20) you will get a line from (20, 20) to (30, 40)
	)
}
```

###### Path.lineTo

```js
getPaths = () => {
	return	(
		new Path()
		.lineTo(10, 20)
		
		// This means draw a line from current position to absolute coordinate (10, 20)
		// for example ctx now at (20, 20) point
		// after lineTo(10, 20) you will get a line from (20, 20) to (10, 20)
	)
}
```
###### Path.arc
Draw an arc with specific arguments.

```
arc(xPosition, xPosition, xRadius, yRadius[,outer,counterClockWise,rotation])
```

```js
path.arc(10, 10, 30, 40, true, false, 1)
```

###### Path.arcTo
Just like arc, instead of passing relative position, `arcTo` accept an absolute point coorid to be the arc end.
```
arcTo(xPosition, xPosition, xRadius, yRadius[,outer,counterClockWise,rotation])
```

```js
path.arcTo(60, 90, 30, 40, true, false, 1)
```

###### Path.counterArc

Same as arc, opposite clockwise.

###### Path.counterArcTo

Same as arcTo, opposite clockwise.

###### Path.curve
Draw a cubic bezier curve to relative position.

```
curve(ControlPoint1.x, ControlPoint1.y, ControlPoint2.x, ControlPoint2.y, deltaX, deltaY)
```

```js
path.curve(10, 20, 30, 40, 12, 32);

// If now we are at (10, 10), it draw a cubic bezier curve from (10, 10) to (22, 42)
// and use (10, 20) as first control point and (30, 40) the second one
```
###### Path.curveTo
Draw a bezier curve to absolute position.

```
curve(ControlPoint1.x, ControlPoint1.y, ControlPoint2.x, ControlPoint2.y, endPoint.x, endPoint.y)
```

```js
path.curve(10, 20, 30, 40, 12, 32);

// if now we are at (10, 10), it draw a cubic bezier curve from (10, 10) to (12, 32)
// and use (10, 20) as first control point and (30, 40) the second one
```


###### Path.reset
Reset the current path. Just like `beginPath` in canvasRenderingContext2d.
```js
// path.points = [...]
path.reset();
// path.points = [];
```

###### Path.close
Draws a line to the first point in the current sub-path and begins a new sub-path.

```js
Path.close();
// It returns the current Path instance.
```

###### Path.toJson
Return the current path points, which can be used on Shape `d` attribute.

```js
var d = new Path(path).toJSON();
...
return (
	<Shape d={d}></Shape>
)
```

##### LinearGradient
```jsx

/* Crate linear gradient
 * @param stops Object linear gradient stops
 * @demo {'0.1':'green', '1':'blue'}
 * @param x1 Number x-axis coordinate of start point
 * @param y1 Number y-axis coordinate of start point
 * @param x2 Number x-axis coordinate of end point
 * @param y2 Number y-axis coordinate of end point
 */

var linearGradient = new LinearGradient({
	'.1': 'blue', // blue in 1% position
	'1': 'rgba(255, 255, 255, 0)' // opacity white in 100% position
  },
  "0", "0", "0", "400"
)

<Shape fill={linearGradient}>
```
##### RadialGradient
```jsx
/* Create radial gradient
 * @param stops Object linear gradient stops
 * @demo {'0.1':'green', '1':'blue'}
 * @param fx Number x-axis coordinate of the focal point
 * @param fy Number y-axis coordinate of the focal point
 * @param rx Number x-axis coordinate direction radius length
 * @param ry Number y-axis coordinate direction radius length
 * @param cx Number x-axis coordinate of the origin point
 * @param cy Number y-axis coordinate of the origin point
 */
 
 var radialGradient = new RadialGradient({
	'.1': 'blue', // blue in 1% position
	'1': 'rgba(255, 255, 255, 0)' // opacity white in 100% position
  },
  "200", "200", "0", "0", "0", "400"
)

<Shape fill={radialGradient}>
```
##### Pattern

##### Transform

###### move
Move target shape, each time you call this method the translate position will superposition.
```
new Transform().move(deltaX[,deltaY])
```
```js
new Transform().move(20, 20)
// or you can only move x
new Transform().move(20)
```

###### moveTo
Move the shape to absolute coordinate position.

```
new Transform().moveTo(x[,y])
```
```js
new Transform().moveTo(120, 120)
// or you can only move to x
new Transform().moveTo(120)
```

###### scale
Scale the shape, each time you call this method the scale value will superposition.
```
new Transform().scale(scale[X,scaleY]);
```
```js
new Transform().scale(2, 3);
// or pass only one param to scale both x and y axis value
new Transform().scale(3)
```
###### scaleTo
Scale the shape to a fixed multiple to origin graphic.
```
new Transform().scaleTo(scale[X,scaleY]);
```
```js
new Transform().scaleTo(1, 1);
// or you can use only one param to set both x and y axis value
new Transform().scaleTo(1);
```

###### rotate
Rotate the shape, each time you call this method the angle will superposition. Pass transformed origin x and y axis coordinate as the second and third param, relative to left top corner of outer Surface.
```
new Transform().rotate(deg[,transformOriginX,transformOriginY])
```
```js
new Transform().rotate(180);
// attention, the angel is in angel system inestead of radian system.
// or you can specify transform origin with extra params
new Transform().rotate(180, 100, 200)
```
###### rotateTo
Rotate the shape to an absolute angle. Pass the transformed origin x and y axis coordinate as the second and third param, relative to left top corner of outer Surface.
```
new Transform.rotateTo(deg[,transformOriginX,transformOriginY])
```
```js
new Transform().rotateTo(72);
// attention, the angel is in angel system inestead of radian system.
// or you can specify transform origin with extra params
new Transform().rotateTo(72, 100, 200)
```
###### resizeTo

###### transform
Use this to make transform with a matrix-like method.[`Reference`](http://sebmarkbage.github.io/art/docs/ART/ART.Transform.html). Each time you call this method the transformed value will superposition.
```
new Transform.transform(scaleX, skewX, skewY, scaleY, translateX, translateY);
// change target's position and shape with six arguments。
```
```js
new Transform.transform(2, 0, 1, 1, 0, 0)
```
###### transformTo
Use this to make transform with a matrix-like method.[`Reference`](http://sebmarkbage.github.io/art/docs/ART/ART.Transform.html). Each time you call this method the transformed value will be reset to the arguments.
```
new Transform.transformTo(scaleX, skewX, skewY, scaleY, translateX, translateY);
// change target's position and shape with six arguments。
```
```js
new Transform.transformTo(1, 0, 0, 1, 0, 0)
```

###### inversePoint

##### ClippingRectangle

#### Morph
This can create transition between two paths.
> To use morph method, you have to import morph first

```js
import Morph from 'art/morph/path';
```

##### Tween
```js
Morph.Tween(from, to)
```

```js
Morph.Tween(
	"M 256, 213 C 245, 181 206, 187 234, 262Z",
	"M 212, 220 C 197, 171 156, 153 123, 221Z"
);
```
##### Path
Extends from Path.[`Reference`](https://github.com/sebmarkbage/art/blob/master/morph/path.js#L6-L29);

Alternate Events

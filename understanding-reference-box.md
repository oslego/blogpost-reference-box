# Understanding Reference Boxes for CSS Shapes

CSS Shapes are used to wrap content around custom paths. The paths are defined with shape function values, like `circle()`, `ellipse()` or `polygon()`, and they are positioned within a virtual box -- the reference box.

A reference box defines the shape's coordinate system, so it influences how the shape will be drawn and positioned. There are four reference boxes to choose from: `margin-box`, `padding-box`, `border-box` and `content-box`. Each of them yields subtly different results. Read on to learn how they work.

We'll consider a simple circle shape around which we will wrap content. We'll use percentages for the circle radius to observe how reference boxes influence the resulting shape.

```css
.shape{
  shape-outside: circle(50%);
  float: left;
  width: 100px;
  height: 100px;
}
```

[illustration]

## The `margin-box`

If unspecified, the default reference box for a shape is `margin-box`. The CSS shape rule above is equivalent to `shape-outside: circle(50%) margin-box;`.

The `margin-box` reference box means that a shape is positioned in a virtual box defined by the outer edges of the host element's margin properties. The origin of the coordinate system is at the upper left corner of this box, with the X axis going from left to right and the Y axis going from top to bottom.

We didn't specify a margin in our sample yet, so the `margin-box` reference box does not extend beyond the element. It's still ok to imagine the origin of the coordinate system for the shape placed at the upper-left corner of the element.

[illustration of coordinate system without margin-box]

At this point, the circle's 50% radius yields an actual length of 50px -- half the element's width or height.

The shape changes when we do specify a margin.

```css
.shape{
  margin: 50px;
  shape-outside: circle(50%);
  float: left;
  width: 100px;
  height: 100px;
}
```

After setting `margin: 100px`, the `margin-box` reference box extends around the element by 50px in all directions. It builds on the element's dimensions, so the reference box is now 200px by 200px.

The circle's 50% radius now means 100px and the origin is outside the element, at the upper left corner of the box defined by the margin.

[illustration of margin-box]

## The `padding-box`

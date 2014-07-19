# Understanding Reference Boxes for CSS Shapes

CSS Shapes are used to wrap content around custom paths. The paths are defined with shape function values, like `circle()`, `ellipse()` or `polygon()`, and they are positioned within a virtual box, the reference box.

A reference box defines the shape's coordinate system, so it influences how the shape will be drawn and positioned. There are four reference boxes to choose from: `margin-box`, `padding-box`, `border-box` and `content-box`. Each of them yields subtly different results. Read on to learn how they work.

[illustration of boxes]

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

The `margin-box` reference box means that a shape is positioned in a virtual box defined by the outer edges of the host element's margin. The origin of the coordinate system is at the upper left corner of this box, with the X axis going from left to right and the Y axis going from top to bottom.

We didn't specify a margin in our sample yet, so the `margin-box` reference box does not extend beyond the element. It's still ok to imagine the origin of the coordinate system for the shape placed at the upper-left corner of the element.

[illustration of coordinate system without margin-box]

At this point, the circle's 50% radius yields an actual length of 50px (half the element's width or height).

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

[illustration of margin-box and coordinate system]

We use the `margin-box` reference box when it's important to wrap content around a shape which stretches beyond the dimensions of the host element.

## The `padding-box`

The `padding-box` reference box constrains the shape's coordinate system to the box defined by the outer edges of the element's padding.

Let's use the `padding-box` reference box and specify a padding.

```css
.shape{
  padding: 25px;
  shape-outside: circle(50%) padding-box;
  float: left;
  width: 100px;
  height: 100px;
}
```

After setting `padding: 25px` the element grows by 25px in all directions. This effect occurs only if the element's `box-sizing` property value remains unchanged from the browser's default of `content-box`.

In this scenario, the reference box becomes 150px by 150px, (width + 2 * 25px) by (height + 2 * 25px). The circle's 50% radius now means 75px, and the coordinate system origin is at the upper left corner of the box defined by the padding.

[illustration of padding-box reference box]

However, if we use `box-sizing: border-box`, a different algorithm for computing the box model applies and the padding value is subtracted from the element's dimensions in such a way that the element's overall size does not change, but its surface for inner content is reduced. In this scenario, the reference box becomes 100px by 100px and the circle's 50% radius yields 50px.

[illustration of padding-box reference box + box-sizing: border-box]

In both scenarios, the margin property can be used to adjust the position of the element together with its shape.

We use the `padding-box` reference box when it's necessary for the wrapped content to overlap part of the host element.

[illustration of element with radial gradient which is overlapped by content]

## The `border-box`

As its name implies, the `border-box` reference box constrains the shape's coordinate system to the box defined by the outer edges of the element's border. In this case, the shape may overlap the element's border, but it can't extend beyond it.

```css
.shape{
  border: 25px solid pink;
  shape-outside: circle(50%) border-box;
  float: left;
  width: 100px;
  height: 100px;
}
```

After applying a thick 25px solid border, which, by default, grows the element in all directions, the reference box will be 150px by 150px, (width + 2 * 25px) by (height + 2 * 25px), and the circle's 50% radius yields 75px.

If we use `box-sizing: border-box`, this makes the browser subtract the necessary space for the border from the element's dimensions, thus reducing the surface for inner content. In that case, the reference-box ends up being 100px by 100px.

Similar to `padding-box`, the `border-box` reference box helps keep the shape synchornized with the position of the element if margins are used.

We use the `border-box` reference box when it's necessary for the shape to overlap the element's border and keep in sync with the border if it changes.

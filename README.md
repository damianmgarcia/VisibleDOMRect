# **VisibleDOMRect**

## **Description**

**VisibleDOMRect** includes the function `getVisibleRect` for getting the size and position of the visible, viewport-clipped bounding client rectangle of a DOM element. In essence, it returns a visible, viewport-clipped subset of the results returned by the `getBoundingClientRect` method of the `Element` interface.

## **How It Works**

The search algorithm uses a binary search strategy to scan the area within an element's viewport-clipped DOMRect for a visible point, and if a point is found, it then uses this point as a reference to find the visible top, bottom, left, and right edges.

## **Assumptions and Limitations**

### **Clickability = Visibility**

The search algorithm uses the `elementFromPoint` method of the `document` interface to determine the clickable boundaries of an element, and thus clickability is used as a proxy for visibility. This assumption may not work for some use cases.

### **Visible rectangle does not mean visible area**

The search algorithm only returns the coordinates of the first visible boundary found around the first visible point. It does not scan and return a matrix of all visible points within an element. Thus, only the rectangle's edges, and not its area, may be assumed to be visible.

### **Search area**

The search algorithm uses a binary search strategy that, by default, divides the element in half along each axis up to 8 times (256 sections per axis) in search of a visible point. Thus, a visible point may not be found in cases where the visible area of an element is smaller than the smallest binary search division. The number of divisions can be increased by supplying a `maxDivisions` argument greater than 8, but this could lead to searches that take significantly more time to complete.

## **Installation**

### Import getVisibleRect:

```javascript
import { getVisibleRect } from "https://damianmgarcia.com/scripts/modules/VisibleDOMRect.js";
```

## **Usage**

### Get the size and position of an element's visible, viewport-clipped bounding client rectangle:

```javascript
getVisibleRect(element);
```

## Functions

### `getVisibleRect`

Get the visible, viewport-clipped subset of a DOM element's bounding client rectangle.

### Parameters

- `element` **{Element}** - The DOM element.
- `maxDivisions` **{number}** _(optional, default=8)_ - The maximum number of times to divide the element in half in search of a visible point.

### Returns

- **{[VisibleDOMRect](#visibledomrect-1)}**

## Objects

### `VisibleDOMRect`

The `VisibleDOMRect` represents the visible, viewport-clipped subset of an element's DOMRect rectangle. It is returned by the [`getVisibleRect`](#getvisiblerect) function.

### Properties

- **`top`** **{number}** - The y-coordinate of the top edge of the visible rectangle.
- **`bottom`** **{number}** - The y-coordinate of the bottom edge of the visible rectangle.
- **`left`** **{number}** - The x-coordinate of the left edge of the visible rectangle.
- **`right`** **{number}** - The x-coordinate of the right edge of the visible rectangle.
- **`height`** **{number}** - The height of the visible rectangle.
- **`width`** **{number}** - The width of the visible rectangle.

### Special Case

If all the values (`top`, `bottom`, `left`, `right`, `height`, and `width`) are `0`, this is a sentinel value indicating that no `VisibleDOMRect` was found. This occurs when the element is completely outside the viewport, fully obscured, or not visible within the viewport.

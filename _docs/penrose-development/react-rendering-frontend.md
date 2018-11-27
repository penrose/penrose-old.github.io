---
title: React Rendering and Frontend
category: Penrose Development
order: 8
---

To develop for the React Rendering Frontend (at the time of this writing, located in `src/react-renderer` in the `end-user` branch), get set up with Typescript and Node.

Run `npm install` on first clone.

Run `npm start` to open a hot-reloading environment in the browser.

# Debug Logging

A scoped logger is available throughout the entire project. It's auto-enabled in the development context, and in production, can be enabled by setting the localstorage key `debug` to `renderer:*` (in addition to any other comma-separated debug scopes you want).

Use it by importing `src/Log.ts` e.g. `import Log from "./Log";`. Then you can use `Log.info(message: string, source?: string)`, `Log.warn(..)`, `Log.error(..)`.

# Using the Rendering Frontend as a Dependency (npm package)

First, you must build a static copy of the frontend by running `npm run build-lib` in the directory. This is different from `npm run build` because the latter builds everything as a standalone website bundle, while we only want the Javascript modules.

Next, you must link the built package to the global npm scope on your machine. Run `npm link`.

Next, navigate to the dependent's directory and run `npm link react-renderer` to allow for importing from the renderer.

You must also supply `MathJax` in the global scope of your dependent, as this is not currently included in the `build-lib` process. Simply using `<script src=...` and copying the `MathJax.min.js` file from the `public/` renderer directory should be sufficient.

Now, you can include the rendering frontend using `import Renderer from "react-renderer";`. Its API is as follows:

* Prop: `ws : WebSocket`: supply a websocket object to override the renderer instantiating its own redundant connection (useful if your dependent also needs to communicate with the server on the same socket).
    * If you do this, you need to pass websocket `onmessage` events down yourself. Use a React ref and in your dependent, call the ref's `current.onMessage(e)` method.
* Prop: `customButtons : boolean`: hides the autostep, resample, and download buttons so that you can use your own interface for them. To call the methods, use a React ref and call the ref's `current.autoStepToggle()`, `current.resample()`, and `current.download()` methods, respectively.

If you make changes to the rendering frontend, you usually only have to repeat the step, `npm run build-lib` in the renderer directory to trigger a hot reload for your dependents.

## Misc. Troubleshooting

If you restart your dependent's build process after a while and it suddenly can't resolve `react-renderer`, just run `npm link react-renderer` again. This also can work if your "websocket is stuck in connecting state".

# Adding Shapes

Each shape ([[GPI]]) is represented as a typed React component. There is typically a 1:1 relationship between GPI and component. These relationships are defined in `componentMap.tsx`.

If you anticipate your shape extending simple dragging behavior a la [`Circle`](Shape-APIs#circle) and [`Rectangle`](Shape-APIs#rectangle), your component should implement the `IGPIPropsDraggable` props found in `types.tsx`. Otherwise, you can implement the less complex parent, `IGPIProps`.

Both interfaces supply the `[width, height]` of the canvas, for convenience of coordinate system calculations (note: the width/height are not necessarily the true dimensions rendered on a page, they are just the common coordinate system of the SVG target). Optionally, the interfaces supply an `onShapeUpdate` event which, when called, sends a shape update packet directly back to the server. They also supply an optional `dragEvent` event, which sends a `dy` and `dx` back to the server if needed. The `Draggable` higher-order component uses this event.


***


For shapes implementing `IGPIPropsDraggable`, additional intermediate `dy` and `dx` props are supplied for the implementer to render the intermediate dragging coordinates and reconcile them on their own with their custom coordinate system. For most use cases, it is simply a matter of subtracting `dx` from the resultant `x` position on render, and adding `dy` to `y`.

The implementer must also accept an `onClick` event to signal the `mouseDown` start of a drag event (if your shape has a finnicky/hard to grab bounding box, look into SVG's `pointerEvents` API, or wrap your shape in a `<g>` with a bigger width/height, use `pointerEvents="bounding-box"`, and apply the `mouseDown` event to the `<g>`).

Finally, to endow the shape with draggable behavior, in your export, compose the shape with `draggable(...)`, the function exported in `Draggable.tsx`. For instance, `export default draggable(Rectangle);`.

***

Finally, for any shape, you must map the `tag` name to the component class in `componentMap.tsx`.

A sample shape, [`Circle`](Shape-APIs#circle), valid at this time of this writing:

```tsx

import * as React from "react";
import { toScreen } from "./Util";
import draggable from "./Draggable";
import { IGPIPropsDraggable } from "./types";

class Circle extends React.Component<IGPIPropsDraggable> {
  public render() {
    const props = this.props.shape;
    const { dx, dy, onClick } = this.props;
    const { canvasSize } = this.props;
    const [x, y] = toScreen([props.x, props.y], canvasSize);
    const color = props.color[0];
    const alpha = props.color[1];
    return (
      <circle
        cx={x - dx}
        cy={y + dy}
        r={props.r}
        fill={color}
        fillOpacity={alpha}
        onMouseDown={onClick}
      />
    );
  }
}
export default draggable(Circle);
```

And its appropriate mapping in `componentMap.tsx`:

```diff
// Map between "tag" and corresponding component
import Circle from "./Circle";
import Label from "./Label";
import Rectangle from "./Rectangle";

// prettier-ignore
const componentMap = {
+  "Circle": Circle,
  "Rectangle": Rectangle,
  "Text": Label
};

export default componentMap;
```

# Upgrading MathJax

1. `git clone https://github.com/pkra/MathJax-single-file.git` somewhere
2. Ensure the version of MathJax you want is in the `devDependencies` of the `package.json` (change it if not)
3. `npm install`
4. `npm run build`
5. Move `dist/TexSVGTeX/MathJax.min.js` to the `public/` directory of the frontend.

**NOTE: at the time of this writing, any dependents of the frontend have to supply their own copies of MathJax in global scope.** So when you follow these steps, put the upgraded `MathJax.min.js` in the dependents' scope too (`<script src=` etc).


***

See also: [[Common TypeScript Gotchas]]

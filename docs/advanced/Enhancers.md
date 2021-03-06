# Enhancers

Enhancers are similar to plugins. While plugins operate on the style object **every** render, enhancers **enhance** the renderer **once**.

### Use Case
They are used to add, remove or modify functionality. They can also be used as a wrapper for change subscriptions e.g. for logging or metrics reasons.

## Using Enhancers
To use plugins we need to add them to the renderer configuration directly. You can do this by passing a configuration object using the `enhancers` key while creating your renderer.

```javascript
import { createRenderer } from 'fela'

const config = {
  // It must be an array to be able
  // to pass multiple enhancers
  enhancers: [ /* your enhancers */ ]
}

const renderer = createRenderer(config)
```

### Example
Lets say we want to automatically beautify the rendered CSS output. This is a common use case as it makes debugging much simpler.

There is a great package called [cssbeautify](https://github.com/senchalabs/cssbeautify), which does exactly what we try to achieve. We just have to write an enhancers that adds the beautifier to the `renderToString`-method

> Note: Every enhancers gets the renderer passed as the only argument.

```javascript
import cssbeautify from 'cssbeautify'

function beautifier(renderer) {
  const existingRenderToString = renderer.renderToString.bind(renderer)

  renderer.renderToString = () => {
    const css = existingRenderToString()
    return cssbeautify(css)
  }

  return renderer
}

const config = {
  enhancers: [ beautifier ]
}

const renderer = createRenderer(config)
```

## Official Enhancers
Fela already ships with a set of useful enhancers. Check out [Introduction - Ecosystem](../introduction/Ecosystem.md#enhancers) for more information.<br>
> **Note**: Official enhancers are wrapped by a configuration function by default.

<br>

---

### Related
* [Renderer Configuration](RendererConfiguration.md)
* [List of enhancers](../introduction/Ecosystem.md#enhancers)
* [API Reference - `enhance`](../api/fela/enhance.md)

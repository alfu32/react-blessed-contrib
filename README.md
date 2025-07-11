# react-blessed-contrib

fork of [github/dundalek/react-blessed-contrib](https://github.com/dundalek/react-blessed-contrib)
on npm [npm/react-blessed-contrib](https://www.npmjs.com/package/react-blessed-contrib)

new release on [npm/alfu32/react-blessed-contrib-17](https://www.npmjs.com/package/react-blessed-contrib-17)

A wrapper for [blessed-contrib](https://github.com/yaronn/blessed-contrib) widgets to be rendered with [react-blessed](https://github.com/Yomguithereal/react-blessed).

## Installation

You can install `react-blessed-contrib-17` through npm:

```bash
npm install react blessed react-blessed
npm install react-blessed-contrib-17
```

## Demo

For a quick demo you can clone this repository and check some of the examples:

```bash
git clone https://github.com/alfu32/react-blessed-contrib
cd react-blessed-contrib
npm install

# Some examples (code is in `examples/`)
npm run dashboard
npm run charts
npm run basic

# Run any example with babel-node
./node_modules/.bin/babel-node examples/charts.js
```

## Usage

### Using components

Import components and render with React. You can mix them with native react-blessed components. Most components can be used directly as shown in the example. Refer to following sections to see how to use layout components like Grid and Carousel.

**Note:** When using refs, the actual blessed widget instance (which you use to call methods like `scrollTo`) is located on the `widget` property of the component ref that's passed to your ref callback. See the ref on the `Bar` component below for a usage example.

```js
import React, { Component } from 'react';
import blessed from 'blessed';
import { render } from 'react-blessed';
import { Bar } from 'react-blessed-contrib-17';

// Rendering a simple centered box with a bar chart
class App extends Component {
  render() {
    return (
      <box top="center"
           left="center"
           width="80%"
           height="80%"
           border={{type: 'line'}}
           style={{border: {fg: 'blue'}}}>
        <Bar
          ref={r => this.barRef = r ? r.widget : null}
          label="Server Utilization (%)"
          barWidth={4}
          barSpacing={6}
          xOffset={0}
          maxHeight={9}
          data={{
            titles: ['bar1', 'bar2'],
            data: [5, 10]
          }}
        />
      </box>
    );
  }
}

// Creating our screen
const screen = blessed.screen();

// Rendering the React app using our screen
const component = render(<App />, screen);
```

### Grid

Pass in children components to use a grid layout:

```js
import { Grid, Map } from 'react-blessed-contrib-17';

<Grid rows={12} cols={12}>
  <Map row={0} col={0} rowSpan={4} colSpan={4} label="World Map" />
  <box row={4} col={4} rowSpan={4} colSpan={4}>My Box</box>
</Grid>
```

If you don't specify `rowSpan` or `colSpan` then `1` is used as a default:

```js
import { Grid, Map } from 'react-blessed-contrib-17';

<Grid rows={1} cols={2}>
  <Map col={0} label="World Map" />
  <box col={1}>My Box</box>
</Grid>
```

In case there would be name conflicts with props (`row`, `col`, `rowSpan`, `colSpan`), you can use alternative notation:

```js
import { Grid, GridItem, Map } from 'react-blessed-contrib-17';

<Grid rows={12} cols={12}>
  <GridItem row={0} col={0} rowSpan={4} colSpan={4} component={Map} options={{label: 'World Map'}} />
  <GridItem row={4} col={4} rowSpan={4} colSpan={4} component={'box'} options={{content: 'My Box'}} />
</Grid>
```

### Carousel

Pass in subcomponents as children. Refer to `examples/carousel.js` for full example.

```js
import { Carousel } from 'react-blessed-contrib-17';

<Carousel interval={3000} controlKeys={true} screen={screen}>
  <Page1 />
  <Page2 />
</Carousel>
```

### Wrapping a custom blessed widget

Say you have a custom blessed widget:
```js
const widget = myBlessedWidget();
screen.append(widget);
```

You can wrap it in a component and use like:
```js
import React from 'react';
import { render } from 'react-blessed';
import { createBlessedComponent } = 'react-blessed-contrib-17';

const MyBlessedWidget = createBlessedComponent(myBlessedWidget);

render(<MyBlessedWidget />, screen);
```

## License

MIT

## Resources

Useful resources for learning more about React internals:

- https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html
- http://goshakkk.name/react-custom-renderers/

# generic-jsx-transpiler [![npm version](https://badge.fury.io/js/generic-jsx-transpiler.svg)](https://badge.fury.io/js/generic-jsx-transpiler) [![Build Status](https://travis-ci.org/c-mcg/generic-jsx-transpiler.svg?branch=master)](https://travis-ci.org/c-mcg/generic-jsx-transpiler)

A library for transpiling JSX for use without React.

# Setup

`npm install generic-jsx-transpiler --save-dev`

`npm install` if running tests or building dist

## Usage

```
const transpiler = require('generic-jsx-transpiler').default;

const testJsx = `function hello() {
    return (
      <div>
        <span></span>
        <span></span>
      </div>
    );
  }`

  function serialize(parsedComponent){
    const { tag, props, children } = parsedComponent;
    
    const childJS = children.reduce((acc, child, index) => {
          acc += `${serialize(child)}`;
          if (index != children.length - 1) acc += ', ';
          return acc;
      }, "");
    
    //Transpile to a React style `createElement` function
    return `createElement("${tag}", ${props.toJS()}${childJS ? `, ${childJS}` : ''})`;
  };
const serializer = {serialize};

const parser = new transpiler.Parser({ source: testJsx, serializer });

console.log(parser.start());
```

## Limitations

 - Currently can't parse raw text inside JSX

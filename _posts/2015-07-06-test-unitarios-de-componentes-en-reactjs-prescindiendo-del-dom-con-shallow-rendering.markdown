---
author: dandel
comments: true
date: {}
layout: post
slug: "test-unitarios-de-componentes-en-reactjs-prescindiendo-del-dom-con-shallow-rendering"
title: Test unitarios de componentes en ReactJS prescindiendo del DOM con Shallow Rendering
wordpress_id: 168
categories: 
  - Frontend
tags: 
  - React
  - shallow render
  - TDD
  - unit testing
published: true
excerpt: En este post explico cómo implementar tests unitarios sobre tus componentes de UI utilizando shallow rendering.
---


En un [artículo](https://thecraftsmansjourney.wordpress.com/2015/06/18/configurando-un-entorno-en-webpack-para-trabajar-con-react/) publicado previamente hablé sobre el entorno que hemos preparado para desarrollar componentes atómicos con **ReactJS** y de todas las micro-decisiones que hemos tomado sobre el mismo. Con lo que respecta a los tests unitarios, hasta ahora hemos utilizado [Karma](http://karma-runner.github.io/0.12/index.html) debido a su compatibilidad con [webpack](http://webpack.github.io) y su facilidad de configuración, pero este mundo evoluciona muy rápido y ya hemos encontrado una opción mejor: implementar [**shallow rendering**](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering). Esta opción además, es la [recomendada por Facebook](https://discuss.reactjs.org/t/whats-the-prefered-way-to-test-react-js-components/26/2) a partir de la versión 0.13, así que no se puede estar más a la última.

```javascript
    import React from 'react';
    import TestUtils from 'react/lib/ReactTestUtils';
    import expect from 'expect';
    import MyComponent from '../src/my-component';
    
    describe('my component test suite', function () {
       it('renders into document', function() {
          var root = TestUtils.renderIntoDocument();
          expect(root).toExist();
       });
    });
```

Tal y como explico en mi [post](https://thecraftsmansjourney.wordpress.com/2015/07/06/reactjs-agnostico-el-dom-como-un-detalle-de-implementacion/) anterior, el DOM es simplemente un detalle de implementación, así que, ¿por qué testar sobre un DOM, si es solo una de las posibilidades que tiene ese componente de renderizarse? He aquí el por qué de que los creadores de Facebook recomienden utilizar _**shallow rendering**_. Veamos cómo hacerlo.

## Convirtiendo nuestros tests unitarios sobre el DOM a tests agnósticos a la implementación

Cojamos por ejemplo el test anterior. Por si solo no podría funcionar, ya que **depende** de Karma para levantar un browser que nos ofrezca un DOM sobre el que montar los componentes. En nuestro caso utilizábamos PhantomJS, que además requería de un [polyfill](https://www.npmjs.com/package/phantomjs-polyfill) para soportar algunas funciones que faltan. Pero podemos prescindir de él. Si miramos la [especificación](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering) que nos da Facebook para utilizar _shallow rendering_, vemos que primero necesitamos instancia un _renderer_:

```javascript
    import React from 'react/addons';
    const TestUtils = React.addons.TestUtils;
    const shallowRenderer = TestUtils.createRenderer();
```

A continuación, debemos obtener un ReactElement, para poder acceder a sus propiedades y realizar los tests unitarios que deseemos. Hay dos formas de hacerlo, podemos renderizar nuestro componente utilizando sintaxis JSX:

```javascript
    shallowRenderer.render();
```

O mediante la utilidad `React.createElement`:

```javascript 
    shallowRenderer.render(React.createElement(MyComponent));
```

A partir de este punto, ya podemos obtener el componente:

```javascript
    const component = shallowRenderer.getRenderOutput();
```    

Esto nos dará un objeto similar al siguiente:

```javascript
    {
      "type": "article",
      "_store": {
        "props": {
          "className": "my-component",
          "children": [{
            "type": "h1",
            "_store": {
              "props": {
                "className": "my-component__title",
                "children": "Title"
              },
              "originalProps": {
                "className": "my-component__title",
                "children": "Title"
              }
            }
          }]
        }
      }
    }
```

El cual podemos testear:

```javascript 
    expect(component.props.className).toBe('my-component');
```

El ejemplo anterior quedaría de la siguiente forma:

```javascript   
    import React from 'react';
    import TestUtils from 'react/lib/ReactTestUtils';
    import expect from 'expect';
    import MyComponent from '../src/my-component';
    
    const shallowRenderer = TestUtils.createRenderer();
    shallowRenderer.render(React.createElement(MyComponent));
    const component = shallowRenderer.getRenderOutput();
    
    describe('my component test suite', function () {
       it('renders properly', function() {
          expect(component).toExist();
       });
    });
```


##Referencias##

[Unit testing React components without a DOM](http://simonsmith.io/unit-testing-react-components-without-a-dom/), con varios ejemplos más extensos que el que yo he expuesto aquí [@blinkdesign](http://twitter.com/blinkdesign)

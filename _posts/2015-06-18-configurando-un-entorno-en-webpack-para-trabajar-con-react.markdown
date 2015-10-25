---
author: dandel
comments: true
date: 2015-06-18T10:06:56.000Z
layout: post
slug: "configurando-un-entorno-en-webpack-para-trabajar-con-react"
title: Configurando un entorno en Webpack para trabajar con React
wordpress_id: 156
categories: 
  - Frontend
tags: 
  - Babel
  - ES6
  - karma
  - mocha
  - node
  - React
  - Webpack
published: true
excerpt: En este post explico el entorno de trabajo que utilizamos en Schibsted Spain para trabajar con React
---


Tengo la inmensa suerte de estar trabajando desde hace un mes y medio con [@d4vecarter](https://twitter.com/d4vecarter) y [@carlosvillu](https://twitter.com/carlosvillu) en un equipo en el que estoy aprendiendo decenas de cosas nuevas cada día. La tecnología que estamos utilizando es [React](http://facebook.github.io/react/), la librería de javascript mas _trendy_ del momento.

Durante estas semanas hemos ido tomando muchísimas micro decisiones sobre el entorno de trabajo a utilizar, que condicionarán nuestros futuros desarrollos a partir de ahora. Después de probar con varias combinaciones, creemos que ya tenemos la configuración ideal y que más se adapta a nuestras necesidades.

El motivo de este post es describir ese entorno para compartirlo con aquellos que quieran empezar a trabajar con esta tecnología y se pierdan entre el inmenso mar de opciones que proporciona la comunidad de desarrolladores.

<!-- more -->


## Entorno de desarrollo con Webpack


![webpack](https://thecraftsmansjourney.files.wordpress.com/2015/06/webpack.png)

Webpack es la nueva niña bonita de las soluciones de _bundling _para JavaScript, una vez superado el hype de Grunt y Gulp. La verdad es que miro atrás y me doy cuenta de que descartamos estas dos herramientas prácticamente desde el primer día, sin ni siquiera haberles dado una oportunidad. Supongo que el motivo es que los primeros tutoriales que encontramos en la red se basaban en Webpack y una vez aprendimos a configurarlo ya no queríamos otra cosa. Aún así, hubo otras opciones que sí probamos y descartamos:


### Browserify


Browserify es una opción que funciona bastante bien manejando módulos CommonJS, pero tiene una cosa que nos tocaba bastante las narices: el [error EMFILE](https://github.com/substack/node-browserify/issues/1003). Aunque tu código tenga cinco líneas, carga una cantidad indecente de archivos solo para compactarlo, incurriendo en este error en máquinas con Mac OS X. Además, no es una solución global que te resuelva el precompilado de SASS o te proporcione un entorno de desarrollo en local con hot reloading, a no ser que la combines con otras soluciones, obligándote a tener demasiadas dependencias.


## npm


Hubo un momento en el que estuvimos a punto de tener el entorno de desarrollo configurado solo con scripts de npm, tal y como recomienda [@keithamus](https://twitter.com/keithamus) en un [artículo](http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/) muy inspirador que publicó en diciembre del año pasado. Sin embargo, vimos que el entorno final (muy cercano al de su ejemplo) tenía dos problemas: por un lado, era lento reflejando los cambios en el navegador, ya que tenía que pasar por varios pasos de preprocesado y escribir en disco el resultado antes de hacer el reload. Por otro lado, el número de dependencias necesario para tener todo funcionando es mayor que el que necesita webpack. Como ventaja, sí he de reconocer que la configuración es mínima y elimina la capa de complejidad introducida por Gulp, Grunt o Webpack.


## Por qué Webpack


Webpack permite disponer de un archivo de configuración para especificar el workflow de tareas que se deben realizar antes de empaquetar los módulos requeridos por tu aplicación. Tiene la gran ventaja de ser rapidísimo, al proporcionar el módulo [webpack-dev-server](http://webpack.github.io/docs/webpack-dev-server.html), que levanta un servidor http en local y sirve los archivos js y css compilados desde memoria, sin escribir en disco. Otra gran ventaja si trabajas con React es que te permite usar [react-hot-loader](https://github.com/gaearon/react-hot-loader) (también disponible para browserify), que vigila los cambios a nivel de componente y solo recarga esa parte de la página, haciendo que el proceso de build sea fugaz.

Esta es la configuración de webpack que tenemos actualmente:

    
    var path = require('path');
    var webpack = require('webpack');
    
    module.exports = {
    entry: [
     'webpack-dev-server/client?http://0.0.0.0:8080',
     'webpack/hot/only-dev-server',
     path.resolve(__dirname, 'index.jsx'),
     ],
     output: {
      path: path.resolve(__dirname, 'docs/dist'),
      filename: 'dist/index.js',
     },
     resolve: {
      extensions: ['', '.js', '.jsx'],
     },
     module: {
      loaders: [
       {
        test: /\.jsx?$/,
        loaders: [ 'react-hot-loader', 'babel-loader'],
        exclude: path.join(__dirname, 'node_modules')
       },
       {
        test: /\.css$/,
        loaders: ['style', 'css']
       },
       {
        test: /\.scss$/,
        loader: "style!css!sass"
       }
      ]
     },
     plugins: [
      new webpack.HotModuleReplacementPlugin()
     ]
    };


Como veis, no es demasiado difícil de interpretar lo que hace Webpack en nuestro entorno: levanta un server en local en el puerto 8080, proporciona un path público para los bundles en el path ./dist/ y procesa archivos de distinto tipo con sus loaders correspondientes.

Otra de las ventajas de Webpack es que te permite hacer `require` en tus componentes de cualquier tipo de archivo. De este modo, si en el archivo de configuración dispone del _loader_ adecuado, lo procesará antes de construir el bundle. Por ejemplo, en nuestra configuración, tenemos _loaders _de archivos .jsx de React, .css y .scss. Pero podríamos tener también imágenes y tratarlas con un loader que las redimensione y comprima adecuadamente, por ejemplo.


## ECMAScript 6


![js](https://thecraftsmansjourney.files.wordpress.com/2015/06/js.png)ECMAScript 6 es el nuevo estándar para JavaScript que [se acaba de aprobar ](http://www.ecma-international.org/publications/standards/Ecma-262.htm)en el momento en que escribo estas líneas, así que no podría estar más calentito. Debido a esto, el soporte de [la mayoría de navegadores](https://kangax.github.io/compat-table/es6/) a día de hoy no es muy bueno, pero nosotros llevamos todo este tiempo trabajando con él gracias al uso de [Babel](https://babeljs.io/), un transpiler que traduce el código escrito en ES6 a ES5, que es la versión anterior del estándar y está ampliamente soportada.

Eso nos ha permitido poder estar utilizando desde ya las ventajas del nuevo estándar, como el uso de las palabras reservadas `let` y `const` para declaración de variables, uso de _promises_, clases, decoradores y otras [ventajas de ES6](http://carlosvillu.com/tag/es6/).


## Tests unitarios con mocha y Karma


Una de las múltiples ventajas de usar React es que te permite crear componentes de interfaz de usuario atómicos y testeables de forma unitaria. Por eso hemos pensado que era buena idea proporcionar tests unitarios a nivel de componente, de forma que podamos liberar nuevas versiones asegurando que el funcionamiento esperado se mantiene. Cubrir el código con tests automáticos es un paso fundamental para montar un entorno de integración continua.

El framework de testeo elegido ha sido mocha. Facebook está recomendando [Jest](https://facebook.github.io/jest/) para testeo de componentes en React, pero tras investigar un poco por blogs y comunidades de desarrolladores, nos dio la sensación de que no estaba del todo probado para utilizarlo en un entorno de producción. Así que decidimos ser conservadores en este aspecto y elegimos [mocha](http://mochajs.org/), ya que algunos compañeros de la empresa ya lo estaban utilizando con éxito.

Como test runner elegimos [Karma](http://karma-runner.github.io/0.12/index.html), ya que se integra muy bien con webpack y hay un buen soporte de la comunidad.


## JS y CSS linting


En un equipo en el que trabajan varias personas, es importante que todos se pongan de acuerdo en seguir las mismas reglas de codificación. Desde nombres de variables al tipo de tabulación, espaciado... además de palabras restringidas y otras utilidades.

Pero como somos todos humanos, estas reglas se nos pueden olvidar o se pueden colar erratas en el código liberado en producción simplemente por un despiste. Así que para asegurarnos de que todo el mundo las cumple, utilizamos herramientas de [linting](https://en.wikipedia.org/wiki/Lint_%28software%29).

Las herramientas utilizadas son **[eslint](http://eslint.org/), [jsrc](http://jscs.info/) **para el código JavaScript y **[CSSLint](http://csslint.net/) **para el CSS. Con CSSLint nos encontramos el problema de que no es capaz de analizar SASS, así que lo precompilamos antes del análisis. Es un workaround que no nos gusta mucho, pero la alternativa es utilizar [SCSS Lint,](https://github.com/brigade/scss-lint) el cual tiene dependencia de Ruby. No queríamos añadir más complejidad a la hora de instalar todo el entorno, ya que procesamos SASS con node, así que de momento lo hemos descartado hasta encontrar una solución mejor.

Todos estos linters permiten indicar las reglas en ficheros específicos. Hemos puesto estos ficheros en un repositorio de github aparte y lo hemos publicado en un repositorio de npm privado. De modo que si actualizamos las reglas, los clientes solo tendrán que hacer `npm update` del paquete de reglas y adecuarse a la nueva convención.

La gran ventaja de estos ficheros es que si utilizas un editor tipo Sublime Text, mediante la instalación de un plugin, tendrás las reglas aplicadas en tiempo de codificación, sin tener que ejecutar la tarea al final. Con esto se reduce el feedback loop a la mínima expresión y un desarrollador nuevo se puede adaptar a las convenciones de codificación de la compañía en cuestión de horas.


## Orquestándolo todo con precommit-hook


De nada serviría haber creado todos los tests unitarios de una app hecha en React y todas las reglas de linting para JS y CSS si luego la comunidad de desarrolladores de la compañía no las utiliza.

Por este motivo, hemos decidido utilizar [precommit-hook](https://github.com/nlf/precommit-hook) para GitHub. Añadiendo un sencillo script a la configuración del package.json hacemos que antes de hacer commit, se ejecuten las tareas de linting y tests unitarios:

    
    {
      <span class="pl-s"><span class="pl-pds">"</span>name<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span class="pl-s"><span class="pl-pds">"</span>your_project<span class="pl-pds">"</span></span>,
      <span class="pl-s"><span class="pl-pds">"</span>description<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span class="pl-s"><span class="pl-pds">"</span>just an example<span class="pl-pds">"</span></span>,
      <span class="pl-s"><span class="pl-pds">"</span>scripts<span class="pl-pds">"</span></span><span class="pl-k">:</span> {
        <span class="pl-s"><span class="pl-pds">"</span>lint<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span class="pl-s"><span class="pl-pds">"</span>jshint --with --different-options<span class="pl-pds">"</span></span>,
        <span class="pl-s"><span class="pl-pds">"</span>validate<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span class="pl-s"><span class="pl-pds">"</span>./command/to/run<span class="pl-pds">"</span></span>,
        <span class="pl-s"><span class="pl-pds">"</span>test<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span class="pl-s"><span class="pl-pds">"</span>./other/command<span class="pl-pds">"</span></span>
      },
      <span class="pl-s"><span class="pl-pds">"</span>pre-commit<span class="pl-pds">"</span></span><span class="pl-k">:</span> [<span class="pl-s"><span class="pl-pds">"</span>lint<span class="pl-pds">"</span></span>, <span class="pl-s"><span class="pl-pds">"</span>test<span class="pl-pds">"</span></span>]
    }




## Recursos


[SurviveJS](http://survivejs.com/): De todos los recursos online que he encontrado para aprender a usar Webpack y React, el más completo con diferencia. Se trata de un libro online que te guía paso a paso explicándote cada línea de configuración detalladamente.

[Nodeschool.io](http://nodeschool.io/): Si la barrera de entrada para empezar a trabajar con Webpack y React os resulta demasiado insalvable, tal vez es buena idea que empecéis aprendiendo algo de **node**. Este es uno de los mejores recursos online que encontraréis, si no el que más.

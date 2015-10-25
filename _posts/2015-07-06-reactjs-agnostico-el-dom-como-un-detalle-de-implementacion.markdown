---
author: dandel
comments: true
date: 2015-07-06 19:27:16+00:00
layout: post
slug: reactjs-agnostico-el-dom-como-un-detalle-de-implementacion
title: 'ReactJS agnóstico: El DOM como un detalle de implementación'
wordpress_id: 173
categories:
- Frontend
tags:
- DOM
- React
---

En su día, cuando se anunció ReactJS, se habló de esta librería como **_una capa de visualización que utiliza un DOM virtual para optimizar el rendimiento_**. Otra de las colillas que se leían en todas partes es la de **_ReactJS es la V en MVC_**.

Estas definiciones le restan importancia al papel de ReactJS dentro del stack del frontend, pero tal y como [reconocieron](https://groups.google.com/forum/#!msg/reactjs/sB6IPgiXGe4/1os3fnQRAegJ) sus creadores más adelante, esta definición tan acotada fue intencionada. El objetivo era que la gente se quitase prejuicios a la hora de empezar a jugar con él, ya que su propuesta agrupa un buen número de prácticas que hasta ahora se consideraban **anti-patterns**.

Imaginad el rechazo de la comunidad, a la que ya le cuesta aceptar que estilos, layout y comportamiento no tienen sentido por separado, si además descubren que ReactJS en realidad no solo está pensado para ser la vista, **sino también el controlador**.

> WAT? React is the VC in MVC?

Sí... y no realmente. De hecho, los creadores enseguida se quisieron desmarcar del denostado patrón MVC, para proponer su propia arquitectura: [Flux](https://facebook.github.io/flux/). Parece que su estrategia con ReactJS es ir enseñando poco a poco el potencial de lo que han creado, para educar a la comunidad de frontend y cambiar progresivamente el _mindset_ general hacia un nuevo paradigma. Y poco a poco, nos vamos dando cuenta de que en realidad estamos ante una **herramienta de creación de UI universal**, que ha llegado para cambiar la filosofía con la que se construían webs hasta ahora. Sus puntos fuertes son la **composición, inmutabilidad, flujo de datos unidireccional, control absoluto sobre el estado y universalidad**.

> Entonces, ¿no necesitamos el DOM?

Necesitamos un DOM virtual sobre el que renderizar componentes de React, para medios web, pero [React Native](https://facebook.github.io/react-native/) ya ha demostrado que se pueden crear aplicaciones con componentes sin utilizar el DOM en absoluto. Y ya hay por ahí alguna prueba de concepto de una aplicación renderizada en consola simplemente con una TUI (Text User Interface). Y es que ReactJS va mucho más allá de ser una librería para renderizar componentes DOM, es una herramienta para **construir interfaces de usuario**.

En la última actualización de la librería, la **[versión 0.14 beta 1](http://facebook.github.io/react/blog/)**, [Ben Alpert](https://twitter.com/soprano) ya habla sin ambages de esta idea, con la que se nos ha evangelizado en varias de las charlas de la [**React Europe Conference**](https://www.react-europe.org). La belleza y la esencia de React no tiene nada que ver con navegadores o el DOM. Su verdadero fundamento se basa en ideas de componentes y elementos, el ser capaz de describir lo que queremos renderizar de forma **declarativa**. La separación es tan clara que en esta versión han separado las funciones en dos paquetes: `react` y `react-dom. `

¿Qué implicaciones tiene esto? Imaginad: a partir de ahora es posible construir una **interfaz de usuario agnóstica a la api de presentación**, librándonos del DOM para conseguir un rendimiento superior. Y no solo eso: es posible reutilizar componentes para otros dispositivos (apps en iOS, Android...). Todo es posible.

El futuro de esta librería cada vez es más apasionante.

##Referencias:

  * [You are missing the point of React](https://medium.com/@dan_abramov/youre-missing-the-point-of-react-a20e34a51e1a) - [@dan_abramov](https://twitter.com/dan_abramov?lang=es)
  * [React 0.14 Beta 1](https://facebook.github.io/react/blog/2015/07/03/react-v0.14-beta-1.html)
  * [@carlosvillu](https://twitter.com/carlosvillu) y su _evangelización persistente_ sobre el tema principal de este post :)



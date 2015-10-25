---
author: dandel
comments: true
date: 2015-02-20 08:26:30+00:00
layout: post
slug: monitoring-tools
title: Monitoring tools
wordpress_id: 68
categories:
- Journey
tags:
- continuous delivery
- monitoring
---

Hoy he tenido una reunión con Tina H Krekke, lead developer del SOA Team en Finn. Tina me da la impresión de ser alguien súper inteligente, siempre tiene respuestas y soluciones para todo y genera un clima de confianza y respeto en el equipo. En los días que llevo aquí, me ha hablado de muchísimas herramientas, frameworks y lenguajes que ni siquiera conocía. Es alguien admirable.

En la reunión de hoy ha tenido la paciencia de explicarme cómo monitorizan el estado de sus aplicaciones en producción. Aunque ha reconocido que aún les queda trabajo por hacer, tienen una serie de herramientas que les facilitan mucho el trabajo a la hora de saber qué ocurre en producción y diagnosticar problemas.

## Infraestructura

Comenzamos comentando una de las cosas que también había salido en alguna conversación con la gente del equipo de DevOps de fotocasa. Les gustaría mejorar el sistema de alertas para que sea más flexible y escalable a más desarrolladores. Ahora mismo solo las recibe la gente que está de guardia, pero su idea es que las pueda recibir cualquier desarrollador. La teoría es que si tú recibes cada día un sms que te saca de la cama a las 2 de la madrugada, **serás el primer interesado en solucionar el problema**. Tiene todo el sentido del mundo. Por eso están planteándose dejar de usar [Nagios](http://www.nagios.org/) como monitor de infraestructura para pasar a [Sensu](http://sensuapp.org/) (más escalable) e involucrar a todos los desarrolladores.

## Visualización

[![Captura de pantalla 2015-02-20 a las 9.04.30](https://thecraftsmansjourney.files.wordpress.com/2015/02/captura-de-pantalla-2015-02-20-a-las-9-04-30-e1424419511990.png)](https://thecraftsmansjourney.files.wordpress.com/2015/02/captura-de-pantalla-2015-02-20-a-las-9-04-30-e1424419511990.png)

Tan importante como la monitorización es la visualización de los logs, y para ello disponen de dos herramientas: [Graphana](http://grafana.org/), para gráficos de carga y disponibilidad (conexiones, cpu, memoria, espacio en disco... etc) y [Kibana](http://www.elasticsearch.org/overview/kibana/) para los logs de aplicación y excepciones. Ambas son igual de importantes y requieren la misma atención. Graphana funciona sobre [Graphite](http://graphite.wikidot.com/), proporcionando una mejor visualización de gráficas y usabilidad. Kibana funciona sobre [ElasticSearch](http://www.elasticsearch.org/), proporcionando todo tipo de búsquedas y filtros sobre los logs indexados en tiempo real.

La responsabilidad de vigilar los gráficos de estas herramientas es rotativa dentro del equipo. Cada semana, le toca a uno de ellos hacer de **guardián** y estar pendiente de las gráficas, para ver si algo está funcionando de manera anómala. Esto ayuda a repartir la responsabilidad y descargar de tareas a Tina, así siempre hay alguien haciendo lo que nadie quiere hacer.

En cuanto a medición de contadores de operaciones realizadas y tiempo por operación, utilizan [StatsD](https://github.com/etsy/statsd/), lo que les permite enviar también la información a Graphana para tener visibilidad.

## Calidad de código

Para medir la calidad del código, utilizan SonarQube. Tienen reglas para todos los lenguajes que utilizan, que no son muchos. Básicamente todo está hecho con Java, Groovy, Javascript y cómo no, HTML+CSS. Se ejecuta un análisis de Sonar tras cada compilación en el entorno de CI, aunque hay un detalle que tienen implementado que me parece muy interesante: en los entornos de local de cada desarrollador, **si se rompe una regla de sonar, no le deja compilar**. Esto lo tienen configurado como una regla de compilación más. Aunque hay gente a la que no le gusta esto, me parece una idea fantástica.

Al igual que con los bugs, cuanto antes tengas feedback sobre si la calidad de tu código es la correcta, mejor. De nada sirve esperar a una compilación en otro entorno que no es el tuyo, ya que probablemente ya hayas liberado tu historia de usuario y pasado a hacer otra cosa. Hacerlo así implica que "no subir el technical debt" es parte de la _definition of done_ para los programadores.

Todo este tipo de acciones diluyen la responsabilidad de los Lead Developers entre todos los miembros del equipo, lo cual me parece una actitud muy inteligente para hacer crecer el sentimiento de _ownership_.

[![Captura de pantalla 2015-02-20 a las 8.42.46](https://thecraftsmansjourney.files.wordpress.com/2015/02/captura-de-pantalla-2015-02-20-a-las-8-42-46-e1424419577908.png)](https://thecraftsmansjourney.files.wordpress.com/2015/02/captura-de-pantalla-2015-02-20-a-las-8-42-46.png)

## Continuous Delivery

Cualquier equipo tiene acceso a la herramienta **Pipeline**, desarrollada en la casa. Esta herramienta permite visualizar y gestionar la subida de "artefactos" que suben a producción. Todo el proceso está automatizado y muestra en una interfaz visual el estado de cada fase. Si en una fase fallan los tests automáticos, no te deja avanzar a la siguiente. Si una compilación falla en uno de los frontales, hace rollback automáticamente a la versión anterior. La herramienta te proporciona estadísticas muy útiles, por ejemplo los tiempos de ejecución de las últimas compilaciones o la frecuencia con la que cada test da error. También son capaces de configurar un conjunto de tests específico para cada fase y artefacto. Cualquier desarrollador tiene acceso a ella, así que cualquiera puede crear un _pipeline_ para subir a producción. Es muy potente.

Hay que recalcar que en Finn aún no están en el cloud, por lo que esta herramienta se encarga de subir a sus front ends físicos.

[caption id="attachment_70" align="aligncenter" width="1000"][![Los nodos de color verde, indican que esa fase del deploy ha tenido éxito. El nodo rojo, indica un fallo que tiene que ser verificado antes de pasar a la siguiente fase.](https://thecraftsmansjourney.files.wordpress.com/2015/02/captura-de-pantalla-2015-02-20-a-las-9-02-10-e1424419600705.png)](https://thecraftsmansjourney.files.wordpress.com/2015/02/captura-de-pantalla-2015-02-20-a-las-9-02-10.png) Los nodos de color verde, indican que esa fase del deploy ha tenido éxito. El nodo rojo, indica un fallo que tiene que ser verificado antes de dejarte pasar a la siguiente fase.[/caption]

## ¿Quién implementa todo esto?

Creo que todos estaréis de acuerdo en lo útiles que resultan todas estas herramientas en el día a día. Sin todo este ecosistema, me cuesta imaginarme un escenario de continuous delivery que funcione con éxito. Todas las piezas son importantes. Pero, ¿quién implementa todo esto? Está claro que los equipos de producto deberían estar centrados en aportar valor a sus clientes, y esto no se desplega en una tarde.

En Finn tienen un equipo de infraestructura que se encarga de desarrollar y mantener todas estas herramientas internas. Y algunas más que no he mencionado, que se escapan del ámbito del post. Este equipo, por ejemplo, fue el encargado de desarrollar pipeline, una herramienta clave que ha supuesto **un antes y un después en la velocidad de la compañía.**

Por otro lado, también hay un equipo de DevOps, probablemente el equipo más grande de todo Finn. Están sentados junto al equipo de infraestructura, ya que es habitual que tengan que trabajar codo con codo.

En el futuro, tienen planeado que un miembro de cada equipo vaya a hacer un _internship_ al equipo de DevOps, para ir repartiendo la responsabilidad de las tareas de DevOps a los equipos. Esto eliminará dependencias y hará que los equipos sean más autosuficientes. Otra idea que vale la pena probar.

## Conclusiones

De cara a conseguir un escenario de continuous delivery en el que cualquier desarrollador sea capaz de subir a producción con seguridad y agilidad, hay que invertir en mejorar las herramientas de las que disponen. Todas las herramientas mencionadas están destinadas a satisfacer tres objetivos:
	
  * Conseguir feedback del estado de cada fase del proceso lo antes posible.
  * Automatización de tareas para reducir la intervención manual a la mínima expresión.
  * Repartir la responsabilidad entre toda la fuerza técnica de la compañía, no solo en unos pocos.

Invertir en disponer de este tipo de herramientas me parece vital para una organización que pretende ser ágil competitiva.

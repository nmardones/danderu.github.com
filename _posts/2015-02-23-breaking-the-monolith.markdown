---
author: dandel
comments: true
date: 2015-02-23T17:59:36.000Z
layout: post
slug: "breaking-the-monolith"
title: Breaking the monolith
wordpress_id: 83
categories: 
  - Journey
tags: 
  - microservices
published: true
excerpt: "Hoy he tenido el placer de hablar con Mick Semb (@mck_sw), un auténtico máquina en lo que arquitectura de software se refiere."
---


Hoy he tenido el placer de hablar con Mick Semb ([@mck_sw](https://twitter.com/mck_sw)), un auténtico máquina en lo que arquitectura de software se refiere. Hemos hablado sobre la arquitectura actual de fotocasa y me ha explicado cómo ellos hace no mucho tenían una arquitectura similar, monolítica e inmantenible. A día de Finn funciona con casi 300 microservicios en producción, así que he aprovechado para preguntarle todas las dudas que tenía sobre el proceso de despliegue de una arquitectura de tal calibre. He tenido la suerte de que pudiese encontrar el tiempo para compartir su conocimiento conmigo.

## Evitar usar APIs

Lo primero que me ha dicho es que procuremos no cometer los mismos errores que cometieron ellos. Una primera idea que se te puede ocurrir cuando tienes una aplicación monolítica con un legacy brutal es desacoplar la capa de presentación del backend y exponer este último tras una API. Puedes reutilizar código o hacerla de cero, pero ambas requieren un coste elevadísimo en relación al beneficio que obtienes, ya que probablemente vas a replicar la misma arquitectura y no vas a solucionar el problema de raíz, sino un síntoma. Pensar en comenzar con una API es empezar con algo demasiado grande, arriesgado y que no va a solucionar nada, ya que será un punto intermedio hacia la arquitectura que pretendes conseguir.

## Empezar rompiendo la base de datos

La arquitectura de una aplicación suele ser (o debería) el reflejo de la arquitectura de datos sobre la que se construye. Si se utiliza una base de datos relacional, es difícil imaginar un escenario en el que no se convierta en un cuello de botella, por mucho que separes componentes en microservicios. Así que para romper el monolito con éxito, lo más efectivo es empezar por los cimientos y empezar a migrar entidades de datos a modelos desnormalizados.

Al preguntarle si podría haber problemas para sincronizar datos entre microservicios, Mick me dio una explicación muy convincente. Según el **teorema de Brewer**, es imposible para un sistema distribuido garantizar la consistencia, la disponibilidad y la tolerancia al particionado al mismo tiempo. Siempre hay que sacrificar alguna de ellas. En los escenarios de aplicaciones web actuales, si se quiere conseguir particionado y una alta disponibilidad, hay que "renunciar" a la consistencia. Aunque luego veremos que se puede tener consistencia entre microservicios utilizando otras estrategias. Tiene sentido para mi.

## Evitar librerías

Trabajar con librerías es un error a corto plazo. Generalmente utilizamos librerías para reutilizar código, pero _**DRY is not evil**_. La contrapartida es que en cuanto empiezas a tener un cierto número de microservicios y equipos de trabajo evolucionándolos a su ritmo, el versionado y la distribución se convierten en un problema muy serio. Además, el uso de librerías normalmente supone un cierto grado de acoplamiento que también puede generar dolores de cabeza. La recomendación es simplemente crear servicios y exponer sus contratos vía REST o [Thrift](https://thrift.apache.org/), ya que son lo suficientemente rápidos como para no notar la diferencia.

## Utilizar una arquitectura orientada a eventos

Es importante intentar que los microservicios que se vayan creando tengan el mínimo de dependencia entre ellos, por eso es adecuada una arquitectura orientada a eventos. En Finn, los microservicios se comunican entre sí utilizando un **Message Bus **implementado con [Kafka](http://kafka.apache.org/documentation.html#introduction). Dichos servicios publican sus acciones en Kafka y los servicios se suscriben a dichos eventos, reaccionando a los cambios y realizando las acciones oportunas. Este planteamiento evita cualquier dependencia entre servicios y permite que, si una parte de la aplicación deja de funcionar, el resto no se vean afectadas.

Obviamente, esta implementación implica una latencia y redundancia de datos (recordemos el teorema CAP). Para ellos, existe un **Data Highway **en el que solo se publican cambios en los datos, que son consumidos por los servicios que necesiten disponer de ellos lo antes posible. Por ejemplo, existe un microservicio que solo se encarga de informar si un anuncio está disponible o ha expirado. Este servicio es consultado por otros, como por ejemplo el que notifica novedades vía push o un recomendador de anuncios. Estos servicios no tienen por qué tener la última versión de los datos, pero el servicio de disponibilidad sí.

He hecho un dibujo vergonzoso en la libreta del escenario (que valdría para cualquier portal del mundo):

[![Archivo 23-2-15 18 30 12](https://thecraftsmansjourney.files.wordpress.com/2015/02/archivo-23-2-15-18-30-12.jpeg)](https://thecraftsmansjourney.files.wordpress.com/2015/02/archivo-23-2-15-18-30-12.jpeg)

Todas las combinaciones son posibles. El µS de "Ad" utiliza otros tres para montar un objeto anuncio completo, aunque no tiene por qué tener implementada la lógica de negocio de cada uno de ellos. Se comunican entre sí utilizando Thrift.

Si se produce un cambio en el µS "Ad Extras", éste se comunica al Data Highway, que solamente es consumido por el recomendador de anuncios. Este último, tiene un repositorio local desnormalizado en el que mantiene actualizada la última versión con los datos, para no atacar directamente a la base de datos principal.

Por encima, tenemos una capa de presentación delgada capaz de atacar a los µS que necesite.

## Monitorización y automatización

Por último, pero no menos importante, hemos vuelto a insistir en la importancia de invertir en la monitorización y automatización. Para los equipos de infraestructura y operaciones, controlar casi 300 microservicios sería imposible si no dispusieran de las herramientas adecuadas, que ya mencioné en [mi otro post al respecto](https://thecraftsmansjourney.wordpress.com/2015/02/20/monitoring-tools/).

## Conclusiones

Bueno, no soy Martin Fowler, así que para mí es difícil explicar con detalle cómo se implementa una arquitectura de microservicios en un blog, así que espero haberlo hecho lo suficientemente bien. Al menos creo que por primera vez después de la charla con Mick, tengo la imagen en mi cabeza. Aunque entendía el concepto, para mi lo más difícil era pensar en la mejor estrategia para empezar a "romper" el monolito e implementar pequeños µS, ya que siempre que aunque es algo que está en boca de todos, nunca he podido hablar con alguien que lo haya realizado con éxito y tuviese una experiencia de campo tan amplia. Así que ha sido un gran día :)

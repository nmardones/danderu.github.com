---
author: dandel
comments: true
date: 2015-02-17 20:44:04+00:00
layout: post
slug: continuous-delivery-un-punto-de-inflexion
title: 'Continuous delivery: un punto de inflexión'
wordpress_id: 26
categories:
- Journey
tags:
- agile
- continuous delivery
- Finn
- kanban
- scrum
---

Mis primeras conversaciones con Marthe fueron dirigidas a la forma de trabajar de su equipo. Según las charlas que tuvimos por email, ellos han superado la etapa de probar con Scrum, kanban y otros _frameworks_ y están desarrollando su propia metodología. Esto ocurre dentro de su equipo, ya que al igual que en fotocasa, no existe una prescripción de la metodología que se deba utilizar.

Esperaba encontrarme algo muy sofisticado, una forma de trabajar que mi nivel de inmadurez no me permitiese ni siquiera imaginar, pero nada más lejos de la realidad. Al ver su tablón, me encontré con algo tan sencillo que mi primera reacción fue de sorpresa. Y la siguiente de vergüenza, al darme cuenta de que todo es mucho más sencillo de lo que parece y nosotros muchas veces lo complicamos innecesariamente.

Su tablón es de tipo kanban, con varias columnas que reflejan su proceso de trabajo, que es el siguiente:

---
ToDo
In Progress
Code Review
Test
Done
Production
---

  * **ToDo**: Las historias de usuario entran ya priorizadas en esta columna. Tanto historias de producto como historias de "mantenimiento". Los bugs, siempre entran con la prioridad máxima, arriba de todo. La prioridad es descendiente de arriba a abajo. Más adelante me detendré a hablar de cómo asignan prioridades.
  * **In Progress**: La tarea está en curso. Pueden trabajar una o dos personas en ella, pero no es prescriptivo. Cada miembro del equipo solo tiene dos imanes, por lo que solo puede encargarse de dos cosas a la vez. Típicamente, solo se encargará de una. El segundo imán se reserva para cuando tienes que ponerte a ayudar a alguien o a resolver un bug urgente y tienes que dejar bloqueada tu tarea principal.
  * **Code Review:** Trabajan con git y cada vez que quieren hacer push, automáticamente se crea un _pull request_, para que Tina (la Lead Developer) revise el código y proporcione el feedback oportuno. Me parece interesante que esta fase esté incrustada dentro del flujo de trabajo de las tareas de forma ineludible.
  * **Test**: Tengo que explorar un poco más sobre esto, pero tengo la impresión de que no hacen TDD, sino que hacen testing manual al final del proceso. Escriben tests automáticos, pero si me tengo que fiar de lo que refleja el tablón, no los hacen al principio. Esto me hace pensar que aquí quizás podría aportar algo al equipo.
  * **Done: **Tarea terminada y pendiente de subir a producción.
  * **Production: **En producción.

Fijaos que no tienen columnas de _fires _ni _prios_. Otro de los habituales en nuestras pizarras, un síntoma de que hay algo en nuestros procesos e infraestructuras que no funciona demasiado bien.

Este equipo en concreto, en lugar de post-its, utilizan fichas algo más elaboradas con una iconografía que indica el tipo de tarea, el responsable y la fecha en la que se mueve a cada columna. Una parte muy interesante de la ficha es el cuadro coste-beneficio, que ayuda a priorizar las tareas en la columna ToDo:

[![user stories](https://thecraftsmansjourney.files.wordpress.com/2015/02/user_story1.jpg)](https://thecraftsmansjourney.files.wordpress.com/2015/02/user_story1.jpg)

Como anécdota, las columnas tenían un límite, tal y como se trabaja en kanban. Al preguntarle por ello a Marthe, se quedó un segundo en blanco y lo borró delante de mí. "**No sé qué hace eso ahí. Hace tiempo que ni lo miramos, no lo necesitamos**".

Esto me produjo un cortocircuito. ¿Eso es todo? Parecía tan "de cajón" que me da mucha vergüenza pensar en la cantidad de problemas que tenemos para implementar esta forma de trabajar con éxito. Marthe vio el humo que salía de mi cabeza y se compadeció de mi: "Nosotros hemos pasado por lo que tú estás pasando, no le des demasiadas vueltas" - me dijo. Y añadió: "No somos mega cracks de la agilidad, pero hubo algo que supuso **un punto de inflexión** en nuestra forma de trabajar que lo cambió todo". Y ese algo fue...


## Continuous delivery


Desde que se implementó **continuous delivery** en Finn cambiaron las reglas del juego. Y por arte de magia, desaparecieron la mayoría de los problemas que frenaban a los equipos para entregar valor. El hecho de ser capaces de realizar varios pases a producción al día redujo al mínimo el ciclo del **_feedback loop_** y les permite reaccionar rápido a las funcionalidades que no funcionan como esperaban. Esto también hizo que apenas tuviesen **bugs en producción**, no tanto por la automatización de testing sino por la rapidez con la que son capaces de detectarlos y solucionarlos.

En este escenario, la gran mayoría de ceremonias involucradas con las metodologías ágiles dejan de tener sentido. Los equipos reaccionan rápido a los cambios y su backlog siempre tiene pocas tareas, ya que viven cómodamente con un nivel de incertidumbre a una semana vista. La priorización es sencilla, lo cual hace que no sea relevante planificar y estimar, ya que se obtienen resultados rápidamente. Aunque por supuesto, hay más detalles que marcan la diferencia, este me parece el más determinante con diferencia.

Esto me hace estar más convencido de lo que llevo un año persiguiendo en fotocasa: implementar un proceso de CD. Todo el tiempo que no invertimos en esto es tiempo que estamos multiplicando por diez para entregar valor a nuestros clientes y usuarios.

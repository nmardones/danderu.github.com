---
author: dandel
comments: true
date: 2015-02-26 05:39:06+00:00
layout: post
slug: lean-the-finns-way
title: Lean - The Finn's way
wordpress_id: 97
categories:
- Journey
tags:
- agile
- lean
---

Es bastante común que las empresas que empiezan a implantar metodologías ágiles, los primeros en adoptarlas sean las personas del departamento de IT.

Motivos hay varios. Generalizando un poco, podría decirse que la gente de IT está más abierta a los cambios y la mejora continua, ya que es algo que está en el ADN de nuestro oficio. Por otro lado, son los responsables del proceso de creación del producto, así que si se quiere mejorar la eficiencia de una compañía, es normal que se intente hacer foco en esta fase de la "cadena de montaje". Porque las empresas que no son ágiles - no nos engañemos - funcionan como una cadena de montaje. Aunque ya sabemos que el desarrollo de software no es un proceso determinista y matemático, precisamente; sino que está mucho más cercano a una artesanía.

La gestión del cambio no puede pretender aplicarse solo en un departamento de una compañía, o en un grupo de individuos en concreto. Esa estrategia está condenada al fracaso. Tiene que ser algo generalizado, que venga de los managers y los líderes de equipo.

No creo haber aprendido demasiado sobre liderazgo de personas durante este año y pico que he ejercido como Tech Lead en fotocasa. Pero sí que me he dado cuenta de algo: la gente a la que lideras **trabajarán como tú, lo intentes o no**. Si tú trabajas solo, no puedes pedir que los demás hagan _pair programming_. No puedes pedir una cosa y hacer la contraria, porque tus actos pesan más que tus palabras.

En Finn lo entendieron tras los primeros años haciendo Scrum. Aunque supuso un punto de mejora importante, terminó convirtiéndose en un pequeño fiasco. Scrum es tan prescriptivo que es difícil hacerlo mal, siempre y cuando se sigan a rajatabla las reglas que nos vienen dadas. Sprint planning meetings, daily stand ups, iteraciones de dos semanas, retrospectivas, demos... Hay medición de la velocidad, aprendizaje, pero tanta ceremonia hace que nos olvidemos de una cosa muy importante: **¿En qué momento se da rienda suelta a la improvisación y la innovación?**

Yo no soy un ScrumMaster of Puppets ni nada por el estilo, pero después de haber visto tres empresas que trataban de implementarlo sin éxito, me da la impresión de que** Scrum es a la Agilidad lo que las ruedecitas de la bicicleta al ciclismo**. Es algo por lo que tienes que pasar al empezar para aspirar a cotas de éxito mayores. Seguramente es suficiente en muchas empresas, pero no me conformo solo con eso. Quiero llegar a desarrollar lo que he visto aquí.

Los líderes de la compañía, decía, son los que deben dar el paso adelante y evangelizar a sus colaboradores. En Finn lo entendieron así y por eso crearon un departamento que hace seguimiento y formación a todos los líderes de equipo. Cada vez que un equipo nuevo empieza a trabajar, deben contestar a las siguientes preguntas:
	
  * ¿Quiénes son tus clientes/usuarios?
  * ¿Cómo vas a aportarles valor?
  * ¿Cómo vas a medir tu éxito aportando valor?

Responder estas tres preguntas ayuda al equipo a alinearse, enfocarse y tener claro qué es lo que tienen que hacer. Pero sobre todo, lo que no. Porque otro de los aspectos más importantes del pensamiento Lean es:

> Be sure to do the right things, and the things right

Pero claro, siempre está el eterno dilema:
	
  * ¿Cómo nos aseguramos de que entregamos el máximo valor posible a nuestros usuarios?	
  * ¿Cómo nos aseguramos de que el producto que desarrollamos mantiene la calidad óptima?
  * ¿Cómo nos aseguramos de que ninguna de las dos anteriores pesa más que la otra para no desequilibrar la balanza y entregar mucho valor pero con poca calidad, o viceversa? ¿Y todo a una velocidad adecuada que no sobrecargue a las personas del equipo?

Encontrar un equilibrio siempre es lo más complicado. En el caso de equipos solo con un responsable de producto, la tendencia será a dar respuesta a la primera pregunta, sacrificando las otras dos.

En un modelo con un responsable de producto y otro de tecnología para dar respuesta a la primera y segunda cuestión, habrá un equilibrio hipotético entre ambas, pero esto solo ocurrirá si ninguno tiene más poder de convicción que el otro. Esta situación se da muy raramente, lo normal es que una de las dos partes termine cediendo frente a la otra para evitar el conflicto. Además, es posible que dejemos desatendida la tercera.

Por eso en Finn han empezado a trabajar en equipos con un modelo de **Tripod**, o 3D. Existen tres personas responsables: product owner, lead developer y team leader. Cada uno es responsable del aporte de valor, calidad del código y velocidad del flujo de trabajo, respectivamente. Los tres son evaluados por diferentes criterios, a saber:
	
  * El **product owner** es responsable de la evolución de los KPIs definidos al decidir quiénes son los clientes del equipo y qué les aporta valor.
  * El **team leader** es responsable del _flow _del equipo: el tiempo medio de realización de una historia de usuario desde que entra en el backlog hasta que sube a producción.

El **lead developer** es responsable del número de tareas de mantenimiento que realiza el equipo, procurando siempre que sea el **mínimo posible**. Esto hay que matizarlo: si se realizan muchas tareas para solucionar bugs o refactorizar el código, es que no se está construyendo el producto con calidad (**build quality in**). Por eso debe ser el mínimo posible. Lo cual no quiere decir que no haya que hacer nada, pero sí el mínimo imprescindible.

Una vez definidos los roles y la responsabilidad de cada uno, la forma de priorizar es la siguiente:

[![Captura de pantalla 2015-02-27 a las 17.01.08](https://thecraftsmansjourney.files.wordpress.com/2015/02/captura-de-pantalla-2015-02-27-a-las-17-01-08.png)](https://thecraftsmansjourney.files.wordpress.com/2015/02/captura-de-pantalla-2015-02-27-a-las-17-01-08.png)
	
  1. **Stabilization: **Se priorizan los bugs y la estabilidad de la aplicación por encima de todo. La responsabilidad del lead developer es entregar con la mejor calidad posible. Un buen KPI sería el tiempo empleado en la estabilización.
  2. **Completion: **Se da salida al trabajo definido en el backlog y se eliminan todos los impedimentos que hagan fluir ese trabajo. Se da prioridad a completar el trabajo ya empezado, MVPs, funcionalidad, etc. Siempre teniendo muy claro la definición de "completado". Un buen KPI sería la media de días empleados para completar las tareas desde que se empiezan hasta que suben a producción.
  3. **New Initiatives: **No se comienzan cosas nuevas si no se han completado los _stages_ de prioridad 1 y 2. La responsabilidad de priorizar el trabajo que entra en el backlog es del product owner. Priorizará tanto funcionalidad como tareas de mantenimiento de la plataforma técnica. Los KPIs empleados podrían ser: tiempo que el usuario emplea en encontrar un anuncio, veces que vuelve al site a la semana, etc.

Todos los responsables tienen claro cuál es su trabajo y por qué es importante el trabajo de los demás. Sobre el trabajo de product owner, me gustaría destacar que las veces que he hablado con Arnstein el PO del Team SOA, su obsesión no es añadir nueva funcionalidad, sino **eliminar todo lo que sea prescindible**. Su mentalidad es totalmente _mobile first_, por lo tanto es consciente de lo importante que es el rendimiento en una aplicación web, el hecho de darle al usuario lo que está buscando lo antes posible. No hay más que ver cómo funciona [mFinn](http://m.finn.no/frontpage.html): como un tiro.

## Conclusiones

Me gusta mucho que Finn tenga un departamento dedicado a formar a sus líderes de equipo y managers para que la cultura de la compañía cale en todos sus estratos, así como para ir evolucionando y eliminando lo que no funciona por otras estrategias que mejoran lo existente.

Sobre el modelo Tripod, no estoy seguro de si debería haber tres responsables en cada equipo, pero sí debería estar claro que hay tres ejes que se deben mantener y compensar mútuamente. Me gusta el respeto mútuo por la responsabilidad del otro y sobre todo que queda claro qué es importante, a qué se le da prioridad y cómo se va a medir cada acción.

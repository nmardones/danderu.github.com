---
author: dandel
comments: true
date: 2015-02-18 16:13:03+00:00
layout: post
slug: solving-bugs
title: Solving bugs
wordpress_id: 45
categories:
- Journey
tags:
- bug fixing
- continuous delivery
- Finn
---

Cuando he llegado esta mañana a la oficina, Marthe y Rida estaban solucionando un par de bugs en producción. Como me parecía interesante y acababan de empezar, me he unido a ellos para empaparme del proceso y ha valido mucho la pena la experiencia.

El primero de los bugs se ha detectado en el entorno de CI (Continuous Integration). Este es un entorno donde se ejecutan los tests de integración del "**mFinn**" (la versión mobile de Finn) con el resto de servicios de la casa. Sobre todo APIs y demás. Todo el proceso está automatizado con un software interno llamado **Pipeline**, que no te permite pasar a producción si no pasan todos los tests (green). Utilizar esta herramienta implica dos cosas:



	
  1. Los equipos son capaces de subir a producción cuando quieren siempre y cuando tengan un verde en su batería de tests.

	
  2. Si hay tests que no pasan, es el propio equipo el que se mueve lo antes posible para solucionarlos, porque son los primeros interesados en poder hacer release lo antes posible. Depende de ellos solucionarlo. Si falla una integración con una API de otro equipo, implican a gente de ese equipo.


He preparado una reunión con los autores de Pipeline para que me expliquen con más detalle como funciona, así que hoy he podido centrarme en el proceso de resolución de bugs trabajando con Rida.

<!-- more -->


## Primera lección: una batería de tests automáticos debe proporcionar feedback lo antes posible


El primero de ellos no hemos conseguido reproducirlo en local, así que se nos ha ocurrido que quizás era inestable. Tienen un ranking de tests poco fiables con un historial de las veces que han fallado y nuestro test aparecía en el top 10. Hemos vuelto a lanzar la batería de tests y, efectivamente, ha dado verde. La hemos lanzado por segunda vez para asegurarnos y ha vuelto a dar verde. Así que, en efecto, había sido un error puntual.

Lo que he aprendido de este primer bug es **lo importante que es que la ejecución de la batería de tests sea rápida**. Una ejecución del conjunto de tests de integración de todo el site mFinn tarda 2 minutos de media. Esto te permite hacer un cambio, lanzar una ejecución de tests y esperar menos de 2 minutos para recibir feedback. Los tests están escritos en **Groovy** y pueden simular, por ejemplo, los resultados de las búsqueda para obtener tests deterministas y una velocidad óptima.

Ni me imagino lo que habríamos tardado en solucionar este error fantasma con una batería de tests que tardase más de una hora en ejecutarse...


## Segunda lección: Fix the problem, not the blame


El segundo bug era más jodido, porque realmente había cazado un error y lo habíamos provocado nosotros con la release de ayer por la tarde, lo que significa que ya estaba en producción. En ningún momento ha habido nervios, voces ni acusaciones. El equipo de la API que provocaba el problema se ha puesto ha solucionarlo de inmediato y Marthe ha enviado un mail explicando por qué era importante solucionar ese bug y sugiriendo varias acciones para que no volviera a pasar.

Esta actitud forma parte de la cultura de Finn. Según me ha comentado Rida, todo el mundo entiende que alguien se puede equivocar. "**Shit happens**". No hay que buscar culpables, sino poner medios para que las cosas no vuelvan a pasar.


## Tercera lección: Comunicación directa con los clientes


Después de nuestro daily, Marthe, Rida y yo hemos bajado al tercer piso para tener un stand-up con el equipo de **customer support**. Este stand-up es importante para comunicar a la gente del equipo qué cambios estamos subiendo y también para que nos informen de problemas o quejas detectadas por los clientes. Normalmente solo bajan Team Leaders, pero como hoy teníamos que informar del estado de la resolución de nuestros bugs, hemos bajado los responsables para explicar cómo lo estábamos solucionando y cuándo teníamos previsto subirlo.

Precisamente hoy nos han dicho que había un problema que desconocíamos, con un mensaje que no aparecía en los detalles de anuncios de Real Estate. Había varios clientes nerviosos, así que en cuanto hemos subido Rida y yo nos hemos puesto a solucionarlo.

Me ha gustado esta forma de bajar a las trincheras a informar y ser informados. No ha habido cruce de emails con copia a todo el mundo ni nadie se ha puesto nervioso. Además, el departamento de customer support es uno de los más Lean de la compañía, según me ha explicado Marthe. Seguro que eso se transmite en todos los niveles operativos.


## Cuarta lección: resolviendo mi primer bug


Para el nuevo bug reportado por customer support, Rida y yo nos hemos encerrado en una sala especial que está habilitada para hacer **pair programming**. La sala tiene un hub usb con dos buenos monitores, dos teclados y dos ratones, por lo que ambos programadores pueden trabajar al mismo tiempo sobre el código. También está aislada, para que nadie te moleste.

Trabajan con git, así que para resolver el bug, hemos creado una branch de hotfix sobre master y nos hemos puesto a inspeccionar el módulo que estaba dando problemas. Supongo que por la suerte del principiante, he encontrado el problema enseguida: un statement mal anidado. Mi faena ha sido explicárselo a Rida, ya que él estaba convencido de que el problema estaba en el backend. Aún así, como podíamos tocar ambos el código, he tocado un par de cosillas para demostrárselo antes de que se empezase a mirar otra cosa. Su cara de felicidad al ver que tenía razón ha sido para hacerle una foto. Me ha dicho que le he ahorrado una hora de trabajo mirando done no era, así que me he sentido muy satisfecho de haber podido ayudar ^_^. Es la magia del **pair programming**.

Cuando hemos terminado, hemos creado un **pull request** para que el resto del equipo revisase el cambio antes de hacer merge con master. Antes de mediodía estaba solucionado en producción algo detectado a media mañana.

[![pair programming room](https://thecraftsmansjourney.files.wordpress.com/2015/02/pair-programming-room1.jpg)](https://thecraftsmansjourney.files.wordpress.com/2015/02/pair-programming-room1.jpg)

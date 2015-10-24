---
author: dandel
comments: true
date: 2015-10-22 06:37:11+00:00
layout: post
slug: por-que-no-puedes-librarte-del-legacy-code-y-por-que-no-solo-es-culpa-tuya
title: Por qué no puedes librarte del legacy code y por qué no (solo) es culpa tuya
wordpress_id: 187
categories:
- Journey
---

[![Death March](https://thecraftsmansjourney.files.wordpress.com/2015/10/ww2-131.jpg)](https://thecraftsmansjourney.files.wordpress.com/2015/10/ww2-131.jpg)

Si estás leyendo esto, probablemente sufres en tus carnes cada día la problemática de trabajar con código _legacy_, o código legado. Y digo que la sufres, porque eres consciente del problema, de que tiene solución y no estás cómodo con el status quo actual. Sabes que te deberías librar de ese problema pero, ¿por qué no lo haces? Déjame intentar adivinarlo: porque no puedes.

<!-- more -->

El problema con el _legacy code _está en el nombre. Incluso en la definición de Michael Feathers, que seguro que habrás leído millones de veces:


<blockquote>Legacy code is code without tests</blockquote>


Esta sentencia, tan breve como lapidaria, si se utiliza fuera de contexto puede ser aún más peligrosa que el código al que te enfrentas cada día. La frase tiene sentido en el momento en que te lees su maravilloso libro: **[Working Effectively with Legacy Code](http://www.amazon.com/Working-Effectively-Legacy-Robert-Martin-ebook/dp/B005OYHF0A/ref=mt_kindle?_encoding=UTF8&me=)** (el cual te recomiendo, si no lo has leído ya). Pero no se puede ser tan cándido como para deducir a partir de ella que la solución al problema de trabajar con legacy code es cubrirlo de tests.

Es fácil llegar a esta conclusión, cuando empiezas a buscar soluciones y lees a gente como Feathers, o Uncle Bob. Este último, al que respeto enormemente, se quedó bien a gusto cuando enunció las tres reglas del TDD:


<blockquote>

> 
> 
	
>   1. You are not allowed to write any production code unless it is to make a failing unit test pass.
> 
	
>   2. You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
> 
	
>   3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.
> 

</blockquote>


Y con esto, dio por terminado el día. Se grabó en video, lo colgó en su web y se fue a leer en su sofá en bata esperando a ver cómo su cuenta corriente estaba rebosante por la mañana gracias a los pagos de sus subscriptores. Una vez más, las tres reglas del TDD no son leyes universales que se puedan aplicar en cualquier contexto. Son reglas que funcionan genial cuando estás construyendo una aplicación o funcionalidad nueva ya que el desarrollo orientado a tests te ayuda a resolver problemas de forma metódica y a documentar tu código. Pero si pretendes resolver un problema de una aplicación legacy cubriéndola de tests, cuando termines, probablemente tengas dos problemas.


## La cobertura de test no soluciona un problema de arquitectura de software


Llevo algo más de un año aplicando TDD a mis desarrollos sobre una plataforma que tiene más de nueve años y sobre otra de casi cinco años. Ambas tienen problemas, sobre todo de acotación de contextos, no hay _single responsibility_, no hay interfaces, acoplamiento entre lógica de negocio y presentación, código spaghetti... todo lo que te puedas imaginar y más. Seguro que te suena. En un escenario así, para cubrir de test tienes dos opciones:



	
  1. Refactorizar a lo bestia código que no es tuyo, que no está documentado y que no está cubierto de test, incumpliendo las tres reglas de Uncle Bob.

	
  2. Cubrir de tests de integración la funcionalidad básica de tu aplicación, para garantizar que, en caso de romper algo refactorizando, no sea algo demasiado importante y el core de tu negocio permanezca intacto.


La opción (1) implica una inversión de tiempo de investigación y definición de arquitectura importante, porque es difícil cubrir de tests unitarios un código que no ha sido diseñado para ser testado. Difícil, pero no imposible. Aunque con ciertos límites. Si un método tiene 100 líneas de código, puedes llegar a asumir un refactoring que te puede llevar un par de días. Pero si estamos hablando de clases de 10.000 líneas de código con reglas de negocio de hace más de cinco años que no están documentadas en ninguna parte... el tiempo necesario para refactorizar eso es inasumible y vale más la pena tirarlo y hacerlo de cero.

En nuestro caso, optamos por la opción (2). Invertimos varias semanas en crear tests de integración, después de discutir y negociar mucho con la gente de producto para que entendieran lo importante que era hacer algo que hasta ahora no habíamos necesitado. Conseguimos vendérselo explicando que todo el test manual que hacían antes de subir a producción iban a dejar de hacerlo tras tener todo bien cubierto de test. También que íbamos a generar menos incidencias.

Ninguna se cumplió, desde luego. En la práctica, dilatamos el tiempo necesario para el desarrollo, ya que no permitíamos liberar una funcionalidad en la rama principal si no pasaba todos los tests de integración. Pero este tipo de test, por definición, son inestables y lentos, por lo que nuestra batería tardaba alrededor de una hora en darnos feedback. Y normalmente daba errores, ya que los datos de prueba no eran deterministas.

Después del tiempo que ganamos para crear toda esa batería de test, era muy difícil explicar que no solo no nos servía, sino que nos iba a hacer ser más lentos entregando valor. Así que el refactoring que teníamos pensado hacer tras la primera fase, nunca llegó.

Tras leer mi historia, ¿cual crees que fue el problema?

Tal vez pensarás que deberíamos haber optado por la opción (1), aunque nos hubiera llevado más tiempo. Pero, ¿y si te digo que era la opción que a todos nos hubiera gustado tomar? Estábamos convencidos de que era la única que solucionaría el problema de raíz, junto con una buena redefinición de arquitectura. Entonces, ¿por qué no lo hicimos? La respuesta, tras varios meses de aquello, es dolorosa:

**El problema es que estábamos solos intentando solucionando un problema que es de toda la compañía.**


## No se puede cambiar el motor de un Ferrari mientras corre a 200Km/h


Decía un párrafo que el problema con el _legacy code_es el nombre. Yo eliminaría la palabra _code_ de la definición y lo dejaría en _legacy. _Porque si le empiezas a explicar a tu product manager o al CEO de la compañía que tienes problemas con el legacy **code**, mientras estés pronunciando la siguiente frase estará pensando:


<blockquote>¿Tiene problemas con el código? Pues que lo arregle, ¿a mí que me cuenta?</blockquote>


Nuestros problemas no vienen por el código legado. Vienen por las reglas de negocio del producto legado. Si fuera tan famoso como Bob Martin y toda esta gente que tanto me ha inspirado, empezaría a acuñar el término **legacy product **y evitaría mencionar la palabra "código" cada vez que intentase hablarle a alguien del problema.

El **legacy product no te permite ser ágil**. El tiempo que inviertes en tareas que no están directamente relacionadas con el desarrollo puro de tu producto te hace perder ventaja competitiva con tus rivales del mercado. Mientras tú inviertes tiempo solucionando bugs, deployando en producción, levantando un entorno en local o tratando de entender reglas de negocio no documentadas; la startup de turno te está adelantando por la derecha con su plataforma recién salida de fábrica.

El **legacy product** te ata a tus clientes actuales. Me he encontrado casos de product managers que prefieren no tocar nada del producto actual por miedo a que a los clientes que ya están pagando por lo que hay no les guste y

El **legacy product** no te permite tomar datos. Si lo hicieras, sabrías qué funcionalidades de tu aplicación se usan y qué otras no. Dónde pierdes clientes. Qué flujos de tu aplicación no están bien pensados y hay que redefinir. Si el coste de implementación de una plataforma de medición e instrumentacación de tu producto es tan elevado que no puedes hacerlo sin miedo a romper nada, necesitas otro producto. Si no puedes probar fácilmente un cambio en la experiencia de usuario o en el diseño con un conjunto reducido de tus usuarios sin tener que hacer un pase a producción o sin la intervención de un desarrollador, necesitas otro producto.

El mantenimiento de tu **legacy product **te está haciendo perder dinero. Si un ingeniero de front end no puede desarrollar sin la ayuda de un back end, estás invirtiendo más recursos de los que necesitas. ¿Cuánto tiempo dedican tus desarrolladores a tareas no directamente relacionadas con la funcionalidad que se quiere poner en producción? Tradúcelo en horas. Luego, en dinero.

Todos estos problemas, no son problemas del código. Son problemas del producto y por lo tanto, de la empresa. No se puede tratar de resolver un problema del producto desde el departamento de IT mientras el departamento de producto presiona para sacar su funcionalidad nueva antes del próximo Q, el de marketing quiere su rediseño de cara a la próxima campaña de publicidad en medios y el CEO llega un día con una integración de un cliente importantísimo que se tiene que hacer para ayer.

**No se le puede dar la espalda a un problema que te impide ser competitivo**.

Y si se hace, al menos estaría genial no poner palos en la rueda del que intenta salvarte el culo.


## Conclusión


De verdad, me encantaría acuñar el término **legacy product**. Creo que libros como Clean Code deberían leérselo todas las personas de la compañía, no solo los devs que lloran sangre cada día mientras les piden explicaciones sobre por qué les cuesta tanto cambiar una plantilla de email y ponerla en producción. O sobre por qué después de cada release se tienen que invertir cuatro días solucionando bugs.

Tú, como ingeniero/a de software, tienes de sobras el conocimiento necesario para diseñar una arquitectura sólida, escalable y mantenible. Bien documentada y cubierta de tests. Ya te has leído todos esos libros y practicado tus katas. Pero si no lo haces, no es porque no quieras. Tienes que encontrar la forma de hacer entender a toda la gente que te rodea que **la empresa tiene un problema **y que es responsabilidad de todos encontrar la manera de solucionarlo.

A lo mejor, lo más sensato es dejar al Ferrari corriendo a 200Km/h mientras construyes uno nuevo que lo sustituya en un futuro. Si decidís no hacerlo, no importa. Ya lo está haciendo vuestra competencia.

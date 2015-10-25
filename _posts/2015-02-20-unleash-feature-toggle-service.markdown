---
author: dandel
comments: true
date: 2015-02-20T10:19:22.000Z
layout: post
slug: "unleash-feature-toggle-service"
title: "Unleash: a feature toggle service"
wordpress_id: 76
categories: 
  - Journey
published: true
excerpt: "Ayer descubrí otra herramienta estupenda que utilizan aquí:\_Unleash. Se trata de un servicio que permite activar o desactivar funcionalidades en producción mediante una página de administración"
---


Ayer descubrí otra herramienta estupenda que utilizan aquí: **[Unleash](https://github.com/finn-no/unleash)**. Se trata de un servicio que permite activar o desactivar funcionalidades en producción mediante una página de administración. Ha sido desarrollada internamente, pero el código es open source, así que la puede utilizar cualquiera. También tienen liberado un [cliente para Java](https://github.com/finn-no/unleash-client-java).

El funcionamiento de Unleash es el siguiente: se hace el desarrollo en el código rodeándolo de un condicional, que hará que solo se ejecute en caso de que la regla en Unleash esté activa. En caso contrario, se ejecutará el viejo.[![Captura de pantalla 2015-02-20 a las 11.14.54](https://thecraftsmansjourney.files.wordpress.com/2015/02/captura-de-pantalla-2015-02-20-a-las-11-14-54.png)](https://thecraftsmansjourney.files.wordpress.com/2015/02/captura-de-pantalla-2015-02-20-a-las-11-14-54.png)

Hasta aquí, nada fuera del otro mundo. La potencia está en el código que se instala en un servidor, capaz de gestionar todas las reglas con una interfaz amigable y de establecer diferentes estrategias de activación. Por ejemplo, puedes activar un experimento para un conjunto de usuarios concreto, solo para ciertos anuncios, etc. Si quieres liberar una feature (o eliminarla) y quieres ver cómo reaccionan los usuarios, puedes enviarles un opt-in, activar la regla en Unleash solo para sus e-mails y recoger su feedback. Fácil.

Esta herramienta combinada con la capacidad de subir código a producción en cualquier momento de forma segura, les da una agilidad para experimentar brutal. Y sin las limitaciones que te dan otras soluciones (como Optimizely), ya que el experimento se puede activar en cualquier capa de la aplicación.

Awesome!

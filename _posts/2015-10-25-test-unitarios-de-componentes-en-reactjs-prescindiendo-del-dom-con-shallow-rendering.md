---
published: true
---



En un [artículo](https://thecraftsmansjourney.wordpress.com/2015/06/18/configurando-un-entorno-en-webpack-para-trabajar-con-react/) publicado previamente hablé sobre el entorno que hemos preparado para desarrollar componentes atómicos con **ReactJS** y de todas las micro-decisiones que hemos tomado sobre el mismo. Con lo que respecta a los tests unitarios, hasta ahora hemos utilizado [Karma](http://karma-runner.github.io/0.12/index.html) debido a su compatibilidad con [webpack](http://webpack.github.io) y su facilidad de configuración, pero este mundo evoluciona muy rápido y ya hemos encontrado una opción mejor: implementar [**shallow rendering**](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering). Esta opción además, es la [recomendada por Facebook](https://discuss.reactjs.org/t/whats-the-prefered-way-to-test-react-js-components/26/2) a partir de la versión 0.13, así que no se puede estar más a la última.

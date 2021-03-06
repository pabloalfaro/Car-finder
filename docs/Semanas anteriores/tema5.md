## Integración continua

Previamente había implementado una serie de test para el código que había programado. Existen herramientas que automatizan la ejecución de los test. Estas herramientas hacen que  conforme se avance con el proyecto se compruebe que las funcionalidades previas no tienen errores además de ir comprobrando las nuevas con nuevos tests. [Travis-CI](https://travis-ci.com/) es un sistema de integración continua con un uso muy extendido en GitHub. Tiene una integración directa con GitHub y permite testear y construir apliaciones en una extensa lista de lenguajes de programación.

En primer lugar me he registrado en Travis con mi cuenta de GitHub. Ya registrado he podido elegir en que repos quería activar la integración continua. El siguiente paso es añadir un fichero [.travis.yml](https://github.com/pabloalfaro/Car-finder/blob/main/.travis.yml). He preparado una primera configuración que lanze los tests usando mi fichero Makefile.

~~~
language: go

go:
- 1.15

script: 
- make test
~~~

Tras hacer la prueba y ver que ejecuta los test correctamente, decidí implementar la ejecución de los tests usando el docker que había configurado previamente. Para estas pruebas no especifico el lenguaje ni la versión porque los test se ejecutan sobre el Docker. En este caso el fichero de configuración de Travis se quedó de la siguiente forma:

~~~
services:
  - docker

install:
  - docker pull pabloalfaro/car-finder

script:
  - docker run -t -v $TRAVIS_BUILD_DIR:/app/test pabloalfaro/car-finder
~~~


Con Travis ya configurado he probado otro sistema. El que he elegido ha sido [Shippable](https://app.shippable.com/). La implementación de este sistema es muy similar a Travis, hay que registrarse con una cuenta de GitHub y elegir los repos sobre los que quieres ejecutar la integración continua. A diferencia de Travis, Shippable si que deja que te registres con otros sistemas como Bitbucket o GitLab. Después de configurar lo anterior, he añadido un fichero [shippable.yml](https://github.com/pabloalfaro/Car-finder/blob/main/shippable.yml). En este caso, la configuración que he elegido ha sido la de mi primera opcion para Travis, ejecutando los test con la orden de mi fichero Makefile. A diferencia de la anterior, esta vez compruebo más versiones de mi lenguaje.

~~~
language: go

go:
- 1.15
- 1.14
- 1.13
- 1.12
- 1.11
- 1.10

script: 
- make test
~~~

Después de ejecutar los test he podido comprobar que los tests fallan a partir de la versión 1.12 por lo que he actualizado el fichero para que las pruebas se hagan hasta la versión 1.13.

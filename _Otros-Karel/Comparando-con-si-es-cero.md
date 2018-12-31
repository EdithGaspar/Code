---
layout: G-Article
title: Comparando con si-es-cero
author: rivel_co
tags: [Recursión, Variables, Parámetros]
Hide_Tags: false
olimpiada: [OMIPS]
subject: [Karel pascal]
---

Un [criterio o condición]({{ site.baseurl }}/Karel/Instrucciones/Condicionales/ "Instrucciones condicionales en Karel &vert; #iP Code") que podemos meter dentro de un `si` es `si-es-cero()`. Esta es una función que se utiliza en problemas más avanzados pero muy útil. Como su nombre lo indica, la función es *verdadera* si el parámetro que está dentro de sus paréntesis es $$0$$ y es *falsa* si no es así.

{: #ListaContenido}
- Comprobando su función
- Buscando un nueve
- Números negativos
- Reto adicional

## Comprobando su función

Podemos elaborar un código muy sencillo para que compruebes cómo funciona esta función. A continuación un código donde si el valor del parámetro es $$0$$ Karel dejará 2 zumbadores y si no es $$0$$ dejará 4.

<textarea class="karelp">
iniciar-programa
    define-nueva-instruccion prueba(x) como
    inicio
        si si-es-cero(x) entonces inicio
            repetir 2 veces deja-zumbador;
        fin sino inicio
            repetir 4 veces deja-zumbador;
        fin;
    fin;

    inicia-ejecucion
        prueba(3);
        apagate;
    termina-ejecucion
finalizar-programa</textarea>

El resultado del programa anterior es dejar 4 zumbadores, pues le hemos dado un valor inicial de $$3$$ en la línea 12. Verás que si cambias ese `prueba(3)` por un `prueba(0)` Karel dejará sólo 2 zumbadores. Como notarás `si-es-cero()` necesita estar *dentro* de una instrucción que tome valores booleanos, por eso hay que poner el `si si-es-cero(x) ...`. <span>Suena raro, lo sé, pero tiene sentido</span>.

## Buscando un nueve

Hay muchos problemas que requieren que Karel identifique un numero determinado de zumbadores y haga algo al respecto. Por ejemplo, encontrar el montón de 9 zumbadores en un mapa lleno de zumbadores y que se apague ahí. Claro que ese nueve puede variar pero *esa es la idea*. Lo que vamos a hacer aquí es decir si un montón de zumbadores es igual a un número determinado.

Como primer paso, es importante que sepas que vamos a usar recursión y recursión con parámetros <s>claro está</s>. Lo que haremos es ubicarnos sobre el montón de zumbadores que está frente a Karel y comenzar a quitar zumbadores, uno por uno. Cada que retiremos un zumbador vamos a restarle uno a nuestro parámetro, de esa forma, si quitamos $$6$$ zumbadores a nuestro parámetro le quitaremos $$6$$ también.

<span>¿Y eso para qué?</span> Pues recordando el problema de buscar el nueve, si iniciamos nuestro parámetro como un $$9$$ y retiramos $$9$$ zumbadores (y le restamos la misma cantidad al parámetro) entonces al final el valor del parámetro será $$0$$. Lo que faltará es saber si el parámetro es en efecto $$0$$ utilizando un `si-es-cero()`. Si esa *condición* se cumple, entonces en efecto habremos encontrado el nueve. Para fines gráficos diremos que si el montón tiene los nueve zumbadores que Karel se oriente al este, sino que se oriente al oeste.

<textarea class="karelp">
iniciar-programa
    define-nueva-instruccion nueveZumba(x) como
    inicio
        si junto-a-zumbador entonces inicio
            coge-zumbador;
            nueveZumba(precede(x));
        fin sino inicio
            si si-es-cero(x) entonces inicio
                mientras no-orientado-al-este hacer gira-izquierda;
            fin sino inicio
                mientras no-orientado-al-oeste hacer gira-izquierda;
            fin;
        fin;
    fin;

    inicia-ejecucion
        avanza;
        nueveZumba(9);
        apagate;
    termina-ejecucion
finalizar-programa</textarea>

Entonces, con la línea 17 nos ubicamos en el montón que está al frente, comenzamos la recursión llamando a la función en la línea 18 dándole un valor inicial al parámetro de $$9$$ pues estamos buscando los nueve zumbadores. La condición de la recursión es que tomaremos tantos zumbadores como podamos (`si junto-a-zumbador ...`) en la línea 4. Cada que quitamos un zumbador hacemos la llamada recursiva (línea 6) restando $$1$$ al parámetro (usando el `precede()`).

Cuando terminamos de tomar los zumbadores nos vamos a la línea 8 donde vamos a ver si el valor del parámetro es $$0$$ utilizando `si-es-cero()`. De ser así entonces ubicamos a Karel al este (línea 9) sino al oeste (línea 11). 

## Números negativos

Es importante plantearse qué sucede cuando quito tantos zumbadores que mi parámetro se vuelve negativo. Lo que sucede es justamente eso, el parámetro se vuelve negativo. Es decir, si en el código anterior ponemos como mundo de prueba un montón con $$20$$ zumbadores Karel empezará a restarle al $$9$$ con el que empezó y se irá con números negativos. Sin embargo no hay de qué asustarnos, los números negativos **no son iguales a** $$0$$ y eso lo sabe `si-es-cero()`. 

En *[Karel Azul](http://www.cmirg.com/karelotitlan/Pantallas/descargas.aspx "Karelotitlán")* el simulador hacía algo curioso con cuando manejabas números negativos, sin embargo esto está corregido en *[Karel.js](https://omegaup.com/karel.js/ "Karel.js")* que es la versión que actualmente se usa y con la que deberías estar trabajando.

## Reto adicional

Con la solución aquí propuesta es necesario quitar todos los zumbadores del sitio para entonces comparar el parámetro pero, imagina un nuevo problema donde el montón a evaluar puede tener una cantidad infinita de zumbadores <span>¿cómo resolverías el problema?</span> Recuerda que Karel nunca terminaría de quitar una cantidad infinita.
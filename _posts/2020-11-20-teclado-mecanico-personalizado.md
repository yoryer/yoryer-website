---
layout: post
title:  "Teclado mecánico personalizado"
date:   2020-11-20
categories: mechanical-keyboards keyboards keebs
author: Jorge Noguera
---
Hace unos pocos años descubrí un mundo totalmente nuevo y muy emocionante para mí, los *teclados mecánicos*. Siempre me gustó crear cosas con mis manos y por eso le tomé mucho cariño a este **caro y poco común** hobbie.

<!--more-->

Hoy día es más común escuchar sobre teclados mecánicos, en el mundo de los videojuegos se volvieron completamente virales desde que varios streamers y jugadores profesionales de videojuegos comenzaran a mencionar que utilizan este tipo de teclados por la fiabilidad que estos tienen a la hora de presionar las teclas.

Anteriormente era un hobbie poco difundido, eran como grupos de élite compuesto por personas que entendían tanto de electrónica como de programación para convertir las ideas más increíbles en un teclado de computadora.

Si bien hay mucha información en Internet sobre este tema, este post es más de apreciación a algo que hice hace ya un tiempo atrás: **construir complemtamente un teclado desde cero**.

Cuando digo desde cero me refiero a desde cero, sin nada en las manos y con una idea en la cabeza.

# 1. La idea

En mi cabeza estaba hacer algo que cumpliera con algunos deseos que tenía luego de haber estado un par de años utilizando principalmente dos teclados; el [CODE Keyboard][code-keyboard-site]{:target="_blank"} y un [Ergodox][ergodox-site]{:target="_blank"}.

En mi cabeza el teclado tenía que cumplir con los siguientes puntos:

1. Ergonómico y ortolineal, igual al Ergodox.
2. Separado en dos secciones especificas para cada mano pero construido en una sola pieza.
3. Distancia justa entre ambos sectores para cada mano.
4. Teclas de flechas direccionales en formato T invertida.
5. Teclas específicas para saltos de página y ir al inicio y final de cada línea.
6. Utilizar el formato de keycaps de un teclado estándar.

Esas fueron las bases del concepto que venía planeando, al final el resultado final del diseño fue el siguiente:

<center><img src="{{ "/media/art002-keeb-00.png" | relative_url }}" /></center>
<br/>

Con esto en mi cabeza, el siguiente paso fue conseguir las piezas para comenzar a armar el teclado.

*Si querés hacer tu propia versión, acá esta el enlace al sitio [Keyboard Layout Editor][keeb-layout]{:target="_blank"} donde configuré este diseño.*

# 2. Las partes

En definitiva no existía ninguna placa de teclado con nada similar a lo que yo quería, por lo que no existía la posibilidad de utilizar un PCB ya que tampoco sabía como diseñar uno. La solución a esto fue **cableado a mano**.

Lo que iba a necesitar para ensamblar el teclado además de lo común como cables y un cautín era lo siguiente:

1. **Microcontrolador:** Arduino Pro Micro.
2. **Switches:** Gateron Yellow.
3. **Diodos:** 1N4148.
4. **Lubricante:** Chrysto Lube MC 129.
5. **Estabilizadores:** Tipo Cherry para plates.
6. **Cables:** Grosor 22 AWG.
7. **Cortes de MDF:** Placas realizadas en corte láser para ensamblado tipo sandwich.
8. **Keyset:** Chocolate Keycaps de perfil SA.
9. **Silicona:** para sellar los switches por el plate.
10. **Cautín, estaño y un multimetro.**.

<center><img src="{{ "/media/art002-keeb-01.jpg" | relative_url }}" /></center>
<br/>

# 3. Ensamblado

Como primer comencé por lubricar los switches, desarmé cada uno de ellos, los lubriqué y los volví a ensamblar más de 70 swiches necesarios para este teclado.

Luego la siguiente tarea fue presentar los switches en el plate MDF y luego asegurarlos por el plate utilizando silicona, de esta forma se aseguran los switches por el plate y eso permite reducir el nivel de flexibilidad a la hora de escribir y también que sea más fácil intercambiar los keycaps.

Una vez que la silicona secó y los switches estaban lo suficientemente sujetos procedí a soldar los diodos por cada switch.

<center><img src="{{ "/media/art002-keeb-03.jpg" | relative_url }}" /></center>
<br/>

Una vez puestos los diodos comenzó el paso del cableado a mano que consiste básicamente en lo siguiente:

1. Se debe solar un cable por cada diodo para formar las filas (cable rojo).
2. Se debe soldar un cable por cada switch formando las columnas (cable azul).
3. Se debe soldar cada fila completa al microcontrolador (cable amarillo).
4. Se debe soldar cada columna al microcontrolador (cable verde).

Con paciencia se puede lograr realizar correctamente cada soldadura sin que haya ningún toque entre los cables.

<center><img src="{{ "/media/art002-keeb-04.jpg" | relative_url }}" /></center>

Lo siguiente fue ir al sitio [Keyboard Firmware Builder][keyboard-firmware-builder]{:target="_blank"}, copiar y pegar el diseño (RAW Data) desde la web de Keyboard Layout Editor y empezar a modificar la disposición de pines para que pudiese funcionar con la cantidad de pines que tiene el Arduino Pro Micro.

Una vez establecida la disposición de pines y configuradas las capas del teclado, exportamos la configuración a un archivo *.hex* y luego flasheamos nuestro Arduino y **LISTO!**

# 4. Resultado final

<center><img src="{{ "/media/art002-keeb-05.jpg" | relative_url }}" /></center>

Es un teclado que sin dudas reúne varias de las características que busco, sin dudas voy a realizar una siguiente versión y quizá con mejor detalle en el proceso de construcción.

A medida que vaya recordando cosas particulares de la construcción del teclado voy a ir actualizando el post.

# 5. Typing test

La parte más linda de construir algo para vos es cuando podés empezar a usar lo que construiste.

Siempre estoy abierto a dar recomendaciones sobre teclados mecánicos si alguien está interesado, más abajo están los links de contacto.

Dejo un video de cómo se ve y como suena el teclado al escribir :)

<center><iframe width="560" height="315" src="https://www.youtube.com/embed/SxFczmAr2y8" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

[code-keyboard-site]: https://codekeyboards.com
[ergodox-site]: https://www.ergodox.io
[keyboard-firmware-builder]: https://kbfirmware.com
[keeb-layout]: http://www.keyboard-layout-editor.com/##@_name=yoryerkeeb%20v2&author=Jorge%20Noguera&switchMount=cherry&switchBrand=kailh&switchType=PG151101D49%2F%2FD09&plate:true%3B&@_y:0.125%3B&=Esc&_x:3.25%3B&=%23%0A3&_x:5.25%3B&=*%0A8%3B&@_y:-0.875&x:3.25%3B&=%2F@%0A2&_x:1%3B&=$%0A4&_x:3.25%3B&=%2F&%0A7&_x:1%3B&=(%0A9%3B&@_y:-0.875&x:6.25%3B&=%25%0A5&_x:1.25%3B&=%5E%0A6%3B&@_y:-0.875&x:1.25%3B&=~%0A%60&=!%0A1&_x:9.25%3B&=)%0A0&=%2F_%0A-&=+%0A%2F=&_w:2%3B&=Backspace&_x:0.25%3B&=Home&=PgUp%3B&@_y:-0.375&x:4.25%3B&=E&_x:5.25%3B&=I%3B&@_y:-0.875&x:3.25%3B&=W&_x:1%3B&=R&_x:3.25%3B&=U&_x:1%3B&=O%3B&@_y:-0.875&x:6.25%3B&=T&_x:1.25%3B&=Y%3B&@_y:-0.875&x:0.75&w:1.5%3B&=Tab&=Q&_x:9.25%3B&=P&=%7B%0A%5B&=%7D%0A%5D&_w:1.5%3B&=%7C%0A%5C&_x:0.75%3B&=End&=PgDn%3B&@_y:-0.375&x:4.25%3B&=D&_x:5.25%3B&=K%3B&@_y:-0.875&x:3.25%3B&=S&_x:1%3B&=F&_x:3.25%3B&=J&_x:1%3B&=L%3B&@_y:-0.875&x:6.25%3B&=G&_x:1.25%3B&=H%3B&@_y:-0.875&x:0.5&w:1.75%3B&=Caps%20Lock&=A&_x:9.25%3B&=%2F:%0A%2F%3B&=%22%0A'&_w:2.25%3B&=Enter%3B&@_y:-0.375&x:4.25%3B&=C&_x:5.25%3B&=%3C%0A,%3B&@_y:-0.875&x:3.25%3B&=X&_x:1%3B&=V&_x:3.25%3B&=M&_x:1%3B&=%3E%0A.%3B&@_y:-0.875&x:6.25%3B&=B&_x:1.25%3B&=N%3B&@_y:-0.875&w:2.25%3B&=Shift&=Z&_x:9.25%3B&=%3F%0A%2F%2F&_w:2.75%3B&=Shift&_x:1.5%3B&=Up%3B&@_x:1&w:1.25%3B&=Ctrl&_w:1.25%3B&=Alt&_w:1.25%3B&=Cmd&_x:0.25&w:2.25%3B&=Space&_x:1.25&w:2.25%3B&=Space&_x:0.25&w:1.25%3B&=Cmd&_w:1.25%3B&=Alt&_w:1.25%3B&=Ctrl&_x:0.5%3B&=Fn&_x:0.5%3B&=Left&=Down&=Right

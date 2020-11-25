---
layout: post
title:  "Certificate Pinning con Flutter"
date:   2020-11-11
categories: dart flutter certificate certificate-pinning
author: Jorge Noguera
---
Una de las recomendaciones más comunes a la hora de desarrollar aplicaciones móviles es el *Certificate Pinning*, y en este artículo vamos a ver un método sencillo para incluir en nuestras aplicaciones hechas con **Flutter**.

<!--more-->

## Qué es?

El *Certificate Pinning* es en pocas palabras una forma de verificar la identidad del servidor con el que nos estamos comunicando y de esa forma asegurar el tráfico de red de una aplicación. Así nuestra aplicación solo confía de manera *exclusiva* en las peticiones desde y hasta el servidor cuyo certificado estemos especificando dentro de nuestra aplicación.

Todo el resto del flujo de red a servidores cuyos certificados nuestra aplicación no reconozca serán rechazados.

## Implementar Certificate Pinning

Vamos a ver en pocos pasos una manera sencilla y fácil de implementar *Certificate Pinning* en **Flutter**. En este ejemplo vamos a utilizar una de las APIs abiertas más conocidas y reconocidas [**PokeAPI**][pokeapi-docs]{:target="_blank"}.

*PokeAPI es una RestfulAPI abierta al público que no requiere registro ni clave de acceso, y que dispone de manera gratuita la lista de todos los Pokemon.*

### Paso 1: Obtener el Certificado SSL del servidor

Lo primero que debemos hacer es obtener el *Certificado SSL* en el formato *.pem*, un formato amigable para trabajar. En este caso para obtener el certificado vamos a usar el navegador web *Firefox*, el cual tiene como parte del propio navegador una herramienta muy útil para revisar los certificados de las páginas web que visitamos.

Ingresamos al sitio web de *PokeAPI* y para poder ver el certificado vamos a la opción *Tools → Page Info* o también utilizando el atajo de teclado *Cmd + i*.

<center><img width="250px" src="{{ "/media/art001-cp-firefox-01.png" | relative_url }}" /></center>
<br/>
Una vez hayamos seleccionado la opción para visualizar la información de la página, vamos a la pestaña *Security* y pulsamos sobre el botón *View Certificate*.

<center><img width="700px" src="{{ "/media/art001-cp-firefox-02.png" | relative_url }}" /></center>

Al pulsar sobre la opción que nos va a permitir ver el certificado se nos va a abrir una nueva pestaña en el navegador, mostrandonos una serie de cabeceras, normalmente van a ser tres elementos y el primero que está seleccionado es el que nos importa.

La cuestión en este punto es poder descargar el archivo *.pem* del certificado del servidor, las otras opciones que se pueden ver son los *certificados intermedios* y los *certificados en las entidades emisoras* que forman parte de la [cadena de confianza][certificate-chain-wiki] entre certificados.

Moviéndonos un poco hacia abajo en la pestaña que contiene la información del certificado vamos a encontrar la opción para descargar el certificado dentro del apartado *Miscellaneous*, la opción que nos interesa es la primera **PEM (cert)**, que descargará solamente el certificado del servidor.

<center><img width="600px" src="{{ "/media/art001-cp-firefox-03.png" | relative_url }}" /></center>
<br/>
Luego de haber descargado el archivo en nuestra computadora, deberíamos tener un archivo con la extensión *.pem* que es el certificado en el que estamos interesados, su aspecto debería ser similar al siguiente.

<pre><code class="language-shellsession">-----BEGIN CERTIFICATE-----
MIIEvjCCBGSgAwIBAgIQAp2/CYVt9VZ7Sf2GIJyGuDAKBggqhkjOPQQDAjBKMQsw
CQYDVQQGEwJVUzEZMBcGA1UEChMQQ2xvdWRmbGFyZSwgSW5jLjEgMB4GA1UEAxMX
Q2xvdWRmbGFyZSBJbmMgRUNDIENBLTMwHhcNMjAwODE0MDAwMDAwWhcNMjEwODE0
MTIwMDAwWjBtMQswCQYDVQQGEwJVUzELMAkGA1UECBMCQ0ExFjAUBgNVBAcTDVNh
biBGcmFuY2lzY28xGTAXBgNVBAoTEENsb3VkZmxhcmUsIEluYy4xHjAcBgNVBAMT
FXNuaS5jbG91ZGZsYXJlc3NsLmNvbTBZMBMGByqGSM49AgEGCCqGSM49AwEHA0IA
BO3d/E4ml2Adhff/ziIlpAr+ULUlG3RyG+fxngarxdh/8h2p3ChO+0EWSOo1y5rN
ryTIKgUcafhwYI3Q0ApzgHyjggMHMIIDAzAfBgNVHSMEGDAWgBSlzjfq67B1DpRn
iLRF+tkkEIeWHzAdBgNVHQ4EFgQUwQrggFiZrQJLGp2FJqeYx+qMKC4wOgYDVR0R
BDMwMYIMKi5wb2tlYXBpLmNvghVzbmkuY2xvdWRmbGFyZXNzbC5jb22CCnBva2Vh
cGkuY28wDgYDVR0PAQH/BAQDAgeAMB0GA1UdJQQWMBQGCCsGAQUFBwMBBggrBgEF
BQcDAjB7BgNVHR8EdDByMDegNaAzhjFodHRwOi8vY3JsMy5kaWdpY2VydC5jb20v
Q2xvdWRmbGFyZUluY0VDQ0NBLTMuY3JsMDegNaAzhjFodHRwOi8vY3JsNC5kaWdp
Y2VydC5jb20vQ2xvdWRmbGFyZUluY0VDQ0NBLTMuY3JsMEwGA1UdIARFMEMwNwYJ
YIZIAYb9bAEBMCowKAYIKwYBBQUHAgEWHGh0dHBzOi8vd3d3LmRpZ2ljZXJ0LmNv
bS9DUFMwCAYGZ4EMAQICMHYGCCsGAQUFBwEBBGowaDAkBggrBgEFBQcwAYYYaHR0
cDovL29jc3AuZGlnaWNlcnQuY29tMEAGCCsGAQUFBzAChjRodHRwOi8vY2FjZXJ0
cy5kaWdpY2VydC5jb20vQ2xvdWRmbGFyZUluY0VDQ0NBLTMuY3J0MAwGA1UdEwEB
/wQCMAAwggEDBgorBgEEAdZ5AgQCBIH0BIHxAO8AdgD2XJQv0XcwIhRUGAgwlFaO
400TGTO/3wwvIAvMTvFk4wAAAXPsGBnRAAAEAwBHMEUCIANDBwmRfQuryBQGuJEC
jrQpU5gEjxdz/oFLrIlhgzsOAiEA8oCU/zVLpBmSFgXSOnbQyRhQgBV9PYmcAI6p
+F7ApEEAdQBc3EOS/uarRUSxXprUVuYQN/vV+kfcoXOUsl7m9scOygAAAXPsGBn9
AAAEAwBGMEQCIDcY6cPBaLt7+6aOKLZUn1ke3DhnObmXcYlJ3pa8jVu9AiBwAgik
HldztAA2V0bRbny+mBmwhxjwJfYpO/MEOCJ20TAKBggqhkjOPQQDAgNIADBFAiAf
SveArpf/TS8nWvx58hjlZZFSgus5CI/Tqg7ws9Nm0wIhALJSYFQM6oHVOJHvYHrb
UvrcjElb+g5XwjIEeFVJudnI
-----END CERTIFICATE-----
</code></pre>
<br />

### Paso 2: Convertir el archivo PEM en una variable de Dart

Para poder utilizar el certificado debemos convertir el contenido del archivo *.pem* en una variable de Dart, que pueda ser usada a la hora de construir el objeto que nos va a permitir realizar consultas http.

Con este objetivo en mente creé un pequeño **gist** que puede ser usado para convertir este archivo en una variable de tipo Uint8List.

<script src="https://gist.github.com/yoryer/52ced7b661003e9ad3d85569e114b0d2.js"></script>
<br/>

Preparamos los directorios y el script para ejecutar la conversión de los archivos.

<center><img width="250px" src="{{ "/media/art001-cp-finder.png" | relative_url }}" /></center>
<br/>

Para ejecutar el script sencillamente nos dirigimos al directorio donde está nuestro script y ejecutamos lo siguiente:
<pre><code class="language-shellsession">dart main.dart</code></pre> 
<br/>

Al finalizar la ejecución debe de aparecer un nuevo archivo en el directorio *generated*, el contenido de ese archivo debe ser parecido a esto:

<pre><code class="language-dart">import 'dart:typed_data';

Uint8List certificate = Uint8List.fromList([
  45,
  45,
  45,
  ...
  45,
  13,
  10
]);</code></pre>

Con esto ya tendremos nuestro certificado listo para ser utilizado en nuestras peticiones!


### Paso 3: Incluir el certificado en el cliente HTTP

En este paso vamos a crear nuestro cliente HTTP para poder comenzar a enviarle nuestras peticiones al servidor de PokeAPI, para esto vamos a ver algunos puntos a tomar en cuenta.

Incluir la dependencia del paquete *http*

<pre><code class="language-yaml">...
dependencies:
  flutter:
    sdk: flutter
  http: ^0.12.0+2
...
</code></pre>
<br/>

Con las siguientes líneas podemos generar el cliente HTTP con el certificado incluído.

<pre><code class="language-dart">import 'package:http/http.dart' as http;
import 'package:http/io_client.dart';

import 'sni-cloudflaressl-com.dart';

SecurityContext securityContext = SecurityContext(withTrustedRoots: false);
securityContext.setTrustedCertificatesBytes(certificate);

HttpClient httpClient = HttpClient(context: securityContext);

http.Client client = IOClient(httpClient);
</code></pre>

1. **import 'sni-cloudflaressl-com.dart';** Importamos el archivo que contiene la variable con la información del certificado.
2. **withTrustedRoots: false** Especificamos que no queremos incluir los certificados de las entidades emisoras.
3. **securityContext.setTrustedCertificatesBytes(certificate)** Establecemos el certificado de confianza en el *SecurityContext* usando la variable *certificate*.
4. **http.Client** Crear el http.Client a partir del IOClient que puede incluir el certificado.

### Paso 4: Probar una solicitud a la API

Juntamos todo lo que vimos y debajo de nuestro nuevo cliente HTTP que ya incluye el certificate pinning realizamos al llamada a la API para consultar los datos que estamos buscando.

<pre><code class="language-dart">String result;

try {
  http.Response response = await client.get(
    'https://pokeapi.co/api/v2/pokemon/pikachu',
  );
  result = response.body;
} catch (exception) {
  result = exception.toString();
}
</code></pre>

Con el código de arriba estamos pidiendo a PokeAPI los datos del pókemon **Pikachu**. En caso de que nuestra petición sea exitosa nos va a retornar un texto en formato JSON con toda la información del Pokemon, caso contrario nos devolverá un error.

Si quieres ver un ejemplo más claro de la implementación de un certificate pinning puedes revisar el [repositorio en Github][github-repo]{:target="_blank"} donde muestro un caso exitoso y otro fallido.

<center><img width="250px" src="{{ "/media/art001-cp-pikachu.png" | relative_url }}" /></center>
<br/>

[uint8list-docs]: https://api.dart.dev/stable/2.10.4/dart-typed_data/Uint8List-class.html
[pokeapi-docs]: https://pokeapi.co/
[certificate-chain-wiki]: https://es.wikipedia.org/wiki/Cadena_de_confianza
[github-repo]: https://github.com/yoryer/flutter_certificate_pinning



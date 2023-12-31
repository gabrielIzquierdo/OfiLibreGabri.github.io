---
title: Impresión desde Linux en las impresoras de la URJC
logo: impresoras/impresora.png
date: 2019-11-19
published: true
type: guias
---

Esta guía se refiere a las impresoras accesibles mediante el
portal de impresión de la URJC. Son las impresoras instaladas en edificios departamentales,
edificios de gestión, etc., a disposición del personal de la Universidad.
Las instrucciones que se ofrecen aquí han sido probadas en
Ubuntu 19.04 y 18.04, y en Debian 10.

## Qué vamos a hacer

Vamos a instalar el servicio de impresión como una cola
de impresión virtual via SMB.
Los trabajos que se envíen a esta cola se podrán recoger luego
en cualquier impresora gestionada por el portal de impresión de la
URJC, pasando el carnet por el frontal de la impresora,
justo debajo de la pantalla.

El servicio de impresión está configurado en la dirección `iveco.urjc.es`.
Dependiendo de cómo tengas configurado DNS en tu caja Linux,
puede que lo tengas directamente accesible o no. Ver sección
"[Configuración de DNS](#configuracion-dns)" al final.

## Instalación de los paquetes necesarios

Para instalar la cola de impresión virtual, comenzamos por instalar
los paquetes necesarios: python3-smbc smbclient.

```
$ sudo apt install python3-smbc smbclient
```

En Debian, es también conveniente instalar `system-config-printer`
para poder configurar impresoras CUPS (alternativamente,
se pueden configurar desde la interfaz web):

```
$ sudo apt install system-config-printer
```

Dependiendo del sistema operativo exacto que tengas, puede que
durante el proceso de instalación te de opción de usuar
información de servidores de WINS recibida por DHCP.
En principio, en nuestro caso, no parece que la respuesta
a esa pregunta tenga un efecto sobre el resto del proceso.

En algunos casos (por ejemplo, en Ubuntu 16.04), puede hacer falta
también editar el fichero `/etc/samba/smb.conf`,
añadiendo en la sección `global`, normalmente al principio del fichero,
las siguientes líneas:

```
client min protocol = SMB2
client max protocol = SMB3
```

Si se hace esto, luego hay que reiniciar el servicio:

```
$ sudo service smbd restart
```

## Configuración de la impresora

La impresora es una Konica Minolta bizhub C458,
que configuraremos como "C759SeriesPS(P) BEU".
Comenzamos por descargar los ficheros necesarios
de la página de la
[bizhub C458](https://kmbs.konicaminolta.us/kmbs/technology/multifunction/color-multi-function/bizhub-c458):
"Drivers", "Drivers", "Linux", "Version 1.21" (última versión al editar esta guía, 1.21, el nombre de los ficheros variará si aparecen nuevas versiones), y en esa entrada se descargan los tres ficheros que ofrece:

* GenericBeuUXv1_21_multi_language.tar.gz
* BEU Linux CUPS Driver Guide.pdf
* KMbeuUXv1_21_multi_language.tar.gz

Guardamos los tres en el mismo directorio.que

A partir de aquí podemos usar dos procedimientos alternativos.que

### Procedimiento más automático

Empezamos por usar el primero de los ficheros descargados, descomprimiéndolo:

```
$ tar xvzf GenericBeuUXv1_20_multi_language.tar.gz
```

Una vez descomprimido, veremos un directorio `GenericBeuUXv1_20_multi_language`
en el que hay un fichero de instalación (requiere tener Perl instalado),
que ejecutaremos. Al terminar, reiniciaremos el servicio de CUPS con la
nueva configuración:

```
$ cd GenericBeuUXv1_20_multi_language
$ sudo ./install.pl
$ sudo /etc/init.d/cups restart
```

Este proceso instalará los ficheros PPD (descriptores de impresoras para CUPS)
para la impresora.

A continuación, se ejecuta el programa `system-config-printer`
(en Ubuntu, panel de configuración, opción "Impresoras"),
o se abre en el navegador la
[página de configuración de CUPS](http://localhost:631/admin).
En el programa, se elige "Añadir", "Impresora de red",
"Impresora Windows vía SAMBA".
Cuando nos pide la dirección de la impresora, ponemos
`smb://<dominio>/iveco.urjc.es/URJC_IMPRESORA_VIRTUAL`

El `<dominio>` es el dominio WINS en el que estás, puede ser
`people`, `escet`, `cct`, u otros (ver más abajo "[En qué dominio estoy](#dominio)").

Asegúrate tambien de marcar "avisar al usuario si hace falta autenticación"
(o si prefieres, deja ya grabado tu nombre de usuario y tu contraseña)
y a continuación elige la impresora
`KONICA MINOLTA C759SeriesPS(P) BEU v1.1 (color)` que encontrarás
en el menú corresponiente.

### Procedimiento más manual

Alternativamente, se pueden instalar estos ficheros mediante
un proceso más manual, a partir del fichero (tamibén descargado anteriormente)
`KMbeuUXv1_20_multi_language.tar.gz`:

```
$ tar xvzf KMbeuUXv1_20_multi_language.tar.gz
$ cd KMbeuUXv1_20_multi_language
$ sudo cp KMbeuEmpPS.pl /usr/lib/cups/filter/
$ sudo chown root:root /usr/lib/cups/filter/KMbeuEmpPS.pl
$ sudo chmod 755 /usr/lib/cups/filter/KMbeuEmpPS.pl
```

A continuación, se añade la impresora usando el
programa `system-config-printer`
(en Ubuntu, panel de configuración, opción "Impresoras"),
o en la [página de configuración de CUPS](http://localhost:631/admin),
con los mismos datos y procedimiento que en la opción automática
(`smb://<dominio>/iveco.urjc.es/URJC_IMPRESORA_VIRTUAL`, como
dirección de la impresora, y marca
"avisar al usuario si hace falta autenticación").

Pero ahora eliges, en lugar de la impresora directamente,
"Configurar a partir de fichero ppd". Y cuando te da
opción de elegir el fichero, eliges el que está en directorio
para esta configuración, con mombre `KMbeuC759ux.ppd`.

## Cómo imprimimos

Con el procedimiento que hayas elegido, habrás terminado de
instalar la impresora, y ahora deberías poder usarla.
Para imprimir, utiliza la aplicación que quieras, y elige
al imprimir la impresora que acabas de instalar.
Esto hará que el trabajo quede en la cola de impresión,
pendiente de autenticación.

En la cola de impresión (que puedes acceder
[vía la página de CUPS](http://localhost:631))
podrás autenticar el trabajo. También puedes guardar el usuario
y contraseña, si prefieres (y consideras tu equipo suficientemente seguro para ello).

Las credenciales que te pedirá para la autenticación son:

* Usuario: dirección de correo electrónico de la URJC (nombre de dominio único)
sin `@urjc.es`
* Contraseña: la del dominio único

**Atención**: No siempre tu nombre de usuario se determina de esta forma.y
Ver más abajo "[En qué dominio estoy, qué usuario soy](#dominio)" para obtener
más información sobre cómo saber tu nombre de usuario, y
qué hacer si no te funciona el que crees que deberías tener.

## <a name="configuracion-dns"></a>Configuración de DNS

Antes de empezar, asegurarse de que se llega a iveco.urjc.es. El problema es que el nombre puede no estar en DNS.

En un terminal, ejecutar:

dig +noall +answer iveco.urjc.es

y debería decir la ip, si no dice nada, hay que añadirla.
Para añadirla, se edita como root el fichero de /etc/hosts
sudo vi /etc/hosts

y se añade la línea:

192.168.46.181 iveco.urjc.es

## <a name="dominio"></a>En qué dominio estoy, qué usuario soy

Normalmente el nombre de usuario será el de dominio único, sin `@urjc.es`.
Por ejemplo, si tu dirección de dominio único es
`nombre.apellidos@urjc.es`, tu nombre de usuario será normalmente
`nombre.apellidos`.
Pero si tu nombre de usuario (nombre de dominio único sin `@urjc.es`)
es de más de 20 caracteres, es muy probable que el nombre que tengas que usar sean
los 20 primeros caracteres. En algunos casos excepcionales, puede que
tu nombre de usuario sea distinto del de dominio único.
En general, hemos visto que el nombre que puedes ver en la impresora
cuando te has autenticado con tu tarjeta es el que tendrás que utilizar
(podrás verlo en la pantalla que te sale en la impresora nada más autenticarte).

También necesitas saber en qué dominio (workgroup) estás.
Hay varios dominios separados, entre ellos: `people`, `cct` y ninguno.
Aparentemente, cada usuario estamos en alguno de estos, así
que tendrás que saber en cuál estás, o probar.

Una forma de probar es usando el programa `smbclient`.
Por ejemplo, si tu dirección de dominio único es
`nombre.apellidos@urjc.es`, puedes probar así si estás en el
dominio `people`:

```
$ smbclient -U nombre.apellidos -W people -L iveco.urjc.es
Enter PEOPLE\nombre.apellidos's password:

    Sharename       Type      Comment
    ---------       ----      -------
    ....
    URJC_IMPRESORA_VIRTUAL Printer   Impresion virtual en todos los Campus de URJC
```

De esta forma pruebas, en realidad, la combinación nombre / dominio,
así que podrás usarla para validar ambos.

Si no consigues encontrar una combinación que te funcione,
puedes abrir una incidencia en el [CAU de la URJC](https://cau.urjc.es),
y preguntar tu nombre de usuario y tu dominio (workgroup) Windows.

---------
Guía preparada con la ayuda de Gorka Guadiola, José Centeno (primeras versiones)
y Miguel Ortuño (notas sobre Ubuntu 16.04).
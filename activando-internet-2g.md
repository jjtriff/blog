# Activando el servicio de Internet por Datos para usuarios 2G de Etecsa

## Introducción

Recientemente, a principios de diciembre de 2018, Etecsa ha activado para sus usuarios el servicio de Internet por datos móviles. Esto suena súper :) pero lo cierto es que tiene algo de "tramposo": **El servicio es solo para los móviles que soporte 3G en la banda de los 900 MHz**.

Para quienes no sepan qué es Etecsa vean [www.etecsa.cu](http://etecsa.cu). Es el monopolio de las comunicaciones en Cuba. La única empresa autorizada por el Estado a brindar servicios de telecomunicaciones.

Cualquiera pudiera decir:

>Qué se le va a hacer, en Cuba solo usan los 900 MHz.

A esto respondemos:

>Pero sí hay clientes que se conectan en 2G en la banda de los 900 MHz, ¿por qué a ellos no?

Creo que esto tiene 2 objetivos fundamentales:

1. Mantener una menor cantidad de usuarios conectados a la red: lo que redunda en un mejor servicio para los conectados
2. Mantener una mayor cantidad de ciudadanos desconectados sin que intercambien información libremente

## Quick background sobre los móviles en Cuba

No es un secreto para nadie que muchos de los cubanos de la isla tenemos familia en Estados Unidos, y principalmente en Miami, FL. Especulo cuando digo (pero me atrevo a decir) que la mayoría de los móviles, smartphones, que hoy día usan los cubanos son importados por esta vía. Muchos iPhones, muchos más Android.

En Miami, es muy fácil/barato obtener un teléfono de una de las compañías como MetroPCS (la favorita de los recién llegados), Cricket, BoostMobile, etc. que vienen anclados a esta compañía (es decir, que no se pueden usar con otros números/líneas que no sean los de la compañía). Esta condición de "anclados" es lo que abarata tanto el costo del smartphone, pues parte del precio está cubierto (en cubano "subsidiado") por la compañía. Una vez comprado el móvil lo traen a Cuba, donde lo "desbloquean" o remueven el impedimento de anclaje, para que se pueda usar con el servicio Cubacel/Etecsa.

De esta forma se puede conseguir un smartphone en un precio relativamente bajo. *Hasta ahí todo bien.*

La mayoría de estos teléfonos soportan hasta **4G LTE** en la compañía original, en Estados Unidos. Pero la mayoría **NO SOPORTA 3G en los 900 MHz** pues están diseñados para otras bandas (850 MHz, etc.)

No obstante se conectan a 2G utilizando el APN `nauta` que desde hace años venimos usando para revisar nuestro correo `nauta.cu`.

## Pasos para activar el servicio en tu móvil 2G

Los pasos que seguí y finalmente me permitieron usar internet con mi móvil [LG HD Tribute de Boostmobile](https://www.lg.com/us/cell-phones/lg-LS676-Boost-tribute-hd) fueron los siguientes:

1. Buscar un amigo con un móvil que soporte 3G en los 900 MHz
1. Conectar mi SIM (la línea de móvil) en su teléfono
1. Crear el APN `nauta`. Si no sabe cómo hacerlo puede seguir esta [guía publicada por Etecsa](http://www.etecsa.cu/data/Configuracion_terminales_moviles_para_el_acceso_al_correo_nauta.pdf) en lo que crear el APN se refiere.
1. Encender los datos móviles.
1. Abrir algún navegador. Todos los smartphones traen uno: `Chrome, Safari, Firefox, Opera Mini`. En los teléfonos Samsung muchas veces dice sencillamente `Internet`.
1. En el navegador ir a la dirección https://mi.cubacel.net:8443/login/jsp/registerNew.jsp
1. Seguir los pasos de registro que incluyen esperar a recibir un SMS e introducir el código que viene en ese SMS en la página.
    > Nota: Paciencia con el campo que dice: `Ingrese el código tal como aparece en la imagen inferior`. Siempre introduzca las letras respetando las mayúsculas y minúsculas.
1. Una vez terminado el proceso de registro (cuando ya se vea `Bienvenido FULANO DE TAL a MiCubacel`) apagar los datos móviles.
1. Sacar la SIM del celular del amigo y conectarla en el nuestro.
1. **No activar los datos móviles en nuestro celular** (de esta manera evitamos que lo vuelvan a poner en la base de datos de 2G, o al menos que no lo quiten de la base de datos de 3G)
1. Esperar a que llegue el ansiado mensaje de Cubacel (que no recuerdo cómo dice exactamente) al menos unas 48h.
1. Una vez llegado el mensaje comprar alguno de los paquetes de datos, que se pueden comprar desde la misma página mi.cubacel.net o marcando `*133#` y siguiendo las instrucciones del menú.

Estos pasos me permitieron usar internet en mi móvil, desde casa ¡¡¡yes!!! :) .

> **Importante**: Debemos recordar siempre que al estar conectados con 2G la velocidad de conexión es más lenta que 3G pero no imposible. De hecho se puede usar muy bien para llamadas de audio de IMO y WhatsApp.

## Conclusiones

Cuando se trata de internet mejor lento que ninguno. Por último recomendar algunas aplicaciones que servirán para el control, ahorro y mejor uso de datos:

1. `Datally de Google`: controla el tráfico de todas las aplicaciones y facilita contabilizar y no desperdiciar datos en segundo plano.
1. `Opera Mini`: navegador para ahorro de datos, que en modo ahorrativo utiliza un proxy para ahorrarnos datos (perdón la redundancia) y al mismo tiempo puede evitar la censura de algunos sitios web restringidos en Cuba
1. `WhatsApp`: para video llamadas, llamadas de audio y compartir fotos. Naturalmente, no tiene tantos usuarios como Facebook o Messenger.
1. `Messenger Lite`: si TIEEEEEEENES que usar Messenger mejor que uses la versión Lite que ahorra en datos
1. `Facebook Lite`: por último si TIEEEEEEEEEENES que usar Facebook esta versión también ahorra datos en comparación con la versión original.
1. Instagram Lite: **ESTO NO EXISTE** por lo tanto prepárate para gastar el paquete :(

Espero que haya sido de utilidad y que Etecsa no se disponga a troncharnos el servicio a los que hayamos activado por esta vía. Porque más vale cubanos informados que desinformados. 

En algún momento (seguro para el mes o la recarga que viene, pues no me quedan muchos datos del paquete) subiré una foto comparativa de la velocidad de los datos entre 2G y 3G según el sitio http://fast.com

#Etecsa #datos-moviles #internet #apps #cuba

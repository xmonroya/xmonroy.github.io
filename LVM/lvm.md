# Logical Volume manager (LVM)
LVM es un sistema de gestión del almacenamiento que te libera de las limitaciones de las particiones de disco fijas, ofreciendo una manera mucho más flexible y dinámica de organizar y redimensionar tu espacio.

## ¿Qué es LVM y por qué usarlo?
En los servidores, es común particionar los discos para manejar distintos sistemas de archivos. Este esquema produce particiones de tamaño fijo, cuyo tamaño no se puede redimensionar fácilmente. Redimensionarlas exige varios requisitos; si estos no se cumple, a menudo se debe optar por migrar la información hacia otro disco más grande. Todos estos procesos implican el reinicio del servidor, lo que produce períodos de interrupción de los servicios.

LVM resuelve este problema introduciendo una capa de abstracción. En lugar de lidiar con particiones fias, creas volúmenes lógicos que pueden abarcar múltiples discos fisicos o particiones y ser fácilmente redimensionados sobre la marcha. Esto proporciona numerosas ventajas:

* Redimensionamiento dinámico: Expande o contrae volúmenes lógicos sin desmontar o reiniciar el sistema. Esto es crucial para servidores y aplicaciones que necesitan permanecer en línea.
* Asignación flexible: Agrupa almacenamiento de diferentes discos duros y lo asigna a volúmenes lógicos según sea necesario, evitando espacio desperdiciado.
* Gestión simplificada: Administra el almacenamiento utilizando nombre de volúmenes lógicos ( por ejemplo, "databases", "webserver") en lugar de nombres complejos de dispositivos como /dev/sda1
* Funciones avanzadas: LVM ofrece funciones como snapshots (para copias de seguridad o pruebas), striping (para rendimiento) y mirroring (para redundancia), que no son fácilmente alcanzables con particiones tradicionales.

## Componentes clave de LVM

LVM funciona al apilar estos tres componentes principales:

* Volúmenes físicos (PV): Estos son la base de LVM. Un PV puede ser un disco duro completo, una partición de un disco duro u otro dispositivo de bloque. Para marcar una partición como un PV se utiliza el comando pvcreate.
* Grupos de volúmenes (VG): Los PV se agrupan en VG, que actúan como un solo grupo de almacenamiento. El comando vgcreate crea un VG.
* Volúmenes lógicos (LV): los LV se extraen del VG. Estos son los volúmenes con los que interactúan tu sistema operativo y las aplicaciones. Puedes formatearlos con un sistema de archivos(como EXT4 o XFS) y montarlos como particiones regulares. El comando lvcreate se utiliza para crear LV.


Ejemplo práctico

Digamos que tienes un servidor web con dos discos duros /dev/sda y /dev/sdb. Aquí tienes cómo podrías organizar el almacenamiento usando LVM.

a. Crear los PV
```BASH
pvcreate /dev/sda /dev/sdb
```
b. Crear un VG a partir de los PV recién creados
```BASH
vgcreate webserver_vg /dev/sda /dev/sdb
```
c. Crear los Volumenes lógicos
```BASH
#Para los archivos del sitio web
lvcreate -L 20G -n websites webserver_vg

#Para los archivos de la base de datos
lvcreate -L 10G -n database webserver_vg
#Para los archivos de registro, utilizando el espacio restante
lvcreate -l 100%FREE -n logs webserver_vg
```

Ahora tienes tres volúmenes lógicos, "website", "database" y "logs", que puedes formtear y montar. Si el volumen "website" necesita mas espacio, puedes expandirlo fácilmente usando lvextend sin afectar el servidor web en ejecución.

## Beneficios para adminitradores de sistemas
LVM simplifica las tareas de administración de almacenamiento para administradores de sistemas:

* Expansión más fácil: Agregar más almacenamiento y expandir volúmenes se convierte en un proceso sencillo.

* Tiempo de inactividad reducido: Muchas operaciones se pueden realizar en línea, minimizando las interrupciones del servicio.

* Flexibilidad: LVM proporciona mayor control sobre la asignación de almacenamiento y facilita la adaptación a los requisitos cambiantes.

* Protección de datos: Funciones como snapshots y mirroring mejoran las capacidades de protección de datos.

## ¿Cuándo es más útil LVM?

LVM es particularmente valioso en:

* Entornos de servidor: Donde la flexibilidad, el tiempo de actividad y la utilización eficiente del almacenamiento son críticos.

* Virtualización: LVM se utiliza a menudo para administrar almacenamiento para máquinas virtuales, permitiendo el fácil redimensionamiento y creación de snapshots de discos virtuales.

* Sistemas con necesidades de almacenamiento en crecimiento: LVM facilita la adaptación a las demandas crecientes de almacenamiento sin complejos redimensionamientos.

LVM es una tecnología poderosa y versátil que mejora la administración de almacenamiento en sistemas Linux. Al comprender sus conceptos clave y ventajas, puedes desbloquear una mayor flexibilidad y eficiencia en la gestión de tu espacio en disco.

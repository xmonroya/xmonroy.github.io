# Creación de un volumen group (VG)
Un Grupo de Volumen es una colección de uno o más volumenes físicos (PVs). los VGs forman la base del sistema de administación de volúmenes lógicos y actúan como un conjunto de almacenamiento del cual puedes asignar volúmenes lógicos.

Partamos de la base que ya tenemos uno o más Volumenes Físicos (PV) creados, llamémolos /dev/sda y /dev/sdb.

Y a parti de aquí veamos las operación de creación de un Grupo de Volumenes.

El comando utilizado para la creación de un VG corresponde a:

```BASH
sudo vgcreate my_volume_group /dev/sda /dev/sdb
```

este comando básicamente crea un nuevo VG de nombre "my_volume_group" que contendrá los Volumenes Físicos /dev/sda y /dev/sdb.

Este nuevo VG tendrá como espacio la suma de los tamaños de /dev/sda y /dev/sdb, y podrá ser utilizado para crear Volumenes Lógicos de acuerdo a lo que necesitemos.


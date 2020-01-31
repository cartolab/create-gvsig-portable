Kit de herramientas desarollado por [Cartolab](http://cartolab.udc.es) y mantenido actualmente por [iCarto](http://icarto.es) para crear versiones personalizadas y portables de gvSIG 1.x

Este proyecto es básicamente un script de Ant que permite crear versiones portable de gvSIG para Windows y Linux.

# Requisitos

* El proceso debe ejecutarse en una máquina Linux
* Configurar un workspace válido con los proyectos que quieras incluir en tu versión
* Descargar el `binaries/portable.tgz` de este mismo repositorio. Contiene la máquina virtual de Java y algunos ficheros adicionales. Por defecto se espera que este fichero sea descomprimido en /var/tmp
* Descomprimir con `tar xzf portable.tgz --preserve-permission`. Dentro del tar van ficheros ejecutables. Si no se descomprime de esa forma se perderá el "executable bit"
* Añadir al workspace el proyecto **create-gvsig-portable**, o copiar:
 * El script **deploy.xml**
 * El directorio **portable** a tu propio proyecto

# Como construir la versión portable

* Editar el fichero **portable/projects.xml**. Eliminar de este proyecto los plugins que no nos interesen, y añadir los que queramos
* Editar las properties *this-folder*, *base-path*, *custom-portable-path*, *name-lin, *name-win* de **deploy.xml** con los valores que queramos
* Ejectuar el target **createPortables** del script **deploy.xml**. Llega con lanzar uno de los targets, generalmente `makePortableWindows`

```
Click derecho sobre deploy.xml -> Run as ... -> Ant build ... -> Marcar unicamente makePortableWindows -> Run

En tmp se creará un directorio con la versión portable que se puede lanzar directamente (en una máquina virtual para probar) y un zip que se puede entregar

Debido a un bug no identificado en ocasiones aleatorias borra algunos ficheros de /var/etc/portable. Simplemente abrir el directorio de /tmp/xxx/jre/bin y comprobar que está el ejecutable de java. Si no es así descomprimir de nuevo portable.tgz en la ruta adecuada y volver a ejecutar.
```

Esto generará un .zip y un .tgz en el directorio /tmp que contendrá el gvSIG portable.


# Personalizaciones

Ademas de tocar el fichero proyect.xml para decidir que plugins añadir en tu versión se puede:

* Incluir ficheros adicionales añadiéndolos con la estructura de directorios completa bajo el directorio **portable**. Mira por ejemplo como se modifican el config.xml de extAnnotation creando un nuevo fichero en create-gvsig-portable/portable/common/bin/gvSIG/extensiones/com.iver.cit.gvsig.annotation/config.xml

* Los ficheros de configuración van el directorio **cfg**. Puedes acceder a este directorio por código llamando a Launcher.getAppHomeDir()

* Cualquier contenido que coloques bajo el directorio **portable/common** Será copiado bajo la misma ruta en la portable para Windows y para Linux. Los ficheros que coloques bajo **portable/lin** o **portable/win** irán sólo a la portable de ese sistema operativo.

# Crear un entorno de desarrollo

Para configurar un entorno de desarrollo se puede ejecutar el target **createTestEnviroment** del fichero **deploy.xml**. Esto limpiará andami y comilará todos los proyectos que le hayamos indicado, además de sobreescribir los ficheros que hayamos definido bajo el directorio **portable**

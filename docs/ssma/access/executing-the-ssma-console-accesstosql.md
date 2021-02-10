---
description: Ejecutar la consola SSMA (AccessToSQL)
title: Ejecución de la consola de SSMA (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: aa1bf665-8dc0-4259-b36f-46ae67197a43
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: cf369ee1104f4b686dc0e7b6ab4bbde7bab458de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044705"
---
# <a name="executing-the-ssma-console-accesstosql"></a>Ejecutar la consola SSMA (AccessToSQL)
Microsoft proporciona un conjunto sólido de comandos de archivo de script y opciones de línea de comandos para ejecutar y controlar las actividades de SSMA. Las secciones siguientes detallan el mismo.  
  
## <a name="project--script-file-commands"></a>Comandos de archivo de script de proyecto  
Los comandos del proyecto controlan la creación de proyectos, la apertura, el guardado y el cierre de proyectos.  
  
**Comando**  
  
Create-New-Project: crea un nuevo proyecto de SSMA.  
  
**Script**  
  
-   `project-folder` indica la carpeta del proyecto que se va a crear.  
  
-   `project-name` indica el nombre del proyecto. {string}  
  
-   `overwrite-if-exists`Atributo opcional indica si se debe sobrescribir un proyecto existente. booleano  
  
-   `project-type` es un atributo opcional.  Las siguientes opciones están disponibles para el tipo de proyecto:  
  
    -   SQL Server-2005  
  
    -   SQL Server-2008  
  
    -   SQL Server-2012  
  
    -   SQL Server-2014  
  
    -   SQL Server-2016  
  
    -   SQL: Azure  
  
    El valor predeterminado es "SQL-Server-2008".  
  
**Ejemplo**:  
  
```xml  
<create-new-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
  overwrite-if-exists="<true/false>"  
  
  project-type="<sql-server-2008 | sql-server-2005 | sql-server-2012 | sql-server-2014 | sql-azure>"  
  
/>  
```  
El atributo ' overwrite-if-exists ' es **false** de forma predeterminada.  
  
El atributo ' Project-type ' es **SQL-Server-2008** de forma predeterminada.  
  
**Comando**  
  
Abrir proyecto: abre un proyecto existente.  
  
**Script**  
  
-   `project-folder` indica la carpeta del proyecto que se va a crear. El comando genera un error si la carpeta especificada no existe.  {string}  
  
-   `project-name` indica el nombre del proyecto. El comando genera un error si el proyecto especificado no existe.  {string}  
  
**Ejemplo de sintaxis:**  
  
```xml  
<open-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
/>  
```  
**Nota:** La aplicación de consola de SSMA para Access admite la compatibilidad con versiones anteriores. Puede abrir proyectos creados por la versión anterior de SSMA.  
  
**Comando**  
  
guardar-proyecto: guarda el proyecto de migración.  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<save-project/>  
```  
**Comando**  
  
Close-Project: cierra el proyecto de migración.  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<close-project  
  
  if-modified="<save/error/ignore>"   (optional)  
  
/>  
```  
El atributo ' If-Modified ' es opcional y se **omite** de forma predeterminada.  
  
## <a name="database-connection-script-file-commands"></a>Comandos de archivo de script de conexión de base de datos  
Los comandos de conexión de base de datos ayudan a conectarse a la base de datos.  
  
La característica de **exploración** de la interfaz de usuario no se admite en la consola de.  
  
Los parámetros de **Puerto** y **autenticación de Windows** no son aplicables al conectarse a SQL Azure.  
  
Para obtener más información sobre cómo crear archivos de scripts, vea [crear archivos de script &#40;AccessToSQL&#41;](../../ssma/access/creating-script-files-accesstosql.md).  
  
**Comando**
  
Connect-Source-Database  
  
- Realiza la conexión a la base de datos de origen y carga los metadatos de alto nivel de la base de datos de origen, pero no todos los metadatos.  
  
- Si no se puede establecer la conexión con el origen, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**
  
La definición del servidor se recupera del atributo name definido para cada conexión en la sección Server del archivo de conexión del servidor o del archivo de script.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
**Comando**  
  
Load-Access-Database: se usa para cargar archivos de base de datos de Access  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<load-access-database  database-file="<Access-database>"/>  
```  
or  
  
```xml  
<load-access-database>  
  
  <access-database database-file="<Access-database1>"/>  
  
  <access-database database-file="<Access-database2>"/>  
  
</load-access-database>  
```  
**Comando**  
  
forzar carga-origen/destino-base de datos  
  
-   Carga los metadatos de origen.  
  
-   Resulta útil para trabajar en el proyecto de migración sin conexión.  
  
-   Si no se puede establecer la conexión con el origen o el destino, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**  
  
Requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<force-load  
  
  object-name="<object-name>"  
  
  metabase="<source/target>"/>  
```  
or  
  
```xml  
<force-load>  
  
  <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
**Comando**  
  
reconnect-Source-Database  
  
- Vuelve a conectar con la base de datos de origen, pero no carga ningún metadato, a diferencia del comando Connect-Source-Database.  
  
- Si no se puede establecer la conexión con el origen, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**
  
**Ejemplo de sintaxis:**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```

**Comando**
  
Connect-Target-Database  
  
- Se conecta al SQL Server de destino o SQL Azure base de datos y carga los metadatos de alto nivel de la base de datos de destino, pero no los metadatos por completo.  
  
- Si no se puede establecer la conexión con el destino, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**
  
La definición del servidor se recupera del atributo name definido para cada conexión en la sección Server del archivo de conexión del servidor o del archivo de script.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  

**Comando**
  
reconnect-Target-Database  
  
- Vuelve a conectar con la base de datos de destino, pero no carga ningún metadato, a diferencia del comando Connect-Target-Database.  
  
- Si no se puede establecer la conexión con el destino, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**
  
**Ejemplo de sintaxis:**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  

## <a name="report-script-file-commands"></a>Comandos de archivo de script de informe

Los comandos de informe generan informes sobre el rendimiento de diversas actividades de la consola de SSMA

**Comando**
  
generar informe de evaluación  
  
- Genera informes de evaluación en la base de datos de origen.  
  
- Si no se realiza la conexión a la base de datos de origen antes de ejecutar este comando, se genera un error y se cierra la aplicación de consola.  
  
- No se puede conectar con el servidor de base de datos de origen durante la ejecución del comando, lo que da lugar a la finalización de la aplicación de consola.  
  
**Script**

- `assessment-report-folder:` Especifica la carpeta donde se puede almacenar el informe de evaluación. (atributo opcional)  
  
- `object-name:` Especifica los objetos que se han tenido en cuenta para la generación de informes de evaluación (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
- `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
- `assessment-report-overwrite:` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
- `write-summary-report-to:` Especifica la ruta de acceso donde se generará el informe.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **AssessmentReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    - `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    - `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<generate-assessment-report  
  
  object-name="ssma.Procedures"  
  
  object-type="category"  
  
  write-summary-report-to="<file>"             (optional)  
  
  verbose="<true/false>"                       (optional)  
  
  report-errors="<true/false>"                 (optional)  
  
  conversion-report-folder="<folder>"          (optional)  
  
  conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```

or
  
```xml  
<generate-assessment-report  
  
  conversion-report-folder="<folder>"            (optional)  
  
  conversion-report-overwrite="<true/false>"     (optional)  
  
>  
    <metabase-object object-name="ssma.Procedures"  
  
        object-type="category"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-script-file-commands"></a>Comandos del archivo de script de migración

Los comandos de migración convierten el esquema de la base de datos de destino en el esquema de origen y migran los datos al servidor de destino.  
  
La configuración predeterminada de salida de la consola para los comandos de migración es el informe de salida ' completo ' sin informes de errores detallados: solo Resumen en el nodo raíz del árbol de objetos de origen.  
  
**Comando**

Convert-Schema  
  
- Realiza la conversión del esquema del origen al esquema de destino.  
  
- Si no se realiza la conexión a la base de datos de origen o de destino antes de ejecutar este comando o si se produce un error en la conexión con el servidor de base de datos de origen o de destino durante la ejecución del comando, se genera un error y se cierra la aplicación de consola.  
  
**Script**

- `conversion-report-folder:` Especifica la carpeta donde se puede almacenar el informe de evaluación. (atributo opcional)  
  
- `object-name:` Especifica los objetos de origen que se han tenido en cuenta para convertir el esquema (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
- `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
- `conversion-report-overwrite:` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
- `write-summary-report-to:` Especifica la ruta de acceso donde se generará el informe.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **SchemaConversionReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    - `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    - `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<convert-schema  
  
  object-name="ssma.Procedures"  
  
  object-type="category"  
  write-summary-report-to="<filepath/folder>"     (optional)  
  
  verbose="<true/false>"                          (optional)  
  
  report-errors="<true/false>"                    (optional)  
  
  conversion-report-folder="<folder>"             (optional)  
  
  conversion-report-overwrite="<true/false>"      (optional)  
  
/>  
```

or  
  
```xml  
<convert-schema  
  
  conversion-report-folder="<folder>"         (optional)  
  
  conversion-report-overwrite="<true/false>"  (optional)  
  
  <metabase-object object-name="ssma.Procedures"  
  
    object-type="category"/>  
  
</convert-schema>  
```

**Comando**
  
migrar datos  
  
- Migra los datos de origen al destino.  
  
**Script**
  
- `object-name:` Especifica los objetos de origen que se han tenido en cuenta para migrar datos (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
- `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
- `write-summary-report-to:` Especifica la ruta de acceso donde se generará el informe.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **DataMigrationReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    - `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    - `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<migrate-data  
  
  write-summary-report-to="<filepath/folder>"  
  
  report-errors="true" verbose="true">  
  
    <metabase-object object-name="ssma.TT1"/>  
  
    <metabase-object object-name="ssma.TT2"/>  
  
    <metabase-object object-name="ssma.TT3"/>  
  
    <data-migration-connection  
  
      source-use-last-used="true"/source-server="<server-unique-name>"  
  
      target-use-last-used="true"/target-server="<server-unique-name>"/>  
  
</migrate-data>  
```

or  
  
```xml  
<migrate-data  
  
  object-name="ssma.Tables"  
  
  object-type="category"  
  
  write-summary-report-to="<filepath/folder>"  
  
  report-errors="true" verbose="true"/>  
```

**Comando**
  
Link-Tables: este comando vincula la tabla de origen (de acceso) con la tabla de destino.  
  
**Script**

**Ejemplo de sintaxis:**  
  
```xml  
<link-tables>  
  
  <metabase-object object-name="AccessDatabase.table1" object-type="Tables"/>  
  
  <metabase-object object-name="AccessDatabase.table2" object-type="Tables"/>  
  
</link-tables>  
```

or  
  
```xml  
<link-tables>  
  
  <metabase-object object-name="AccessDatabase.Tables" object-type="category"/>  
  
</link-tables>  
```

**Comando**
  
unlink-Tables: este comando desvincula la tabla de origen (acceso) de la tabla de destino.  
  
**Script**
  
**Ejemplos de sintaxis:**  
  
```xml  
<unlink-tables>  
  
  <metabase-object object-name="AccessDatabase.table1" object-type="Tables"/>  
  
  <metabase-object object-name="AccessDatabase.table2" object-type="Tables"/>  
  
</unlink-tables>  
```

or  
  
```xml  
<unlink-tables>  
  
  <metabase-object object-name="AccessDatabase.Tables" object-type="category"/>  
  
</unlink-tables>  
```  
  
## <a name="migration-preparation-script-file-commands"></a>Comandos del archivo de script de preparación de la migración

El comando de preparación de la migración inicia la asignación de esquemas entre las bases de datos de origen y de destino.  
  
**Comando**
  
Map-Schema: asignación de esquema de la base de datos de origen al esquema de destino.  
  
**Script**
  
- `source-schema` especifica el esquema de origen que se va a migrar.  
  
- `sql-server-schema` especifica el esquema de destino en el que se desea migrar.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<map-schema source-schema="source-schema"  
  
            sql-server-schema="target-schema"/>  
```  
  
## <a name="manageability-commands"></a>Comandos de administración

Los comandos de administración ayudan a sincronizar los objetos de base de datos de destino con la base de datos de origen.  
  
La configuración predeterminada de salida de la consola para los comandos de migración es el informe de salida ' completo ' sin informes de errores detallados: solo Resumen en el nodo raíz del árbol de objetos de origen.  
  
**Comando**

sincronizar-destino  
  
- Sincroniza los objetos de destino con la base de datos de destino.  
  
- Si este comando se ejecuta en la base de datos de origen, obtendrá un error.  
  
- Si no se realiza la conexión a la base de datos de destino antes de ejecutar este comando o se produce un error en la conexión con el servidor de base de datos de destino durante la ejecución del comando, se genera un error y se cierra la aplicación de consola.  
  
**Script**
  
- `object-name:` Especifica los objetos de destino que se tienen en cuenta para la sincronización con la base de datos de destino (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
- `object-type:` Especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
- `on-error:` Especifica si se deben especificar errores de sincronización como advertencias o errores. Opciones disponibles para en caso de error:  
  
    - Informe-total como ADVERTENCIA  
  
    - Informe cada ADVERTENCIA  
  
    - error: script  
  
- `report-errors-to:` Especifica la ubicación del informe de errores para la operación de sincronización (atributo opcional) si solo se especifica la ruta de acceso de la carpeta, se crea el archivo por nombre **TargetSynchronizationReport.XML** .  
  
**Ejemplo de sintaxis:**  
  
```xml  
<synchronize-target  
  
  object-name="ots_triggers.dbo"  
  
  on-error="<report-total-as-warning|  
  
             report-each-as-warning|  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name>"        (optional)  
  
/>  
```

or  
  
```xml  
<synchronize-target  
  
  object-name="ssma.dbo.Procedures"  
  
  object-type="category"/>  
```

or  
  
```xml  
<synchronize-target>  
  
  <metabase-object object-name="ssma.dbo.TT1"/>  
  
  <metabase-object object-name="ssma.dbo.TT2"/>  
  
  <metabase-object object-name="ssma.dbo.TT3"/>  
  
</synchronize-target>  
```

**Comando**
  
actualizar desde la base de datos  
  
- Actualiza los objetos de origen de la base de datos.  
  
- Si este comando se ejecuta en la base de datos de destino, se genera un error.  
  
**Script**
  
Requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
- `object-name:` Especifica los objetos de origen que se tienen en cuenta para actualizar desde la base de datos de origen (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
- `object-type:` Especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
- `on-error:` Especifica si se deben especificar errores de actualización como advertencias o errores. Opciones disponibles para en caso de error:  
  
    - Informe-total como ADVERTENCIA  
  
    - Informe cada ADVERTENCIA  
  
    - error: script  
  
- `report-errors-to:` Especifica la ubicación del informe de errores para la operación de actualización (atributo opcional) si solo se especifica la ruta de acceso de la carpeta, se crea el archivo por nombre **SourceDBRefreshReport.XML** .  
  
**Ejemplo de sintaxis:**  
  
```xml  
<refresh-from-database  
  
  object-name="ssma.TT1"  
  
  on-error="<report-total-as-warning|  
  
             report-each-as-warning|  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name>"        (optional)  
  
/>  
```

or  
  
```xml  
<refresh-from-database  
  
  object-name="ssma.Procedures"  
  
  object-type="category"/>  
```

or  
  
```xml  
<refresh-from-database>  
  
  <metabase-object object-name="ssma.TT_1"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-script-file-commands"></a>Comandos del archivo de script de generación de script

Los comandos de generación de script ayudan a guardar la salida de la consola en un archivo de script.  
  
**Comando**
  
guardar como script  
  
Se usa para guardar los scripts de los objetos en un archivo mencionado cuando metabase = Target, es una alternativa al comando de sincronización, en el que obtenemos los scripts y ejecutamos lo mismo en la base de datos de destino.  
  
**Script**
  
Requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
- `object-name:` Especifica los objetos cuyos scripts se van a guardar. (Puede tener nombres de objeto individuales o un nombre de objeto de grupo)  
  
- `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
- `metabase:` Especifica si se trata de la metabase de origen o de destino.  
  
- `destination:` Especifica la ruta de acceso o la carpeta donde se debe guardar el script. Si no se proporciona el nombre de archivo, un nombre de archivo con el formato (object_name valor de atributo). out  
  
- `overwrite:` Si es true, se sobrescribe si existen los mismos nombres de archivo. Puede tener los valores (true/false).  
  
**Ejemplo de sintaxis:**  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  object-name="ssma.dbo.Procedures"  
  
  object-type="category"  
  
  destination="<file/folder>"  
  
  overwrite="<true/false>"             (optional)  
  
/>  
```

or  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  destination="<file/folder>"  
  
    <metabase-object object-name="ssma.dbo.Procedures"  
  
                     object-type="category"/>  
  
</save-as-script>  
```  
  
## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre las opciones de línea de comandos, vea [Opciones de la línea de comandos en la consola de SSMA &#40;AccessToSQL&#41;](../../ssma/access/command-line-options-in-ssma-console-accesstosql.md) .  
  
Para obtener información sobre los archivos de script de la consola de ejemplo, vea [trabajar con el script de la consola de ejemplo FilesExecuting la consola de SSMA &#40;AccessToSQL&#41;](../../ssma/access/working-sample-console-script-filesexecuting-ssma-console-accesstosql.md)  
  
El paso siguiente depende de los requisitos del proyecto:  
  
- Para especificar una contraseña o exportar o importar contraseñas, consulte [Administración de contraseñas &#40;AccessToSQL&#41;](../../ssma/access/managing-passwords-accesstosql.md).  
  
- Para generar informes, consulte [generación de informes &#40;AccessToSQL&#41;](../../ssma/access/generating-reports-accesstosql.md).  
  
- Para solucionar problemas de la consola de, consulte solución de problemas de [&#40;AccessToSQL&#41;](../../ssma/access/troubleshooting-accesstosql.md).  

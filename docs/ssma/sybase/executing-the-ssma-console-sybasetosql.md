---
description: Ejecución de la consola de SSMA (SybaseToSQL)
title: Ejecución de la consola de SSMA (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Database Connection Commands
- Sybase Console,Manageability Commands
- Sybase Console,Migration Commands
- Sybase Console,Migration Preparation Command
- Sybase Console,Project Commands
- Sybase Console,Report Commands
- Sybase Console,Script File Commands
- Sybase Console,Script Generation Commands
ms.assetid: ea8950b7-fabc-4aa4-89f8-9573a2617d70
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a7eed4ea61ada5979dbecdb106e5954620ae9c81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079376"
---
# <a name="executing-the-ssma-console-sybasetosql"></a>Ejecución de la consola de SSMA (SybaseToSQL)
Microsoft proporciona un conjunto sólido de comandos de archivo de script para ejecutar y controlar las actividades de SSMA. Las secciones siguientes detallan el mismo.  
  
## <a name="script-file-commands"></a>Comandos de archivo de script  
La aplicación de consola usa determinados comandos de archivo de script estándar que se enumeran en esta sección.  
  
## <a name="project-commands"></a>Comandos del proyecto  
Los comandos del proyecto controlan la creación de proyectos, la apertura, el guardado y el cierre de proyectos.  
  
### <a name="create-new-project"></a>crear-nuevo-proyecto  
Este comando crea un nuevo proyecto de SSMA.  
  
-   `project-folder` indica la carpeta del proyecto que se va a crear.  
  
-   `project-name` indica el nombre del proyecto. {string}  
  
-   `overwrite-if-exists`Atributo opcional indica si se debe sobrescribir un proyecto existente. booleano  
  
-   `project-type:`Atributo opcional. Indica el tipo de proyecto, es decir, proyecto "SQL Server-2005" o proyecto "SQL-Server-2008" o proyecto "SQL-Server-2012" o proyecto "SQL-Server-2014" o proyecto "SQL-Azure". El valor predeterminado es "SQL-Server-2008".  
  
**Ejemplo de sintaxis:**  
  
```xml  
<create-new-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
  overwrite-if-exists="<true/false>" (optional)  
  
   project-type="<sql-server-2008/sql-server-2005/sql-server-2012/sql-server-2014/sql-azure>"  
/>  
```  
El atributo ' overwrite-if-exists ' es **false** de forma predeterminada.  
  
El atributo ' Project-type ' es **SQL-Server-2008** de forma predeterminada.  
  
### <a name="open-project"></a>proyecto abierto  
Este comando abre el proyecto.

-   `project-folder` indica la carpeta del proyecto que se va a crear. El comando genera un error si la carpeta especificada no existe.  {string}  
  
-   `project-name` indica el nombre del proyecto. El comando genera un error si el proyecto especificado no existe.  {string}  
  
**Ejemplo de sintaxis:**  
  
```xml  
<open-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
/>  
```  
> [!NOTE]  
> La aplicación de consola SSMA for SAP ASE admite la compatibilidad con versiones anteriores. Puede utilizarlo para abrir proyectos creados por la versión anterior de SSMA.  
  
### <a name="save-project"></a>save-project  
Este comando guarda el proyecto de migración.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<save-project/>  
```  
  
### <a name="close-project"></a>cerrar proyecto  
Este comando cierra el proyecto de migración.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<close-project   
  if-modified="<save/error/ignore>"   (optional)  
/>  
```  
El atributo ' If-Modified ' es opcional y se **omite** de forma predeterminada.  
  
## <a name="database-connection-commands"></a>Comandos de conexión de base de datos  
Los comandos de conexión de base de datos ayudan a conectarse a la base de datos.  
  
> [!NOTE]  
> - La característica de **exploración** de la interfaz de usuario no se admite en la consola de.  
> - Para obtener más información sobre cómo crear archivos de scripts, vea [crear archivos de script &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-script-files-sybasetosql.md).  
  
### <a name="connect-source-database"></a>Connect-Source-Database  
Este comando realiza la conexión a la base de datos de origen y carga los metadatos de alto nivel de la base de datos de origen, pero no todos los metadatos.
  
Si no se puede establecer la conexión con el origen, se genera un error y la aplicación de consola detiene la ejecución.
  
La definición del servidor se recupera del atributo name definido para cada conexión en la sección Server del archivo de conexión del servidor o del archivo de script.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
  
### <a name="force-load-sourcetarget-database"></a>forzar carga-origen/destino-base de datos  
Este comando carga los metadatos de origen y resulta útil para trabajar en el proyecto de migración sin conexión.  
  
Si no se puede establecer la conexión con el origen o el destino, se genera un error y la aplicación de consola detiene la ejecución.  
  
Este comando requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<force-load metabase="<source/target>" >  
  
  <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
  
### <a name="reconnect-source-database"></a>reconnect-Source-Database  
Este comando vuelve a conectarse a la base de datos de origen, pero no carga ningún metadato a diferencia del comando Connect-Source-Database.  
  
Si no se puede establecer la conexión con el origen, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```  
  
### <a name="connect-target-database"></a>Connect-Target-Database  
Este comando conecta con la base de datos de SQL Server de destino y carga los metadatos de alto nivel de la base de datos de destino, pero no los metadatos por completo.  
  
Si no se puede establecer la conexión con el destino, se genera un error y la aplicación de consola detiene la ejecución.  
  
La definición del servidor se recupera del atributo name definido para cada conexión en la sección Server del archivo de conexión del servidor o del archivo de script.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  
  
### <a name="reconnect-target-database"></a>reconnect-Target-Database  
  
Este comando vuelve a conectarse a la base de datos de destino, pero no carga ningún metadato, a diferencia del comando Connect-Target-Database.  
  
Si no se puede establecer la conexión con el destino, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  
  
## <a name="report-commands"></a>Comandos de informe  
Los comandos de informe generan informes sobre el rendimiento de las diversas actividades de la consola de SSMA.  
  
### <a name="generate-assessment-report"></a>generar informe de evaluación  
  
Este comando genera informes de evaluación en la base de datos de origen.  
  
Si no se realiza la conexión a la base de datos de origen antes de ejecutar este comando, se genera un error y se cierra la aplicación de consola.  
  
No se puede conectar con el servidor de base de datos de origen durante la ejecución del comando, lo que da lugar a la finalización de la aplicación de consola.  
  
-   `conversion-report-folder:` Especifica la carpeta en la que se puede almacenar el informe de evaluación. (atributo opcional)  
  
-   `object-name:` Especifica los objetos que se han tenido en cuenta para la generación de informes de evaluación (admite nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` Especifica el tipo del objeto al que se ha llamado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "Category").  
  
-   `conversion-report-overwrite:` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
-   `write-summary-report-to:` Especifica la ruta de acceso en la que se generará el informe.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **AssessmentReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    -   `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    -   `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<generate-assessment-report  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  write-summary-report-to="<file-name/folder-name>"             (optional)  
  
  verbose="<true/false>"                       (optional)  
  
  report-errors="<true/false>"                 (optional)  
  
  assessment-report-folder="<folder-name>"          (optional)  
  
  conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
or  
  
```xml  
<generate-assessment-report  
  
  assessment-report-folder="<folder-name>"            (optional)  
  
  conversion-report-overwrite="<true/false>"     (optional)  
  
>  
<metabase-object object-name="<object-name>"  
  
        object-type="<object-category>"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-commands"></a>Comandos de migración  
Los comandos de migración convierten el esquema de la base de datos de destino en el esquema de origen y migran los datos al servidor de destino.  
  
### <a name="convert-schema"></a>Convert-Schema  
Este comando realiza la conversión del esquema del origen al esquema de destino.  
  
Si no se realiza la conexión a la base de datos de origen o de destino antes de ejecutar este comando o se produce un error en la conexión con el servidor de origen o de destino durante la ejecución del comando, se genera un error y se cierra la aplicación de consola.  
  
-   `conversion-report-folder:` Especifica la carpeta en la que se puede almacenar el informe de evaluación. (atributo opcional)  
  
-   `object-name:` Especifica los objetos de origen que se han tenido en cuenta para convertir el esquema (admite nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` Especifica el tipo del objeto al que se ha llamado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "Category").  
  
-   `conversion-report-overwrite:` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
-   `write-summary-report-to:` Especifica la ruta de acceso en la que se generará el informe de resumen.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **SchemaConversionReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    -   `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    -   `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<convert-schema  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  write-summary-report-to="<file-name/folder-name>"     (optional)  
  
  verbose="<true/false>"                          (optional)  
  
  report-errors="<true/false>"                    (optional)  
  
  conversion-report-folder="<folder-name>"             (optional)  
  
  conversion-report-overwrite="<true/false>"      (optional)  
  
/>  
```  
or  
  
```xml  
<convert-schema  
  
  conversion-report-folder="<folder-name>"         (optional)  
  
  conversion-report-overwrite="<true/false>"> (optional)  
  
  <metabase-object object-name="<object-name>"  
  
    object-type="<object-category>"/>  
  
</convert-schema>  
```  
  
### <a name="migrate-data"></a>migrar datos  
Este comando migra los datos de origen al destino.  
  
-   `object-name:` Especifica los objetos de origen que se han tenido en cuenta para migrar datos (admite nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` especifica el tipo del objeto al que se ha llamado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "Category").  
  
-   `write-summary-report-to:` Especifica la ruta de acceso en la que se generará el informe.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **DataMigrationReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    -   `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    -   `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<migrate-data  
  
  write-summary-report-to="<file-name/folder-name>"  
  
  report-errors="<true/false>" verbose="<true/false>">  
  
    <metabase-object object-name="<object-name>"/>  
  
    <metabase-object object-name="<object-name>"/>  
  
    <metabase-object object-name="<object-name>"/>  
  
    <data-migration-connection  
  
      source-use-last-used="true"/source-server="<server-unique-name>"  
  
      target-use-last-used="true"/target-server="<server-unique-name>"/>  
  
</migrate-data>  
```  
or  
  
```xml  
<migrate-data  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  write-summary-report-to="<file-name/folder-name>"  
  
  report-errors="<true/false>" verbose="<true/false>"/>  
```  
  
## <a name="migration-preparation-command"></a>Comando de preparación de la migración  
El comando de preparación de la migración inicia la asignación de esquemas entre las bases de datos de origen y de destino.  
  
> [!NOTE]  
> La configuración predeterminada de salida de la consola para los comandos de migración es el informe de salida ' completo ' sin informes de errores detallados: solo Resumen en el nodo raíz del árbol de objetos de origen.  
  
### <a name="map-schema"></a>Map-Schema  
Este comando proporciona la asignación de esquema de la base de datos de origen al esquema de destino.  
  
-   `source-schema` Especifica el esquema de origen que se va a migrar.  
  
-   `sql-server-schema` Especifica el esquema de destino al que se migrará el esquema de origen.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<map-schema source-schema="<source-schema>"  
  
sql-server-schema="<target-schema>"/>  
```  
  
## <a name="manageability-commands"></a>Comandos de administración  
Los comandos de administración ayudan a sincronizar los objetos de base de datos de destino con la base de datos de origen.  
  
> [!NOTE]  
> La configuración predeterminada de salida de la consola para los comandos de migración es el informe de salida ' completo ' sin informes de errores detallados: solo Resumen en el nodo raíz del árbol de objetos de origen.  
  
### <a name="synchronize-target"></a>sincronizar-destino  
Este comando sincroniza los objetos de destino con la base de datos de destino.  
 
Si este comando se ejecuta en la base de datos de origen, se produce un error.  
  
Si no se realiza la conexión a la base de datos de destino antes de ejecutar este comando o se produce un error en la conexión con el servidor de base de datos de destino durante la ejecución del comando, se genera un error y se cierra la aplicación de consola.  
  
-   `object-name:` Especifica los objetos de destino que se tienen en cuenta para la sincronización con la base de datos de destino (admite nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` Especifica el tipo del objeto al que se ha llamado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "Category").  
  
-   `on-error:` Especifica si se deben especificar errores de sincronización como advertencias o errores. Opciones disponibles para en caso de error:  
  
    -   Informe-total como ADVERTENCIA  
  
    -   Informe cada ADVERTENCIA  
  
    -   error: script  
  
-   `report-errors-to:` Especifica la ubicación del informe de errores para la operación de sincronización (atributo opcional). Si solo se especifica la ruta de acceso de la carpeta, se crea el archivo por nombre **TargetSynchronizationReport.XML** .  
  
**Ejemplo de sintaxis:**  
  
```xml  
<synchronize-target  
  
object-name="<object-name>"  
  
on-error="<report-total-as-warning/  
  
report-each-as-warning/  
  
fail-script>" (optional)  
  
  report-errors-to="<file-name/folder-name>"        (optional)  
  
/>  
```  
or  
  
```xml  
<synchronize-target  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"/>  
```  
or  
  
```xml  
<synchronize-target>  
  
  <metabase-object object-name="<object-name>"/>  
  
  <metabase-object object-name="<object-name>"/>  
  
  <metabase-object object-name="<object-name>"/>  
  
</synchronize-target>  
```  
  
### <a name="refresh-from-database"></a>actualizar desde la base de datos  
Este comando actualiza los objetos de origen de la base de datos.  
  
Si este comando se ejecuta en la base de datos de destino, se genera un error.  
  
Este comando requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
-   `object-name:` Especifica los objetos de origen que se tienen en cuenta para actualizar desde la base de datos de origen (admite nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` Especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
-   `on-error:` Especifica si se va a llamar a los errores de actualización como advertencias o errores. Opciones disponibles para en caso de error:  
  
    -   Informe-total como ADVERTENCIA  
  
    -   Informe cada ADVERTENCIA  
  
    -   error: script  
  
-   `report-errors-to:` Especifica la ubicación del informe de errores para la operación de actualización (atributo opcional). Si solo se especifica la ruta de acceso de la carpeta, se crea el archivo por nombre **SourceDBRefreshReport.XML** .  
  
**Ejemplo de sintaxis:**  
  
```xml  
<refresh-from-database  
  
  object-name="<object-name>"  
  
  on-error="<report-total-as-warning/  
  
             report-each-as-warning/  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name/folder-name>"        (optional)  
  
/>  
```  
or  
  
```xml  
<refresh-from-database  
  
  object-name="<object-name>"  
  
  object-type="<object-category>" />  
```  
or  
  
```xml  
<refresh-from-database>  
  
  <metabase-object object-name="<object-name>"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-commands"></a>Comandos de generación de scripts  
Los comandos de generación de script realizan tareas dobles: ayudan a guardar la salida de la consola en un archivo de script y registran la salida de T-SQL en la consola o en un archivo basado en el parámetro que se especifique.  
  
### <a name="save-as-script"></a>guardar como script  
Este comando se usa para guardar los scripts de los objetos en un archivo mencionado cuando metabase = Target. Se trata de una alternativa al comando de sincronización en que obtenemos los scripts y ejecutamos lo mismo en la base de datos de destino.  
  
Este comando requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
-   `object-name:` Especifica los objetos cuyos scripts se van a guardar (admite nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` Especifica el tipo del objeto al que se ha llamado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "Category").  
  
-   `metabase:` Especifica si se trata de la metabase de origen o de destino.  
  
-   `destination:` Especifica la ruta de acceso o la carpeta en la que se debe guardar el script. Si no se especifica el nombre de archivo, se proporcionará un nombre de archivo con el formato (object_name valor de atributo). out.
  
-   `overwrite:` Si es true, sobrescribe el mismo nombre de archivo si existe. Puede tener los valores (true/false).  
  
**Ejemplo de sintaxis:**  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  destination="<file-name/folder-name>"  
  
  overwrite="<true/false>"   (optional)  
  
/>  
```  
or  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  destination="<file-name/folder-name>"  
  
    <metabase-object object-name="<object-name>"  
  
                     object-type="<object-category>"/>  
  
</save-as-script>  
```  
  
### <a name="convert-sql-statement"></a>Convert-SQL-Statement
Este comando convierte la instrucción SQL.  
  
-   `context` Especifica el nombre del esquema.  
  
-   `destination` Especifica si la salida se debe almacenar en un archivo.  
  
    Si no se especifica este atributo, se muestra la instrucción T-SQL convertida en la consola. (atributo opcional)  
  
-   `conversion-report-folder` Especifica la carpeta en la que se puede almacenar el informe de evaluación. (atributo opcional)  
  
-   `conversion-report-overwrite` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
-   `write-converted-sql-to` Especifica la ruta de acceso de la carpeta del archivo (o) en la que se debe almacenar el T-SQL convertido. Cuando se especifica una ruta de acceso de carpeta junto con el `sql-files` atributo, cada archivo de código fuente tiene un archivo T-SQL de destino correspondiente creado en la carpeta especificada. Cuando se especifica una ruta de acceso de carpeta junto con el `sql` atributo, el T-SQL convertido se escribe en un archivo denominado result. out en la carpeta especificada.  
  
-   `sql` especifica las instrucciones SQL de Sybase que se van a convertir; se pueden separar una o varias instrucciones mediante un ";"  
  
-   `sql-files` Especifica la ruta de acceso de los archivos SQL que se deben convertir al código T-SQL.  
  
-   `write-summary-report-to` Especifica la ruta de acceso donde se generará el informe de resumen. Si solo se menciona la ruta de acceso de la carpeta, se crea el archivo por nombre **ConvertSQLReport.XML** . (atributo opcional)  
  
    La creación de informes de resumen tiene dos subcategorías más, es decir:  
  
    -   Informe: errores (= "true/false", con el valor predeterminado "false" (atributos opcionales)).  
  
    -   verbose (= "true/false", con el valor predeterminado "false" (atributos opcionales)).  
  
Este comando requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
**Ejemplo de sintaxis:**  
  
```  
<convert-sql-statement  
  
       context="<database-name>.<schema-name>"  
  
        conversion-report-folder="<folder-name>"  
  
        conversion-report-overwrite="<true/false>"  
  
        write-summary-report-to="<file-name/folder-name>"   (optional)  
  
        verbose="<true/false>"   (optional)  
  
        report-errors="<true/false>"   (optional)  
  
        destination="<stdout/file>"   (optional)  
  
        write-converted-sql-to ="<file-name/folder-name>"  
  
        sql="SELECT 1 FROM DUAL;">  
  
    <output-window suppress-messages="<true/false>" />  
  
</convert-sql-statement>  
```  
or  
  
```  
<convert-sql-statement  
  
         context="<database-name>.<schema-name>"  
  
         conversion-report-folder="<folder-name>"  
  
         conversion-report-overwrite="<true/false>"  
  
         write-summary-report-to="<file-name/folder-name>"   (optional)  
  
         verbose="<true/false>"   (optional)  
  
         report-errors="<true/false>"   (optional)  
  
         destination="<stdout/file>"   (optional)  
  
         write-converted-sql-to ="<file-name/folder-name>"  
  
         sql-files="<folder-name>\*.sql"  
  
/>  
```  
or  
  
```  
<convert-sql-statement  
  
         context="<database-name>.<schema-name>"  
  
         conversion-report-folder="<folder-name>"  
  
         conversion-report-overwrite="<true/false>"  
  
         sql-files="<folder-name>\*.sql"  
  
/>  
```  
  
## <a name="next-steps"></a>Pasos siguientes  
Para obtener información sobre las opciones de línea de comandos, vea [Opciones de la línea de comandos en la consola de SSMA (AccessToSQL)](../access/command-line-options-in-ssma-console-accesstosql.md).  
  
Para obtener información sobre un archivo de script de consola de ejemplo, vea [trabajar con los archivos de script de la consola de ejemplo &#40;SybaseToSQL&#41;](../../ssma/sybase/working-with-the-sample-console-script-files-sybasetosql.md)  
  
El paso siguiente depende de los requisitos del proyecto:  
  
-   Para especificar una contraseña o exportar o importar contraseñas, consulte [Administración de contraseñas &#40;SybaseToSQL&#41;](../../ssma/sybase/managing-passwords-sybasetosql.md).  
  
-   Para generar informes, consulte [generación de informes &#40;SybaseToSQL&#41;](../../ssma/sybase/generating-reports-sybasetosql.md).  
  
-   Para solucionar problemas de la consola de, consulte solución de problemas de [&#40;SybaseToSQL&#41;](../../ssma/sybase/troubleshooting-sybasetosql.md).  
  

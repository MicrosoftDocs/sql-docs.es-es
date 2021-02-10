---
description: Ejecución de la consola de SSMA (OracleToSQL)
title: Ejecución de la consola de SSMA (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Oracle SSMA Console
- Script File Commands, Script Generation Commands,Manageability Commands
- Script File Commands,Project Commands
ms.assetid: 7228ccba-c69f-4b4c-8664-01a2750183c5
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 97bbd0ae40fff78df4142437d32456f3c542f8e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044425"
---
# <a name="executing-the-ssma-console-oracletosql"></a>Ejecución de la consola de SSMA (OracleToSQL)
Microsoft proporciona un conjunto sólido de comandos de archivo de script para ejecutar y controlar las actividades de SSMA. La aplicación de consola usa determinados comandos de archivo de script estándar que se enumeran en esta sección.  
  
## <a name="project-script-file-commands"></a>Comandos de archivo de script de proyecto  
Los comandos del proyecto controlan la creación de proyectos, la apertura, el guardado y el cierre de proyectos.  
  
**Comando**  
  
crear-nuevo-proyecto  
                  : Crea un nuevo proyecto de SSMA.  
  
**Script**  
  
-   `project-folder` indica la carpeta del proyecto que se va a crear.  
  
-   `project-name` indica el nombre del proyecto. {string}  
  
-   `overwrite-if-exists`Atributo opcional indica si se debe sobrescribir un proyecto existente. booleano  
  
-   `project-type:`Atributo opcional. Indica el tipo de proyecto, es decir, el proyecto "SQL-Server-2005" o el proyecto "SQL-Server-2008" o el proyecto "SQL-Server-2012" o "SQL-Server-2014" o "SQL-Azure". El valor predeterminado es "SQL-Server-2014".  
  
**Ejemplo**:  
  
```xml  
<create-new-project  
  
   project-folder="<project-folder>"  
  
   project-name="<project-name>"  
  
   overwrite-if-exists="<true/false>"   (optional)  
  
   project-type="<sql-server-2008 | sql-server-2005 | sql-server-2012 | sql-server-2014>"   (optional)  
  
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
SSMA para la aplicación de consola de Oracle admite la compatibilidad con versiones anteriores. Podrá abrir proyectos creados por la versión anterior de SSMA.  
  
**Comando**  
  
save-project  
  
Guarda el proyecto de migración.  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<save-project/>  
```  
**Comando**  
  
cerrar proyecto  
  
Cierra el proyecto de migración.  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<close-project  
  
   if-modified="<save/error/ignore>"   (optional)  
  
/>  
```  
  
## <a name="database-connection-script-file-commands"></a>Comandos de archivo de script de conexión de base de datos  
Los comandos de conexión de base de datos ayudan a conectarse a la base de datos.  
  
-   La característica de **exploración** de la interfaz de usuario no se admite en la consola de.  
  
-   Para obtener más información sobre cómo crear archivos de scripts, vea [crear archivos de script &#40;OracleToSQL&#41;](../../ssma/oracle/creating-script-files-oracletosql.md).  
  
**Comando**  
  
Connect-Source-Database  
  
-   Realiza la conexión a la base de datos de origen y carga los metadatos de alto nivel de la base de datos de origen, pero no todos los metadatos.  
  
-   Si no se puede establecer la conexión con el origen, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**  
  
La definición del servidor se recupera del atributo name definido para cada conexión en la sección Server del archivo de conexión del servidor o del archivo de script.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
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
<force-load object-name="<object-name>"  
  
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
  
-   Vuelve a conectar con la base de datos de origen, pero no carga ningún metadato a diferencia del comando Connect-Source-Database.  
  
-   Si no se puede establecer la conexión con el origen, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```  
**Comando**  
  
Connect-Target-Database  
  
-   Se conecta al SQL Server la base de datos de destino y carga los metadatos de alto nivel de la base de datos de destino, pero no los metadatos por completo.  
  
-   Si no se puede establecer la conexión con el destino, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**  
  
La definición del servidor se recupera del atributo name definido para cada conexión en la sección Server del archivo de conexión del servidor o del archivo de script.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  
**Comando**  
  
reconnect-Target-Database  
  
-   Vuelve a conectar con la base de datos de destino, pero no carga ningún metadato, a diferencia del comando Connect-Target-Database.  
  
-   Si no se puede establecer la conexión con el destino, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  
  
## <a name="report-script-file--commands"></a>Comandos de archivo de script de informe  
Los comandos de informe generan informes sobre el rendimiento de las diversas actividades de la consola de SSMA.  
  
**Comando**  
  
generar informe de evaluación  
  
-   Genera informes de evaluación en la base de datos de origen.  
  
-   Si no se realiza la conexión a la base de datos de origen antes de ejecutar este comando, se genera un error y se cierra la aplicación de consola.  
  
-   No se puede conectar con el servidor de base de datos de origen durante la ejecución del comando, lo que da lugar a la finalización de la aplicación de consola.  
  
**Script**  
  
-   `conversion-report-folder:` Especifica la carpeta donde se puede almacenar el informe de evaluación. (atributo opcional)  
  
-   `object-name:` Especifica los objetos que se han tenido en cuenta para la generación de informes de evaluación (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
-   `conversion-report-overwrite:` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
-   `write-summary-report-to:` Especifica la ruta de acceso donde se generará el informe de resumen.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **AssessmentReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    -   `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    -   `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<generate-assessment-report  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   write-summary-report-to="<file>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   assessment-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
or  
  
```xml  
<generate-assessment-report  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
>  
  
      <metabase-object object-name="<object-name>"  
  
         object-type="<object-category>"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-script-file-commands"></a>Comandos del archivo de script de migración  
Los comandos de migración convierten el esquema de la base de datos de destino en el esquema de origen y migran los datos al servidor de destino.  
  
La configuración predeterminada de salida de la consola para los comandos de migración es el informe de salida ' completo ' sin informes de errores detallados: solo Resumen en el nodo raíz del árbol de objetos de origen.  
  
**Comando**  
  
Convert-Schema  
  
-   Realiza la conversión del esquema del origen al esquema de destino.  
  
-   Si no se realiza la conexión a la base de datos de origen o de destino antes de ejecutar este comando o se produce un error en la conexión con el servidor de origen o de destino durante la ejecución del comando, se genera un error y se cierra la aplicación de consola.  
  
**Script**  
  
-   `conversion-report-folder:` Especifica la carpeta donde se puede almacenar el informe de evaluación. (atributo opcional)  
  
-   `object-name:` Especifica los objetos de origen que se han tenido en cuenta para convertir el esquema (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
-   `conversion-report-overwrite:` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
-   `write-summary-report-to:` Especifica la ruta de acceso donde se generará el informe de resumen.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **SchemaConversionReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    -   `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    -   `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<convert-schema  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
or  
  
```xml  
<convert-schema  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
      <metabase-object object-name="<object-name>"  
  
         object-type="<object-category>"/>  
  
</convert-schema>  
```  
**Comando**  
  
migrar datos  
  
Migra los datos de origen al destino.  
  
**Script**  
  
-   `conversion-report-folder:` Especifica la carpeta donde se puede almacenar el informe de evaluación. (atributo opcional)  
  
-   `object-name:` Especifica los objetos de origen que se han tenido en cuenta para migrar datos (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
-   `conversion-report-overwrite:` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
-   `write-summary-report-to:` Especifica la ruta de acceso donde se generará el informe de resumen.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **DataMigrationReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    -   `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    -   `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<migrate-data  
  
   write-summary-report-to="<file-name/folder-name>"  
  
   report-errors="<true/false>"  
  
   verbose="<true/false>">  
  
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
  
   report-errors="<true/false>"  
  
   verbose="<true/false>"/>  
```  
  
## <a name="migration-preparation-script-file-commands"></a>Comandos del archivo de script de preparación de la migración  
El comando de preparación de la migración inicia la asignación de esquemas entre las bases de datos de origen y de destino.  
  
**Comando**  
  
Map-Schema  
  
Asignación de esquema de la base de datos de origen al esquema de destino.  
  
Migra los datos de origen al destino.  
  
**Script**  
  
-   `source-schema` especifica el esquema de origen que se va a migrar.  
  
-   `sql-server-schema` especifica el esquema de destino en el que se desea migrar.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<map-schema  
  
   source-schema="<source-schema>"  
  
   sql-server-schema="<target-schema>"/>  
```  
  
## <a name="manageability-script-file-commands"></a>Comandos del archivo de script de administración  
Los comandos de administración ayudan a sincronizar los objetos de base de datos de destino con la base de datos de origen. La configuración predeterminada de salida de la consola para los comandos de migración es el informe de salida ' completo ' sin informes de errores detallados: solo Resumen en el nodo raíz del árbol de objetos de origen.  
  
**Comando**  
  
sincronizar-destino  
  
-   Sincroniza los objetos de destino con la base de datos de destino.  
  
-   Si este comando se ejecuta en la base de datos de origen, se produce un error.  
  
-   Si no se realiza la conexión a la base de datos de destino antes de ejecutar este comando o se produce un error en la conexión con el servidor de base de datos de destino durante la ejecución del comando, se genera un error y se cierra la aplicación de consola.  
  
**Script**  
  
-   `object-name:` Especifica los objetos de destino que se tienen en cuenta para la sincronización con la base de datos de destino (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
-   `on-error:` Especifica si se deben especificar errores de sincronización como advertencias o errores. Opciones disponibles para en caso de error:  
  
    -   Informe-total como ADVERTENCIA  
  
    -   Informe cada ADVERTENCIA  
  
    -   error: script  
  
-   `report-errors-to:` Especifica la ubicación del informe de errores para la operación de sincronización (atributo opcional) si solo se especifica la ruta de acceso de la carpeta, se crea el archivo por nombre **TargetSynchronizationReport.XML** .  
  
**Ejemplo de sintaxis:**  
  
```xml  
<synchronize-target  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/  
  
               report-each-as-warning/  
  
               fail-script>"   (optional)  
  
   report-errors-to="<file-name/folder-name>"   (optional)  
  
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
**Comando**  
  
actualizar desde la base de datos  
  
-   Actualiza los objetos de origen de la base de datos.  
  
-   Si este comando se ejecuta en la base de datos de destino, se genera un error.  
  
**Script**  
  
Requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
-   `object-name:` Especifica los objetos de origen que se tienen en cuenta para actualizar desde la base de datos de origen (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
-   `object-type:` Especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
-   `on-error:` Especifica si se deben especificar errores de actualización como advertencias o errores. Opciones disponibles para en caso de error:  
  
    -   Informe-total como ADVERTENCIA  
  
    -   Informe cada ADVERTENCIA  
  
    -   error: script  
  
-   `report-errors-to:` Especifica la ubicación del informe de errores para la operación de actualización (atributo opcional) si solo se especifica la ruta de acceso de la carpeta, se crea el archivo por nombre **SourceDBRefreshReport.XML** .  
  
**Ejemplo de sintaxis:**  
  
```xml  
<refresh-from-database  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/  
  
               report-each-as-warning/  
  
               fail-script>"   (optional)  
  
   report-errors-to="<file-name/folder-name>"   (optional)  
  
/>  
```  
or  
  
```xml  
<refresh-from-database  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"/>  
```  
or  
  
```xml  
<refresh-from-database>  
  
   <metabase-object object-name="<object-name>"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-script-file-commands"></a>Comandos del archivo de script de generación de script  
Los comandos de generación de script realizan tareas dobles: ayudan a guardar la salida de la consola en un archivo de script. y registre la salida de T-SQL en la consola o en un archivo en función del parámetro que especifique.  
  
**Comando**  
  
guardar como script  
  
Se usa para guardar los scripts de los objetos en un archivo mencionado cuando metabase = Target, es una alternativa al comando de sincronización, en el que obtenemos los scripts y ejecutamos lo mismo en la base de datos de destino.  
  
**Script**  
  
Requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
-   `object-name:` Especifica los objetos cuyos scripts se van a guardar. (Puede tener nombres de objeto individuales o un nombre de objeto de grupo)  
  
-   `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
-   `metabase:` Especifica si se ha puesto en la metabase de origen o de destino.  
  
-   `destination:` Especifica la ruta de acceso o la carpeta en la que se debe guardar el script, si no se proporciona el nombre de archivo con el formato (object_name valor de atributo). out  
  
-   `overwrite:` Si es true, se sobrescribe si existe el mismo nombre de archivo. Puede tener los valores (true/false).  
  
**Ejemplo de sintaxis:**  
  
```xml  
<save-as-script  
  
   metabase="<source/target>"  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   destination="<file/folder>"  
  
   overwrite="<true/false>"   (optional)  
  
/>  
```  
or  
  
```xml  
<save-as-script  
  
   metabase="<source/target>"  
  
   destination="<file/folder>"  
  
      <metabase-object object-name="<object-name>"  
  
         object-type="<object-category>"/>  
  
</save-as-script>  
```  
**Comando**  
  
Convert-SQL-Statement  
  
-   `context` especifica el nombre del esquema.  
  
-   `destination` Especifica si la salida se debe almacenar en un archivo.  
  
    Si no se especifica este atributo, se muestra la instrucción T-SQL convertida en la consola. (atributo opcional)  
  
-   `conversion-report-folder` Especifica la carpeta donde se puede almacenar el informe de evaluación. (atributo opcional)  
  
-   `conversion-report-overwrite` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
-   `write-converted-sql-to` Especifica la ruta de acceso de la carpeta del archivo (o) donde se va a almacenar el T-SQL convertido. Cuando se especifica una ruta de acceso de carpeta junto con el `sql-files` atributo, cada archivo de código fuente tendrá un archivo T-SQL de destino correspondiente creado en la carpeta especificada. Cuando se especifica una ruta de acceso de carpeta junto con el `sql` atributo, el T-SQL convertido se escribe en un archivo denominado **result. out** en la carpeta especificada.  
  
-   `sql` especifica las instrucciones SQL de Oracle que se van a convertir; se pueden separar una o varias instrucciones usando un ";"  
  
-   `sql-files` Especifica la ruta de acceso de los archivos SQL que se deben convertir al código T-SQL.  
  
-   `write-summary-report-to` Especifica la ruta de acceso donde se generará el informe. Si solo se menciona la ruta de acceso de la carpeta, se crea el archivo por nombre **ConvertSQLReport.XML** . (atributo opcional)  
  
    La creación de informes tiene 2 subcategorías más, es decir:  
  
    -   Informe: errores (= "true/false", con el valor predeterminado "false" (atributos opcionales)).  
  
    -   verbose (= "true/false", con el valor predeterminado "false" (atributos opcionales)).  
  
**Script**  
  
Requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
**Ejemplo de sintaxis:**  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   destination="<stdout/file>"   (optional)  
  
   file-name="<file-name>"  
  
   sql="SELECT 1 FROM DUAL;">  
  
   <output-window suppress-messages="<true/false>" />  
  
</convert-sql-statement>  
```  
or  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   write-summary-report-to="<file-name/folder-name>" (optional)  
  
   verbose="<true/false>" (optional)  
  
   report-errors="<true/false>"  
  
   destination="<stdout/file>"   (optional)  
  
   write-converted-sql-to="<file-name/folder-name>"  
  
   sql-files="<folder-name>\*.sql" />  
```  
or  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   sql-files="<folder-name>\*.sql" />  
```  
  
## <a name="next-step"></a>siguiente paso  
Para obtener información sobre las opciones de línea de comandos, vea [Opciones de la línea de comandos en la consola de SSMA &#40;OracleToSQL&#41;](../../ssma/oracle/command-line-options-in-ssma-console-oracletosql.md) .  
  
Para obtener información sobre los archivos de script de la consola de ejemplo, vea [trabajar con los archivos de script de la consola de ejemplo &#40;OracleToSQL&#41;](../../ssma/oracle/working-with-the-sample-console-script-files-oracletosql.md)  
  
El paso siguiente depende de los requisitos del proyecto:  
  
-   Para especificar una contraseña o exportar o importar contraseñas, consulte [Administración de contraseñas &#40;OracleToSQL&#41;](../../ssma/oracle/managing-passwords-oracletosql.md).  
  
-   Para generar informes, consulte [generación de informes &#40;OracleToSQL&#41;](../../ssma/oracle/generating-reports-oracletosql.md).  
  
-   Para solucionar problemas de la consola de, consulte solución de problemas de [&#40;OracleToSQL&#41;](../../ssma/oracle/troubleshooting-oracletosql.md).  
  

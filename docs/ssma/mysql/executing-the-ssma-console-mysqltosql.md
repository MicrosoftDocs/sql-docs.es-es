---
description: Ejecución de la consola de SSMA (MySQLToSQL)
title: Ejecución de la consola de SSMA (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Script file commands, Database connection commands
- Script file commands, Manageability commands
- Script file commands, Migration commands
- Script file commands, Migration preparation commands
- Script file commands, project commands
- Script file commands, Report commands
- Script file commands, Script generation commands
ms.assetid: e3e9f7e4-0619-4861-a202-3d5d39953b26
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9f9fa74533d7887f968d45dde715b191ac00be88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015184"
---
# <a name="executing-the-ssma-console-mysqltosql"></a>Ejecución de la consola de SSMA (MySQLToSQL)
Microsoft proporciona un conjunto sólido de comandos de archivo de script para ejecutar y controlar las actividades de SSMA.  
  
La aplicación de consola usa determinados comandos de archivo de script estándar que se enumeran en esta sección.  
  
## <a name="project--script-file-commands"></a>Comandos de archivo de script de proyecto  
**Comando**  
  
crear nuevo proyecto:   
                   Crea un nuevo proyecto de SSMA.  
  
Los comandos del proyecto controlan la creación de proyectos, la apertura, el guardado y el cierre de proyectos.  
  
**Script**  
  
1.  `project-folder` indica la carpeta del proyecto que se va a crear.  
  
2.  `project-name` indica el nombre del proyecto. {string}  
  
3.  `overwrite-if-exists`Atributo opcional indica si se debe sobrescribir un proyecto existente. booleano  
  
4.  `project-type:`Atributo opcional. Indica el tipo de proyecto, es decir, el proyecto "SQL-Server-2005" o el proyecto "SQL-Server-2008" o el proyecto "SQL-Server-2012" o "SQL-Server-2014" o el proyecto "SQL-Azure". El valor predeterminado es "SQL-Server-2008".  
  
**Ejemplo de sintaxis:**  
  
```xml  
<create-new-project  
  
   project-folder="<project-folder>"  
  
   project-name="<project-name>"  
  
   overwrite-if-exists="<true/false>"   (optional)  
  
   project-type=="<sql-server-2008 | sql-server-2005 | sql-server-2012 | sql-server-2014 | sql-azure>"   (optional)  
  
/>  
```  
El atributo ' overwrite-if-exists ' es **false** de forma predeterminada.  
  
El atributo ' Project-type ' es **SQL-Server-2008** de forma predeterminada.  
  
**Comando**  
  
proyecto abierto:   
                  Abre un proyecto existente.  
  
**Script**  
  
1.  `project-folder` indica la carpeta del proyecto que se va a crear. El comando genera un error si la carpeta especificada no existe.  {string}  
  
2.  `project-name` indica el nombre del proyecto. El comando genera un error si el proyecto especificado no existe.  {string}  
  
**Ejemplo de sintaxis:**  
  
```xml  
<open-project  
  
   project-folder="<project-folder>"  
  
   project-name="<project-name>"  
  
/>  
```  
> [!IMPORTANT]  
> La aplicación de consola SSMA para MySQL admite la compatibilidad con versiones anteriores. Podrá abrir proyectos creados por la versión anterior de SSMA.  
  
**Comando**  
  
guardar-proyecto: guarda el proyecto de migración.  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<save-project/>  
```  
**Comando**  
  
cerrar proyecto  
                  : Cierra el proyecto de migración.  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<save-project/>  
```  
**Comando**  
  
cerrar proyecto  
                  : Cierra el proyecto de migración.  
  
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
  
1.  La característica de **exploración** de la interfaz de usuario no se admite en la consola de.  
  
2.  Los parámetros de **Puerto** y **autenticación de Windows** no son aplicables al conectarse a SQL Azure.  
  
3.  Para obtener más información sobre cómo crear archivos de scripts, vea [crear archivos de script &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-script-files-mysqltosql.md).  
  
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
<force-load metabase="<source/target>"  
  
      <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
**Comando**  
  
reconnect-Source-Database  
  
1.  Vuelve a conectar con la base de datos de origen, pero no carga ningún metadato a diferencia del comando Connect-Source-Database.  
  
2.  Si no se puede establecer la conexión con el origen, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```  
**Comando**  
  
Connect-Target-Database  
  
1.  Se conecta al SQL Server de destino o Azure SQL Database y carga los metadatos de alto nivel de la base de datos de destino, pero no los metadatos por completo.  
  
2.  Si no se puede establecer la conexión con el destino, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**  
  
La definición del servidor se recupera del atributo name definido para cada conexión en la sección Server del archivo de conexión del servidor o del archivo de script.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  
**Comando**  
  
reconnect-Target-Database  
  
1.  Vuelve a conectar con la base de datos de destino, pero no carga ningún metadato, a diferencia del comando Connect-Target-Database.  
  
2.  Si no se puede establecer la conexión con el destino, se genera un error y la aplicación de consola detiene la ejecución.  
  
**Script**  
  
**Ejemplo de sintaxis:**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  
  
## <a name="report-script-file-commands"></a>Comandos de archivo de script de informe  
Los comandos de informe generan informes sobre el rendimiento de las diversas actividades de la consola de SSMA.  
  
**Comando**  
  
generar informe de evaluación  
  
1.  Genera informes de evaluación en la base de datos de origen.  
  
2.  Si no se realiza la conexión a la base de datos de origen antes de ejecutar este comando, se genera un error y se cierra la aplicación de consola.  
  
3.  No se puede conectar con el servidor de base de datos de origen durante la ejecución del comando, lo que da lugar a la finalización de la aplicación de consola.  
  
**Script**  
  
1.  `assessment-report-folder:` Especifica la carpeta donde se almacena el informe de evaluación. (atributo opcional)  
  
2.  `object-name:` Especifica los objetos que se han tenido en cuenta para la generación de informes de evaluación (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
3.  `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
4.  `assessment-report-overwrite:` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
5.  `write-summary-report-to:` Especifica la ruta de acceso donde se generará el informe de resumen.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **AssessmentReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    -   `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    -   `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<generate-assessment-report  
  
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
<generate-assessment-report  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
>  
  
      <metabase-object object-name="<object-name>"  
  
         object-type="<object-category>"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration--script-file-commands"></a>Comandos del archivo de script de migración  
Los comandos de migración convierten el esquema de la base de datos de destino en el esquema de origen y migran los datos al servidor de destino.  
  
La configuración predeterminada de salida de la consola para los comandos de migración es el informe de salida ' completo ' sin informes de errores detallados: solo Resumen en el nodo raíz del árbol de objetos de origen.  
  
**Comando**  
  
Convert-Schema  
  
1.  Realiza la conversión del esquema del origen al esquema de destino.  
  
2.  Si no se realiza la conexión a la base de datos de origen o de destino antes de ejecutar este comando o se produce un error en la conexión con el servidor de origen o de destino durante la ejecución del comando, se genera un error y se cierra la aplicación de consola.  
  
**Script**  
  
1.  `conversion-report-folder:` Especifica la carpeta donde se almacena el informe de evaluación. (atributo opcional)  
  
2.  `object-name:` Especifica los objetos que se tienen en cuenta para convertir el esquema (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
3.  `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
4.  `conversion-report-overwrite:` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
5.  `write-summary-report-to:` Especifica la ruta de acceso donde se generará el informe de resumen.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **SchemaConversionReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes de resumen tiene dos subcategorías más:  
  
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
  
      <metabase-object object-name="<object-names>"  
  
         object-type="<object-category>"/>  
  
</convert-schema>  
```  
**Comando**  
  
migrar datos  
  
1.  Migra los datos de origen al destino.  
  
**Script**  
  
1.  `object-name:` Especifica los objetos de origen que se han tenido en cuenta para migrar datos (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
2.  `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
3.  `write-summary-report-to:` Especifica la ruta de acceso donde se generará el informe de resumen.  
  
    Si solo se menciona la ruta de acceso de la carpeta, archivo por nombre **DataMigrationReport &lt; n &gt; .** Se crea XML. (atributo opcional)  
  
    La creación de informes tiene dos subcategorías más:  
  
    -   `report-errors` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
    -   `verbose` (= "true/false", con el valor predeterminado "false" (atributos opcionales))  
  
**Ejemplo de sintaxis:**  
  
```xml  
<migrate-data  
  
   write-summary-report-to="<file-name/folder-name>"  
  
   report-errors="true" verbose="true">  
  
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
  
   report-errors="true" verbose="true"/>  
```  
  
## <a name="migration-preparation-script-file-command"></a>Comando de archivo de script de preparación de la migración  
El comando de preparación de la migración inicia la asignación de esquemas entre las bases de datos de origen y de destino.  
  
**Comando**  
  
Map-Schema  
  
Asignación de esquema de la base de datos de origen al esquema de destino.  
  
**Script**  
  
1.  `source-schema` especifica el esquema de origen que se va a migrar.  
  
2.  `sql-server-schema` especifica el esquema de destino en el que se desea migrar.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<map-schema  
  
   source-schema="<source-schema>"  
  
   sql-server-schema="<target-schema>"/>  
```  
  
## <a name="manageability-script-file-commands"></a>Comandos del archivo de script de administración  
Los comandos de administración ayudan a sincronizar los objetos de base de datos de destino con la base de datos de origen.  
  
> [!NOTE]  
> La configuración predeterminada de salida de la consola para los comandos de migración es el informe de salida ' completo ' sin informes de errores detallados: solo Resumen en el nodo raíz del árbol de objetos de origen.  
  
**Comando**  
  
sincronizar-destino  
  
1.  Sincroniza los objetos de destino con la base de datos de destino.  
  
2.  Si este comando se ejecuta en la base de datos de origen, se produce un error.  
  
3.  Si no se realiza la conexión a la base de datos de destino antes de ejecutar este comando o se produce un error en la conexión con el servidor de base de datos de destino durante la ejecución del comando, se genera un error y se cierra la aplicación de consola.  
  
**Script**  
  
1.  `object-name:` Especifica los objetos que se tienen en cuenta para la sincronización con la base de datos de destino (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
2.  `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
3.  `on-error:` Especifica si se deben especificar errores de sincronización como advertencias o errores. Opciones disponibles para en caso de error:  
  
    -   Informe-total como ADVERTENCIA  
  
    -   Informe cada ADVERTENCIA  
  
    -   error: script  
  
4.  `report-errors-to:` Especifica la ubicación del informe de errores para la operación de sincronización (atributo opcional) si solo se especifica la ruta de acceso de la carpeta, se crea el archivo por nombre **TargetSynchronizationReport.XML** .  
  
**Ejemplo de sintaxis:**  
  
```xml  
<synchronize-target  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/  
  
               report-each-as-warning/  
  
               fail-script>"   (optional)  
  
   report-errors-to="<file-name>"   (optional)  
  
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
  
1.  Actualiza los objetos de origen de la base de datos.  
  
2.  Si este comando se ejecuta en la base de datos de destino, se genera un error.  
  
**Script**  
  
1.  `object-name:` Especifica los objetos de origen que se tienen en cuenta para actualizar desde la base de datos de origen (puede tener nombres de objeto individuales o un nombre de objeto de grupo).  
  
2.  `object-type:` Especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
3.  `on-error:` Especifica si se deben especificar errores de sincronización como advertencias o errores. Opciones disponibles para en caso de error:  
  
    -   Informe-total como ADVERTENCIA  
  
    -   Informe cada ADVERTENCIA  
  
    -   error: script  
  
4.  `report-errors-to:` Especifica la ubicación del informe de errores para la operación de sincronización (atributo opcional) si solo se especifica la ruta de acceso de la carpeta, se crea el archivo por nombre **SourceDBRefreshReport.XML** .  
  
Requiere uno o varios nodos de metabase como parámetro de línea de comandos.  
  
**Ejemplo de sintaxis:**  
  
```xml  
<refresh-from-database  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/  
  
               report-each-as-warning/  
  
               fail-script>"   (optional)  
  
   report-errors-to="<file-name>"   (optional)  
  
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
  
1.  `object-name:` Especifica los objetos cuyos scripts se van a guardar. (Puede tener nombres de objeto individuales o un nombre de objeto de grupo)  
  
2.  `object-type:` especifica el tipo del objeto especificado en el atributo de nombre de objeto (si se especifica la categoría de objeto, el tipo de objeto será "categoría").  
  
3.  `metabase:` Especifica si se trata de la metabase de origen o de destino.  
  
4.  `destination:` Especifica la ruta de acceso o la carpeta en la que se debe guardar el script, si no se proporciona el nombre de archivo con el formato (object_name valor de atributo). out  
  
5.  `overwrite:` Si es true, se sobrescribe si existe el mismo nombre de archivo. Puede tener los valores (true/false).  
  
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
**Comando**  
  
Convert-SQL-Statement  
  
1.  `context` especifica el nombre del esquema.  
  
2.  `destination` Especifica si la salida se debe almacenar en un archivo.  
  
    Si no se especifica este atributo, se muestra la instrucción T-SQL convertida en la consola. (atributo opcional)  
  
3.  `conversion-report-folder` Especifica la carpeta donde se almacena el informe de evaluación. (atributo opcional)  
  
4.  `conversion-report-overwrite` Especifica si se va a sobrescribir la carpeta de informes de evaluación si ya existe.  
  
    **Valor predeterminado:** false. (atributo opcional)  
  
5.  `write-converted-sql-to` Especifica la ruta de acceso de la carpeta del archivo (o) donde se va a almacenar el T-SQL convertido. Cuando se especifica una ruta de acceso de carpeta junto con el `sql-files` atributo, cada archivo de código fuente tendrá un archivo T-SQL de destino correspondiente creado en la carpeta especificada. Cuando se especifica una ruta de acceso de carpeta junto con el `sql` atributo, el T-SQL convertido se escribe en un archivo denominado result. out en la carpeta especificada.  
  
6.  `sql` especifica las instrucciones SQL de MySQL que se van a convertir; una o varias instrucciones se pueden separar con un ";"  
  
7.  `sql-files` Especifica la ruta de acceso de los archivos SQL que se deben convertir al código T-SQL.  
  
8.  `write-summary-report-to` Especifica la ruta de acceso donde se generará el informe de resumen. Si solo se menciona la ruta de acceso de la carpeta, se crea el archivo por nombre **ConvertSQLReport.XML** . (atributo opcional)  
  
    La creación de informes tiene 2 subcategorías más, es decir,...:  
  
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
  
   destination="stdout/file"   (optional)  
  
   write-converted-sql-to="<file-name/folder-name>"  
  
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
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   destination="<stdout/file>"   (optional)  
  
   write-converted-sql-to="<file-name/folder-name>"  
  
   sql-files="<folder-name>\*.sql"  
  
/>  
```  
or  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   sql-files="<folder-name>\*.sql"  
  
/>  
```  
  
## <a name="next-step"></a>siguiente paso  
Para obtener información sobre las opciones de línea de comandos, vea [Opciones de la línea de comandos en la consola de SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/command-line-options-in-ssma-console-mysqltosql.md) .  
  
Para obtener más información acerca de los archivos de script de la consola de ejemplo, vea [trabajar con los archivos de script de la consola de ejemplo &#40;MySQLToSQL&#41;](../../ssma/mysql/working-with-the-sample-console-script-files-mysqltosql.md)  
  
El paso siguiente depende de los requisitos del proyecto:  
  
1.  Para especificar una contraseña o exportar o importar contraseñas, consulte [Administración de contraseñas &#40;MySQLToSQL&#41;](../../ssma/mysql/managing-passwords-mysqltosql.md).  
  
2.  Para generar informes, consulte [generación de informes &#40;MySQLToSQL&#41;](../../ssma/mysql/generating-reports-mysqltosql.md).  
  
3.  Para solucionar problemas de la consola de, consulte solución de problemas de [&#40;MySQLToSQL&#41;](../../ssma/mysql/troubleshooting-mysqltosql.md).  
  

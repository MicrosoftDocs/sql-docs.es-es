---
title: Objetos de scripts de SSMS
description: Genere scripts de Transact-SQL (T-SQL) para distintos objetos de SQL Server Management Studio (SSMS).
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
keywords: SQL Server, SSMS, SQL Server Management Studio, scripts, scripting
helpviewer_keywords:
- projects [SQL Server Management Studio], tutorials
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- solutions [SQL Server Management Studio], tutorials
- SQL Server Management Studio [SQL Server], tutorials
- scripts [SQL Server], SQL Server Management Studio
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.date: 03/13/2018
ms.openlocfilehash: 92f2d7c06116ef9c97fbf03c7849dd8ffef9dbf6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075876"
---
# <a name="script-objects-in-sql-server-management-studio"></a>Creación de scripts de objetos en SQL Server Management Studio

En este tutorial aprenderá a crear scripts de Transact-SQL (T-SQL) para distintos objetos de SQL Server Management Studio (SSMS). En este tutorial encontrará ejemplos de cómo crear scripts de los siguientes objetos:

> [!div class="checklist"]
> * Consultas, al llevar a cabo acciones en la GUI
> * Bases de datos de dos formas diferentes (Script como y Generar script)
> * Tablas
> * Procedimientos almacenados
> * Eventos extendidos

Para crear un script de cualquier objeto en el **Explorador de objetos**, haga clic con el botón derecho en él y seleccione la opción correspondiente para **crear un script de un objeto como**. En este tutorial se muestra el proceso.

## <a name="prerequisites"></a>Prerequisites

Para llevar a cabo este tutorial necesita tener SQL Server Management Studio, acceso a un servidor que ejecute SQL Server y una base de datos de AdventureWorks.

* Instale [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).
* Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
* Descargue las [bases de datos de ejemplo de AdventureWorks2016](https://github.com/Microsoft/sql-server-samples/releases).

Aquí encontrará instrucciones para restaurar bases de datos en SSMS: [Restaurar una base de datos](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md). 

## <a name="script-queries-from-the-gui"></a>Creación de scripts de consultas desde la interfaz gráfica de usuario

Puede generar el código T-SQL asociado para una tarea siempre que utilice la GUI en SSMS para completarla. En los siguientes ejemplos se muestra cómo hacerlo al hacer una copia de seguridad de una base de datos y al reducir el registro de transacciones. Estos mismos pasos se pueden aplicar a cualquier acción que se lleve a cabo con la GUI.

### <a name="script-t-sql-when-you-back-up-a-database"></a>Creación de un script de T-SQL al hacer una copia de seguridad de una base de datos

1. Conéctese a un servidor que ejecuta SQL Server.

2. Expanda el nodo **Bases de datos** .

3. Haga clic con el botón derecho en la base de datos **Adventureworks2016** > **Tareas** > **Hacer una copia de seguridad**:

    ![Realizar una copia de seguridad de una base de datos](media/scripting-ssms/backupdb.png)

4. Configure la copia de seguridad del modo que quiera. Para este tutorial, se ha dejado todo en los valores predeterminados, aunque todos los cambios efectuados en la ventana también se van a ver reflejados en el script. 

5. Seleccione **Script** > **Generar script de acción en ventana Nueva consulta**:

    ![Creación de un script de la copia de seguridad de base de datos: acción de script](media/scripting-ssms/scriptdbbackup.PNG)
6. Revise el código T-SQL rellenado en la ventana de consulta.

    ![Creación de un script de la copia de seguridad de base de datos: revisión de T-SQL](media/scripting-ssms/dbbackupscript.PNG)
7. Seleccione **Ejecutar** para ejecutar la consulta para hacer una copia de seguridad de la base de datos mediante T-SQL. 

### <a name="script-t-sql-when-you-shrink-the-transaction-log"></a>Creación de un script de T-SQL al reducir el registro de transacciones

1. Haga clic con el botón derecho en la base de datos **Adventureworks2016** > **Tareas** > **Reducir** > **Archivos**:

     ![Reducción de archivos](media/scripting-ssms/shrinkfiles.png)

2. Seleccione **Registro**  en el cuadro de lista desplegable **Tipo de archivo**:

    ![Reducción del registro de transacciones](media/scripting-ssms/shrinktlog.png)

3. Seleccione **Generar script** y **Generar script de acción en Portapapeles**:

    ![Generación de script en Portapapeles](media/scripting-ssms/scriptactiontoclipboard.png)

4. Abra una ventana de **Nueva consulta** y péguelo. (Haga clic con el botón derecho en la ventana. Después, seleccione **Pegar**).

    ![Pegado de script](media/scripting-ssms/paste.png)

5. Seleccione **Ejecutar** para ejecutar la consulta y reducir el registro de transacciones.

## <a name="script-databases"></a>Crear un script de bases de datos

En la siguiente sección se explica cómo crear un script de la base de datos con las opciones **Script como** y **Generar scripts**. La opción **Script como** permite volver a crear la base de datos y sus opciones de configuración. Puede crear un script para el esquema y los datos con la opción **Generar scripts**. En esta sección, va a crear dos nuevas bases de datos. Puede usar la opción **Script como** para crear *AdventureWorks2016a*. Puede usar la opción **Generar scripts** para crear *AdventureWorks2016b*.

### <a name="script-a-database-by-using-the-script-option"></a>Creación de un script de base de datos con la opción Script

1. Conéctese a un servidor que ejecuta SQL Server.

2. Expanda el nodo **Bases de datos** .

3. Haga clic con el botón derecho en la base de datos **AdventureWorks2016Script** > **Generar script de base de datos como** > **Crear en** > **Nueva ventana del Editor de consultas**:

    ![Creación de script de la base de datos](media/scripting-ssms/scriptdb.png)

4. Revise la consulta de creación de la base de datos en la ventana:

    ![Eliminación de scripts de la base de datos](media/scripting-ssms/scriptedoutdb.png) Esta opción solo elimina los scripts de las opciones de configuración de la base de datos.

5. En el teclado, seleccione Ctrl + F para abrir el cuadro de diálogo **Buscar**. Seleccione la flecha hacia abajo para abrir la opción **Reemplazar**. En la línea superior de **Buscar**, escriba AdventureWorks2016; en la línea inferior de **Reemplazar**, escriba AdventureWorks2016a.

6. Seleccione **Reemplazar todo** para reemplazar todas las instancias de *AdventureWorks2016* por *AdventureWorks2016a*. 

    ![Buscar y reemplazar](media/scripting-ssms/findandreplace.png)

7. Seleccione **Ejecutar** para ejecutar la consulta y crear la nueva base de datos AdventureWorks2016a. 

### <a name="script-a-database-by-using-the-generate-scripts-option"></a>Creación de un script de base de datos con la opción Generar script

1. Conéctese a un servidor que ejecuta SQL Server.

2. Expanda el nodo **Bases de datos** .

3. Haga clic con el botón derecho en **AdventureWorks2016** > **Tareas** > **Generar script**:

    ![Generación de scripts para bases de datos](media/scripting-ssms/generatescriptsfordb.png)

4. Se abre la página **Introducción**. Seleccione **Siguiente** para abrir la página **Elegir objetos**. Puede seleccionar toda la base de datos o determinados objetos de la base de datos. Seleccione **Crear un script a partir de toda la base de datos y todos los objetos de esta**.

    ![Generación de scripts para objetos](media/scripting-ssms/scriptobjects.png)

5. Seleccione **Siguiente** para abrir la página **Establecer opciones de scripting**. Aquí puede configurar dónde guardar el script y algunas opciones avanzadas adicionales. 

    a. Seleccione **Guardar en nueva ventana de consulta**.

    b. Seleccione **Opciones avanzadas** y compruebe que estas opciones estén establecidas:

      * **Generar script de estadísticas** establecido en *Generar script de estadísticas*.
      * **Tipos de datos que se deben incluir en el script** establecido en *Solo esquema*.
      * **Generar script de índices** establecido en *true*.

   ![Generación de script de objetos](media/scripting-ssms/advancedscripts.png)

   > [!NOTE]
   > Para crear un script de los datos para la base de datos, seleccione *Esquema y datos* para la opción **Tipos de datos que se deben incluir en el script**. Sin embargo, esto no es ideal con bases de datos grandes. Puede requerir más memoria de la que el SSMS puede asignar. Esta limitación está correcta para bases de datos pequeñas. Si desea mover datos para una base de datos más grande, utilice el [Asistente para importar y exportar](../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md).

6. Seleccione **Aceptar** y después **Siguiente**.

7. Seleccione **Siguiente** en el **resumen**. Después, vuelva a seleccionar **Siguiente** para generar el script en una ventana **Nueva consulta**.

8. En el teclado, abra el cuadro de diálogo **Buscar**  (Ctrl+F). Seleccione la flecha hacia abajo para abrir la opción **Reemplazar**. En la línea **Buscar** superior, escriba *AdventureWorks2016*. En la línea **Reemplazar** inferior, escriba *AdventureWorks2016b*.

9. Seleccione **Reemplazar todo** para reemplazar todas las instancias de *AdventureWorks2016* por *AdventureWorks2016b*.

    ![AdventureWorks 2016](media/scripting-ssms/adventureworks2016b.png)

10. Seleccione **Ejecutar** para ejecutar la consulta y crear la nueva base de datos AdventureWorks2016b.

## <a name="script-tables"></a>Creación de scripts de tablas

En esta sección se explica cómo crear scripts de tablas de la base de datos. Utilice esta opción para crear la tabla, o bien para anularla y volver a crearla. También puede usar esta opción para crear un script de T-SQL asociado a la modificación de la tabla. Un ejemplo es insertarlo en ella o actualizarla. En esta sección, va a anular una tabla y volver a crearla. 

1. Conéctese a un servidor que ejecuta SQL Server.

2. Expanda el nodo **Bases de datos**.

3. Expanda el nodo de la base de datos **AdventureWorks2016**. 

4. Expanda el nodo **Tablas**.

5. Haga clic con el botón derecho en **dbo.ErrorLog** > **Incluir tabla como** > **DROP y CREATE To** > **Nueva ventana del Editor de consultas**:

    ![Creación de script de tabla](media/scripting-ssms/scripttable.png)

6. Seleccione **Ejecutar** para ejecutar la consulta. Esta acción elimina la tabla *Errorlog* y la vuelve a crear. 

    >[!NOTE]
    > De forma predeterminada, la tabla *Errorlog*  está vacía en la base de datos de AdventureWorks2016. Así que no pierde ningún dato al eliminar la tabla. Sin embargo, seguir estos pasos para una tabla que contenga datos provocará la pérdida de estos.

## <a name="script-stored-procedures"></a>Creación de script de procedimientos almacenados

En esta sección, obtendrá información sobre cómo anular y crear un procedimiento almacenado.  

1. Conéctese a un servidor que ejecuta SQL Server.

2. Expanda el nodo **Bases de datos**.

3. Expanda el nodo **Programación**. 

4. Expanda el nodo **Procedimiento almacenado**.

5. Haga clic con el botón derecho en el procedimiento almacenado **dbo.uspGetBillOfMaterials** > **Incluir procedimiento almacenado como** > **DROP y CREATE To** > **Nueva ventana del Editor de consultas**:

    ![Creación de script de procedimientos almacenados](media/scripting-ssms/script-stored-procedure.png)

## <a name="script-extended-events"></a>Creación de script de eventos extendidos

En esta sección se explica cómo crear scripts de [eventos extendidos](../../relational-databases/extended-events/extended-events.md).

1. Conéctese a un servidor que ejecuta SQL Server.

2. Expanda el nodo **Administración**.

3. Expanda el nodo **Eventos extendidos**.

4. Expanda el nodo **Sesiones**.

5. Haga clic con el botón derecho en la sesión extendida que le interese, en **Incluir sesión como** > **CREATE To** > **Nueva ventana del Editor de consultas**:

    ![Sesión ampliada de Nueva ventana del Editor de consultas](media/scripting-ssms/scriptxevents.png)

6. En **Nueva ventana del Editor de consultas**, cambie el nuevo nombre de la sesión *system_health* por *system_health2*. Seleccione **Ejecutar** para ejecutar la consulta.

7. Haga clic con el botón derecho en **Sesiones** en el **Explorador de objetos**. Seleccione **Actualizar** para ver la nueva sesión de eventos extendidos. El icono verde junto a la sesión indica que se está ejecutando. El icono rojo indica que la sesión se ha detenido.

    ![Nueva sesión de eventos extendidos](media/scripting-ssms/newxevent.png)

    >[!NOTE]
    > Puede iniciar sesión haciendo clic en la sesión y seleccionando **Iniciar**. Sin embargo, esta es una copia de la sesión **system_health** en ejecución, por tanto puede omitir este paso. Puede eliminar la copia de la sesión de eventos extendidos haciendo clic con el botón derecho en esta y seleccionando **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

La mejor forma de familiarizarse con SSMS es practicar. Estos *tutoriales* y artículos de *procedimientos* lo ayudan con varias características disponibles dentro de SSMS. Estos artículos le mostrarán cómo administrar los componentes de SSMS y cómo localizar las características que utiliza habitualmente.

* [Conexión a una instancia y realización de consultas](../quickstarts/ssms-connect-query-sql-server.md)
* [Uso de plantillas en SSMS](../template/templates-ssms.md)
* [Configuración de SSMS](ssms-configuration.md)
* [Otras recomendaciones y trucos al usar SSMS](ssms-tricks.md)
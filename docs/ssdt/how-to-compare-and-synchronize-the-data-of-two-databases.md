---
title: Comparación y sincronización de los datos de dos bases de datos
description: Obtenga información sobre cómo comparar y sincronizar los datos de dos bases de datos. Aprenda a configurar la comparación, ver las diferencias y actualizar el destino.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.datacompare.connection.datasources.f1
- sql.data.tools.datacompare.watermark.f1
- sql.data.tools.datacompare.connection.objectselection.f1
ms.assetid: 2148e517-ed42-41c6-b753-1ac625f594c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 73dcdd57b018f980291eceefa1be37d1b51df13a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018445"
---
# <a name="how-to-compare-and-synchronize-the-data-of-two-databases"></a>Procedimientos: Comparación y sincronización de los datos de dos bases de datos

Puede comparar los datos contenidos en dos bases de datos. Las bases de datos que se comparan se conocen como el *origen* y el *destino*.  
  
> [!NOTE]  
> *Los proyectos de base de datos* y los paquetes .dacpac o .bacpac no pueden ser el origen o el destino de una comparación de datos.  
  
Cuando se comparan los datos, se genera un script (DML) de *lenguaje de manipulación de datos*, que puede utilizar para sincronizar las distintas bases de datos actualizando algunos o todos los datos de la base de datos de destino. Cuando finaliza la comparación de los datos, los resultados aparecen en la ventana Comparar datos de Visual Studio.  
  
Una vez finalizada la comparación, puede realizar otras acciones:  
  
-   Puede ver las diferencias entre las dos bases de datos. Para más información, consulte [Ver diferencias de datos](#ViewDifferences).  
  
-   Puede actualizar todo o parte del destino para que coincida con el origen. Para más información, consulte [Sincronizar datos de bases de datos](#Synchronize).  
  
Para más información, consulte [Comparar y sincronizar datos de una o más tablas con datos de una base de datos de referencia](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
> [!NOTE]  
> También puede comparar el *esquema* de dos bases de datos o de dos versiones de la misma base de datos. Para más información, vea: [Cómo: Usar Comparación de esquemas para comparar distintas definiciones de base de datos](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md).  
  
## <a name="comparing-database-data"></a><a name="CompareDatabaseData"></a>Comparar datos de bases de datos  
  
#### <a name="to-compare-data-by-using-the-new-data-comparison-wizard"></a>Para comparar datos utilizando el Nuevo asistente de comparación de datos  
  
1.  En el menú **SQL**, seleccione **Comparar datos** y, a continuación, haga clic en **Nueva comparación de datos**.  
  
    Aparece el Nuevo asistente de comparación de datos. Además, se abrirá la ventana Comparar datos y Visual Studio automáticamente le asigna un nombre como DataCompare1.  
  
2.  Identifique las bases de datos de origen y de destino.  
  
    Si la lista de **Base de datos de origen** o la lista de **Base de datos de destino** está vacía, haga clic en **Nueva conexión**. En el cuadro de diálogo **Propiedades de conexión**, identifique el servidor en el que reside la base de datos y el tipo de autenticación que se utilizará para conectarse a la base de datos. A continuación, haga clic en **Aceptar** para cerrar el cuadro de diálogo **Propiedades** y volver al asistente Comparar datos.  
  
    En la primera página del asistente Comparar datos, compruebe que la información de cada base de datos es correcta, especifique los registros que desea incluir en los resultados y, a continuación, haga clic en **Siguiente**. Aparece la segunda página del asistente Comparar datos y muestra una lista jerárquica de las tablas y vistas de la base de datos.  
  
3.  Active las casillas de las tablas y vistas que desea comparar. Opcionalmente, expanda los nodos para los objetos de base de datos y, a continuación, active las casillas de las columnas dentro de los objetos que desea comparar.  
  
    > [!NOTE]  
    > Las tablas y las vistas deben cumplir dos criterios para aparecer en la lista. Primero, los esquemas de los objetos deben coincidir entre las bases de datos de origen y de destino. En segundo lugar, solo las tablas y vistas que tienen una clave principal, una clave única, un índice único o restricción UNIQUE aparecen en la lista. Si las tablas o vistas cumplen los criterios, la lista estará vacía.  
  
4.  Si más de una clave está presente, puede utilizar la columna de **Clave de comparación** para especificar la clave en la que se basará la comparación de los datos. Por ejemplo, puede especificar si desea basar la comparación en la columna de clave principal o en otra columna de clave (identificable de forma única).  
  
5.  Haga clic en **Finalizar**  
  
    La comparación comienza.  
  
    > [!NOTE]  
    > Puede detener una operación de comparación de los datos que está en curso abriendo el menú **SQL**, haciendo clic en **Comparación de datos** y, a continuación, haciendo clic en **Detener comparación de datos**.  
  
    Cuando finaliza la comparación, puede ver las diferencias de los datos entre las dos bases de datos. También puede actualizar parte o todos los datos de la base de datos de destino para que coincidan con los datos de la base de datos de origen.  
  
#### <a name="to-compare-data-by-using-the-visual-studio-automation-model"></a>Para comparar los datos con el modelo de automatización de Visual Studio  
  
1.  Abra el menú **Ver**, elija **Otras ventanas** y, a continuación, haga clic en **Ventana Comandos**.  
  
2.  Escriba el comando siguiente en la ventana Comandos:  
  
    ```  
    Sql.NewDataComparison /SrcServerName sServerName /SrcDatabaseName sDatabaseName /SrcUserName sUserName /SrcPassword sPassword /SrcDisplayName sDisplayName /TargetServerName tServerName /TargetDatabaseName tDatabaseName /TargeUserName tUserName /TargetPassword tPassword /TargetDisplayName tDisplayName  
    ```  
  
    Reemplace los marcadores de posición (*sServerName*, *sDatabaseName*, *sUserName*, *sPassword*, *sDisplayName*, *tServerName*, *tDatabaseName*, *tUserName*, *tPassword*, y *tDisplayName*) con los valores de las bases de datos de origen y de destino.  
  
    Si no especifica un origen y un destino, aparece el cuadro de diálogo **Nueva comparación de datos**. Para más información acerca de los parámetros del comando Sql.NewDataComparison, consulte [Referencia de comandos de automatización para las características de base de datos de Visual Studio Team System](/previous-versions/visualstudio/visual-studio-2010/dd470565(v=vs.100)).  
  
    Los datos de las bases de datos de origen y de destino especificadas se comparan. Los resultados aparecen en la sesión Comparar datos. Para más información sobre cómo ver los resultados o sincronizar los datos, consulte [Ver diferencias de los datos](#ViewDifferences) y [Sincronizar datos de la base de datos](#Synchronize).  
  
## <a name="viewing-data-differences"></a><a name="ViewDifferences"></a>Ver las Diferencias de los datos  
Al comparar los datos de dos bases de datos, Comparar datos enumera cada *objeto de base de datos* que se comparó y su estado. También puede ver los resultados de los registros dentro de cada objeto, agrupados por estado. Para más información sobre las designaciones de estado, consulte [Comparar y sincronizar datos de una o más tablas con datos de una base de datos de referencia](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
Una vez haya visto las diferencias, puede actualizar el destino para que coincida con el origen de algunos o todos los objetos o registros que son diferentes, faltan o son nuevos. Para más información, consulte [Sincronizar datos de bases de datos](#Synchronize).  
  
#### <a name="to-view-data-differences"></a>Para ver las diferencias de los datos  
  
1.  Compare los datos de una base de datos de origen y de una base de datos de destino. Para más información, consulte [Comparar datos de bases de datos](#CompareDatabaseData).  
  
2.  (Opcional) Realice una o ambas de las acciones siguientes:  
  
    -   De forma predeterminada, los resultados de todos los objetos aparecen, independientemente de su estado. Para mostrar solo los objetos que tienen un estado determinado, haga clic en una opción de la lista **Filtro**.  
  
    -   Para ver los resultados de los registros en un objeto determinado, haga clic en el objeto en el panel de resultados principal y, a continuación, haga clic en la pestaña en el panel vista de registros. Cada pestaña muestra todos los registros en ese objeto que tienen un estado específico: diferente, solo en origen, solo en destino e idénticos. Los datos aparecen en el registro y la columna.  
  
## <a name="synchronizing-database-data"></a><a name="Synchronize"></a>Sincronizar los datos de bases de datos  
Al comparar los datos de dos bases de datos, puede sincronizarlas actualizando todo o parte del destino para que coincida con el origen. Puede comparar los datos en dos tipos de objetos de base de datos: tablas y vistas.  
  
#### <a name="to-update-target-data-by-using-the-write-updates-command"></a>Para actualizar datos de destino mediante el comando Escribir actualizaciones  
  
1.  Compare los datos de una base de datos de origen y de una base de datos de destino. Para más información, consulte [Comparar datos de bases de datos](#CompareDatabaseData).  
  
    Una vez finalizada la comparación, la ventana Comparar datos enumera los resultados para los objetos comparados. Cuatro columnas (denominadas Distintos registros, Solo en origen, Solo en destino y Registros idénticos) muestran información sobre los objetos que no son idénticos. Para cada uno de esos objetos, estas columnas muestran cuántos registros distintos se encontraron y cuántos registros cambiarían con una operación de actualización. Esos dos números coinciden al principio, pero en el paso 4 se puede cambiar qué objetos se van a actualizar.  
  
    Para más información, consulte [Comparar y sincronizar datos de una o más tablas con datos de una base de datos de referencia](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
2.  En la tabla de la ventana Comparar datos, haga clic en una fila.  
  
    El panel de detalles muestra los resultados de registros en el objeto de base de datos que seleccionó. Los registros se agrupan por el estado sobre las pestañas, que puede utilizar para especificar los datos que se propagarán del origen al destino.  
  
3.  En el panel de detalles, haga clic en la pestaña cuyo nombre contiene un número distinto de cero (0).  
  
    La columna **Actualizar** de la tabla **Solo en destino** contiene casillas que puede utilizar para seleccionar las filas que quiera actualizar. De manera predeterminada, se activan todas las casilla.  
  
4.  Desactive casillas de los registros del destino que no desee actualizar con datos del origen.  
  
    Cuando se desactiva la casilla, se reduce el número de registros que se van a actualizar, y los cambios efectuados para reflejar las acciones. Este número aparece en la línea de estado del panel de detalles y de la columna correspondiente en el panel de resultados principal, como se describe en el paso 1.  
  
5.  (Opcional) Haga clic en **Generar script**.  
  
    Una ventana del editor \- se abre y muestra el script de *Lenguaje de manipulación de datos* que se utilizará para actualizar el destino.  
  
6.  Para sincronizar los registros que son distintos, que faltan o son nuevos, haga clic en **Actualizar destino**.  
  
    > [!NOTE]  
    > Mientras se actualiza la base de datos de destino, puede cancelar la operación haciendo clic en **Detener escritura en destino**.  
  
    Los datos de los registros seleccionados en el destino se actualizan con los datos de los registros correspondientes en el origen.  
  
    > [!NOTE]  
    > Si decide actualizar las vistas indexadas, la operación **Actualizar destino** podría producir un error si esta acción duplica claves duplicadas que se van a insertar en la misma tabla.  
  
#### <a name="to-update-target-data-by-using-a-transact-sql-script"></a>Para actualizar los datos de destino mediante un script Transact\-SQL  
  
1.  Compare los datos de una base de datos de origen y de una base de datos de destino. Para más información, consulte [Comparar datos de bases de datos](#CompareDatabaseData).  
  
    Una vez finalizada la comparación, la ventana Comparar datos enumera los objetos comparados. Para más información, consulte [Comparar y sincronizar datos de una o más tablas con datos de una base de datos de referencia](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
2.  (Opcional) En el panel de detalles, desactive las casillas de los registros del destino que desee actualizar, como se describe en el procedimiento anterior.  
  
3.  Haga clic en **Generar script**.  
  
    Una nueva ventana muestra el script Transact\-SQL que propagaría los cambios necesarios para que los datos de destino coincidan con los datos de origen. Se proporciona un nombre a la nueva ventana como **DataUpdate_Database_1.sql**.  
  
    Este script refleja los cambios realizados en el panel de detalles. Por ejemplo, podría haber borrado una casilla para una fila determinada en la página Solo en destino para la tabla [dbo].[Shippers]. En ese caso, el script no actualizaría esa fila.  
  
4.  (Opcional) Edite este script en la ventana **DataUpdate_Database_1.sql**.  
  
5.  (Opcional pero se recomienda) Realice una copia de seguridad de la base de datos de destino.  
  
6.  Haga clic en **Ejecutar** para actualizar la base de datos de destino.  
  
    Especifique una conexión con la base de datos de destino que desee actualizar.  
  
    > [!IMPORTANT]  
    > De forma predeterminada, las actualizaciones tienen lugar en el ámbito de una transacción. Si aparecen errores, puede revertir toda la actualización. Puede cambiar este comportamiento.  
  
    Los datos de los registros seleccionados en el destino se actualizan con los datos de los registros correspondientes en el origen.  
  
## <a name="see-also"></a>Consulte también  
[Comparar y sincronizar datos de una o más tablas con datos de una base de datos de referencia](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md)  

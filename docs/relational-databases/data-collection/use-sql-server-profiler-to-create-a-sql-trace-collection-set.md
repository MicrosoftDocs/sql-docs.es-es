---
description: Usar SQL Server Profiler para crear un conjunto de recopilación de Seguimiento de SQL
title: Creación del conjunto de colecciones de seguimiento de SQL con Profiler
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- SQL Trace collector set
ms.assetid: b6941dc0-50f5-475d-82eb-ce7c68117489
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 4b2f20965b3bdff081368c2e1fcaa2bc255aca47
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250106"
---
# <a name="use-sql-server-profiler-to-create-a-sql-trace-collection-set"></a>Usar SQL Server Profiler para crear un conjunto de recopilación de Seguimiento de SQL
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] puede aprovechar la funcionalidad de seguimiento del lado servidor de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para exportar una definición de seguimiento y emplearla después para crear un conjunto de recopilación que use el tipo de recopilador genérico de Seguimiento de SQL. En este proceso hay dos partes:  
  
1.  Crear y exportar un seguimiento de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
2.  Crear un script de un nuevo conjunto de recopilación basado en un seguimiento exportado.  

 El escenario para los procedimientos siguientes implica recopilar datos sobre cualquier procedimiento almacenado que requiera 80 milisegundos o más en completarse. Para completar estos procedimientos debería ser capaz de:  
  
-   Usar [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para crear y configurar un seguimiento.  
  
-   Usar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para abrir, modificar y ejecutar una consulta.  
  
### <a name="create-and-export-a-sql-server-profiler-trace"></a>Crear y exportar un seguimiento de SQL Server Profiler  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], abra [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. En el menú **Herramientas** , haga clic en **SQL Server Profiler**.  
  
2.  En el cuadro de diálogo **Conectar con el servidor** , haga clic en **Cancelar**.  
  
3.  Para este escenario, asegúrese de que los valores de duración estén configurados para mostrarse en milisegundos (valor predeterminado). Para ello, siga estos pasos.  
  
    1.  En el menú **Herramientas** , haga clic en **Opciones**.  
  
    2.  En el área **Opciones de visualización** , asegúrese de que esté desactivada la casilla Mostrar valores en la columna Duración en microsegundos.  
  
    3.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Opciones generales** .  
  
4.  En el menú **Archivo** , haga clic en **Nuevo seguimiento**.  
  
5.  En el cuadro de diálogo **Conectar con el servidor** , seleccione el servidor con el que desea conectarse y, a continuación, haga clic en **Conectar**.  
  
     Aparecerá el cuadro de diálogo **Propiedades de seguimiento** .  
  
6.  En la pestaña **General**, realice las acciones siguientes:  
  
    1.  En el cuadro **Nombre de seguimiento** , escriba el nombre que desee usar para el seguimiento. En este ejemplo, el nombre del seguimiento es **SPgt80**.  
  
    2.  En la lista **Usar la plantilla**, seleccione la plantilla que desea usar para el seguimiento. Para este ejemplo, haga clic en **TSQL_SPs**.  
  
7.  En la pestaña **Selección de eventos** , haga lo siguiente:  
  
    1.  Identifique los eventos que se van a usar para el seguimiento. En este ejemplo, desactive todas las casillas de la columna **Eventos** excepto **ExistingConnection** y **SP:Completed**.  
  
    2.  En la esquina inferior derecha, active la casilla **Mostrar todas las columnas** .  
  
    3.  Haga clic en la fila **SP:Completed** .  
  
    4.  Desplácese por la fila hasta la columna **Duración** y, a continuación, active la casilla **Duración** .  
  
8.  En la esquina inferior derecha, haga clic en **Filtros de columna** para abrir el cuadro de diálogo **Editar filtro** . En el cuadro de diálogo **Editar filtro** , realice las siguientes operaciones:  
  
    1.  En la lista de filtros, haga clic en **Duración**.  
  
    2.  En la ventana de operadores booleanos, expanda el nodo **Mayor o igual que** , escriba **80** como valor y, a continuación, haga clic en **Aceptar**.  
  
9. Haga clic en **Ejecutar** para iniciar el seguimiento.  
  
10. En la barra de herramientas, haga clic en **Detener seguimiento seleccionado** o **Pausar seguimiento seleccionado**.  
  
11. En el menú **Archivo** , seleccione **Exportar**, **Definición de seguimiento de scripts** y, a continuación, haga clic en **Para el conjunto de recopilación de Seguimiento de SQL**.  
  
12. En el cuadro **Nombre de archivo** del cuadro de diálogo **Guardar como** , escriba el nombre que desea usar para la definición de seguimiento y, a continuación, guárdela en la ubicación que desee. En este ejemplo, el nombre de archivo es igual que el nombre de seguimiento (SPgt80).  
  
13. Haga clic en **Aceptar** cuando aparezca el mensaje que indica que el archivo se guardó correctamente y, a continuación, cierre [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="script-a-new-collection-set-from-a-sql-server-profiler-trace"></a>Crear un script de un nuevo conjunto de recopilación a partir de un seguimiento de SQL Server Profiler  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en el menú **Archivo** , seleccione **Abrir** y haga clic en **Archivo**.  
  
2.  En el cuadro de diálogo **Abrir archivo** , localice el archivo que creó en el procedimiento anterior (SPgt80) y ábralo.  
  
     La información de seguimiento que guardó se abre en una ventana Consulta y se combina en un script que puede ejecutar para crear el nuevo conjunto de recopilación.  
  
3.  Desplácese a través del script y realice las sustituciones siguientes, que están anotadas en el texto de los comentarios del script:  
  
    -   Reemplace **SQLTrace Collection Set Name Here** por el nombre que desea usar para el conjunto de recopilación. En este ejemplo, asigne el nombre **SPROC_CollectionSet** al conjunto de recopilación.  
  
    -   Reemplace **SQLTrace Collection Item Name Here** por el nombre que desea usar para el elemento de recopilación. En este ejemplo, asigne el nombre **SPROC_Collection_Item** al elemento de recopilación.  
  
4.  Haga clic en **Ejecutar** para ejecutar la consulta y crear el conjunto de recopilación.  
  
5.  En el Explorador de objetos, compruebe que se ha creado el conjunto de recopilación. Para ello, siga estos pasos.  
  
    1.  Haga clic con el botón derecho en **Administración** y, después, haga clic en **Actualizar**.  
  
    2.  Expanda **Administración** y, a continuación, expanda **Recopilación de datos**.  
  
     El conjunto de recopilación **SPROC_CollectionSet** aparece en el mismo nivel que el nodo **Conjuntos de recopilación de datos del sistema** . De manera predeterminada, el conjunto de recopilación está deshabilitado.  
  
6.  Use el Explorador de objetos para editar las propiedades de SPROC_CollectionSet, como el modo de recopilación y la programación de carga. Realice los mismos procedimientos que usaría para los conjuntos de recopilación de datos del sistema que se proporcionan con el recopilador de datos.  
  
## <a name="example"></a>Ejemplo  
 El ejemplo de código siguiente es el script final que resulta de los pasos que se documentan en los procedimientos anteriores.  
  
```sql
/*************************************************************/  
-- SQL Trace collection set generated from SQL Server Profiler  
-- Date: 11/19/2007  12:55:31 AM  
/*************************************************************/  
  
USE msdb;
GO  
  
BEGIN TRANSACTION  
BEGIN TRY  
  
-- Define collection set  
-- **_  
-- _*_ Replace 'SqlTrace Collection Set Name Here' in the   
-- _*_ following script with the name you want  
-- _*_ to use for the collection set.  
-- _*_  
DECLARE @collection_set_id int;  
EXEC [dbo].[sp_syscollector_create_collection_set]  
    @name = N'SPROC_CollectionSet',  
    @schedule_name = N'CollectorSchedule_Every_15min',  
    @collection_mode = 0, -- cached mode needed for Trace collections  
    @logging_level = 0, -- minimum logging  
    @days_until_expiration = 5,  
    @description = N'Collection set generated by SQL Server Profiler',  
    @collection_set_id = @collection_set_id output;  
SELECT @collection_set_id;  
  
-- Define input and output variables for the collection item.  
DECLARE @trace_definition xml;  
DECLARE @collection_item_id int;  
  
-- Define the trace parameters as an XML variable  
SELECT @trace_definition = convert(xml,   
N'<ns:SqlTraceCollector xmlns:ns"DataCollectorType" use_default="0">  
<Events>  
  <EventType name="Sessions">  
    <Event id="17" name="ExistingConnection" columnslist="1,2,14,26,3,35,12" />  
  </EventType>  
  <EventType name="Stored Procedures">  
    <Event id="43" name="SP:Completed" columnslist="1,2,26,34,3,35,12,13,14,22" />  
  </EventType>  
</Events>  
<Filters>  
  <Filter columnid="13" columnname="Duration" logical_operator="AND" comparison_operator="GE" value="80000L" />  
</Filters>  
</ns:SqlTraceCollector>  
');  
  
-- Retrieve the collector type GUID for the trace collector type.  
DECLARE @collector_type_GUID uniqueidentifier;  
SELECT @collector_type_GUID = collector_type_uid
  FROM [dbo].[syscollector_collector_types]
  WHERE name = N'Generic SQL Trace Collector Type';  
  
-- Create the trace collection item.  
-- _*_  
-- _*_ Replace 'SqlTrace Collection Item Name Here' in   
-- _*_ the following script with the name you want to  
-- _*_ use for the collection item.  
-- _**  
EXEC [dbo].[sp_syscollector_create_collection_item]  
   @collection_set_id = @collection_set_id,  
   @collector_type_uid = @collector_type_GUID,  
   @name = N'SPROC_Collection_Item',  
   @frequency = 900, -- specified the frequency for checking to see if trace is still running  
   @parameters = @trace_definition,  
   @collection_item_id = @collection_item_id output;  
SELECT @collection_item_id;  
  
COMMIT TRANSACTION;  
END TRY  
  
BEGIN CATCH  
ROLLBACK TRANSACTION;  
DECLARE @ErrorMessage nvarchar(4000);  
DECLARE @ErrorSeverity int;  
DECLARE @ErrorState int;  
DECLARE @ErrorNumber int;  
DECLARE @ErrorLine int;  
DECLARE @ErrorProcedure nvarchar(200);  
SELECT @ErrorLine = ERROR_LINE(),  
       @ErrorSeverity = ERROR_SEVERITY(),  
       @ErrorState = ERROR_STATE(),  
       @ErrorNumber = ERROR_NUMBER(),  
       @ErrorMessage = ERROR_MESSAGE(),  
       @ErrorProcedure = ISNULL(ERROR_PROCEDURE(), '-');  
RAISERROR (14684, @ErrorSeverity, 1 , @ErrorNumber,
  @ErrorSeverity, @ErrorState, @ErrorProcedure, @ErrorLine, @ErrorMessage);  
END CATCH;  
GO  
```  
  
  

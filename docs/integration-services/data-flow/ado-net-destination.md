---
description: Destino ADO NET
title: Destino de ADO NET | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.adonetdest.f1
- sql13.dts.designer.adonetdest.connection.f1
- sql13.dts.designer.adonetdest.mappings.f1
- sql13.dts.designer.adonetdest.erroroutput.f1
helpviewer_keywords:
- destinations [Integration Services], ADO.NET
- ADO.NET destination
ms.assetid: cb883990-d875-4d8b-b868-45f9f15ebeae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4ae905d868554c20948af3eb0191937382eff56f
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536383"
---
# <a name="ado-net-destination"></a>Destino ADO NET

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El destino ADO NET carga datos en una serie de bases de datos compatibles con [!INCLUDE[vstecado](../../includes/vstecado-md.md)]que usan una tabla o vista de base de datos. Tiene la opción de cargar estos datos en una tabla o vista existente, o bien puede crear una nueva tabla y cargar los datos en ella.  
  
 Puede usar el destino de ADO NET para conectarse a [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. No se admite la conexión a [!INCLUDE[ssSDS](../../includes/sssds-md.md)] mediante OLE DB. Para más información sobre [!INCLUDE[ssSDS](../../includes/sssds-md.md)], consulte [Instrucciones y limitaciones generales (Azure SQL Database)](/previous-versions/azure/ee336245(v=azure.100)).  
  
## <a name="troubleshooting-the-ado-net-destination"></a>Solucionar problemas del destino ADO NET  
 Puede registrar las llamadas realizadas por el destino ADO NET a proveedores de datos externos. Puede utilizar esta nueva capacidad de registro para solucionar problemas relacionados con el almacenamiento de datos en orígenes de datos externos que realiza el destino ADO NET. Para registrar las llamadas realizadas por el destino ADO NET a proveedores de datos externos, habilite el registro de paquetes y seleccione el evento **Diagnostic** en el nivel de paquete. Para más información, vea [Herramientas para solucionar problemas con la ejecución de paquetes](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
## <a name="configuring-the-ado-net-destination"></a>Configuración del destino ADO NET  
 Este destino usa un administrador de conexiones [!INCLUDE[vstecado](../../includes/vstecado-md.md)] para conectarse a un origen de datos, administrador de conexiones que especifica el proveedor [!INCLUDE[vstecado](../../includes/vstecado-md.md)] que se debe usar. Para más información, consulte [ADO.NET Connection Manager](../../integration-services/connection-manager/ado-net-connection-manager.md).  
  
 Un destino ADO NET incluye asignaciones entre columnas de entrada y columnas en el origen de datos de destino. No es preciso que asigne columnas de entrada a todas las columnas de destino. Sin embargo, las propiedades de algunas columnas de destino pueden requerir la asignación de columnas de entrada. De lo contrario, se podrían producir errores. Por ejemplo, si una columna de destino no permite valores NULL, se debe asignar una columna de entrada a esa columna de destino. Además, los tipos de datos de las columnas asignadas deben ser compatibles. Por ejemplo, no es posible asignar una columna de entrada con un tipo de datos de cadena a una columna de destino con un tipo de datos numéricos si el proveedor [!INCLUDE[vstecado](../../includes/vstecado-md.md)] no admite esta asignación.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no permite insertar texto en las columnas cuyo tipo de datos se haya establecido como imagen. Para obtener más información sobre los tipos de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).  
  
> [!NOTE]  
>  El destino ADO NET no permite asignar una columna de entrada cuyo tipo sea DT_DBTIME a una columna de base de datos cuyo tipo sea datetime. Para obtener más información sobre los tipos de datos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vea [Tipos de datos de Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 El destino ADO NET tiene una entrada normal y una salida de error.  
  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 El cuadro de diálogo **Editor avanzado** indica las propiedades que se pueden establecer mediante programación. Para obtener más información acerca de las propiedades que puede establecer a través del cuadro de diálogo **Editor avanzado** o mediante programación, haga clic en uno de los temas siguientes:  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [Propiedades personalizadas de ADO NET](../../integration-services/data-flow/ado-net-custom-properties.md)  
  
 Para más información sobre cómo establecer propiedades, vea [Establecer las propiedades de un componente de flujo de datos](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="ado-net-destination-editor-connection-manager-page"></a>Editor de destinos de ADO NET (página Administrador de conexiones)
  Utilice la página **Administrador de conexiones** del cuadro de diálogo **Editor de destinos de ADO NET** para seleccionar la conexión [!INCLUDE[vstecado](../../includes/vstecado-md.md)] del destino. Esta página también permite seleccionar una tabla o vista de la base de datos.  
  
 **Para abrir la página Administrador de conexiones**  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el paquete [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que tiene el destino de ADO NET.  
  
2.  En la pestaña **Flujo de datos** , haga doble clic en el destino de ADO NET.  
  
3.  En el **Editor de destinos de ADO NET**, haga clic en **Administrador de conexiones**.  
  
### <a name="static-options"></a>Opciones estáticas  
 **Connection manager**  
 Seleccione un administrador de conexiones de la lista o cree una conexión haciendo clic en **Nuevo**.  
  
 **Nuevo**  
 Cree un administrador de conexiones mediante el cuadro de diálogo **Configurar el administrador de conexiones ADO.NET** .  
  
 **Usar una tabla o una vista**  
 Seleccione una tabla o vista de la lista o cree una tabla haciendo clic en **Nueva**.  
  
 **Nuevo**  
 Cree un tabla o una vista mediante el cuadro de diálogo **Crear tabla** .  
  
> [!NOTE]  
>  Al hacer clic en **Nueva**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] genera una instrucción predeterminada CREATE TABLE basada en el origen de datos conectado. La instrucción predeterminada CREATE TABLE no incluirá el atributo FILESTREAM, aunque la tabla de origen tenga una columna con el atributo FILESTREAM declarado. Para ejecutar un componente [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con el atributo FILESTREAM, implemente en primer lugar el almacenamiento de FILESTREAM en la base de datos de destino. A continuación, agregue el atributo FILESTREAM a la instrucción CREATE TABLE en el cuadro de diálogo **Crear tabla** . Para más información, vea [Datos de objeto binario grande &#40;Blob&#41; &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).  
  
 **Versión preliminar**  
 Obtenga una vista previa de los resultados mediante el cuadro de diálogo **Vista previa de los resultados de la consulta** . La vista previa puede mostrar hasta 200 filas.  
  
 **Usar inserción masiva cuando esté disponible**  
 Especifique si se debe usar la interfaz <xref:System.Data.SqlClient.SqlBulkCopy> para mejorar el rendimiento de las operaciones de inserción masiva.  
  
 Solamente los proveedores ADO.NET que devuelven un objeto <xref:System.Data.SqlClient.SqlConnection> admiten el uso de la interfaz <xref:System.Data.SqlClient.SqlBulkCopy> . El Proveedor de datos .NET para SQL Server (SqlClient) devuelve un objeto <xref:System.Data.SqlClient.SqlConnection> y, por otra parte, un proveedor personalizado puede devolver un objeto <xref:System.Data.SqlClient.SqlConnection> .  
  
 Puede usar el proveedor de datos .NET para SQL Server (SqlClient) a fin de conectarse a [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
 Si selecciona **Use bulk insert when available**(Usar la inserción masiva cuando esté disponible) y establece la opción **Error** en **Redirect the row**(Redirigir la fila), el lote de datos que el destino redirige a la salida de error puede incluir filas correctas. Para obtener más información sobre cómo administrar errores en operaciones masivas, vea [Control de errores en los datos](../../integration-services/data-flow/error-handling-in-data.md). Para obtener más información sobre la opción **Error** , vea [Editor de destinos de ADO NET &#40;página Salida de error&#41;]().  
  
> [!NOTE]
>  Si una tabla de origen de SQL Server o de Sybase incluye una columna de identidad, debe usar las tareas Ejecutar SQL para habilitar IDENTITY_INSERT antes del destino de ADO NET y para deshabilitarlo nuevamente después. (La propiedad de la columna de identidad especifica un valor incremental de la columna. La instrucción SET IDENTITY_INSERT permite que valores explícitos de la tabla de origen se inserten en la columna de identidad de la tabla de destino).  
> 
>   Para ejecutar las instrucciones SET IDENTITY_INSERT y la carga de datos correctamente, se debe hacer lo siguiente.  
>       1. Use el mismo administrador de conexiones de ADO.NET para las tareas Ejecutar SQL y para el destino de ADO NET.  
>       2. En el administrador de conexiones, establezca la propiedad **RetainSameConnection** y la propiedad **MultipleActiveResultSets** en True.  
>       3. En el destino de ADO.NET, establezca la propiedad **UseBulkInsertWhenPossible** en False.   
> 
>  Para obtener más información, vea [SET IDENTITY_INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/set-identity-insert-transact-sql.md) y [IDENTITY &#40;propiedad de Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md).  
  
## <a name="external-resources"></a>Recursos externos  
 Artículo técnico sobre cómo [cargar datos en Azure SQL Database de la forma más rápida](https://techcommunity.microsoft.com/t5/datacat/loading-data-to-sql-azure-the-fast-way/ba-p/305066), en sqlcat.com  
  
## <a name="ado-net-destination-editor-mappings-page"></a>Editor de destinos de ADO NET (página Asignaciones)
  Use la página **Asignaciones** del cuadro de diálogo **Editor de destinos de ADO NET** para asignar columnas de entrada a columnas de destino.  
  
 **Para abrir la página Asignaciones**  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el paquete [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que tiene el destino de ADO NET.  
  
2.  En la pestaña **Flujo de datos** , haga doble clic en el destino de ADO NET.  
  
3.  En el **Editor de destinos de ADO NET**, haga clic en **Asignaciones**.  
  
### <a name="options"></a>Opciones  
 **Columnas de entrada disponibles**  
 Muestra la lista de columnas de entrada disponibles. Utilice una operación de arrastrar y colocar para asignar columnas de entrada disponibles de la tabla a columnas de destino.  
  
 **Columnas de destino disponibles**  
 Muestra la lista de columnas de destino disponibles. Utilice una operación de arrastrar y colocar para asignar columnas de destino disponibles de la tabla a columnas de entrada.  
  
 **Columna de entrada**  
 Permite ver las columnas de entrada seleccionadas. Para quitar asignaciones, seleccione **\<ignore>** con el fin de excluir columnas de la salida.  
  
 **Columna de destino**  
 Muestra todas las columnas de destino disponibles, tanto si están asignadas como si no lo están.  
  
## <a name="ado-net-destination-editor-error-output-page"></a>Editor de destinos de ADO NET (página Salida de error)
  Utilice la página **Salida de error** del cuadro de diálogo **Editor de destinos de ADO NET** para especificar las opciones de control de errores.  
  
 **Para abrir la página Salida de error**  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el paquete [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que tiene el destino de ADO NET.  
  
2.  En la pestaña **Flujo de datos** , haga doble clic en el destino de ADO NET.  
  
3.  En el **Editor de destinos de ADO NET**, haga clic en **Salida de error**.  
  
### <a name="options"></a>Opciones  
 **Entrada o salida**  
 Muestra el nombre de la entrada.  
  
 **Columna**  
 No se usa.  
  
 **Error**  
 Permite especificar qué debe ocurrir cuando se produce un error: omitir el error, redirigir la fila o hacer que el componente no funcione.  
  
 **Temas relacionados:** [Control de errores en los datos](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **Truncamiento**  
 No se usa.  
  
 **Descripción**  
 Muestra la descripción de la operación.  
  
 **Establecer este valor en las celdas seleccionadas**  
 Permite especificar qué debe ocurrir en todas las celdas seleccionadas cuando se produce un error o un truncamiento: omitir el error, redirigir la fila o hacer que el componente no funcione.  
  
 **Aplicar**  
 Aplica la opción de control de errores a las celdas seleccionadas.  
  

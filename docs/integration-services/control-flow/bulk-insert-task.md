---
description: Inserción masiva, tarea
title: Tarea Inserción masiva| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.bulkinserttask.f1
- sql13.dts.designer.bulkinserttask.connection.f1
- sql13.dts.designer.bulkinserttask.general.f1
- sql13.dts.designer.bulkinserttask.options.f1
helpviewer_keywords:
- Bulk Insert task
- copying data [Integration Services]
ms.assetid: c5166156-6b4c-4369-81ed-27c4ce7040ae
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 739086916d6b7bdea630e8327aa0ede0fb899c87
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348969"
---
# <a name="bulk-insert-task"></a>Inserción masiva, tarea

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Inserción masiva proporciona una forma muy eficaz de copiar grandes cantidades de datos a una tabla o vista de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Por ejemplo, suponga que su empresa almacena una lista de productos de un millón de filas en un sistema central, pero el sistema de comercio electrónico de la empresa usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para rellenar páginas web. Debe actualizar la tabla de productos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] todas las noches con la lista maestra de productos del gran sistema. Para ello, debe guardar la lista de productos con un formato delimitado por tabuladores y utilizar la tarea Inserción masiva para copiar los datos directamente a la tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Para garantizar que la copia de datos se realice a gran velocidad, no se permite la aplicación de transformaciones a los datos mientras se mueven desde el archivo de origen a la tabla o la vista.  
  
## <a name="usage-considerations"></a>Consideraciones de uso  
 Antes de utilizar la tarea Inserción masiva, tenga en cuenta que:  
  
-   La tarea Inserción masiva solo puede transferir datos de un archivo de texto a una tabla o una vista de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para usar la tarea Inserción masiva para transferir datos de otros sistemas de administración de bases de datos (DBMS), debe exportar los datos del origen a un archivo de texto y después importar los datos del archivo de texto en una tabla o vista de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   El destino debe ser una tabla o una vista de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si la tabla o vista de destino ya contiene datos, los datos nuevos se anexarán a los datos existentes cuando se ejecute la tarea Inserción masiva. Si desea reemplazar los datos, antes de ejecutar la tarea Inserción masiva, debe ejecutar una tarea Ejecutar SQL que ejecute una instrucción DELETE o TRUNCATE. Para más información, consulte [Execute SQL Task](../../integration-services/control-flow/execute-sql-task.md).  
  
-   Puede utilizar un archivo de formato en el objeto de tarea Inserción masiva. Si tiene un archivo de formato creado con la utilidad **bcp** , puede especificar su ruta de acceso en la tarea Inserción masiva. La tarea Inserción masiva admite tanto archivos XML como archivos con otros formatos. Para obtener más información sobre archivos de formato, vea [Archivos de formato para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
-   Solo los miembros del rol fijo de servidor sysadmin pueden ejecutar un paquete que contiene una tarea Inserción masiva.  
  
## <a name="bulk-insert-task-with-transactions"></a>Tarea Inserción masiva en las transacciones  
 Si el tamaño de un lote no está establecido, la operación completa de copia masiva se trata como una transacción. Si el tamaño del lote es **0** , significa que los datos se insertan en un lote. Si se ha establecido un tamaño de lote, cada lote constituye una transacción que será confirmada cuando finalice la ejecución del lote.  
  
 El comportamiento de la tarea Inserción masiva en lo que se refiere a las transacciones dependerá de si la tarea se combina con la transacción del paquete. Si la tarea Inserción masiva no se combina con la transacción de paquete, se confirmará cada lote sin errores como una unidad antes de procesar el siguiente lote. Si se combina la tarea de inserción masiva con la transacción del paquete, los lotes sin errores permanecerán en la transacción tras la finalización de la tarea. Estos lotes están sujetos a las operaciones para confirmar o revertir del paquete.  
  
 Un error en la tarea Inserción masiva no revierte automáticamente los lotes cargados correctamente; de forma similar, si la tarea se completa correctamente, los lotes no se confirman automáticamente. Solo se realizan operaciones para confirmar o revertir en respuesta a valores de propiedades de un paquete y un flujo de trabajo.  
  
## <a name="source-and-destination"></a>Origen y destino  
 Si se especifica la ubicación del archivo de origen de texto, tenga en cuenta los siguientes aspectos:  
  
-   El servidor debe tener permiso de acceso al archivo y a la base de datos de destino.  
  
-   El servidor ejecuta la tarea Inserción masiva. Por tanto, cualquier archivo de formato utilizado por la tarea debe encontrarse en el servidor.  
  
-   El archivo de código fuente que carga la tarea Inserción masiva puede estar en el mismo servidor que la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que se van a insertar los datos o en un servidor remoto. Si el archivo está en un servidor remoto, debe especificar el nombre de archivo en la ruta mediante un nombre conforme con la convención de nomenclatura universal (UNC).  
  
## <a name="performance-optimization"></a>Optimización del rendimiento  
 Para optimizar el rendimiento, tenga en cuenta que:  
  
-   Si el archivo de texto se encuentra en el mismo equipo que la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que se van a insertar los datos, la operación de copia se producirá aun más rápido, ya que no hay que mover los datos por la red.  
  
-   La tarea de inserción masiva no registrará las filas que produzcan errores. Si tiene que capturar esta información, use las salidas de error de componentes de flujo de datos para capturar las filas que provocan errores en un archivo de excepciones.  
  
## <a name="custom-log-entries-available-on-the-bulk-insert-task"></a>Entradas del registro personalizadas disponibles en la tarea Inserción masiva  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Inserción masiva. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**BulkInsertTaskBegin**|Indica que se inició la inserción masiva.|  
|**BulkInsertTaskEnd**|Indica que finalizó la inserción masiva.|  
|**BulkInsertTaskInfos**|Proporciona información descriptiva sobre la tarea.|  
  
## <a name="bulk-insert-task-configuration"></a>Configuración de la tarea Inserción masiva  
 Puede configurar la tarea Inserción masiva de las maneras siguientes:  
  
-   Especificar el administrador de conexiones OLE DB para conectar con la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino y la tabla o la vista en la que se insertarán los datos. La tarea Inserción masiva solo admite conexiones de OLE DB para la base de datos de destino.  
  
-   Especifique el archivo o administrador de conexiones de archivos planos para tener acceso al archivo de origen. La tarea Inserción masiva solo utiliza el administrador de conexiones para la ubicación del archivo de destino. La tarea omite otras opciones seleccionadas en el editor de administrador de conexiones.  
  
-   Definir el formato usado por la tarea Inserción masiva mediante un archivo de formato o definiendo la columna y los delimitadores de filas de los datos de origen. Si utiliza un archivo de formato, especifique el administrador de conexiones de archivos para tener acceso al archivo de formato.  
  
-   Especificar acciones que se deben realizar en la tabla o la vista de destino cuando la tarea inserta los datos. Entre las opciones figuran restricciones CHECK, habilitar inserciones de identidad, conservar valores NULL, activar desencadenadores y bloquear la tabla.  
  
-   Proporcionar información acerca del lote de datos que se va a insertar, como el tamaño del lote, la primera y la última fila del archivo que se va a insertar, el número de errores de inserción que se pueden producir antes de que la tarea deje de insertar filas y los nombres de las columnas que se van a ordenar.  
  
 Si la tarea Inserción masiva utiliza un administrador de conexiones de archivos planos para obtener acceso al archivo de origen, la tarea no utiliza el formato especificado en dicho administrador. En su lugar, la tarea Inserción masiva usa el formato especificado en un archivo de formato o los valores de las propiedades RowDelimiter y ColumnDelimiter de la tarea.  
  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 Para obtener más información acerca de las propiedades que puede establecer en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el tema siguiente:  
  
-   [Página Expresiones](../../integration-services/expressions/expressions-page.md)  
  
 Para obtener más información sobre cómo establecer estas propiedades en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el tema siguiente:  
  
-   [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="programmatic-configuration-of-the-bulk-insert-task"></a>Configuración mediante programación de la tarea Inserción masiva  
 Para obtener más información sobre cómo establecer estas propiedades mediante programación, haga clic en el tema siguiente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>  
  
## <a name="related-tasks"></a>Related Tasks  
 [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   Artículo técnico, sobre la [posible aparición del error "No se puede preparar la inserción masiva de SSIS para la inserción de datos" en sistemas habilitados para UAC](https://go.microsoft.com/fwlink/?LinkId=233693), en support.microsoft.com.  
  
-   Artículo técnico, [The Data Loading Performance Guide](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100)), en msdn.microsoft.com.  
  
-   Artículo técnico sobre cómo [usar SQL Server Integration Services para la carga masiva de datos](https://go.microsoft.com/fwlink/?LinkId=233701), en simple-talk.com.  
  
## <a name="bulk-insert-task-editor-connection-page"></a>Editor de la tarea Inserción masiva (página Conexión)
  Use la página **Conexión** del cuadro de diálogo **Editor de la tarea Inserción masiva** para especificar el origen y el destino de la operación de inserción masiva y el formato que se debe utilizar.  
  
 Para más información sobre las inserciones masivas, vea [Tarea Inserción masiva](../../integration-services/control-flow/bulk-insert-task.md) y [Archivos de formato para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
### <a name="options"></a>Opciones  
 **Connection**  
 Seleccione un administrador de conexiones OLE DB de la lista o haga clic en \<**New connection...**> para crear una conexión nueva.  
  
 **Temas relacionados:** [Administrador de conexiones OLE DB](../../integration-services/connection-manager/ole-db-connection-manager.md)  
  
 **Tabla de destino**  
 Escriba el nombre de la tabla o la vista de destino, o seleccione una tabla o una vista de la lista.  
  
 **Formato**  
 Seleccione el origen del formato de la inserción masiva. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Utilizar archivo**|Seleccione un archivo que contenga la especificación de formato. Si se selecciona esta opción, se muestra la opción dinámica **FormatFile**.|  
|**Especificar**|Especifique el formato. Si se selecciona esta opción, se muestran las opciones dinámicas **RowDelimiter** y **ColumnDelimiter**.|  
  
 **Archivo**  
 Seleccione un administrador de conexiones de archivo o de archivo plano de la lista o haga clic en \<**New connection...**> para crear una conexión nueva.  
  
 La ubicación del archivo es relativa al motor de base de datos de SQL Server especificado en el administrador de conexiones para esta tarea. Es preciso que el motor de base de datos de SQL Server tenga acceso al archivo de texto en un disco duro local en el servidor o mediante una unidad compartida o asignada a SQL Server. El tiempo de ejecución de SSIS no tiene acceso al archivo.  
  
 Si obtiene acceso al archivo de origen utilizando el administrador de conexiones de archivos planos, la tarea Inserción masiva no utiliza el formato especificado en el administrador de conexiones de archivos planos. En su lugar, la tarea Inserción masiva usa el formato especificado en un archivo de formato o los valores de las propiedades RowDelimiter y ColumnDelimiter de la tarea.  
  
 **Temas relacionados:** [Administrador de conexiones de archivos](../../integration-services/connection-manager/file-connection-manager.md), [Administrador de conexiones de archivos planos](../../integration-services/connection-manager/flat-file-connection-manager.md) 
  
 **Actualizar tablas**  
 Actualice la lista de tablas y vistas.  
  
### <a name="format-dynamic-options"></a>Opciones dinámicas de formato  
  
#### <a name="format--use-file"></a>Formato = Utilizar archivo  
 **FormatFile**  
 Escriba la ruta de acceso del archivo de formato, o bien haga clic en el botón de puntos suspensivos ( **…** ) para buscar el archivo de formato.  
  
#### <a name="format--specify"></a>Formato = Especificar  
 **RowDelimiter**  
 Especifique el delimitador de filas en el archivo de origen. El valor predeterminado es **{CR}{LF}** .  
  
 **ColumnDelimiter**  
 Especifique el delimitador de columna en el archivo de origen. El valor predeterminado es **Tab**.  
  
## <a name="bulk-insert-task-editor-general-page"></a>Editor de la tarea Inserción masiva (página General)
  Use la página **General** del cuadro de diálogo **Editor de la tarea Inserción masiva** para asignar un nombre y describir la tarea Inserción masiva.  
  
### <a name="options"></a>Opciones  
 **Nombre**  
 Escriba un nombre único para la tarea Inserción masiva. Este nombre se utiliza como etiqueta en el icono de tarea.  
  
> [!NOTE]  
>  Los nombres de tarea deben ser únicos en un paquete.  
  
 **Descripción**  
 Escriba una descripción de la tarea Inserción masiva.  
 
## <a name="bulk-insert-task-editor-options-page"></a>Editor de la tarea Inserción masiva (página Opciones)
  Use la página **Opciones** del cuadro de diálogo **Editor de la tarea Inserción masiva** para establecer las propiedades de la operación de inserción masiva. La tarea Inserción masiva copia gran cantidad de datos en una tabla o vista de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Para más información sobre las inserciones masivas, vea [Tarea Inserción masiva](../../integration-services/control-flow/bulk-insert-task.md) y [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
### <a name="options"></a>Opciones  
 **CodePage**  
 Especifique la página de códigos de los datos incluidos en el archivo de datos.  
  
 **DataFileType**  
 Especifique el valor del tipo de datos que desea utilizar en la operación de carga.  
  
 **BatchSize**  
 Especifique el número de filas de un lote. El valor predeterminado es todo el archivo de datos. Si establece el valor cero en **BatchSize** , se cargarán los datos en un único lote.  
  
 **LastRow**  
 Especifique la última fila que desea copiar.  
  
 **FirstRow**  
 Especifique la primera fila desde la que desea empezar a copiar.  
  
 **Opciones**  
 |Término|Definición|  
|----------|----------------|  
|**Restricciones CHECK**|Seleccione esta opción para comprobar las restricciones de la tabla y de la columna.|  
|**Mantener valores NULL**|Seleccione esta opción para conservar los valores NULL durante la operación de inserción masiva, en lugar de insertar valores predeterminados en las columnas vacías.|  
|**Habilitar la inserción de identidad**|Seleccione esta opción para insertar valores existentes en una columna de identidad.|  
|**Bloqueo de tabla**|Seleccione esta opción para bloquear la tabla durante la inserción masiva.|  
|**Activar desencadenadores**|Seleccione esta opción para activar desencadenadores de inserción, actualización o eliminación en la tabla.|  
  
 **SortedData**  
 Especifique la cláusula ORDER BY en la instrucción de inserción masiva. El nombre de columna que proporcione debe ser una columna válida de la tabla de destino. El valor predeterminado es **false**. Significa que los datos están ordenados mediante una cláusula ORDER BY.  
  
 **MaxErrors**  
 Especifique el número máximo de errores que pueden producirse antes de que se cancele la operación de inserción masiva. El valor 0 indica que se permite un número infinito de errores.  
  
> [!NOTE]  
>  Cada fila que no pueda importarse con la operación de carga masiva se contabilizará como un error.  

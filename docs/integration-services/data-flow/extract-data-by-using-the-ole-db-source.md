---
description: Extraer datos mediante el origen de OLE DB
title: Extraer datos mediante el origen de OLE DB | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- extracting data [Integration Services]
- sources [Integration Services], OLE DB
- OLE DB source [Integration Services]
ms.assetid: 4ca6eeb5-b60e-4b81-86dd-0674be8ae8d8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 864463e65b5857c3d24710f286f17817d2f48a46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339771"
---
# <a name="extract-data-by-using-the-ole-db-source"></a>Extraer datos mediante el origen de OLE DB

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Para agregar y configurar un origen de OLE DB, el paquete ya debe incluir por lo menos una tarea Flujo de datos.  
  
### <a name="to-extract-data-using-an-ole-db-source"></a>Para extraer datos mediante un Origen de OLE DB  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete para abrirlo.  
  
3.  Haga clic en la pestaña **Flujo de datos** y, a continuación, desde el **cuadro de herramientas**, arrastre el origen de OLE DB hasta la superficie de diseño.  
  
4.  Haga doble clic en el origen de OLE DB.  
  
5.  En el **Editor de origen de OLE DB** , en la página **Administrador de conexiones** , seleccione un administrador de conexiones OLE DB o haga clic en **Nuevo** para crear un nuevo administrador de conexiones. Para más información, consulte [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
6.  Seleccione el método de acceso de datos:  
  
    -   **Tabla o vista** Seleccione una tabla o vista en la base de datos a la que se conecta el administrador de conexiones OLE DB.  
  
    -   **Variable de nombre de tabla o nombre de vista** Seleccione la variable definida por el usuario que contiene el nombre de una tabla o vista en la base de datos a la que se conecta el administrador de conexiones OLE DB.  
  
    -   **Comando SQL** Escriba un comando SQL o haga clic en **Generar consulta** para escribir un comando SQL mediante el **Generador de consultas**.  
  
        > [!NOTE]  
        >  El comando puede incluir parámetros. Para obtener más información, vea [Asignar parámetros de consulta a variables en un componente de flujo de datos](../../integration-services/data-flow/map-query-parameters-to-variables-in-a-data-flow-component.md).  
  
    -   **Comando SQL de variable** Seleccione la variable definida por el usuario que contiene el comando SQL.  
  
        > [!NOTE]  
        >  Las variables se deben definir en el ámbito de la misma tarea Flujo de datos que contiene el origen OLE DB, o en el ámbito del mismo paquete. Además, la variable debe tener un tipo de datos de cadena.  
  
7.  Para actualizar la asignación entre las columnas externas y de salida, haga clic en **Columnas** y seleccione diferentes columnas en la lista **Columna externa** .  
  
8.  Opcionalmente, actualice los nombres de las columnas de salida modificando los valores en la lista **Columna de salida** .  
  
9. Para configurar la salida de error, haga clic en **Salida de error**. Para más información, consulte [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md).  
  
10. Puede hacer clic en **Vista previa** para ver hasta 200 filas de los datos extraídos por el origen de OLE DB.  
  
11. Haga clic en **OK**.  
  
12. Para guardar el paquete actualizado, haga clic en **Guardar los elementos seleccionados**, en el menú **Archivo**.  
  
## <a name="see-also"></a>Consulte también  
 [Origen de OLE DB](../../integration-services/data-flow/ole-db-source.md)   
 [Transformaciones de Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Rutas de Integration Services](../../integration-services/data-flow/integration-services-paths.md)   
 [Tarea Flujo de datos](../../integration-services/control-flow/data-flow-task.md)  
  
  

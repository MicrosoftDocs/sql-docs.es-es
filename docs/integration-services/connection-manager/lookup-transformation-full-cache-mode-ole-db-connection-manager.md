---
description: Transformación Búsqueda en el modo de caché completa - Administrador de conexiones OLE DB
title: Transformación Búsqueda en el modo de caché completa - Administrador de conexiones OLE DB | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Lookup transformation [Integration Services]
ms.assetid: c4150e1b-bdff-4f7a-af4c-3401c34def83
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7abcc50e3400e37219c73d8fbee0746557cc33da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350351"
---
# <a name="lookup-transformation-full-cache-mode---ole-db-connection-manager"></a>Transformación Búsqueda en el modo de caché completa - Administrador de conexiones OLE DB

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Puede configurar la transformación Búsqueda para que use el modo de caché completa y un administrador de conexiones OLE DB. En el modo de caché completa, el conjunto de datos de referencia se carga en la memoria caché antes de que se ejecute la transformación Búsqueda.  
  
 La transformación Búsqueda realiza búsquedas mediante la combinación de datos de las columnas de entrada procedentes de un origen de datos conectado con columnas de un conjunto de datos de referencia. Para más información, consulte [Lookup Transformation](../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
 Al configurar la transformación Búsqueda para que use un administrador de conexiones OLE DB, debe seleccionar una tabla, una vista o una consulta SQL para generar el conjunto de datos de referencia.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-by-using-ole-db-connection-manager"></a>Para implementar una transformación Búsqueda en el modo de caché completa mediante el administrador de conexiones OLE DB  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete con el que desea trabajar y, a continuación, en el Explorador de soluciones, haga doble clic en el paquete.  
  
2.  En la pestaña **Flujo de datos** , en el **Cuadro de herramientas**, arrastre la transformación Búsqueda hasta la superficie de diseño.  
  
3.  Conecte la transformación Búsqueda al flujo de datos arrastrando un conector desde un origen o una transformación anterior hasta la transformación Búsqueda.  
  
    > [!NOTE]  
    >  Una transformación Búsqueda podría no validarse si se conecta a un archivo plano que contiene un campo de fecha vacío. El que la transformación se valide depende de si el administrador de conexiones para el archivo plano se ha configurado para conservar los valores NULL. Para asegurarse de que la transformación Búsqueda se valida, en el **Editor de origen de archivos planos**, de la página **Administrador de conexiones**, seleccione la opción **Conservar los valores null del origen como valores null en el flujo de datos** .  
  
4.  Haga doble clic en el origen o la transformación anterior para configurar el componente.  
  
5.  Haga doble clic en la transformación Búsqueda y, después, en el **Editor de transformación Búsqueda**, en la página **General** , seleccione **Caché completa**.  
  
6.  En el área **Tipo de conexión** , seleccione **Administrador de conexiones OLE DB**.  
  
7.  En la lista **Especificar cómo tratar las filas sin entradas coincidentes** , seleccione una opción de control de errores para las filas sin entradas coincidentes.  
  
8.  En la página Conexión , seleccione un administrador de conexiones en la lista **Administradores de conexión OLE DB** o haga clic en **Nuevo** para crear un nuevo administrador de conexiones. Para más información, consulte [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
9. Realice una de las tareas siguientes:  
  
    -   Haga clic en **Usar una tabla o una vista** y, a continuación, seleccione una tabla o una vista, o haga clic en **Nueva** para crear una tabla o una vista.  
  
         o bien  
  
    -   Haga clic en **Usar los resultados de una consulta SQL** y después genere una consulta en la ventana **Comando SQL** , o haga clic en **Generar consulta** para generar una consulta mediante las herramientas gráficas que proporciona el **Generador de consultas** .  
  
         o bien  
  
    -   Haga clic en **Examinar** para importar una instrucción SQL de un archivo.  
  
     Para validar la consulta SQL, haga clic en **Analizar consulta**.  
  
     Para ver un ejemplo de los datos, haga clic en **Vista previa**.  
  
10. Haga clic en la página **Columnas** y, a continuación, arrastre por lo menos una columna desde la lista **Columnas de entrada disponibles** hasta una columna de la lista **Columnas de búsqueda disponibles** .  
  
    > [!NOTE]  
    >  La transformación Búsqueda asigna automáticamente las columnas que tienen el mismo nombre y el mismo tipo de datos.  
  
    > [!NOTE]  
    >  Las columnas deben tener tipos coincidentes de datos para asignarse. Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
11. Incluya las columnas de búsqueda en el resultado realizando las tareas siguientes:  
  
    1.  En la lista **Columnas de búsqueda disponibles** , seleccione las columnas.  
  
    2.  En la lista **Operación de búsqueda** , especifique si los valores de las columnas de búsqueda reemplazan los valores de la columna de entrada o se escriben en una nueva columna.  
  
12. Para configurar la salida de errores, haga clic en la página **Salida de error** y establezca las opciones de control de errores. Para obtener más información, vea [Editor de transformación Búsqueda &#40;página Salida de error&#41;](../data-flow/transformations/lookup-transformation.md).  
  
13. Haga clic en **Aceptar** para guardar los cambios en la transformación Búsqueda y, a continuación, ejecute el paquete.  
  
## <a name="see-also"></a>Vea también  
 [Implementación de una transformación Búsqueda en el modo de caché completa con el Administrador de conexiones de caché](../../integration-services/connection-manager/lookup-transformation-full-cache-mode-cache-connection-manager.md)   
 [Implementación de una búsqueda en los modos No hay caché o Caché parcial](../../integration-services/data-flow/transformations/implement-a-lookup-in-no-cache-or-partial-cache-mode.md)   
 [Transformaciones de Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  

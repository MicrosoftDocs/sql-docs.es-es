---
description: Editor de origen de SAP BW (página Columnas)
title: Editor de origen de SAP BW (página Columnas) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sapbwsource.columns.f1
ms.assetid: c2ec8bb7-be9b-4783-ad88-32512de784b0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: dab03a43269739bbf77101f8ded9dac68ff2b8f3
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88495768"
---
# <a name="sap-bw-source-editor-columns-page"></a>Editor de origen de SAP BW (página Columnas)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Use la página **Columnas** del **Editor de origen de SAP BW** para asignar una columna de salida a cada columna externa (origen).  
  
 Para obtener más información sobre el componente de origen de SAP BW de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW, vea [Origen de SAP BW](../../integration-services/data-flow/sap-bw-source.md).  
  
> [!IMPORTANT]  
>  La documentación de Microsoft Connector 1.1 for SAP BW da por supuesto que se está familiarizado con el entorno SAP Netweaver BW. Para obtener más información acerca de SAP Netweaver BW, o sobre cómo configurar los objetos y los procesos de SAP Netweaver BW, vea la documentación de SAP.  
  
> [!IMPORTANT]  
>  La extracción de datos de SAP Netweaver BW requiere una licencia SAP adicional. Póngase en contacto con SAP para comprobar estos requisitos.  
  
 **Para abrir la página Columnas**  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el origen de SAP BW.  
  
2.  En la pestaña **Flujo de datos** , haga doble clic en el origen de SAP BW.  
  
3.  En **Editor de origen de SAP BW**, haga clic en **Columnas** para abrir la página **Columnas** del editor.  
  
## <a name="options"></a>Opciones  
  
> [!NOTE]  
>  Si no conoce todos los valores necesarios para configurar el origen, puede que tenga que ponerse en contacto con el administrador de SAP.  
  
 **Columnas externas disponibles**  
 Permite ver la lista de columnas externas disponibles en el origen de datos y, posteriormente, seleccionar las columnas que se van a incluir en el flujo de datos.  
  
 Para incluir una columna en el flujo de datos, active la casilla que se corresponde con esa columna. El orden en el que se activen las casillas determina el orden en el que se generarán las columnas. Es decir, la primera casilla que selecciona será la primera columna de salida, la segunda casilla será la segunda columna de salida y así sucesivamente.  
  
 **Columna externa**  
 Permite ver las columnas externas (origen) seleccionadas. Las columnas seleccionadas aparecen en el orden en el que verá las columnas de salida correspondientes cuando configure los componentes de nivel inferior que utilizan datos de este origen.  
  
 Para cambiar el orden de las columnas, en la lista de **Columnas externas disponibles** , desactive las casillas de todas las columnas. A continuación, seleccione las columnas en el orden en que desea que aparezcan.  
  
 **Columna de salida**  
 Permite proporcionar un nombre único para cada columna de salida. El valor predeterminado es el nombre de la columna externa (origen) seleccionada. Sin embargo, puede especificarse cualquier nombre único que sea descriptivo. [!INCLUDE[ssIS](../../includes/ssis-md.md)] mostrará los nombres de la **Columna de salida** para las columnas al configurar los componentes de nivel inferior que utilizan datos de este origen.  
  
## <a name="see-also"></a>Consulte también  
 [Editor de origen de SAP BW &#40;página Administrador de conexiones&#41;](../../integration-services/data-flow/sap-bw-source-editor-connection-manager-page.md)   
 [Editor de origen de SAP BW &#40;página Salida de error&#41;](../../integration-services/data-flow/sap-bw-source-editor-error-output-page.md)   
 [Editor de origen de SAP BW &#40;página Opciones avanzadas&#41;](../../integration-services/data-flow/sap-bw-source-editor-advanced-page.md)   
 [Ayuda F1 de Microsoft Connector for SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  

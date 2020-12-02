---
description: Crear InfoSource
title: Crear InfoSource | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e7db233b-5464-43de-9d26-6dd24c7ac1b7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c4672ce48890af7202445d283d15c2c167550f5a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127253"
---
# <a name="create-infosource"></a>Crear InfoSource

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Use el cuadro de diálogo **Crear InfoSource** para crear un InfoSource nuevo. Para crear un InfoSource nuevo, puede usar el cuadro de diálogo **Crear InfoSource para los datos de transacción** o **Crear InfoSource para datos maestros** .  
  
 Puede abrir el cuadro de diálogo **Crear InfoSource** desde la página **Administrador de conexiones** del **Editor de destino de SAP BW**. Para más información acerca del destino de SAP BW, consulte [SAP BW Destination](../../integration-services/data-flow/sap-bw-destination.md).  
  
> [!IMPORTANT]  
>  La documentación de Microsoft Connector 1.1 for SAP BW da por supuesto que se está familiarizado con el entorno SAP Netweaver BW. Para obtener más información acerca de SAP Netweaver BW, o sobre cómo configurar los objetos y los procesos de SAP Netweaver BW, vea la documentación de SAP.  
  
 **Para abrir el cuadro de diálogo Crear InfoSource**  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el destino SAP BW.  
  
2.  En la pestaña **Flujo de datos** , haga doble clic en el destino de SAP BW.  
  
3.  En **Editor de destino SAP BW**, haga clic en **Administrador de conexiones** para abrir la página **Administrador de conexiones** del editor.  
  
4.  En la página **Administrador de conexiones** , en el cuadro del grupo **Crear objetos de SAP BW** , seleccione **InfoSource** y, a continuación, haga clic en **Crear**.  
  
## <a name="options"></a>Opciones  
 **Datos de transacción**  
 Permite crear un InfoSource nuevo para los datos de transacción.  
  
 Si selecciona esta opción, se abrirá el cuadro de diálogo **Crear InfoSource para datos de transacción** . Use el cuadro de diálogo **Crear InfoSource para datos de transacción** para crear el InfoSource nuevo. Para obtener más información sobre este cuadro de diálogo, vea [Create InfoSource for Transaction Data](../../integration-services/data-flow/create-infosource-for-transaction-data.md).  
  
 **Datos maestros**  
 Permite crear un InfoSource nuevo para los datos maestros.  
  
 Si selecciona esta opción, se abrirá el cuadro de diálogo **Crear InfoSource para datos maestros** . Use el cuadro de diálogo **Crear InfoSource para datos maestros** para crear el InfoSource nuevo. Para obtener más información sobre este cuadro de diálogo, vea [Create InfoSource for Master Data](../../integration-services/data-flow/create-infosource-for-master-data.md).  
  
## <a name="see-also"></a>Consulte también  
 [Ayuda F1 de Microsoft Connector for SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  

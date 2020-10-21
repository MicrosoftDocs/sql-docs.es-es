---
description: Destino de SAP BW
title: Destino de SAP BW | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: a612ed91-b89b-4173-a0b1-0bce381e1e28
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d46d3598bb309029319c1b74b467983e681b96ac
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92197060"
---
# <a name="sap-bw-destination"></a>Destino de SAP BW

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El destino de SAP BW es el componente de destino de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW. De esta forma, el destino de SAP BW carga los datos desde el flujo de datos de un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en la versión 7 del sistema SAP Netweaver BW.  
  
 Este destino tienen una entrada y una salida de error.  
  
> [!IMPORTANT]  
>  La documentación de Microsoft Connector 1.1 for SAP BW da por supuesto que se está familiarizado con el entorno SAP Netweaver BW. Para obtener más información acerca de SAP Netweaver BW, o sobre cómo configurar los objetos y los procesos de SAP Netweaver BW, vea la documentación de SAP.  
  
 Para usar el destino de SAP BW, deberá realizar las tareas siguientes:  
  
-   [Preparar los objetos de SAP Netweaver BW](#bkmk_Prepare_Objects)  
  
-   [Conectarse al sistema SAP Netweaver BW](#bkmk_Connect_Database)  
  
-   [Configurar el destino de SAP BW](#bkmk_Configure_Destination)  
  
##  <a name="preparing-the-sap-netweaver-bw-objects-that-the-destination-requires"></a><a name="bkmk_Prepare_Objects"></a> Preparar los objetos SAP Netweaver BW que requiera el destino  
 El destino de SAP BW requiere que ciertos objetos estén en el sistema SAP Netweaver BW antes de que pueda funcionar el destino. Si estos objetos no existen aún, debe seguir estos pasos para crear y configurar estos objetos en el sistema SAP Netweaver BW.  
  
> [!NOTE]  
>  Para obtener detalles adicionales sobre estos objetos y pasos de configuración, vea la documentación sobre SAP Netweaver BW.  
  
1.  Crear un nuevo sistema de origen:  
  
    1.  Seleccione el tipo **"Terceros/BAPI de almacenamiento provisional"**.  
  
    2.  Para **Tipo de comunicación con sistema de destino**, seleccione **No Unicode (valores inactivos de MDMP)**.  
  
    3.  Asignar un identificador de programa adecuado  
  
2.  Asigne el sistema de origen a un InfoSource.  
  
3.  Cree un InfoPackage.  
  
     El InfoPackage es el objeto más importante que requiere el destino de SAP BW.  
  
 También puede crear InfoObjects, InfoCubes, InfoSources e InfoPackages adicionales según sean necesarios para respaldar la carga de datos en el sistema SAP Netweaver BW.  
  
##  <a name="connecting-to-the-sap-netweaver-bw-system"></a><a name="bkmk_Connect_Database"></a> Conectarse al sistema SAP Netweaver BW  
 Para conectarse al sistema SAP Netweaver BW de la versión 7, el destino de SAP BW usa un administrador de conexiones para SAP BW que forma parte del paquete [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW. El administrador de conexiones de SAP BW es el único administrador de conexiones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que puede usar el destino de SAP BW.  
  
 Para obtener más información sobre el administrador de conexiones de SAP BW, vea [SAP BW Connection Manager](../../integration-services/connection-manager/sap-bw-connection-manager.md).  
  
##  <a name="configuring-the-sap-bw-destination"></a><a name="bkmk_Configure_Destination"></a> Configurar el destino de SAP BW  
 Puede configurar el destino de SAP BW de las maneras siguientes:  
  
-   Busque y seleccione el InfoPackage que se va a usar para cargar datos.  
  
-   Asigne cada columna del flujo de datos al InfoObject correcto en el InfoPackage.  
  
-   Especifique cuántas filas de datos se transfieren a la vez mediante la configuración de la propiedad **PackageSize** .  
  
-   Elija esperar hasta el sistema SAP Netweaver BW haya completado la carga de datos.  
  
-   Elija que no se desencadene el InfoPackage, pero que espere a recibir la notificación de que el sistema SAP Netweaver BW ha comenzado a cargar datos.  
  
-   Opcionalmente, desencadene otra cadena de procesos después de haber completado la carga de los datos.  
  
-   Opcionalmente, puede crear los objetos de SAP Netweaver BW que necesita el destino. Lo cual incluye la creación de InfoObjects, InfoCubes, InfoSources e InfoPackages.  
  
-   Pruebe la carga de datos con las opciones que ha seleccionado.  
  
 También puede habilitar el registro de las llamadas por parte del destino. (Este registro es independiente del registro opcional que puede habilitar en los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ). Las llamadas a funciones RFC se habilitan cuando se configura el administrador de conexiones SAP BW que va a usar el destino. Para obtener más información sobre cómo configurar el administrador de conexiones de SAP BW, vea [SAP BW Connection Manager](../../integration-services/connection-manager/sap-bw-connection-manager.md).  
  
 Si no conoce todos los valores necesarios para configurar el destino, puede que tenga que ponerse en contacto con el administrador de SAP.  
  
 Para obtener instrucciones sobre cómo configurar y utilizar el administrador de conexiones, el origen y el destino de SAP BW, vea las notas del producto, [Usar SQL Server 2008 Integration Services con SAP BI 7.0](/previous-versions/sql/sql-server-2008/dd299430(v=sql.100)). Estas notas del producto también muestran cómo configurar los objetos necesarios en SAP BW.  
  
### <a name="using-the-ssis-designer-to-configure-the-destination"></a>Usar el Diseñador SSIS para configurar el destino  
 Para obtener más información sobre las propiedades del destino de SAP BW que puede establecer en el Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en uno de los temas siguientes:  
  
-   [Editor de destino de SAP BW &#40;página Administrador de conexiones&#41;](../../integration-services/data-flow/sap-bw-destination-editor-connection-manager-page.md)  
  
-   [Editor de destino de SAP BW &#40;página Asignaciones&#41;](../../integration-services/data-flow/sap-bw-destination-editor-mappings-page.md)  
  
-   [Editor de destino de SAP BW &#40;página Salida de error&#41;](../../integration-services/data-flow/sap-bw-destination-editor-error-output-page.md)  
  
-   [Editor de destino de SAP BW &#40;página Opciones avanzadas&#41;](../../integration-services/data-flow/sap-bw-destination-editor-advanced-page.md)  
  
 Mientras configura el destino de SAP BW, también puede usar varios cuadros de diálogo para buscar o crear objetos de SAP Netweaver BW. Para obtener más información sobre estos cuadros de diálogo, haga clic en uno de los temas siguientes:  
  
-   [Buscar InfoPackage](../../integration-services/data-flow/look-up-infopackage.md)  
  
-   [Crear nuevo InfoObject](../../integration-services/data-flow/create-new-infoobject.md)  
  
-   [Crear InfoCube para los datos de transacción](../../integration-services/data-flow/create-infocube-for-transaction-data.md)  
  
-   [Buscar InfoObject](../../integration-services/data-flow/look-up-infoobject.md)  
  
-   [Crear InfoSource](../../integration-services/data-flow/create-infosource.md)  
  
-   [Crear Infosource para datos de transacción](../../integration-services/data-flow/create-infosource-for-transaction-data.md)  
  
-   [Crear InfoSource para datos maestros](../../integration-services/data-flow/create-infosource-for-master-data.md)  
  
-   [Crear InfoPackage](../../integration-services/data-flow/create-infopackage.md)  
  
## <a name="see-also"></a>Vea también  
 [Componentes de Microsoft Connector for SAP BW](../../integration-services/microsoft-connector-for-sap-bw-components.md)  
  

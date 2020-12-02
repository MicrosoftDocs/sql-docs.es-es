---
description: Manage an Oracle CDC Service
title: Administrar Oracle CDC Service | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- createSrv
ms.assetid: 5972cee3-b1a9-4c56-aed6-bdddf84af283
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aea4d1beb0a77e6bc7a0456c3f3437c919698bcd
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88426077"
---
# <a name="manage-an-oracle-cdc-service"></a>Manage an Oracle CDC Service

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Puede usar la Consola de configuración del servicio CDC para administrar un servicio CDC específico.  
  
 **Para seleccionar el servicio CDC con el que desea trabajar**  
  
1.  En el panel izquierdo de la Consola de configuración del servicio CDC, expanda **Servicios CDC locales**.  
  
2.  Seleccione el servicio CDC con el que desea trabajar.  
  
     También puede hacer clic con el botón secundario en el servicio CDC con el que desea trabajar y seleccionar la acción deseada. Consulte [Lo que puede hacer con un servicio CDC](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md#BKMK_WhatcandowithCDCService).  
  
 **OR**  
  
1.  Seleccione **Servicios CDC locales** en el panel izquierdo de la Consola de configuración del servicio CDC.  
  
2.  En la sección central de la Consola de configuración del servicio CDC, seleccione el servicio con el que desea trabajar.  
  
     También puede hacer clic con el botón secundario en el servicio CDC con el que desea trabajar y seleccionar la acción deseada. Consulte [Lo que puede hacer con un servicio CDC](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md#BKMK_WhatcandowithCDCService).  
  
##  <a name="what-can-you-do-with-a-cdc-service"></a><a name="BKMK_WhatcandowithCDCService"></a> Lo que puede hacer con un servicio CDC  
 Puede realizar las acciones siguientes al trabajar con un servicio CDC.  
  
### <a name="delete-the-service"></a>Eliminar el servicio  
 En el panel **Acciones** del lado derecho de la Consola de configuración del servicio CDC, haga clic en **Eliminar** para eliminar el servicio.  
  
 También puede hacer clic con el botón derecho en el servicio CDC que quiera eliminar y seleccionar **Eliminar**.  
  
 **Nota**: si el servicio se está ejecutando cuando se elimina el servicio, se detendrá el servicio antes de eliminarlo.  
  
 Para eliminar la definición del servicio de Windows CDC de Oracle, el programa necesita acceso de actualización a la base de datos MSXDBCDC en la instancia asociada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Al hacer clic en Aceptar para eliminar el servicio, el programa intenta eliminar el registro del servicio CDC de Oracle en la base de datos MSXDBCDC. Si el programa no puede eliminar el registro del servicio CDC de Oracle porque no tiene los permisos adecuados, pedirá al usuario que especifique un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con permisos de actualización para la base de datos MSXDBCDC.  
  
 Para obtener información acerca de los datos que debe especificar en el cuadro de diálogo Conectar con SQL Server, vea [Connection to SQL Server for Delete](../../integration-services/change-data-capture/connection-to-sql-server-for-delete.md).  
  
### <a name="edit-the-cdc-service-properties"></a>Editar las propiedades del servicio CDC  
 En el panel **Acciones** del lado derecho de la Consola de configuración del servicio CDC, haga clic en **Propiedades**.  
  
 También puede hacer clic con el botón derecho en el servicio CDC cuyas propiedades quiera editar y seleccionar **Propiedades**.  
  
## <a name="see-also"></a>Consulte también  
 [Cómo administrar un servicio CDC local](../../integration-services/change-data-capture/how-to-manage-a-local-cdc-service.md)  
  
  

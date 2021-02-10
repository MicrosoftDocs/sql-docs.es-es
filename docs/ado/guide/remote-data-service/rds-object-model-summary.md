---
description: Resumen del modelo de objetos de RDS
title: Resumen del modelo de objetos RDS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS objects [ADO], object model summary
- RDS object model [ADO]
ms.assetid: 909f9af7-31db-4eec-ad52-650ce74dac2f
author: rothja
ms.author: jroth
ms.openlocfilehash: 67f6cdaeea36e5ed13e28a7674a0bb4b3d6ef3f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036465"
---
# <a name="rds-object-model-summary"></a>Resumen del modelo de objetos de RDS
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
|Object|Descripción|  
|------------|-----------------|  
|[ActiveX. DataSpace](../../reference/rds-api/dataspace-object-rds.md)|Este objeto contiene un método para obtener un proxy de servidor. El proxy puede ser el predeterminado o un programa de servidor personalizado (objeto comercial). El programa de servidor se puede invocar en Internet, en una intranet, en una red de área local o en una biblioteca de vínculos dinámicos local.<br /><br /> El objeto **DataSpace** es seguro para el scripting.|  
|[RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)|Este objeto representa el programa de servidor predeterminado. Ejecuta la recuperación de datos y el comportamiento de actualización de RDS predeterminados.<br /><br /> El objeto **DataFactory** no es seguro para el scripting.|  
|[ActiveX. DataControl](../../reference/rds-api/datacontrol-object-rds.md)|Este objeto puede invocar automáticamente a **RDS. Objetos DataSpace** y **RDSServer. DataFactory** .<br /><br /> Utilice este objeto para invocar la recuperación de datos predeterminada de RDS o el comportamiento de actualización.<br /><br /> Este objeto también proporciona los medios para que los controles visuales tengan acceso al objeto de **conjunto de registros** devuelto.<br /><br /> El objeto **DataControl** es seguro para el scripting.|  
  
## <a name="see-also"></a>Consulte también  
 [Aspectos básicos de RDS](./rds-fundamentals.md)   
 [Escenario de RDS](./rds-scenario.md)   
 [Tutorial de RDS](./rds-tutorial.md)   
 [Seguridad y uso de RDS](./rds-usage-and-security.md)
---
description: Propiedad InternetTimeout (RDS)
title: Propiedad InternetTimeout (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- InternetTimeout property [ADO]
ms.assetid: 4d1c8892-4bbc-4e71-bf4b-ba52c0ea9549
author: rothja
ms.author: jroth
ms.openlocfilehash: 27e3d652a6cecd341fe048ff67182ee7cf89b7a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049365"
---
# <a name="internettimeout-property-rds"></a>Propiedad InternetTimeout (RDS)
Indica el número de milisegundos que hay que esperar antes de que se agote el tiempo de espera de una solicitud.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor **Long** que representa el número de milisegundos antes de que se agote el tiempo de espera de una solicitud.  
  
## <a name="remarks"></a>Observaciones  
 Esta propiedad solo se aplica a las solicitudes enviadas con los protocolos HTTP o HTTPS.  
  
 Las solicitudes en un entorno de tres niveles pueden tardar varios minutos en ejecutarse. Utilice esta propiedad para especificar el tiempo adicional para las solicitudes de ejecución prolongada.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
    :::column-end:::
    :::column:::
        [Objeto DataSpace (RDS)](./dataspace-object-rds.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad InternetTimeout (VB)](./internettimeout-property-example-vb.md)   
 [Ejemplo de la propiedad InternetTimeout (VC ++)](./internettimeout-property-example-vc.md)
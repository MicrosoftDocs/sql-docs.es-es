---
description: Objeto DataSpace (RDS)
title: Objeto DataSpace (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- DataSpace object [RDS]
ms.assetid: 9194bffa-5bdf-4dff-af86-f7158c23bfa7
author: rothja
ms.author: jroth
ms.openlocfilehash: d9ca4337b26a52c28619f703456693a0c715a86a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168969"
---
# <a name="dataspace-object-rds"></a>Objeto DataSpace (RDS)
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Crea proxies del lado cliente para objetos comerciales personalizados ubicados en el nivel intermedio.  
  
 El servicio de datos remoto necesita servidores proxy de objetos empresariales para que los componentes del lado cliente puedan comunicarse con los objetos empresariales ubicados en el nivel intermedio. Los proxies facilitan el empaquetado, desempaquetado y transporte (serialización) de los datos del [conjunto de registros](../ado-api/recordset-object-ado.md) de la aplicación a través de los límites del proceso o del equipo.  
  
 El servicio de datos remotos utiliza **RDS.** Método [CreateObject](./createobject-method-rds.md) del objeto DataSpace para crear servidores proxy de objeto comercial. El proxy de objeto comercial se crea dinámicamente cada vez que se crea una instancia de su equivalente de objeto comercial de nivel intermedio. El servicio de datos remotos admite los siguientes protocolos: HTTP, HTTPS (sockets seguros HTTP), DCOM y en proceso (los componentes de cliente y el objeto comercial residen en el mismo equipo).  
  
> [!NOTE]
>  RDS se comporta de manera "sin estado" cuando el **objeto RDS.** El objeto DataSpace usa los protocolos http o https. Es decir, cualquier información interna sobre una solicitud de cliente se descarta después de que el servidor devuelva una respuesta.  
  
> [!NOTE]
>  Aunque el objeto comercial parece existir durante la vigencia del proxy del objeto comercial, el objeto comercial solo existe hasta que se envía una respuesta a una solicitud. Cuando se emite una solicitud (es decir, se invoca un método en el objeto comercial), el proxy abre una nueva conexión al servidor y el servidor crea una nueva instancia del objeto comercial. Una vez que el objeto comercial responde a la solicitud, el servidor destruye el objeto comercial y cierra la conexión.  
  
> [!NOTE]
>  Este comportamiento significa que no se pueden pasar datos de una solicitud a otra mediante una propiedad o una variable de objeto comercial. Debe emplear algún otro mecanismo, como un archivo o un argumento de método, para conservar los datos de estado.  
  
 IDENTIFICADOR de clase del **objeto RDS.** El objeto DataSpace es BD96C556-65A3-11D0-983A-00C04FC29E36.  
  
 El objeto **DataSpace** es seguro para el scripting.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Eventos, métodos y propiedades del objeto DataSpace (RDS)](./dataspace-object-rds-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [DataSpace y el ejemplo del método CreateObject (VBScript)](./dataspace-object-and-createobject-method-example-vbscript.md)
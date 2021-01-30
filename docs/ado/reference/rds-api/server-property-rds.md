---
description: Propiedad del servidor (RDS)
title: Propiedad Server (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
f1_keywords:
- RDS::IBindMgr21::Server
helpviewer_keywords:
- Server property [RDS]
ms.assetid: d2727ce7-da9f-4271-ae3c-9334ef477c14
author: rothja
ms.author: jroth
ms.openlocfilehash: 373b43f3540d0dcb99349ca34d0a424bc6235f73
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166132"
---
# <a name="server-property-rds"></a>Propiedad del servidor (RDS)
Indica el nombre del Internet Information Services (IIS) y el protocolo de comunicación.  
  
 Puede establecer la propiedad de **servidor** en tiempo de diseño en las etiquetas de objeto de [RDS. Objeto DataControl](./datacontrol-object-rds.md) , o en tiempo de ejecución en el código de scripting.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
 **HTTP**  
  
 Sintaxis en tiempo de diseño  
  
```  
  
<PARAM NAME="Server" VALUE="http:  
//awebsrvr:port  
">  
  
```  
  
 Sintaxis de tiempo de ejecución  
  
```  
  
DataControl  
.Server="https://  
awebsrvr:port  
"  
  
```  
  
 **HTTPS**  
  
 Sintaxis en tiempo de diseño  
  
```  
  
<PARAM NAME="Server" VALUE="https://awebsrvr:port">  
```  
  
 Sintaxis de tiempo de ejecución  
  
```  
  
DataControl.Server="https://awebsrvr:port"  
```  
  
 **DCOM**  
  
 Sintaxis en tiempo de diseño  
  
```  
  
<PARAM NAME="Server" VALUE="  
computername  
">  
  
```  
  
 Sintaxis de tiempo de ejecución  
  
```  
  
DataControl.Server="computername"  
```  
  
 **En proceso**  
  
 Sintaxis en tiempo de diseño  
  
```  
  
<PARAM NAME="Server" VALUE="">  
  
```  
  
 Sintaxis de tiempo de ejecución  
  
```  
  
DataControl.Server=""  
```  
  
## <a name="parameters"></a>Parámetros  
 *awebsrvr* o *ComputerName*  
 Valor de **cadena** que contiene una ruta de acceso de Internet o de la intranet, o un nombre de equipo, si el servidor está en un equipo remoto; o bien, una cadena vacía si el servidor está en el equipo local.  
  
 *port*  
 Opcional. Puerto que se utiliza para conectarse a un servidor que ejecuta IIS. El número de puerto se establece en Internet Explorer (en el menú **Ver** , haga clic en **Opciones** y, a continuación, seleccione la pestaña **conexión** ) o en IIS.  
  
 *DataControl*  
 Variable de objeto que representa un objeto **RDS. Objeto DataControl** .  
  
## <a name="remarks"></a>Observaciones  
 El servidor es la ubicación donde se encuentra el **objeto RDS.** La solicitud DataControl (es decir, una consulta o una actualización) se procesa. De forma predeterminada, todas las solicitudes se procesan mediante el objeto [RDSServer. DataFactory](./datafactory-object-rdsserver.md) , [MSDFMAP. ](../../guide/remote-data-service/datafactory-customization.md) Componente de controlador y [MSDFMAP.INI](../../guide/remote-data-service/understanding-the-customization-file.md) archivo en el servidor especificado. Recuerde que al cambiar los servidores para conciliar la configuración de los archivos de **MSDFMAP.INI** antiguos y nuevos. Las incompatibilidades pueden provocar errores en las solicitudes que se realizan correctamente en un servidor en otro. Si la propiedad del servidor se establece en la cadena vacía "", estos objetos se utilizarán en el equipo local.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad Server (VBScript)](./server-property-example-vbscript.md)   
 [Propiedad Connect (RDS)](./connect-property-rds.md)   
 [Propiedad SQL](./sql-property.md)   
 [Método SubmitChanges (RDS)](./submitchanges-method-rds.md)
---
description: Modelo detallado de programación de RDS
title: Modelo de programación de RDS en detalle | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS programming model [ADO], details
ms.assetid: 3e57af8d-519b-4467-a0bd-af468534cefd
author: rothja
ms.author: jroth
ms.openlocfilehash: 8421340798a308204a0961340e06216f3b0cb24a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031897"
---
# <a name="rds-programming-model-in-detail"></a>Modelo detallado de programación de RDS
Los siguientes son elementos clave del modelo de programación de RDS:  
  
-   ActiveX. DataSpace  
  
-   RDSServer. DataFactory  
  
-   ActiveX. DataControl  
  
-   Evento  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="rdsdataspace"></a>ActiveX. DataSpace  
 La aplicación cliente debe especificar el servidor y el programa de servidor que se va a invocar. A cambio, la aplicación recibe una referencia al programa de servidor y puede tratar la referencia como si fuera el propio programa de servidor.  
  
 El modelo de objetos RDS incluye esta funcionalidad con [RDS. Objeto DataSpace](../../reference/rds-api/dataspace-object-rds.md) .  
  
 El programa de servidor se especifica con un identificador de programa o *ProgID*. El servidor utiliza el *ProgID* y el registro del equipo servidor para buscar información acerca del programa real que se va a iniciar.  
  
 RDS realiza una distinción internamente en función de si el programa de servidor está en un servidor remoto a través de Internet o de una intranet. un servidor en una red de área local; o no en un servidor, sino en una biblioteca de vínculos dinámicos (DLL) local. Esta distinción determina cómo se intercambia información entre el cliente y el servidor, y realiza una diferencia tangible en el tipo de referencia que se devuelve a la aplicación cliente. Sin embargo, desde el punto de vista, esta distinción no tiene ningún significado especial. Lo único que importa es que reciba una referencia de programa utilizable.  
  
## <a name="rdsserverdatafactory"></a>RDSServer. DataFactory  
 RDS proporciona un programa de servidor predeterminado que puede realizar una consulta SQL en el origen de datos y devolver un objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) , o bien tomar un objeto de **conjunto de registros** y actualizar el origen de datos.  
  
 El modelo de objetos RDS incluye esta funcionalidad con el objeto [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) .  
  
 Además, este objeto tiene un método para crear un objeto de **conjunto de registros** vacío que se puede rellenar mediante programación ([CreateRecordset](../../reference/rds-api/createrecordset-method-rds.md)) y otro método para convertir un objeto de **conjunto de registros** en una cadena de texto para compilar una página web ([ConvertToString](../../reference/rds-api/converttostring-method-rds.md)).  
  
 Con ADO, puede invalidar algunos de los comportamientos estándar de conexión y comando de **RDSServer. DataFactory** con un controlador **DataFactory** y un archivo de personalización que contiene los parámetros de conexión, comando y seguridad.  
  
 El programa de servidor se denomina a veces *objeto comercial*. Puede escribir su propio objeto comercial personalizado que puede realizar un acceso a datos complicado, comprobaciones de validez, etc. Incluso al escribir un objeto comercial personalizado, puede crear una instancia de un objeto **RDSServer. DataFactory** y usar algunos de sus métodos para realizar sus propias tareas.  
  
## <a name="rdsdatacontrol"></a>ActiveX. DataControl  
 RDS proporciona un medio para combinar la funcionalidad de **RDS. DataSpace** y **RDSServer. DataFactory**, y también permiten a los controles visuales usar fácilmente el objeto de **conjunto de registros** devuelto por una consulta de un origen de datos. RDS intenta, en el caso más común, hacer lo máximo posible para obtener acceso automáticamente a la información de un servidor y mostrarlo en un control visual.  
  
 El modelo de objetos RDS incluye esta funcionalidad con [RDS. Objeto DataControl](../../reference/rds-api/datacontrol-object-rds.md) .  
  
 **Objeto RDS. DataControl** tiene dos aspectos. Un aspecto pertenece al origen de datos. Si establece la información de conexión y comando mediante las propiedades **Connect** y **SQL** de **RDS. DataControl**, usará automáticamente **RDS. DataSpace** para crear una referencia al objeto **RDSServer. DataFactory** predeterminado. A continuación, **RDSServer. DataFactory** usará el valor de la propiedad **Connect** para conectarse al origen de datos, usar el valor de la propiedad **SQL** para obtener un **conjunto de registros** del origen de datos y devolver el objeto de conjunto de **registros** a **RDS. DataControl**.  
  
 El segundo aspecto pertenece a la presentación de la información de **conjunto de registros** devuelta en un control visual. Puede asociar un control visual a **RDS. DataControl** (en un proceso llamado enlace) y obtener acceso a la información del objeto de **conjunto de registros** asociado, mostrando los resultados de la consulta en una página Web de Microsoft® Internet Explorer. Cada **RDS.** El objeto DataControl enlaza un objeto de **conjunto de registros** , que representa los resultados de una sola consulta, a uno o más controles visuales (por ejemplo, un cuadro de texto, un cuadro combinado, un control de cuadrícula, etc.). Puede haber más de un **RDS. Objeto DataControl** en cada página. Cada **RDS.** El objeto DataControl puede estar conectado a un origen de datos diferente y contener los resultados de una consulta independiente.  
  
 **Objeto RDS.** El objeto DataControl también tiene sus propios métodos para navegar, ordenar y filtrar las filas del objeto de conjunto de **registros** asociado. Estos métodos son similares, pero no son los mismos que los métodos del objeto de **conjunto de registros** de ADO.  
  
## <a name="events"></a>Events  
 RDS admite dos de sus propios eventos, que son independientes del modelo de eventos de ADO. Se llama al evento [onreadystatechange](../../reference/rds-api/onreadystatechange-event-rds.md) siempre que el **objeto RDS.** Los cambios en la propiedad [ReadyState](../../reference/rds-api/readystate-property-rds.md) de DataControl, lo que le notifica cuando una operación asincrónica ha finalizado correctamente, finalizado o se ha producido un error. Se llama al evento [OnError](../../reference/rds-api/onerror-event-rds.md) siempre que se produce un error, incluso si se produce un error durante una operación asincrónica.  
  
> [!NOTE]
>  Microsoft Internet Explorer proporciona dos eventos adicionales a RDS: **onDataSetChanged**, que indica que el **conjunto de registros** es funcional pero sigue recuperando filas, y **onDataSetComplete**, que indica que el **conjunto de registros** ha terminado de recuperar filas.  
  
## <a name="see-also"></a>Consulte también  
 [Modelo de programación de RDS con objetos](./rds-programming-model-with-objects.md)   
 [Objeto DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md)   
 [Objeto DataFactory (RDSServer)](../../reference/rds-api/datafactory-object-rdsserver.md)   
 [Objeto DataSpace (RDS)](../../reference/rds-api/dataspace-object-rds.md)   
 [Escenario de RDS](./rds-scenario.md)   
 [Tutorial de RDS](./rds-tutorial.md)   
 [Seguridad y uso de RDS](./rds-usage-and-security.md)
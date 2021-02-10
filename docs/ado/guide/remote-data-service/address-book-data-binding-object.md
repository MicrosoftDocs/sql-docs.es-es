---
description: Objeto de enlace de datos de la libreta de direcciones
title: Data-Binding objeto de la libreta de direcciones | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS scenarios [ADO], data-binding object
- address book application scenario [ADO], data-binding object
ms.assetid: 080c1925-d453-4b89-92ac-c93591490518
author: rothja
ms.author: jroth
ms.openlocfilehash: 91489445aef2034ed273a6d13cc5d6ae02b7f519
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036595"
---
# <a name="address-book-data-binding-object"></a>Objeto de enlace de datos de la libreta de direcciones
La aplicación de libreta de direcciones utiliza [RDS. Objeto DataControl](../../reference/rds-api/datacontrol-object-rds.md) para enlazar datos de la base de datos SQL Server a un objeto visual (en este caso, una tabla DHTML) en la página HTML del cliente de la aplicación. La lógica del programa VBScript orientada a eventos utiliza [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md) para:  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
-   Consultar la base de datos, enviar actualizaciones a la base de datos y actualizar la cuadrícula de datos.  
  
-   Permitir a los usuarios moverse al primer, siguiente, anterior o último registro en la cuadrícula de datos.  
  
 En el código siguiente se define el **objeto RDS. Componente DataControl** :  
  
```vb
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
   ID=DC1 Width=1 Height=1>  
   <PARAM NAME="SERVER" VALUE="https://<%=Request.ServerVariables("SERVER_NAME")%>">  
   <PARAM NAME="CONNECT" VALUE="Provider=sqloledb;  
Initial Catalog=AddrBookDb;Integrated Security=SSPI;">  
</OBJECT>  
```  
  
 La etiqueta OBJECT define el objeto **RDS. Componente DataControl** en el programa. La etiqueta incluye dos tipos de parámetros:  
  
-   Los asociados a la etiqueta de objeto genérico.  
  
-   Los específicos del **objeto RDS. Objeto DataControl** .  
  
## <a name="generic-object-tag-parameters"></a>Parámetros de etiqueta de objeto genérico  
 En la tabla siguiente se describen los parámetros asociados a la etiqueta de objeto.  
  
|Parámetro|Descripción|  
|---------------|-----------------|  
|***CLASSID** _|Número único de 128 bits que identifica el tipo de objeto incrustado en el sistema. Este identificador se mantiene en el registro del sistema del equipo local. (Para los identificadores de clase de _ *RDS. Objeto DataControl**, vea [RDS. Objeto DataControl](../../reference/rds-api/datacontrol-object-rds.md)).|  
|***Id***|Define un identificador para todo el documento para el objeto incrustado que se utiliza para identificarlo en el código.|  
  
## <a name="rdsdatacontrol-tag-parameters"></a>ActiveX. Parámetros de etiqueta DataControl  
 En la tabla siguiente se describen los parámetros específicos del **objeto RDS. Objeto DataControl** . (Para obtener una lista completa de **RDS.** Los parámetros del objeto DataControl y cuándo implementarlos, vea [RDS. Objeto DataControl](../../reference/rds-api/datacontrol-object-rds.md)).  
  
|Parámetro|Descripción|  
|---------------|-----------------|  
|[SERVIDOR](../../reference/rds-api/server-property-rds.md)|Si usa HTTP, el valor es el nombre del equipo servidor precedido por `https://` .|  
|[CONNECT](../../reference/rds-api/connect-property-rds.md)|Proporciona la información de conexión necesaria para el **objeto RDS. DataControl** para conectarse a SQL Server.|  
|[SQL](../../reference/rds-api/sql-property.md)|Establece o devuelve la cadena de consulta utilizada para recuperar el [conjunto de registros](../../reference/ado-api/recordset-object-ado.md).|  
  
## <a name="see-also"></a>Consulte también  
 [Botones de comando de la libreta de direcciones](./address-book-command-buttons.md)
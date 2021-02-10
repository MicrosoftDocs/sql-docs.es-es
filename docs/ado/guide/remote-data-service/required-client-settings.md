---
description: Configuración de cliente requerida
title: Configuración de cliente requerida | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory handler in RDS [ADO]
ms.assetid: e776b4e3-fcc4-4bfb-a7e8-5ffae1d83833
author: rothja
ms.author: jroth
ms.openlocfilehash: c33a92334c410e7cd2d33cc7f48360a3e44b15eb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031837"
---
# <a name="required-client-settings"></a>Configuración de cliente requerida
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Especifique la siguiente configuración para usar un controlador de **DataFactory** personalizado.  
  
-   Especifique "Provider = MS Remote" en la propiedad de la propiedad del [proveedor](../../reference/ado-api/provider-property-ado.md) de objetos de [objeto de conexión (ADO](../../reference/ado-api/connection-object-ado.md) ) o la palabra clave "**Provider**=" **de la cadena** de conexión del objeto de conexión.  
  
-   Establezca la propiedad [CursorLocation (propiedad de ADO)](../../reference/ado-api/cursorlocation-property-ado.md) en **adUseClient**.  
  
-   Especifique el nombre del controlador que se va a usar en la propiedad de **controlador** del objeto [DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md) o la palabra clave "**handler**=" de la cadena de conexión del objeto de [conjunto de registros (ADO)](../../reference/ado-api/recordset-object-ado.md) . (El controlador no se puede establecer en la cadena de conexión del objeto de **conexión** ).  
  
 RDS proporciona un controlador predeterminado en el servidor denominado **MSDFMAP. Controlador**. (El archivo de personalización predeterminado se denomina MSDFMAP.INI).  
  
 **Ejemplo**  
  
 Supongamos que las siguientes secciones de **MSDFMAP.INI** y el nombre del origen de datos, AdvWorks, se han definido previamente:  
  
```console
[connect CustomerDataBase]  
Access=ReadWrite  
Connect="DSN=AdvWorks"  
  
[sql CustomerById]  
SQL="SELECT * FROM Customers WHERE CustomerID = ?"  
```  
  
 Los fragmentos de código siguientes se escriben en Visual Basic:  
  
## <a name="rdsdatacontrol-version"></a>ActiveX. Versión de control de DataControl  
  
```vb
Dim dc as New RDS.DataControl  
Set dc.Handler = "MSDFMAP.Handler"  
Set dc.Server = "https://yourServer"  
Set dc.Connect = "Data Source=CustomerDatabase"  
Set dc.SQL = "CustomerById(4)"  
dc.Refresh  
```  
  
## <a name="recordset-version"></a>Versión del conjunto de registros  
  
```vb
Dim rs as New ADODB.Recordset  
rs.CursorLocation = adUseClient  
```  
  
 Especifique la propiedad o palabra clave de la [propiedad de controlador (RDS)](../../reference/rds-api/handler-property-rds.md) ; propiedad o palabra clave de la [propiedad del proveedor (ADO)](../../reference/ado-api/provider-property-ado.md) ; y los identificadores *CustomerById* y *CustomerDatabase* . Después, abra el objeto de **conjunto de registros** .  
  
 RS. Abra "CustomerById (4)", "handler = MSDFMAP. Controlador; "& _  
  
```vb
"Provider=MS Remote;Data Source=CustomerDatabase;" & _  
"Remote Server=https://yourServer"  
```  
  
## <a name="see-also"></a>Consulte también  
 [Sección de conexión del archivo de personalización](./customization-file-connect-section.md)   
 [Sección SQL de archivo de personalización](./customization-file-sql-section.md)   
 [Sección UserList del archivo de personalización](./customization-file-userlist-section.md)   
 [Personalización de DataFactory](./datafactory-customization.md)   
 [Descripción del archivo de personalización](./understanding-the-customization-file.md)   
 [Escritura de un controlador personalizado](./writing-your-own-customized-handler.md)
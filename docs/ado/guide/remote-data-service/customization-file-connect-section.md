---
description: Sección de conexión del archivo de personalización
title: Sección de conexión del archivo de personalización | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connect section in RDS [ADO]
- customization file in RDS [ADO]
ms.assetid: d50eb3cc-a822-486f-b80b-65bb50547ecd
author: rothja
ms.author: jroth
ms.openlocfilehash: 0b3508479d6d52799f286421c1bb2c67683088f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032017"
---
# <a name="customization-file-connect-section"></a>Sección de conexión del archivo de personalización
El comportamiento predeterminado del controlador es denegar todas las conexiones. La sección **Connect** especifica excepciones a ese comportamiento. Por ejemplo, si todas las secciones de **conexión** faltan o están vacías, de forma predeterminada no se pueden realizar conexiones.  
  
 La sección **Connect** puede contener:  
  
-   Entrada de acceso predeterminada que especifica las operaciones de lectura y escritura predeterminadas permitidas en esta conexión. Si no hay ninguna entrada de acceso predeterminada en la sección, se omitirá la sección.  
  
-   Nueva cadena de conexión que reemplaza la cadena de conexión del cliente.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
 Una entrada de acceso predeterminada tiene el formato:  
  
```console
  
Access=  
accessRight  
  
```  
  
 Una entrada de cadena de conexión de reemplazo tiene el formato:  
  
```console
  
Connect=  
connectionString  
  
```  
  
## <a name="remarks"></a>Observaciones  
  
|Parte|Descripción|  
|----------|-----------------|  
|**Conexión**|Cadena literal que indica que se trata de una entrada de cadena de conexión.|  
|**_connectionString_**|Cadena que reemplaza la cadena de conexión de cliente completa.|  
|**Acceder**|Cadena literal que indica que se trata de una entrada de acceso.|  
|**_accessRight_**|Uno de los siguientes derechos de acceso:<br /><br /> -   **NoAccess** : el usuario no puede tener acceso al origen de datos.<br />-   **ReadOnly** : el usuario puede leer el origen de datos.<br />-   **ReadWrite** : el usuario puede leer o escribir en el origen de datos.|  
  
 Si desea permitir cualquier conexión (en efecto, deshabilitar el comportamiento predeterminado del controlador), establezca la entrada de acceso en la sección **conectar de forma predeterminada** en `Access=ReadWrite` y elimine o comente cualquier otra sección de _identificador_ de **conexión** .  
  
## <a name="see-also"></a>Consulte también  
 [Sección de registros de archivo de personalización](./customization-file-logs-section.md)   
 [Sección SQL de archivo de personalización](./customization-file-sql-section.md)   
 [Sección UserList del archivo de personalización](./customization-file-userlist-section.md)   
 [Personalización de DataFactory](./datafactory-customization.md)   
 [Configuración de cliente requerida](./required-client-settings.md)   
 [Descripción del archivo de personalización](./understanding-the-customization-file.md)   
 [Escritura de un controlador personalizado](./writing-your-own-customized-handler.md)
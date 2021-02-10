---
description: Descripción del archivo de personalización
title: Descripción del archivo de personalización | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- customization file in RDS [ADO]
ms.assetid: 136f74bf-8d86-4a41-be66-c86cbcf81548
author: rothja
ms.author: jroth
ms.openlocfilehash: 1d2b674e5ecc927989873c82303f2b2410c31d31
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036069"
---
# <a name="understanding-the-customization-file"></a>Descripción del archivo de personalización
Cada encabezado de sección del archivo de personalización consta de corchetes (**[]**) que contienen un tipo y un parámetro. Los cuatro tipos de sección se indican mediante cadenas literales **Connect**, **SQL**, **userList** o **logs**. El parámetro es la cadena literal, el valor predeterminado, un identificador especificado por el usuario o nada.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 Por lo tanto, cada sección se marca con uno de los siguientes encabezados de sección:  
  
```console
  
[ connect default ] [ connect    
identifier   
] [ sql  default ] [ sql    
identifier   
] [ userlist    
identifier   
] [ logs ]  
  
```  
  
 Los encabezados de la sección tienen las siguientes partes.  
  
|Parte|Descripción|  
|----------|-----------------|  
|**connect**|Cadena literal que modifica una cadena de conexión.|  
|**sql**|Cadena literal que modifica una cadena de comando.|  
|**userList**|Cadena literal que modifica los derechos de acceso de un usuario específico.|  
|**logs**|Cadena literal que especifica un archivo de registro que registra los errores operativos.|  
|**default**|Cadena literal que se utiliza si no se especifica o se encuentra ningún identificador.|  
|*identifier*|Cadena que coincide con una cadena de la cadena de **conexión** o de **comando** .<br /><br /> : Use esta sección si el encabezado de sección contiene **Connect** y la cadena de identificador se encuentra en la cadena de conexión.<br />: Use esta sección si el encabezado de sección contiene **SQL** y la cadena de identificador se encuentra en la cadena de comandos.<br />: Use esta sección si el encabezado de sección contiene **userList** y la cadena de identificador coincide con un identificador de sección **Connect** .|  
  
 **DataFactory** llama al controlador y pasa los parámetros de cliente. El controlador busca cadenas completas en los parámetros de cliente que coincidan con los identificadores de los encabezados de sección adecuados. Si se encuentra una coincidencia, el contenido de esa sección se aplica al parámetro Client.  
  
 Una sección determinada se usa en las siguientes circunstancias:  
  
-   Se utiliza una sección **Connect** si la parte del valor de la palabra clave de la cadena de conexión del cliente, "**Data Source =**_Value_", coincide con un identificador de sección **Connect** . 
  
-   Se utiliza una sección **SQL** si la cadena de comandos del cliente contiene una cadena que coincide con un identificador de sección **SQL** .  
  
-   Se utiliza una sección **Connect** o **SQL** con un parámetro predeterminado si no hay ningún identificador coincidente.  
  
-   Se utiliza una sección **userList** si el identificador de la sección **userList** coincide con un identificador de sección **Connect** . Si hay una coincidencia, el contenido de la sección **userList** se aplica a la conexión controlada por la sección **Connect** .  
  
-   Si la cadena de una conexión o una cadena de comandos no coincide con el identificador en ningún encabezado de sección **Connect** o **SQL** y no hay ningún encabezado de sección **Connect** o **SQL** con un parámetro predeterminado, la cadena de cliente se usa sin modificaciones.  
  
-   La sección **logs** se usa siempre que el **DataFactory** está en funcionamiento.  
  
## <a name="see-also"></a>Consulte también  
 [Sección de conexión del archivo de personalización](./customization-file-connect-section.md)   
 [Sección de registros de archivo de personalización](./customization-file-logs-section.md)   
 [Sección SQL de archivo de personalización](./customization-file-sql-section.md)   
 [Sección UserList del archivo de personalización](./customization-file-userlist-section.md)   
 [Personalización de DataFactory](./datafactory-customization.md)   
 [Configuración de cliente requerida](./required-client-settings.md)   
 [Escritura de un controlador personalizado](./writing-your-own-customized-handler.md)
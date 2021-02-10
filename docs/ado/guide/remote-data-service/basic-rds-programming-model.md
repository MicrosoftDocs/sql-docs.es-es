---
description: Modelo básico de programación de RDS
title: Modelo de programación básica de RDS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS programming model [ADO]
ms.assetid: 0bdd236b-edff-4aac-94c3-93e1465ca6c5
author: rothja
ms.author: jroth
ms.openlocfilehash: 81d789c003921b4e8359df281cb9fa51dbdf1f26
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036585"
---
# <a name="basic-rds-programming-model"></a>Modelo básico de programación de RDS
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 RDS direcciona las aplicaciones que existen en el entorno siguiente: una aplicación cliente especifica un programa que se ejecutará en un servidor y los parámetros necesarios para devolver la información deseada. El programa invocado en el servidor obtiene acceso al origen de datos especificado, recupera la información, procesa opcionalmente los datos y, a continuación, devuelve la información resultante a la aplicación cliente en un formulario que puede usar fácilmente. RDS proporciona los medios para realizar la siguiente secuencia de acciones:  
  
-   Especifique el programa que se va a invocar en el servidor y obtenga una manera de hacer referencia a él desde el cliente. (Esta referencia se denomina a veces *proxy*. Representa el programa de servidor remoto. La aplicación cliente "llamará" al proxy como si fuera un programa local, pero realmente invocará el programa de servidor remoto).  
  
-   Invoque el programa de servidor. Pase parámetros al programa de servidor que identifiquen el origen de datos y el comando que se va a emitir. (El programa de servidor utiliza realmente ADO para obtener acceso al origen de datos. ADO establece una conexión con uno de los parámetros especificados y, a continuación, emite el comando especificado en el otro parámetro.)  
  
-   El programa de servidor obtiene un objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) del origen de datos. Opcionalmente, el objeto de **conjunto de registros** se procesa en el servidor.  
  
-   El programa de servidor devuelve el último objeto de **conjunto de registros** a la aplicación cliente.  
  
-   En el cliente, el objeto de **conjunto de registros** se coloca en un formulario que los controles visuales pueden usar fácilmente.  
  
-   Cualquier modificación del objeto de **conjunto de registros** se devuelve al programa de servidor, que los utiliza para actualizar el origen de datos.  
  
 Este modelo de programación contiene ciertas características útiles. Si no necesita un programa de servidor complejo para tener acceso al origen de datos y proporciona los parámetros de conexión y comando necesarios, RDS recuperará automáticamente los datos especificados con un programa de servidor simple y predeterminado.  
  
 Si necesita un procesamiento más complejo, puede especificar su propio programa de servidor personalizado. Por ejemplo, como un programa de servidor personalizado tiene toda la funcionalidad de ADO a su disposición, podría conectarse a varios orígenes de datos diferentes, combinar sus datos de forma compleja y, a continuación, devolver un resultado simple y procesado a la aplicación cliente.  
  
 Por último, si sus necesidades se encuentran en algún lugar entre, ADO ahora admite la personalización del comportamiento del programa de servidor predeterminado.  
  
## <a name="see-also"></a>Consulte también  
 [Modelo de programación de RDS en detalle](./rds-programming-model-in-detail.md)   
 [Escenario de RDS](./rds-scenario.md)   
 [Tutorial de RDS](./rds-tutorial.md)   
 [Objeto de conjunto de registros (ADO)](../../reference/ado-api/recordset-object-ado.md)   
 [Seguridad y uso de RDS](./rds-usage-and-security.md)
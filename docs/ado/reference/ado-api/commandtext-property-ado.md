---
description: Propiedad CommandText (ADO)
title: Propiedad CommandText (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::CommandText
helpviewer_keywords:
- CommandText property [ADO]
ms.assetid: 4dd7e82a-8da5-4a4e-b439-11a29286fa0e
author: rothja
ms.author: jroth
ms.openlocfilehash: a387746928ac92dfa6c31fbe33dab66abba05eeb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164643"
---
# <a name="commandtext-property-ado"></a>Propiedad CommandText (ADO)
Indica el texto de un comando que se va a emitir en un proveedor.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Obtiene o establece un valor de **cadena** que contiene un comando de proveedor, como una instrucción SQL, un nombre de tabla, una dirección URL relativa o una llamada a un procedimiento almacenado. El valor predeterminado es la cadena vacía ("").  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **CommandText** para establecer o devolver el texto de un comando representado por un objeto [Command](./command-object-ado.md) . Normalmente, será una instrucción SQL, pero también puede ser cualquier otro tipo de instrucción de comando que reconozca el proveedor, como una llamada a un procedimiento almacenado. Una instrucción SQL debe ser del dialecto o la versión concretos admitidos por el procesador de consultas del proveedor.  
  
 Si la propiedad [Prepared](./prepared-property-ado.md) del objeto de **comando** está establecida en **true** y el objeto de **comando** está enlazado a una conexión abierta cuando se establece la propiedad **CommandText** , ADO prepara la consulta (es decir, una forma compilada de la consulta almacenada por el proveedor) cuando se llama a los métodos [Execute](./execute-method-ado-command.md) o [Open](./open-method-ado-connection.md) .  
  
 Dependiendo del valor de la propiedad [CommandType](./commandtype-property-ado.md) , ADO puede modificar la propiedad **CommandText** . Puede leer la propiedad **CommandText** en cualquier momento para ver el texto de comando real que ADO usará durante la ejecución.  
  
 Use la propiedad **CommandText** para establecer o devolver una dirección URL relativa que especifique un recurso, como un archivo o un directorio. El recurso es relativo a una ubicación especificada explícitamente por una dirección URL absoluta o implícitamente por un objeto de [conexión](./connection-object-ado.md) abierto.  
  
> [!NOTE]
>  Las direcciones URL que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Command (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Requery (método)](./requery-method.md)   
 [Ejemplo de las propiedades ActiveConnection, CommandText, CommandTimeout, CommandType, size y Direction (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)
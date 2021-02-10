---
description: Objeto Command (ADO)
title: Objeto Command (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command
helpviewer_keywords:
- Command object [ADO]
ms.assetid: a02c22fb-542d-465e-a629-30fd59dcbebf
author: rothja
ms.author: jroth
ms.openlocfilehash: c8cb910eef7ef810b444813c1e6678b745c524e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026961"
---
# <a name="command-object-ado"></a>Objeto Command (ADO)
Define un comando específico que se va a ejecutar en un origen de datos.  
  
## <a name="remarks"></a>Observaciones  
 Use un objeto de **comando** para consultar una base de datos y devolver registros en un objeto de [conjunto de registros](./recordset-object-ado.md) , para ejecutar una operación masiva o para manipular la estructura de una base de datos. Dependiendo de la funcionalidad del proveedor, algunas colecciones de **comandos** , métodos o propiedades pueden generar un error cuando se hace referencia a ellos.  
  
 Con las colecciones, los métodos y las propiedades de un objeto de **comando** , puede hacer lo siguiente:  
  
-   Defina el texto ejecutable del comando (por ejemplo, una instrucción SQL) con la propiedad [CommandText](./commandtext-property-ado.md) . O bien, para estructuras de comandos o de consultas que no sean cadenas simples (por ejemplo, consultas de plantilla XML), defina el comando con la propiedad [CommandStream](./commandstream-property-ado.md) .  
  
-   Opcionalmente, indique el dialecto de comando usado en **CommandText** o **CommandStream** con la propiedad de [dialecto](./dialect-property.md) .  
  
-   Defina consultas con parámetros o argumentos de procedimiento almacenado con objetos de [parámetro](./parameter-object.md) y la colección de [parámetros](./parameters-collection-ado.md) .  
  
-   Indica si los nombres de parámetro se deben pasar al proveedor con la propiedad [NamedParameters](./namedparameters-property-ado.md) .  
  
-   Ejecute un comando y devuelva un objeto de **conjunto de registros** si es adecuado con el método [Execute](./execute-method-ado-command.md) .  
  
-   Especifique el tipo de comando con la propiedad [CommandType](./commandtype-property-ado.md) antes de la ejecución para optimizar el rendimiento.  
  
-   Controla si el proveedor guarda una versión preparada (o compilada) del comando antes de la ejecución con la propiedad [Prepared](./prepared-property-ado.md) .  
  
-   Establezca el número de segundos que un proveedor esperará a que un comando se ejecute con la propiedad [CommandTimeout](./commandtimeout-property-ado.md) .  
  
-   Asocie una conexión abierta a un objeto de **comando** estableciendo su propiedad [ActiveConnection](./activeconnection-property-ado.md) .  
  
-   Establezca la propiedad [nombre](./name-property-ado.md) para identificar el objeto de **comando** como un método en el objeto de [conexión](./connection-object-ado.md) asociado.  
  
-   Pase un objeto de **comando** a la propiedad de [origen](./source-property-ado-recordset.md) de un **conjunto de registros** para obtener los datos.  
  
-   Obtener acceso a los atributos específicos del proveedor con la colección [Properties](./properties-collection-ado.md) .  
  
> [!NOTE]
>  Para ejecutar una consulta sin usar un **objeto Command** , pase una cadena de consulta al método [Execute](./execute-method-ado-connection.md) de un objeto **Connection** o al método [Open](./open-method-ado-recordset.md) de un objeto **Recordset** . Sin embargo, se requiere un objeto de **comando** si desea conservar el texto del comando y volver a ejecutarlo, o usar parámetros de consulta.  
  
 Para crear un objeto de **comando** independientemente de un objeto de **conexión** definido previamente, establezca su propiedad **ActiveConnection** en una cadena de conexión válida. ADO todavía crea un objeto de **conexión** , pero no asigna ese objeto a una variable de objeto. Sin embargo, si va a asociar varios objetos de **comando** con la misma conexión, debe crear y abrir explícitamente un objeto de **conexión** . Esto asigna el objeto de **conexión** a una variable de objeto. Asegúrese de que el objeto de **conexión** se ha abierto correctamente antes de asignarlo a la propiedad **ActiveConnection** del objeto de **comando** , ya que la asignación de un objeto de **conexión** cerrado produce un error. Si no establece la propiedad **ActiveConnection** del objeto de **comando** en esta variable de objeto, ADO crea un nuevo objeto de **conexión** para cada objeto de **comando** , incluso si utiliza la misma cadena de conexión.  
  
 Para ejecutar un **comando**, llámelo mediante su propiedad [Name](./name-property-ado.md) en el objeto de **conexión** asociado. El **comando** debe tener su propiedad **ActiveConnection** establecida en el objeto de **conexión** . Si el **comando** tiene parámetros, pase sus valores como argumentos al método.  
  
 Si dos o más objetos de **comando** se ejecutan en la misma conexión y cualquier objeto de **comando** es un procedimiento almacenado con parámetros de salida, se produce un error. Para ejecutar cada objeto de **comando** , utilice conexiones independientes o desconecte todos los demás objetos de **comando** de la conexión.  
  
 La colección **Parameters** es el miembro predeterminado del objeto **Command** . Como resultado, las dos instrucciones de código siguientes son equivalentes.  
  
```  
objCmd.Parameters.Item(0)  
objCmd(0)  
```  
  
-   El objeto **Command** no es seguro para el scripting.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Command](./command-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Connection (objeto) (ADO)](./connection-object-ado.md)   
 [Parameters (colección) (ADO)](./parameters-collection-ado.md)   
 [Colección Properties (ADO)](./properties-collection-ado.md)   
 [Apéndice A: Proveedores](../../guide/appendixes/appendix-a-providers.md)
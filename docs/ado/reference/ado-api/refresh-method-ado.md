---
description: Actualizar (método, ADO)
title: Refresh (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Collection::Refresh
- Parameters::Refresh
- Properties::Refresh
helpviewer_keywords:
- Refresh method [ADO]
ms.assetid: 089b7ca7-684f-4259-8032-5bd1ecc54426
author: rothja
ms.author: jroth
ms.openlocfilehash: f72f41f58ed43665acd3838a5f8e2228c736fd78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170402"
---
# <a name="refresh-method-ado"></a>Actualizar (método, ADO)
Actualiza los objetos de una colección para reflejar los objetos disponibles en el proveedor y específicos de este.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
collection.Refresh  
```  
  
## <a name="remarks"></a>Observaciones  
 El método **Refresh** realiza diferentes tareas en función de la colección de la que se llama.  
  
### <a name="parameters"></a>Parámetros  
 El uso del método **Refresh** en la colección [Parameters](./parameters-collection-ado.md) de un objeto [Command](./command-object-ado.md) recupera información de parámetros del proveedor para el procedimiento almacenado o la consulta con parámetros especificados en el objeto **Command** . La colección estará vacía para los proveedores que no admitan llamadas a procedimientos almacenados o consultas parametrizadas.  
  
 Debe establecer la propiedad [ActiveConnection](./activeconnection-property-ado.md) del objeto de **comando** en un objeto de [conexión](./connection-object-ado.md) válido, la propiedad [CommandText](./commandtext-property-ado.md) en un comando válido y la propiedad [CommandType](./commandtype-property-ado.md) en **adCmdStoredProc** antes de llamar al método **Refresh** .  
  
 Si obtiene acceso a la colección **Parameters** antes de llamar al método **Refresh** , ADO llamará automáticamente al método y rellenará la colección.  
  
> [!NOTE]
>  Si usa el método **Refresh** para obtener información de parámetros del proveedor y devuelve uno o más objetos de [parámetro](./parameter-object.md) de tipo de datos de longitud variable, ADO puede asignar memoria para los parámetros en función de su tamaño máximo potencial, lo que producirá un error durante la ejecución. Debe establecer explícitamente la propiedad [size](./size-property-ado-parameter.md) para estos parámetros antes de llamar al método [Execute](./execute-method-ado-command.md) para evitar errores.  
  
### <a name="fields"></a>Campos  
 El uso del método **Refresh** en la colección [Fields](./fields-collection-ado.md) no tiene ningún efecto visible. Para recuperar los cambios de la estructura de base de datos subyacente, debe utilizar el método [Requery](./requery-method.md) o, si el objeto de [conjunto de registros](./recordset-object-ado.md) no admite marcadores, el método [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) .  
  
### <a name="properties"></a>Propiedades  
 El uso del método **Refresh** en una colección **Properties** de algunos objetos rellena la colección con las propiedades dinámicas que expone el proveedor. Estas propiedades proporcionan información sobre la funcionalidad específica del proveedor, más allá de las propiedades integradas que admite ADO.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Colección Axes](../ado-md-api/axes-collection-ado-md.md)  
        [Colección de columnas](../adox-api/columns-collection-adox.md)  
        [Colección CubeDefs](../ado-md-api/cubedefs-collection-ado-md.md)  
        [Colección de dimensiones](../ado-md-api/dimensions-collection-ado-md.md)  
        [Colección de errores](./errors-collection-ado.md)  
        [Colección Fields](./fields-collection-ado.md)  
        [Colección de grupos](../adox-api/groups-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Colección Hierarchies](../ado-md-api/hierarchies-collection-ado-md.md)  
        [Colección de índices](../adox-api/indexes-collection-adox.md)  
        [Colección de claves](../adox-api/keys-collection-adox.md)  
        [Colección de niveles](../ado-md-api/levels-collection-ado-md.md)  
        [Colección de miembros](../ado-md-api/members-collection-ado-md.md)  
        [Colección Parameters](./parameters-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Colección de posiciones](../ado-md-api/positions-collection-ado-md.md)  
        [Colección de procedimientos](../adox-api/procedures-collection-adox.md)  
        [Colección de propiedades](./properties-collection-ado.md)  
        [Colección de tablas](../adox-api/tables-collection-adox.md)  
        [Recopilación de usuarios](../adox-api/users-collection-adox.md)  
        [Colección de vistas](../adox-api/views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo del método Refresh (VB)](./refresh-method-example-vb.md)   
 [Ejemplo del método Refresh (VC + +)](./refresh-method-example-vc.md)   
 [Propiedad Count (ADO)](./count-property-ado.md)   
 [Método Refresh (RDS)](../rds-api/refresh-method-rds.md)
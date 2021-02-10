---
description: Objeto de conjunto de registros (ADO)
title: Objeto de conjunto de registros (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset
helpviewer_keywords:
- Recordset object [ADO]
ms.assetid: ede1415f-c3df-4cc5-a05b-2576b2b84b60
author: rothja
ms.author: jroth
ms.openlocfilehash: 7b4e7514d0ca9b9fd94c634c1e6a17dcdb42d976
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051786"
---
# <a name="recordset-object-ado"></a>Objeto de conjunto de registros (ADO)
Representa el conjunto completo de registros de una tabla base o los resultados de un comando ejecutado. En cualquier momento, el objeto de **conjunto de registros** hace referencia a un único registro del conjunto como el registro actual.  
  
## <a name="remarks"></a>Observaciones  
 Los objetos de **conjunto de registros** se utilizan para manipular los datos de un proveedor. Cuando se utiliza ADO, se manipulan los datos casi por completo mediante objetos de **conjunto de registros** . Todos los objetos de **conjunto de registros** se componen de registros (filas) y campos (columnas). Dependiendo de la funcionalidad admitida por el proveedor, es posible que algunos métodos o propiedades del **conjunto de registros** no estén disponibles.  
  
 ADODB. Recordset es el ProgID que se debe usar para crear un objeto de **conjunto de registros** . Aplicaciones existentes que hacen referencia al ADOR obsoleto. El ProgID del conjunto de registros seguirá funcionando sin volver a compilar, pero el nuevo desarrollo debe hacer referencia a ADODB. DataRecordsets.  
  
 Hay cuatro tipos de cursor diferentes definidos en ADO:  
  
-   **Cursor dinámico** Permite ver adiciones, cambios y eliminaciones de otros usuarios. permite todos los tipos de movimiento a través del **conjunto de registros** que no se basan en marcadores; y permite marcadores si el proveedor los admite.  
  
-   **Cursor de conjunto de claves** Se comporta como un cursor dinámico, con la salvedad de que impide ver los registros que otros usuarios agregan y evita el acceso a los registros que otros usuarios eliminan. Los cambios de datos realizados por otros usuarios seguirán siendo visibles. Siempre admite marcadores y, por tanto, permite todos los tipos de movimiento a través del **conjunto de registros**.  
  
-   **Cursor estático** Proporciona una copia estática de un conjunto de registros que se puede usar para buscar datos o generar informes; siempre permite marcadores y, por tanto, permite todos los tipos de movimiento a través del **conjunto de registros**. Las adiciones, cambios o eliminaciones de otros usuarios no estarán visibles. Este es el único tipo de cursor permitido al abrir un objeto de conjunto de **registros** del lado cliente.  
  
-   **Cursor de solo avance** Permite solo desplazarse hacia delante por el **conjunto de registros**. Las adiciones, cambios o eliminaciones de otros usuarios no estarán visibles. Esto mejora el rendimiento en situaciones en las que es necesario hacer un solo paso a través de un **conjunto de registros**.  
  
 Establezca la propiedad [CursorType](./cursortype-property-ado.md) antes de abrir el **conjunto de registros** para elegir el tipo de cursor o pase un argumento *CursorType* con el método [Open](./open-method-ado-recordset.md) . Algunos proveedores no admiten todos los tipos de cursores. Consulte la documentación del proveedor. Si no se especifica un tipo de cursor, ADO abre un cursor de solo avance de forma predeterminada.  
  
 Si la propiedad [CursorLocation](./cursorlocation-property-ado.md) está establecida en **adUseClient** para abrir un **conjunto de registros**, la propiedad **UnderlyingValue** en los objetos de [campo](./field-object.md) no está disponible en el objeto de **conjunto de registros** devuelto. Cuando se usa con algunos proveedores (como el proveedor ODBC de Microsoft para OLE DB junto con Microsoft SQL Server), puede crear objetos de **conjunto de registros** independientemente de un objeto de [conexión](./connection-object-ado.md) definido previamente pasando una cadena de conexión con el método **Open** . ADO todavía crea un objeto de [conexión](./connection-object-ado.md) , pero no asigna ese objeto a una variable de objeto. Sin embargo, si está abriendo varios objetos de **conjunto de registros** en la misma conexión, debe crear y abrir explícitamente un objeto de **conexión** . Esto asigna el objeto de **conexión** a una variable de objeto. Si no usa esta variable de objeto al abrir los objetos de **conjunto de registros** , ADO crea un nuevo objeto de **conexión** para cada nuevo conjunto de **registros**, incluso si pasa la misma cadena de conexión.  
  
 Puede crear tantos objetos de **conjunto de registros** como sea necesario.  
  
 Al abrir un **conjunto de registros**, el registro actual se coloca en el primer registro (si existe) y las propiedades [BOF](./bof-eof-properties-ado.md) y [EOF](./bof-eof-properties-ado.md) se establecen en **false**. Si no hay ningún registro, los valores de la propiedad **BOF** y **EOF** son **true**.  
  
 Puede usar los métodos [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md), **MoveLast**, **MoveNext** y **MovePrevious** ; método [Move](./move-method-ado.md) ; y las propiedades [AbsolutePosition](./absoluteposition-property-ado.md), [AbsolutePage](./absolutepage-property-ado.md)y [Filter](./filter-property.md) para cambiar la posición del registro actual, suponiendo que el proveedor admita la funcionalidad pertinente. Los objetos de **conjunto de registros** de solo avance solo admiten el método [MoveNext](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) . Cuando se usan los métodos **Move** para visitar cada registro (o enumerar el **conjunto de registros**), se pueden usar las propiedades **BOF** y **EOF** para determinar si se ha movido más allá del principio o el final del **conjunto de registros**.  
  
 Antes de usar cualquier funcionalidad de un objeto de **conjunto de registros** , debe llamar al método **Supports** en el objeto para comprobar que la funcionalidad es compatible o disponible. No debe utilizar la funcionalidad cuando el método **Supports** devuelve false. Por ejemplo, puede usar el método **MovePrevious** solo si `Recordset.Supports(adMovePrevious)` devuelve **true**. De lo contrario, obtendrá un error porque el objeto de **conjunto de registros** podría haberse cerrado y la funcionalidad se representara no disponible en la instancia. Si no se admite una característica que le interese, **Supports** también devolverá FALSE. En este caso, debe evitar llamar a la propiedad o al método correspondiente en el objeto de **conjunto de registros** .  
  
 Los objetos de **conjunto de registros** pueden admitir dos tipos de actualización: inmediato y por lotes. En la actualización inmediata, todos los cambios en los datos se escriben inmediatamente en el origen de datos subyacente una vez que se llama al método [Update](./update-method.md) . También puede pasar matrices de valores como parámetros con los métodos [AddNew](./addnew-method-ado.md) y **Update** y actualizar simultáneamente varios campos en un registro.  
  
 Si un proveedor admite la actualización por lotes, puede hacer que la memoria caché del proveedor cambie a más de un registro y, a continuación, transmitirlos en una única llamada a la base de datos con el método [UpdateBatch](./updatebatch-method.md) . Esto se aplica a los cambios realizados con los métodos **AddNew**, **Update** y [Delete](./delete-method-ado-recordset.md) . Después de llamar al método **UpdateBatch** , puede usar la propiedad [status](./status-property-ado-recordset.md) para comprobar los conflictos de datos con el fin de resolverlos.  
  
> [!NOTE]
>  Para ejecutar una consulta sin usar un objeto de [comando](./command-object-ado.md) , pase una cadena de consulta al método **Open** de un objeto de **conjunto de registros** . Sin embargo, se requiere un objeto de **comando** si desea conservar el texto del comando y volver a ejecutarlo, o usar parámetros de consulta.  
  
 La propiedad [mode](./mode-property-ado.md) rige los permisos de acceso.  
  
 La colección **Fields** es el miembro predeterminado del objeto **Recordset** . Como resultado, las dos instrucciones de código siguientes son equivalentes.  
  
```  
Debug.Print objRs.Fields.Item(0)  ' Both statements print   
Debug.Print objRs(0)              '  the Value of Item(0).  
```  
  
 Cuando se pasa un objeto de **conjunto de registros** a través de los procesos, solo se calculan las referencias de los valores del **conjunto de filas** y se omiten las propiedades del objeto de conjunto de **registros** . Durante la desserialización, el **conjunto de filas** se desempaqueta en un objeto de conjunto de **registros** recién creado, que también establece sus propiedades en los valores predeterminados.  
  
 El objeto de **conjunto de registros** es seguro para el scripting.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Eventos, métodos y propiedades del objeto de conjunto de registros](./recordset-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Connection (objeto) (ADO)](./connection-object-ado.md)   
 [Fields (colección) (ADO)](./fields-collection-ado.md)   
 [Colección Properties (ADO)](./properties-collection-ado.md)   
 [Apéndice A: Proveedores](../../guide/appendixes/appendix-a-providers.md)
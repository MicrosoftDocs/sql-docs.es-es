---
description: Append (método) (ADO)
title: Append (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _DynaCollection::Append
helpviewer_keywords:
- Append method [ADO]
ms.assetid: f8a9bbed-ba9c-4698-945d-317ad22d2e92
author: rothja
ms.author: jroth
ms.openlocfilehash: 03e454407c1b01b34ea918f81a404d579f8a3809
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167888"
---
# <a name="append-method-ado"></a>Append (método) (ADO)
Anexa un objeto a una colección. Si la colección es [Fields](./fields-collection-ado.md), se puede crear un nuevo objeto [Field](./field-object.md) antes de que se anexe a la colección.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
collection.Append object  
fields.Append Name, Type, DefinedSize, Attrib, FieldValue  
```  
  
#### <a name="parameters"></a>Parámetros  
 *collection*  
 Objeto de colección.  
  
 *fields*  
 Colección de **campos** .  
  
 *object*  
 Variable de objeto que representa el objeto que se va a anexar.  
  
 *Nombre*  
 Valor de **cadena** que contiene el nombre del nuevo objeto de **campo** y no debe tener el mismo nombre que ningún otro objeto de *los campos*.  
  
 *Type*  
 Un valor [DataTypeEnum](./datatypeenum.md) , cuyo valor predeterminado es **adEmpty**, que especifica el tipo de datos del nuevo campo. ADO no admite los siguientes tipos de datos y no se deben usar al anexar nuevos campos a un objeto de [conjunto de registros (ADO)](./recordset-object-ado.md): **adIDispatch**, **adIUnknown**, **adVariant**.  
  
 *DefinedSize*  
 Opcional. Valor de **tipo Long** que representa el tamaño definido, en caracteres o bytes, del nuevo campo. El valor predeterminado de este parámetro se deriva del *tipo*. Los campos que tienen un *DefinedSize* mayor que 255 bytes se tratan como columnas de longitud variable. No se especifica el valor predeterminado de *DefinedSize* .  
  
 *Atributo*  
 Opcional. Un valor de [FieldAttributeEnum](./fieldattributeenum.md) , cuyo valor predeterminado es **adFldDefault**, que especifica los atributos del nuevo campo. Si no se especifica este valor, el campo contendrá atributos derivados del *tipo*.  
  
 *FieldValue*  
 Opcional. **Variante** que representa el valor del nuevo campo. Si no se especifica, el campo se anexa con un valor null.  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="parameters-collection"></a>Colección Parameters  
 Debe establecer la propiedad [Type](./type-property-ado.md) de un objeto de [parámetro](./parameter-object.md) antes de anexarlo a la colección [Parameters](./parameters-collection-ado.md) . Si selecciona un tipo de datos de longitud variable, también debe establecer la propiedad [size](./size-property-ado-parameter.md) en un valor mayor que cero.  
  
 La descripción de los parámetros minimiza las llamadas al proveedor y, por tanto, mejora el rendimiento cuando se usan procedimientos almacenados o consultas parametrizadas. Sin embargo, debe conocer las propiedades de los parámetros asociados al procedimiento almacenado o a la consulta parametrizada a la que desea llamar.  
  
 Use el método [CreateParameter](./createparameter-method-ado.md) para crear objetos de **parámetro** con los valores de propiedad adecuados y use el método **Append** para agregarlos a la colección de [parámetros](./parameters-collection-ado.md) . Esto le permite establecer y devolver los valores de los parámetros sin tener que llamar al proveedor para obtener la información de los parámetros. Si está escribiendo en un proveedor que no proporciona información de parámetros, debe utilizar este método para rellenar manualmente la colección de **parámetros** con el fin de usar parámetros en absoluto.  
  
## <a name="fields-collection"></a>Colección Fields  
 El parámetro *FieldValue* solo es válido cuando se agrega un objeto de **campo** a un objeto de [registro](./record-object-ado.md) , no a un objeto de **conjunto de registros** . Con un objeto de **registro** , puede anexar campos y proporcionar valores al mismo tiempo. Con un objeto de **conjunto de registros** , debe crear campos mientras el **conjunto de registros** está cerrado y, a continuación, abrir el conjunto de **registros** y asignar valores a los campos.  
  
> [!NOTE]
>  En el caso de los nuevos objetos de **campo** que se han anexado a la colección **Fields** de un objeto **Record** , la propiedad [Value](./value-property-ado.md) debe establecerse antes de que se puedan especificar otras propiedades de **campo** . En primer lugar, se debe haber asignado un valor específico para la propiedad **Value** y [actualizarse](./update-method.md) en la colección **Fields** denominada. A continuación, se puede tener acceso a otras propiedades, como el [tipo](./type-property-ado.md) o [los atributos](./attributes-property-ado.md) . Los objetos de **campo** de los siguientes tipos de datos (**DataTypeEnum**) no se pueden anexar a la colección **Fields** y provocará que se produzca un error: **adArray**, **adChapter**, **adEmpty**, **adPropVariant** y **adUserDefined**. Además, ADO no admite los siguientes tipos de datos: **adIDispatch**, **adIUnknown** y **adIVariant**. Para estos tipos, no se producirá ningún error si se anexa, pero el uso puede producir resultados imprevisibles, incluidas pérdidas de memoria.  
  
## <a name="recordset"></a>DataRecordsets  
 Si no establece la propiedad [CursorLocation](./cursorlocation-property-ado.md) antes de llamar al método **Append** , **CursorLocation** se establecerá en **adUseClient** (un valor [CursorLocationEnum](./cursorlocationenum.md) ) automáticamente cuando se llame al método [Open](./open-method-ado-recordset.md) del objeto de [conjunto de registros](./recordset-object-ado.md) .  
  
 Se producirá un error en tiempo de ejecución si se llama al método **Append** en la colección **Fields** de un **conjunto de registros** abierto o en un **conjunto de registros** donde se ha establecido la propiedad [ActiveConnection](./activeconnection-property-ado.md) . Solo puede anexar campos a un **conjunto de registros** que no esté abierto y que todavía no se haya conectado a un origen de datos. Normalmente, este es el caso cuando un objeto de **conjunto de registros** se fabrica con el método [CreateRecordset](../rds-api/createrecordset-method-rds.md) o se asigna a una variable de objeto.  
  
## <a name="record"></a>Grabar  
 No se producirá un error en tiempo de ejecución si se llama al método **Append** en la colección de **campos** de un **registro** abierto. El nuevo campo se agregará a la colección **Fields** del objeto **Record** . Si el **registro** se ha derivado de un **conjunto de registros**, el nuevo campo no aparecerá en la colección de **campos** del objeto de conjunto de **registros** .  
  
 Se puede crear un campo que no existe y se anexa a la colección **Fields** asignando un valor al objeto de campo como si ya existiera en la colección. La asignación desencadenará la creación y anexión automática del objeto de **campo** y, a continuación, se completará la asignación.  
  
 Después de anexar un **campo** a la colección **Fields** de un objeto **Record** , llame al método **Update** de la colección **Fields** para guardar el cambio.  
  
## <a name="applies-to"></a>Se aplica a  
  
- [Fields (colección) (ADO)](./fields-collection-ado.md)  
- [Colección de parámetros (ADO)](./parameters-collection-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos Append y CreateParameter (VB)](./append-and-createparameter-methods-example-vb.md)   
 [Ejemplo de los métodos Append y CreateParameter (VC + +)](./append-and-createparameter-methods-example-vc.md)   
 [CreateParameter (método) (ADO)](./createparameter-method-ado.md)   
 [Método Delete (colección Fields de ADO)](./delete-method-ado-fields-collection.md)   
 [Método Delete (colección de parámetros de ADO)](./delete-method-ado-parameters-collection.md)   
 [Delete (método) (conjunto de registros ADO)](./delete-method-ado-recordset.md)   
 [Update (método)](./update-method.md)
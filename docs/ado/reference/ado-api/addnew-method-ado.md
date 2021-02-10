---
description: AddNew (método) (ADO)
title: AddNew (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::AddNew
- Recordset15::raw_AddNew
helpviewer_keywords:
- AddNew method [ADO]
ms.assetid: a9f54be9-5763-45d0-a6eb-09981b03bc08
author: rothja
ms.author: jroth
ms.openlocfilehash: 1dd85acf2d16ad998294bae5ee6490d227e8ede2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031457"
---
# <a name="addnew-method-ado"></a>AddNew (método) (ADO)
Crea un nuevo registro para un objeto de [conjunto de registros](./recordset-object-ado.md) actualizable.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
recordset.AddNew FieldList, Values  
```  
  
#### <a name="parameters"></a>Parámetros  
 *recordset*  
 Objeto de **conjunto de registros** .  
  
 *FieldList*  
 Opcional. Un nombre único, o una matriz de nombres o posiciones ordinales de los campos del nuevo registro.  
  
 *Valores*  
 Opcional. Un valor único, o una matriz de valores para los campos del nuevo registro. Si *FieldList* es una matriz, *los valores* también deben ser una matriz con el mismo número de miembros; de lo contrario, se produce un error. El orden de los nombres de campo debe coincidir con el orden de los valores de campo de cada matriz.  
  
## <a name="remarks"></a>Observaciones  
 Use el método **AddNew** para crear e inicializar un nuevo registro. Use el método [Supports](./supports-method.md) con **adAddNew** (un valor [CursorOptionEnum](./cursoroptionenum.md) ) para comprobar si puede agregar registros al objeto de **conjunto de registros** actual.  
  
 Después de llamar al método **AddNew** , el nuevo registro se convierte en el registro actual y permanece actual después de llamar al método [Update](./update-method.md) . Dado que el nuevo registro se anexa al **conjunto de registros**, una llamada a **MoveNext** después de la actualización se moverá más allá del final del **conjunto de registros**, haciendo que **EOF** sea true. Si el objeto de **conjunto de registros** no admite marcadores, es posible que no pueda obtener acceso al nuevo registro una vez que se haya movido a otro registro. Dependiendo del tipo de cursor, puede que tenga que llamar al método [Requery](./requery-method.md) para que el nuevo registro sea accesible.  
  
 Si llama a **AddNew** mientras edita el registro actual o al agregar un nuevo registro, ADO llama al método **Update** para guardar los cambios y, a continuación, crea el nuevo registro.  
  
 El comportamiento del método **AddNew** depende del modo de actualización del objeto de **conjunto de registros** y de si se pasan los argumentos *FieldList* y *Values* .  
  
 En *el modo de actualización inmediata* (en el que el proveedor escribe los cambios en el origen de datos subyacente una vez que se llama al método **Update** ), al llamar al método **AddNew** sin argumentos se establece la propiedad [EditMode](./editmode-property.md) en **adEditAdd** (un valor de [EditModeEnum](./editmodeenum.md) ). El proveedor almacena en caché los cambios de los valores de campo de forma local. La llamada al método **Update** expone el nuevo registro en la base de datos y restablece la propiedad **EditMode** en **AdEditNone** (un valor de **EditModeEnum** ). Si se pasan los argumentos *FieldList* y *Values* , ADO envía inmediatamente el nuevo registro a la base de datos (no se necesita ninguna llamada de **actualización** ); el valor de la propiedad **EditMode** no cambia (**adEditNone**).  
  
 En *el modo de actualización por lotes* (en el que el proveedor almacena en caché varios cambios y los escribe en el origen de datos subyacente solo cuando se llama al método [UpdateBatch](./updatebatch-method.md) ), al llamar al método **AddNew** sin argumentos se establece la propiedad **EditMode** en **adEditAdd**. El proveedor almacena en caché los cambios de los valores de campo de forma local. Al llamar al método **Update** se agrega el nuevo registro al **conjunto de registros** actual, pero el proveedor no publica los cambios en la base de datos subyacente o restablece el objeto **EditMode** en **adEditNone**, hasta que se llama al método **UpdateBatch** . Si se pasan los argumentos *FieldList* y *Values* , ADO envía el nuevo registro al proveedor para su almacenamiento en una memoria caché y establece **EditMode** en **adEditAdd**; debe llamar al método **UpdateBatch** para publicar el nuevo registro en la base de datos subyacente.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se muestra cómo usar el método AddNew con la lista de campos y la lista de valores incluida, para ver cómo incluir la lista de campos y la lista de valores como matrices.  
  
```  
create table aa1 (intf int, charf char(10))  
insert into aa1 values (2, 'aa')  
  
Dim cn As New adodb.Connection  
Dim rs As New adodb.Recordset  
Dim cmd As New adodb.Command  
  
cn.ConnectionString = "Provider=SQLOLEDB;Data Source=alexverb2;uid=sa;pwd=foo$bar00;"  
  
cn.Open  
rs.Open "select * from xxx..aa1", cn, adOpenKeyset, adLockOptimistic  
  
Dim fieldsArray(1) As Variant  
fieldsArray(0) = "intf"  
fieldsArray(1) = "charf"  
Dim values(1) As Variant  
values(0) = 4  
values(1) = "as"  
rs.AddNew fieldsArray, values  
rs.Update  
```  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del método AddNew (VB)](./addnew-method-example-vb.md)   
 [Ejemplo del método AddNew (VBScript)](./addnew-method-example-vbscript.md)   
 [Ejemplo del método AddNew (VC + +)](./addnew-method-example-vc.md)   
 [CancelUpdate (método) (ADO)](./cancelupdate-method-ado.md)   
 [Propiedad EditMode](./editmode-property.md)   
 [Requery (método)](./requery-method.md)   
 [Supports (método)](./supports-method.md)   
 [Update (método)](./update-method.md)   
 [Método UpdateBatch](./updatebatch-method.md)
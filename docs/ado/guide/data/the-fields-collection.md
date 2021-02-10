---
description: Fields (colección)
title: Colección Fields | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Field object [ADO], fields collection
- Fields collection [ADO]
ms.assetid: 574cf36e-e5f5-403b-983c-749ef93c108f
author: rothja
ms.author: jroth
ms.openlocfilehash: 7dee12a0cdb274f58bb3dc79fddeb057e4913e81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032417"
---
# <a name="the-fields-collection"></a>Fields (colección)
La colección **Fields** es una de las colecciones intrínsecas de ADO. Una colección es un conjunto ordenado de elementos a los que se puede hacer referencia como una unidad. Para obtener más información acerca de las colecciones de ADO, vea [el modelo de objetos ADO](../../../ado/guide/data/ado-objects-and-collections.md).  
  
 La colección **Fields** contiene un objeto **Field** para cada campo (columna) del **conjunto de registros**. Al igual que todas las colecciones de ADO, tiene las propiedades **recuento** y **elemento** , así como los métodos **Append** y **Refresh** . También tiene métodos **CancelUpdate**, **Delete**, **Resync** y **Update** , que no están disponibles para otras colecciones de ADO.  
  
## <a name="examining-the-fields-collection"></a>Examinar la colección Fields  
 Considere la colección **Fields** del **conjunto de registros** de ejemplo que se presentó en esta sección. El **conjunto de registros** de ejemplo se deriva de la instrucción SQL  
  
```  
SELECT ProductID, ProductName, UnitPrice FROM Products WHERE CategoryID = 7  
```  
  
 Por lo tanto, debería encontrar que la colección de **campos del conjunto de registros** contiene tres campos.  
  
```  
'BeginWalkFields  
    Dim objFields As ADODB.Fields  
    Dim intLoop As Integer  
  
    objRs.Open strSQL, strConnStr, adOpenForwardOnly, adLockReadOnly, adCmdText  
  
    Set objFields = objRs.Fields  
  
    For intLoop = 0 To (objFields.Count - 1)  
        Debug.Print objFields.Item(intLoop).Name  
    Next  
'EndWalkFields  
```  
  
 Este código simplemente determina el número de objetos de **campo** de la colección **Fields** mediante la propiedad **Count** y recorre en bucle la colección, devolviendo el valor de la propiedad **Name** de cada objeto **Field** . Puede usar muchas más propiedades de **campo** para obtener información acerca de un campo. Para obtener más información sobre cómo realizar consultas en un **campo**, vea [el objeto Field](../../../ado/guide/data/the-field-object.md).  
  
## <a name="counting-columns"></a>Recuento de columnas  
 Como cabría esperar, la propiedad **Count** devuelve el número real de objetos **Field** de la colección **Fields** . Dado que la numeración de los miembros de una colección comienza con cero, siempre debe codificar los bucles empezando por el miembro cero y terminando con el valor de la propiedad **Count** menos 1. Si usa Microsoft Visual Basic y desea crear un bucle a través de los miembros de una colección sin comprobar la propiedad Count, utilice el **parámetro** **for each... Comando siguiente** .  
  
 Si la propiedad **Count** es cero, no hay ningún objeto en la colección.  
  
## <a name="getting-to-the-field"></a>Obtener el campo  
 Como con cualquier colección de ADO, la propiedad **Item** es la propiedad predeterminada de la colección. Devuelve el objeto de **campo** individual especificado por el nombre o el índice que se le ha pasado. Por lo tanto, las instrucciones siguientes son equivalentes para el **conjunto de registros** de ejemplo:  
  
```  
objField = objRecordset.Fields.Item("ProductID")  
objField = objRecordset.Fields("ProductID")  
objField = objRecordset.Fields.Item(0)  
objField = objRecordset.Fields(0)  
```  
  
 Si estos métodos son equivalentes, ¿cuál es el mejor? Depende. El uso de un índice para recuperar un **campo** de la colección es más rápido porque tiene acceso al **campo** directamente sin tener que realizar una búsqueda de cadena. Por otro lado, el orden de los **campos** dentro de la colección debe ser conocido y, si el orden cambia, la referencia al índice **del campo** tendrá que cambiarse dondequiera que se produzca. Aunque es ligeramente más lento, el uso del nombre del **campo** es más flexible, ya que no depende del orden de los **campos** de la colección.  
  
## <a name="using-the-refresh-method"></a>Usar el método Refresh  
 A diferencia de otras colecciones de ADO, el uso del método **Refresh** en la colección **Fields** no tiene ningún efecto visible. Para recuperar los cambios de la estructura de base de datos subyacente, debe utilizar el método **Requery** , o bien, si el objeto de **conjunto de registros** no admite marcadores, el método **MoveFirst** , que hará que el comando se ejecute en el proveedor de nuevo.  
  
## <a name="adding-fields-to-a-recordset"></a>Agregar campos a un conjunto de registros  
 El método **Append** se utiliza para agregar campos a un **conjunto de registros**.  
  
 Puede utilizar el método **Append** para fabricar un conjunto de **registros** mediante programación sin abrir una conexión a un origen de datos. Se producirá un error en tiempo de ejecución si se llama al método **Append** en la colección **Fields** de un **conjunto de registros** abierto o en un **conjunto de registros** donde se ha establecido la propiedad **ActiveConnection** . Solo puede anexar campos a un **conjunto de registros** que no esté abierto y que todavía no se haya conectado a un origen de datos. Sin embargo, para especificar valores para los **campos** que se acaban de anexar, primero se debe abrir el **conjunto de registros** .  
  
 A menudo, los desarrolladores necesitan un lugar para almacenar temporalmente algunos datos o quieren que algunos datos actúen como si provinieran de un servidor para que puedan participar en el enlace de datos en una interfaz de usuario. ADO (junto con el [servicio de cursores de Microsoft para OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md)) permite al desarrollador crear un objeto de **conjunto de registros** vacío especificando la información de columna y llamando a **Open**. En el ejemplo siguiente, se anexan tres nuevos campos a un nuevo objeto de **conjunto de registros** . A continuación, se abre el **conjunto de registros** , se agregan dos nuevos registros y el **conjunto de registros** se conserva en un archivo. (Para obtener más información acerca de la persistencia del **conjunto de registros** , vea [actualizar y conservar datos](../../../ado/guide/data/updating-and-persisting-data.md)).  
  
```  
'BeginFabricate  
    Dim objRs As ADODB.Recordset  
    Set objRs = New ADODB.Recordset  
  
    With objRs.Fields  
        .Append "StudentID", adChar, 11, adFldUpdatable  
        .Append "FullName", adVarChar, 50, adFldUpdatable  
        .Append "PhoneNmbr", adVarChar, 20, adFldUpdatable  
    End With  
  
    With objRs  
        .Open  
  
        .AddNew  
        .Fields(0) = "123-45-6789"  
        .Fields(1) = "John Doe"  
        .Fields(2) = "(425) 555-5555"  
        .Update  
  
        .AddNew  
        .Fields(0) = "123-45-6780"  
        .Fields(1) = "Jane Doe"  
        .Fields(2) = "(615) 555-1212"  
        .Update  
    End With  
  
    objRs.Save App.Path & "FabriTest.adtg", adPersistADTG  
  
    objRs.Close  
'EndFabricate  
```  
  
 El uso del método **Append de campos** difiere entre el objeto de **conjunto de registros** y el objeto de **registro** . Para obtener más información sobre el objeto de **registro** , vea [registros y secuencias](../../../ado/guide/data/records-and-streams.md).  
  
## <a name="see-also"></a>Consulte también  
 [Fabricación de conjuntos de registros jerárquicos](../../../ado/guide/data/fabricating-hierarchical-recordsets.md)

---
description: El objeto de campo
title: El objeto de campo | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Field object [ADO]
ms.assetid: 7d1c4ad5-4be3-42ab-b516-e7133ca300bc
author: rothja
ms.author: jroth
ms.openlocfilehash: bf3a704b26e35c77d9ba7ba059978091002cc424
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036945"
---
# <a name="the-field-object"></a>El objeto de campo
Cada objeto de **campo** suele corresponder a una columna de una tabla de base de datos. Sin embargo, un **campo** también puede representar un puntero a otro **conjunto de registros**, denominado capítulo. Las excepciones, como las columnas de capítulo, se tratarán más adelante en esta guía.  
  
 Utilice la propiedad **Value** de los objetos de **campo** para establecer o devolver datos para el registro actual. Dependiendo de la funcionalidad que expone el proveedor, puede que algunas colecciones, métodos o propiedades de un objeto de **campo** no estén disponibles.  
  
 Con las colecciones, los métodos y las propiedades de un objeto de **campo** , puede hacer lo siguiente:  
  
-   Devuelve el nombre de un campo utilizando la propiedad **Name** .  
  
-   Ver o cambiar los datos del campo con la propiedad **valor** . **Value** es la propiedad predeterminada del objeto **Field** .  
  
-   Devuelve las características básicas de un campo mediante las propiedades **Type**, **Precision** y **NumericScale** .  
  
-   Devuelve el tamaño declarado de un campo mediante la propiedad **DefinedSize** .  
  
-   Devuelve el tamaño real de los datos de un campo determinado mediante la propiedad **ActualSize** .  
  
-   Determine qué tipos de funcionalidad se admiten para un campo dado mediante la propiedad **attributes** y la colección **Properties** .  
  
-   Manipular los valores de los campos que contienen datos binarios largos o de caracteres largos mediante los métodos **AppendChunk** y **GetChunk** .  
  
 Resuelva las discrepancias en los valores de campo durante la actualización por lotes mediante las propiedades **OriginalValue** y **UnderlyingValue** , si el proveedor admite las actualizaciones por lotes.  
  
## <a name="describing-a-field"></a>Descripción de un campo  
 En los temas siguientes se explican las propiedades del objeto de [campo](../../../ado/reference/ado-api/field-object.md) que representan información que describe el propio objeto de **campo** , es decir, metadatos sobre el campo. Esta información se puede usar para determinar mucho sobre el esquema del **conjunto de registros**. Entre estas propiedades se incluyen **Type**, **DefinedSize** y **ActualSize**, **Name** y **NumericScale** y **Precision**.  
  
### <a name="discovering-the-data-type"></a>Detección del tipo de datos  
 La propiedad **Type** indica el tipo de datos del campo. Las constantes enumeradas de tipo de datos que admite ADO se describen en [DataTypeEnum](../../../ado/reference/ado-api/datatypeenum.md) en la *Referencia del programador de ADO*.  
  
 En el caso de los tipos numéricos de punto flotante, como **adNumeric**, puede obtener más información. La propiedad **NumericScale** indica cuántos dígitos a la derecha del separador decimal se utilizarán para representar valores para el **campo**. La propiedad **Precision** especifica el número máximo de dígitos que se usan para representar valores para el **campo**.  
  
### <a name="determining-field-size"></a>Determinar el tamaño del campo  
 Use la propiedad **DefinedSize** para determinar la capacidad de los datos de un objeto de **campo** .  
  
 Use la propiedad **ActualSize** para devolver la longitud real del valor de un objeto de **campo** . En todos los campos, la propiedad **ActualSize** es de solo lectura. Si ADO no puede determinar la longitud del valor del objeto de **campo** , la propiedad **ActualSize** devuelve **adUnknown**.  
  
 Las propiedades **DefinedSize** y **ActualSize** tienen fines diferentes. Por ejemplo, considere un objeto de **campo** con un tipo declarado de **advarchar** y un valor de propiedad **DefinedSize** de 50, que contiene un solo carácter. El valor de la propiedad **ActualSize** que devuelve es la longitud en bytes del carácter único.  
  
### <a name="determining-field-contents"></a>Determinar el contenido del campo  
 El identificador de la columna del origen de datos se representa mediante la propiedad **nombre** del **campo**. La propiedad **Value** del objeto de **campo** devuelve o establece el contenido de datos real del campo. Esta es la propiedad predeterminada.  
  
 Para cambiar los datos de un campo, establezca la propiedad **Value** en un nuevo valor del tipo correcto. El tipo de cursor debe admitir las actualizaciones para cambiar el contenido de un campo. La validación de la base de datos no se realiza aquí en modo por lotes, por lo que tendrá que comprobar si hay errores al llamar a **UpdateBatch** en ese caso. Algunos proveedores también admiten las propiedades **UnderlyingValue** y **OriginalValue** del objeto de **campo** de ADO para ayudarle a resolver conflictos al intentar realizar actualizaciones por lotes. Para obtener más información sobre cómo resolver estos conflictos, vea [Editar datos](../../../ado/guide/data/editing-data.md).  
  
> [!NOTE]
>  No se pueden establecer los valores de los **campos del conjunto de registros** al anexar nuevos **campos** a un **conjunto de registros**. En su lugar, se pueden anexar nuevos **campos** a un **conjunto de registros** cerrado. A continuación, se debe abrir el **conjunto de registros** y solo se pueden asignar valores a estos **campos**.  
  
### <a name="getting-more-field-information"></a>Obtener más información de campo  
 Los objetos ADO tienen dos tipos de propiedades: integrado y dinámico. Hasta este momento, solo se han analizado las propiedades integradas del objeto de **campo** .  
  
 Las propiedades integradas son aquellas que se implementan en ADO y que están inmediatamente disponibles para cualquier objeto nuevo, utilizando la `MyObject.Property` Sintaxis. No aparecen como objetos de **propiedad** en la colección de **propiedades** de un objeto.  
  
 El proveedor de datos subyacente define las propiedades dinámicas y aparecen en la colección de **propiedades** para el objeto ADO adecuado. Por ejemplo, una propiedad específica del proveedor puede indicar si un objeto de **conjunto de registros** admite transacciones o actualizaciones. Estas propiedades adicionales aparecerán como objetos de **propiedad** en la colección de **propiedades** del objeto de **conjunto de registros** . Solo se puede hacer referencia a las propiedades dinámicas a través de la colección utilizando la sintaxis `MyObject.Properties(0)` o `MyObject.Properties("Name")` .  
  
 No se puede eliminar ningún tipo de propiedad.  
  
 Un objeto de **propiedad** dinámica tiene cuatro propiedades integradas propias:  
  
-   La propiedad **Name** es una cadena que identifica la propiedad.  
  
-   La propiedad **Type** es un entero que especifica el tipo de datos de la propiedad.  
  
-   La propiedad **Value** es una variante que contiene el valor de la propiedad. **Value** es la propiedad predeterminada de un objeto **Property** .  
  
-   La propiedad **attributes** es un valor **largo** que indica las características de la propiedad específica del proveedor.  
  
 La colección de **propiedades** para el objeto de **campo** contiene metadatos adicionales sobre el campo. El contenido de esta colección varía en función del proveedor. En el ejemplo de código siguiente se examina la colección de **propiedades** del **conjunto de registros** de ejemplo que se presentó al principio de esta sección. Primero examina el contenido de la colección. Este código utiliza el [proveedor de OLE DB para SQL Server](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md), por lo que la colección **Properties** contiene información relevante para ese proveedor.  
  
```  
'BeginFieldProps  
    Dim objProp As ADODB.Property  
  
    For intLoop = 0 To (objFields.Count - 1)  
        Debug.Print objFields.Item(intLoop).Name  
  
        For Each objProp In objFields(intLoop).Properties  
            Debug.Print vbTab & objProp.Name & " = " & objProp.Value  
        Next objProp  
    Next intLoop  
'EndFieldProps  
```  
  
### <a name="dealing-with-binary-data"></a>Trabajar con datos binarios  
 Use el método [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) en un objeto **Field** para rellenarlo con datos binarios o de caracteres largos. En situaciones en las que la memoria del sistema es limitada, puede utilizar el método **AppendChunk** para manipular valores largos en partes en lugar de en su totalidad.  
  
 Si el **bit adFldLong** en la propiedad **attributes** de un objeto **Field** está establecido en **true**, puede usar el método **AppendChunk** para ese campo.  
  
 La primera llamada a **AppendChunk** en un objeto de **campo** escribe datos en el campo, sobrescribiendo los datos existentes. Las llamadas posteriores a **AppendChunk** se agregan a los datos existentes. Si va a anexar datos a un campo y, a continuación, establece o lee el valor de otro campo en el registro actual, ADO supone que ha terminado de anexar los datos al primer campo. Si llama de nuevo al método **AppendChunk** en el primer campo, ADO interpreta la llamada como una nueva operación de **AppendChunk** y sobrescribe los datos existentes. El acceso a los campos de otros objetos de **conjunto de registros** que no son clones del primer objeto de **conjunto de registros** no interrumpirá las operaciones de **AppendChunk** .  
  
 Use el método **GetChunk** en un objeto de **campo** para recuperar parte o todos los datos binarios o de caracteres largos. En situaciones en las que la memoria del sistema es limitada, puede utilizar el método **GetChunk** para manipular valores largos en partes, en lugar de en su totalidad.  
  
 Los datos que devuelve una llamada a **GetChunk** se asignan a la *variable*. Si el *tamaño* es mayor que el resto de los datos, el método **GetChunk** devuelve solo los datos restantes sin la *variable* padding con espacios vacíos. Si el campo está vacío, el método **GetChunk** devuelve un valor null.  
  
 Cada llamada a **GetChunk** subsiguiente recupera datos a partir de donde se dejó la anterior llamada a **GetChunk** . Sin embargo, si va a recuperar datos de un campo y, a continuación, establece o lee el valor de otro campo en el registro actual, ADO supone que ha terminado de recuperar los datos del primer campo. Si llama de nuevo al método **GetChunk** en el primer campo, ADO interpreta la llamada como una nueva operación **GetChunk** y comienza a leer desde el principio de los datos. El acceso a los campos de otros objetos de **conjunto de registros** que no sean clones del primer objeto de **conjunto de registros** no interrumpirá las operaciones de **GetChunk** .  
  
 Si el **bit adFldLong** en la propiedad **attributes** de un objeto **Field** está establecido en **true**, puede utilizar el método **GetChunk** para ese campo.  
  
 Si no hay ningún registro actual al utilizar el método **GetChunk** o **AppendChunk** en un objeto de **campo** , se produce el error 3021 (no hay registro actual).  
  
 Para obtener un ejemplo del uso de estos métodos para manipular los datos binarios, vea los ejemplos de métodos [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) y [GetChunk](../../../ado/reference/ado-api/getchunk-method-ado.md) en la *Referencia del programador de ADO*.

---
description: Extensiones de Visual C++
title: Usar extensiones de Visual C++ | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- Visual C++ [ADO], using VC++ extensions
- ADO, Visual C++
ms.assetid: ff759185-df41-4507-8d12-0921894ffbd9
author: rothja
ms.author: jroth
ms.openlocfilehash: b5acd1b06bbaad8476ac48261ba49a6ca90cb0b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028660"
---
# <a name="visual-c-extensions"></a>Extensiones de Visual C++
## <a name="the-iadorecordbinding-interface"></a>La Interfaz IADORecordBinding
 Las extensiones de Microsoft Visual C++ para ADO asocian o enlazan campos de un objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) a variables de C/C++. Siempre que cambia la fila actual del **conjunto de registros** enlazado, todos los campos enlazados del **conjunto de registros** se copian en las variables de C/C++. Si es necesario, los datos copiados se convierten al tipo de datos declarado de la variable de C/C++.

 El método **BindToRecordset** de la interfaz **IADORecordBinding** enlaza los campos a las variables de C/C++. El método **AddNew** agrega una nueva fila al conjunto de **registros** enlazado. El método **Update** rellena los campos de las nuevas filas del **conjunto de registros** o actualiza los campos de las filas existentes, con el valor de las variables de C/C++.

 El objeto **Recordset** implementa la interfaz **IADORecordBinding** . No codifique la implementación usted mismo.

## <a name="binding-entries"></a>Entradas de enlace
 Las extensiones de Visual C++ para los campos de asignación de ADO de un objeto de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) a variables de C/C++. La definición de una asignación entre un campo y una variable se denomina *entrada de enlace*. Las macros proporcionan entradas de enlace para datos numéricos, de longitud fija y de longitud variable. Las entradas de enlace y las variables de C/C++ se declaran en una clase derivada de la clase Visual C++ Extensions, **CADORecordBinding**. Las macros de entrada de enlace definen internamente la clase **CADORecordBinding** .

 ADO asigna internamente los parámetros de estas macros a una estructura OLE DB **DBBINDING** y crea un objeto de **descriptor de acceso** OLE DB para administrar el movimiento y la conversión de datos entre los campos y las variables. OLE DB define los datos que se componen de tres partes: un *búfer* donde se almacenan los datos; un *Estado* que indica si un campo se ha almacenado correctamente en el búfer o cómo se debe restaurar la variable en el campo; y la *longitud* de los datos. (Vea [obtener y establecer datos (OLE DB)](/previous-versions/windows/desktop/ms713700(v=vs.85))en la referencia del programador de OLE DB para obtener más información).

## <a name="header-file"></a>Archivo de encabezado
 Incluya el siguiente archivo en la aplicación para poder usar las extensiones de Visual C++ para ADO:

```cpp
#include <icrsint.h>
```

## <a name="binding-recordset-fields"></a>Enlazar campos de conjunto de registros

#### <a name="to-bind-recordset-fields-to-cc-variables"></a>Para enlazar campos de conjunto de registros a variables de C/C++

1.  Cree una clase derivada de la clase **CADORecordBinding** .

2.  Especifique las entradas de enlace y las variables de C/C++ correspondientes en la clase derivada. Corchete las entradas de enlace entre las macros **BEGIN_ADO_BINDING** y **END_ADO_BINDING** . No finalice las macros con comas o punto y coma. Cada macro especifica automáticamente los delimitadores correspondientes.

     Especifique una entrada de enlace para cada campo que se va a asignar a una variable de C/C++. Use un miembro adecuado de la familia de macros **ADO_FIXED_LENGTH_ENTRY**, **ADO_NUMERIC_ENTRY** o **ADO_VARIABLE_LENGTH_ENTRY** .

3.  En la aplicación, cree una instancia de la clase derivada de **CADORecordBinding**. Obtiene la interfaz **IADORecordBinding** del **conjunto de registros**. A continuación, llame al método **BindToRecordset** para enlazar los campos de **conjunto de registros** a las variables de C/C++.

 Para obtener más información, vea el [ejemplo de extensiones de Visual C++](./visual-c-extensions-example.md).

## <a name="interface-methods"></a>Métodos de interfaz
 La interfaz **IADORecordBinding** tiene tres métodos: **BindToRecordset**, **AddNew** y **Update**. El único argumento de cada método es un puntero a una instancia de la clase derivada de **CADORecordBinding**. Por lo tanto, los métodos **AddNew** y **Update** no pueden especificar ninguno de los parámetros de su método ADO namesakes.

## <a name="syntax"></a>Sintaxis
 El método **BindToRecordset** asocia los campos de **conjunto de registros** con variables de C/C++.

```cpp
BindToRecordset(CADORecordBinding *binding)
```

 El método **AddNew** invoca su homónimo, el método [AddNew](../../reference/ado-api/addnew-method-ado.md) de ADO, para agregar una nueva fila al **conjunto de registros**.

```cpp
AddNew(CADORecordBinding *binding)
```

 El método **Update** invoca su homónimo, el método [Update](../../reference/ado-api/update-method.md) de ADO, para actualizar el **conjunto de registros**.

```cpp
Update(CADORecordBinding *binding)
```

## <a name="binding-entry-macros"></a>Macros de entrada de enlace
 Las macros de entrada de enlace definen la Asociación de un campo de **conjunto de registros** y una variable. Una macro de inicio y finalización delimita el conjunto de entradas de enlace.

 Se proporcionan familias de macros para los datos de longitud fija, como **adDate** o **adBoolean**; datos numéricos, como **adTinyInt**, **adInteger** o **adDouble**; y datos de longitud variable, como **adChar**, **advarchar** o **adVarBinary**. Todos los tipos numéricos, excepto para **adVarNumeric**, también son tipos de longitud fija. Cada familia tiene diferentes conjuntos de parámetros para que pueda excluir la información de enlace que no es de interés.

 Para obtener más información, vea el [Apéndice A: tipos de datos](/previous-versions/windows/desktop/ms723969(v=vs.85))de la referencia del programador de OLE DB.

### <a name="begin-binding-entries"></a>Iniciar entradas de enlace
 **BEGIN_ADO_BINDING**(*clase*)

### <a name="fixed-length-data"></a>Fixed-Length de datos
 **ADO_FIXED_LENGTH_ENTRY**(*ordinal, DataType, buffer, status, Modify*)

 **ADO_FIXED_LENGTH_ENTRY2**(*ordinal, DataType, buffer, Modify*)

### <a name="numeric-data"></a>Datos numéricos
 **ADO_NUMERIC_ENTRY**(*ordinal, DataType, buffer, Precision, Scale, status, Modify*)

 **ADO_NUMERIC_ENTRY2**(*ordinal, DataType, buffer, Precision, Scale, Modify*)

### <a name="variable-length-data"></a>Variable-Length de datos
 **ADO_VARIABLE_LENGTH_ENTRY**(*ordinal, DataType, buffer, size, status, length, Modify*)

 **ADO_VARIABLE_LENGTH_ENTRY2**(*ordinal, DataType, buffer, size, status, Modify*)

 **ADO_VARIABLE_LENGTH_ENTRY3**(*ordinal, DataType, buffer, size, length, Modify*)

 **ADO_VARIABLE_LENGTH_ENTRY4**(*ordinal, DataType, buffer, size, Modify*)

### <a name="end-binding-entries"></a>Finalizar entradas de enlace
 **END_ADO_BINDING**()

|Parámetro|Descripción|
|---------------|-----------------|
|*Clase*|Clase en la que se definen las entradas de enlace y las variables de C/C++.|
|*Ordinal*|Número ordinal, contando desde uno, del campo de **conjunto de registros** correspondiente a la variable de C/C++.|
|*DataType*|Tipo de datos ADO equivalente de la variable C/C++ (vea [DataTypeEnum](../../reference/ado-api/datatypeenum.md) para obtener una lista de tipos de datos válidos). Si es necesario, el valor del campo de **conjunto de registros** se convertirá en este tipo de datos.|
|*Buffer*|Nombre de la variable de C/C++ en la que se almacenará el campo de **conjunto de registros** .|
|*Tamaño*|Tamaño máximo en bytes del *búfer*. Si el *búfer* va a contener una cadena de longitud variable, deje espacio para un cero final.|
|*Estado*|Nombre de una variable que indica si el contenido del *búfer* es válido y si la conversión del campo en *DataType* era correcta.<br /><br /> Los dos valores más importantes para esta variable son **adFldOK**, lo que significa que la conversión se realizó correctamente; y **adFldNull**, lo que significa que el valor del campo sería una variante de tipo VT_NULL y no solo está vacía.<br /><br /> Los valores posibles para *status* se muestran en la tabla siguiente, "status Values".|
|*Modify*|Marca booleana; Si es TRUE, indica que ADO puede actualizar el campo de **conjunto de registros** correspondiente con el valor contenido en el *búfer*.<br /><br /> Establezca el parámetro booleano *Modify* en true para permitir que ADO actualice el campo enlazado y false si desea examinar el campo pero no cambiarlo.|
|*Precisión*|Número de dígitos que se pueden representar en una variable numérica.|
|*Escala*|Número de posiciones decimales en una variable numérica.|
|*Duración*|Nombre de una variable de cuatro bytes que contendrá la longitud real de los datos en el *búfer*.|

## <a name="status-values"></a>Valores de estado
 El valor de la variable de *Estado* indica si un campo se copió correctamente en una variable.

 Al establecer los datos, el *Estado* puede establecerse en **adFldNull** para indicar que el campo de **conjunto de registros** debe establecerse en NULL.

|Constante|Value|Descripción|
|--------------|-----------|-----------------|
|**adFldOK**|0|Se devolvió un valor de campo que no es NULL.|
|**adFldBadAccessor**|1|El enlace no era válido.|
|**adFldCantConvertValue**|2|No se pudo convertir el valor por motivos distintos a un error de coincidencia de signos o desbordamiento de datos.|
|**adFldNull**|3|Al obtener un campo, indica que se devolvió un valor null.<br /><br /> Al establecer un campo, indica que el campo debe establecerse en **null** cuando el campo no puede codificar **null** (por ejemplo, una matriz de caracteres o un entero).|
|**adFldTruncated**|4|Los datos de longitud variable o los dígitos numéricos se truncaron.|
|**adFldSignMismatch**|5|El valor es signed y el tipo de datos de variable es sin signo.|
|**adFldDataOverFlow**|6|El valor es mayor que el que se puede almacenar en el tipo de datos de la variable.|
|**adFldCantCreate**|7|El tipo de columna y el campo desconocidos ya están abiertos.|
|**adFldUnavailable**|8|No se pudo determinar el valor del campo, por ejemplo, en un nuevo campo sin asignar sin ningún valor predeterminado.|
|**adFldPermissionDenied**|9|Al actualizar, no tiene permiso para escribir datos.|
|**adFldIntegrityViolation**|10|Al actualizar, el valor de campo infringiría la integridad de la columna.|
|**adFldSchemaViolation**|11|Al actualizar, el valor de campo infringiría el esquema de columna.|
|**adFldBadStatus**|12|Al actualizar, parámetro de estado no válido.|
|**adFldDefault**|13|Al actualizar, se utiliza un valor predeterminado.|

## <a name="see-also"></a>Consulte también
 [Ejemplo de Visual C++ extensions](./visual-c-extensions-example.md) [Visual C++ encabezado Extensions](./visual-c-extensions-header.md)
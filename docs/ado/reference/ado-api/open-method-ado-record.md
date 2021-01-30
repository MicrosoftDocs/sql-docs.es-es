---
description: Open (método) (registro de ADO)
title: Open (método) (record de ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_Open
- _Record::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: ab79a623-88a9-40b6-a017-a658bf19b778
author: rothja
ms.author: jroth
ms.openlocfilehash: cdac33c235e50abf35fbed670d872ca631403b5d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166985"
---
# <a name="open-method-ado-record"></a>Open (método) (registro de ADO)
Abre un objeto [Record](./record-object-ado.md) existente o crea un nuevo elemento representado por el **registro**, como un archivo o un directorio.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Open Source, ActiveConnection, Mode, CreateOptions, Options, UserName, Password  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Origen*  
 Opcional. Una **variante** que puede representar la dirección URL de la entidad que va a representar este objeto de **registro** , un **comando**, un [conjunto de registros](./recordset-object-ado.md) abierto u otro objeto de **registro** , una cadena que contiene una instrucción SELECT de SQL o un nombre de tabla.  
  
 *ActiveConnection*  
 Opcional. **Variante** que representa la cadena de conexión o el objeto de [conexión](./connection-object-ado.md) abierto.  
  
 *Modo*  
 Opcional. Valor [ConnectModeEnum](./connectmodeenum.md) que especifica el modo de acceso para el objeto de **registro** resultante. El valor predeterminado es **adModeUnknown**.  
  
 *CreateOptions*  
 Opcional. Un valor [RecordCreateOptionsEnum](./recordcreateoptionsenum.md) que especifica si se debe abrir un archivo o directorio existente o se debe crear un nuevo archivo o directorio. El valor predeterminado es **adFailIfNotExists**. Si se establece en el valor predeterminado, el modo de acceso se obtiene de la propiedad [mode](./mode-property-ado.md) . Este parámetro se omite cuando el parámetro de *origen* no contiene una dirección URL.  
  
 *Opciones*  
 Opcional. Valor de [RecordOpenOptionsEnum](./recordopenoptionsenum.md) que especifica las opciones para abrir el **registro**. El valor predeterminado es **adOpenRecordUnspecified**. Estos valores se pueden combinar.  
  
 *UserName*  
 Opcional. Valor de **cadena** que contiene el ID. de usuario que, si es necesario, autoriza el acceso al *origen*.  
  
 *Contraseña*  
 Opcional. Valor de **cadena** que contiene la contraseña que, si es necesaria, comprueba el *nombre de usuario*.  
  
## <a name="remarks"></a>Observaciones  
 El *origen* puede ser:  
  
-   Dirección URL. Si el protocolo de la dirección URL es http, se invocará el proveedor de Internet de forma predeterminada. Si la dirección URL apunta a un nodo que contiene un script ejecutable (como. Página ASP), se abre de forma predeterminada un **registro** que contiene el origen en lugar del contenido ejecutado. Use el argumento *Options* para modificar este comportamiento.  
  
-   Objeto de **registro** . Un objeto de **registro** abierto desde otro **registro** clonará el objeto de **registro** original.  
  
-   Objeto de **comando** . El objeto de **registro** abierto representa la fila única devuelta al ejecutar el **comando**. Si los resultados contienen más de una fila, el contenido de la primera fila se coloca en el registro y se puede Agregar un error a la colección de **errores** .  
  
-   Instrucción SELECT de SQL. El objeto de **registro** abierto representa la fila única devuelta al ejecutar el contenido de la cadena. Si los resultados contienen más de una fila, el contenido de la primera fila se coloca en el registro y se puede Agregar un error a la colección de **errores** .  
  
-   Un nombre de tabla.  
  
 Si el objeto **Record** representa una entidad a la que no se puede tener acceso con una dirección URL (por ejemplo, una fila de un **conjunto de registros** derivado de una base de datos), los valores de la propiedad [ParentURL](./parenturl-property-ado.md) y el campo al que se obtiene acceso con la constante **adRecordURL** son NULL.  
  
> [!NOTE]
>  Las direcciones URL que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Record (ADO)](./record-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Open (método) (conexión de ADO)](./open-method-ado-connection.md)   
 [Open (método) (conjunto de registros ADO)](./open-method-ado-recordset.md)   
 [Open (método, secuencia de ADO)](./open-method-ado-stream.md)   
 [Método OpenSchema](./openschema-method.md)
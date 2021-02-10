---
description: Open (método) (conjunto de registros ADO)
title: Open (método) (conjunto de registros ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_Open
- Recordset15::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: 3236749c-4b71-4235-89e2-ccdfaaa9319d
author: rothja
ms.author: jroth
ms.openlocfilehash: 0f9cf81ef9845875db984e4cba0fcd7bc23e380c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041414"
---
# <a name="open-method-ado-recordset"></a>Open (método) (conjunto de registros ADO)
Abre un cursor en un objeto de [conjunto de registros](./recordset-object-ado.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
recordset.Open Source, ActiveConnection, CursorType, LockType, Options  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Origen*  
 Opcional. **Variante** que se evalúa como un objeto de [comando](./command-object-ado.md) válido, una instrucción SQL, un nombre de tabla, una llamada a un procedimiento almacenado, una dirección URL o el nombre de un archivo o un objeto de [flujo](./stream-object-ado.md) que contiene un [conjunto de registros](./recordset-object-ado.md)almacenado de forma persistente.  
  
 *ActiveConnection*  
 Opcional. Una **variante** que se evalúa como un nombre de variable de objeto de [conexión](./connection-object-ado.md) válido o una **cadena** que contiene parámetros [ConnectionString](./connectionstring-property-ado.md) .  
  
 *CursorType*  
 Opcional. Valor de [CursorTypeEnum](./cursortypeenum.md) que determina el tipo de cursor que el proveedor debe usar al abrir el **conjunto de registros**. El valor predeterminado es **adOpenForwardOnly**.  
  
 *LockType*  
 Opcional. Valor de [LockTypeEnum](./locktypeenum.md) que determina qué tipo de bloqueo (simultaneidad) debe utilizar el proveedor al abrir el **conjunto de registros**. El valor predeterminado es **adLockReadOnly**.  
  
 *Opciones*  
 Opcional. Un valor de **tipo Long** que indica cómo el proveedor debe evaluar el argumento de *origen* si representa algo distinto de un objeto **Command** o si el **conjunto de registros** debe restaurarse a partir de un archivo en el que se guardó previamente. Puede ser uno o varios valores [CommandTypeEnum](./commandtypeenum.md) o [ExecuteOptionEnum](./executeoptionenum.md) , que se pueden combinar con un operador OR bit a bit.  
  
> [!NOTE]
>  Si abre un **conjunto de registros** desde una **secuencia** que contiene un **conjunto de registros** persistente, el uso de un valor de [ExecuteOptionEnum](./executeoptionenum.md) de **adAsyncFetchNonBlocking** no tendrá ningún efecto; la captura será sincrónica y se bloqueará.  
  
> [!NOTE]
>  Los valores de **ExecuteOpenEnum** de **adExecuteNoRecords** o **adExecuteStream** no deben usarse con **Open**.  
  
## <a name="remarks"></a>Observaciones  
 El cursor predeterminado para un **conjunto de registros** ADO es un cursor de solo avance y de solo lectura que se encuentra en el servidor.  
  
 Al usar el método **Open** en un objeto de **conjunto de registros** , se abre un cursor que representa los registros de una tabla base, los resultados de una consulta o un **conjunto de registros** previamente guardado.  
  
 Use el argumento *source* opcional para especificar un origen de datos mediante uno de los siguientes: una variable de objeto de **comando** , una instrucción SQL, un procedimiento almacenado, un nombre de tabla, una dirección URL o un nombre de ruta de acceso de archivo completo. Si el *origen* es un nombre de ruta de acceso de archivo, puede ser una ruta de acceso completa ("c:\dir\file.RST"), una ruta de acceso relativa (".. \file.RST ") o una dirección URL ( `https://files/file.rst` ).  
  
 No es recomendable usar el argumento *source* del método **Open** para realizar una consulta de acción que no devuelva registros porque no hay ninguna manera fácil de determinar si la llamada se realizó correctamente. Se cerrará el **conjunto de registros** devuelto por esta consulta. Para realizar una consulta que no devuelva registros, como una instrucción INSERT de SQL, llame al método [Execute](./execute-method-ado-command.md) de un objeto **Command** o al método [Execute](./execute-method-ado-connection.md) de un objeto [Connection](./connection-object-ado.md) en su lugar.  
  
 El argumento *ActiveConnection* corresponde a la propiedad [ActiveConnection](./activeconnection-property-ado.md) y especifica en qué conexión se debe abrir el objeto de **conjunto de registros** . Si pasa una definición de conexión para este argumento, ADO abrirá una nueva conexión con los parámetros especificados. Después de abrir el **conjunto de registros** con un cursor del lado cliente estableciendo la propiedad [CursorLocation](./cursorlocation-property-ado.md) en **adUseClient**, puede cambiar el valor de esta propiedad para enviar las actualizaciones a otro proveedor. También puede establecer esta propiedad en **Nothing** (en Microsoft Visual Basic) o null para desconectar el **conjunto de registros** de cualquier proveedor. Sin embargo, si se cambia *ActiveConnection* para un cursor del servidor, se genera un error.  
  
 Para los demás argumentos que corresponden directamente a las propiedades de un objeto de **conjunto de registros** (*source*, *CursorType* y *LockType*), la relación de los argumentos con las propiedades es la siguiente:  
  
-   La propiedad es de lectura y escritura antes de que se abra el objeto de **conjunto de registros** .  
  
-   Se usa la configuración de la propiedad a menos que se pasen los argumentos correspondientes al ejecutar el método **Open** . Si se pasa un argumento, reemplaza el valor de la propiedad correspondiente y el valor de la propiedad se actualiza con el valor del argumento.  
  
-   Después de abrir el objeto de **conjunto de registros** , estas propiedades pasan a ser de solo lectura.  
  
> [!NOTE]
>  La propiedad **ActiveConnection** es de solo lectura para los objetos **Recordset** cuya propiedad [source](./source-property-ado-recordset.md) se establece en un objeto **Command** válido, incluso si el objeto **Recordset** no está abierto.  
  
 Si pasa un objeto **Command** en el argumento *source* y también pasa un argumento *ActiveConnection* , se produce un error. La propiedad **ActiveConnection** del objeto de **comando** ya debe estar establecida en un objeto de **conexión** o una cadena de conexión válidos.  
  
 Si pasa algo que no sea un objeto **Command** en el argumento *source* , puede usar el argumento *Options* para optimizar la evaluación del argumento *source* . Si el argumento *Options* no está definido, puede experimentar un rendimiento reducido porque ADO debe realizar llamadas al proveedor para determinar si el argumento es una instrucción SQL, un procedimiento almacenado, una dirección URL o un nombre de tabla. Si sabe qué tipo de *origen* está utilizando, al establecer el argumento *Opciones* se indica a ADO que salte directamente al código relevante. Si el argumento *Options* no coincide con el tipo de *origen* , se produce un error.  
  
 Si pasa un objeto de **flujo** en el argumento de *origen* , no debe pasar información a los demás argumentos. Si lo hace, se generará un error. La información de **ActiveConnection** no se conserva cuando se abre un **conjunto de registros** desde una **secuencia**.  
  
 El valor predeterminado para el argumento *Options* es **adCmdFile** si no hay ninguna conexión asociada al **conjunto de registros**. Este suele ser el caso de los objetos de **conjunto de registros** almacenados de forma persistente.  
  
 Si el origen de datos no devuelve ningún registro, el proveedor establece las propiedades [BOF](./bof-eof-properties-ado.md) y [EOF](./bof-eof-properties-ado.md) en **true** y la posición del registro actual es indefinida. Todavía puede agregar nuevos datos a este objeto de **conjunto de registros** vacío si el tipo de cursor lo permite.  
  
 Cuando haya finalizado las operaciones sobre un objeto de **conjunto de registros** abierto, use el método [Close](./close-method-ado.md) para liberar los recursos del sistema asociados. Al cerrar un objeto no se quita de la memoria; puede cambiar la configuración de sus propiedades y usar el método **Open** para abrirla de nuevo más adelante. Para eliminar completamente un objeto de la memoria, establezca la variable de objeto en *Nothing*.  
  
 Antes de establecer la propiedad **ActiveConnection** , llame **a Open** sin operandos para crear una instancia de un **conjunto de registros** creado anexando los campos a la colección de [campos](./fields-collection-ado.md) de **conjunto de registros** .  
  
 Si ha establecido la propiedad [CursorLocation](./cursorlocation-property-ado.md) en **adUseClient**, puede recuperar las filas de forma asincrónica de una de estas dos maneras. El método recomendado consiste en establecer *las opciones* en **adAsyncFetch**. Como alternativa, puede usar la propiedad dinámica "procesamiento de conjuntos de filas asíncronos" en la colección de [propiedades](./properties-collection-ado.md) , pero se pueden perder eventos recuperados relacionados si no establece el parámetro *Options* en **adAsyncFetch**.  
  
> [!NOTE]
>  La captura en segundo plano en el proveedor de MS Remote solo se admite a través del parámetro *Options* del método **Open** .  
  
> [!NOTE]
>  Las direcciones URL que usan el esquema http invocarán automáticamente el [proveedor de Microsoft OLE DB para la publicación en Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Para obtener más información, consulte [direcciones URL absolutas y relativas](../../guide/data/absolute-and-relative-urls.md).  
  
 Ciertas combinaciones de valores [CommandTypeEnum](./commandtypeenum.md) y [ExecuteOptionEnum](./executeoptionenum.md) no son válidas. Para obtener información sobre las opciones que no se pueden combinar, vea los temas para [ExecuteOptionEnum](./executeoptionenum.md)y [CommandTypeEnum](./commandtypeenum.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos Open y Close (VB)](./open-and-close-methods-example-vb.md)   
 [Ejemplo de los métodos Open y Close (VBScript)](./open-and-close-methods-example-vbscript.md)   
 [Ejemplo de los métodos Open y Close (VC + +)](./open-and-close-methods-example-vc.md)   
 [Ejemplo de métodos Save y Open (VB)](./save-and-open-methods-example-vb.md)   
 [Open (método) (conexión de ADO)](./open-method-ado-connection.md)   
 [Open (método) (record de ADO)](./open-method-ado-record.md)   
 [Open (método, secuencia de ADO)](./open-method-ado-stream.md)   
 [OpenSchema (método)](./openschema-method.md)   
 [Save (método)](./save-method.md)
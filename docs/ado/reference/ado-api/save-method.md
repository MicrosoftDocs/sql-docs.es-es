---
description: Save (método)
title: Save (método) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Recordset::Save
- _Recordset::raw_Save
helpviewer_keywords:
- Save method [ADO]
ms.assetid: ed3d9678-5c28-4e61-8bb3-7dfb66d99cf5
author: rothja
ms.author: jroth
ms.openlocfilehash: a15cb79b3dc6a00e8096cec5289a5c8ce5304938
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051522"
---
# <a name="save-method"></a>Save (método)
Guarda el [conjunto de registros](./recordset-object-ado.md) en un archivo o en un objeto de [secuencia](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
recordset.Save Destination, PersistFormat  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Destino*  
 Opcional. **Variante** que representa el nombre completo de la ruta de acceso del archivo donde se va a guardar el **conjunto de registros** o una referencia a un objeto de **secuencia** .  
  
 *PersistFormat*  
 Opcional. Valor [PersistFormatEnum](./persistformatenum.md) que especifica el formato en el que se va a guardar el **conjunto de registros** (XML o ADTG). El valor predeterminado es **adPersistADTG**.  
  
## <a name="remarks"></a>Observaciones  
 El método **Save método** solo se puede invocar en un **conjunto de registros** abierto. Utilice el método [Open (conjunto de registros ADO)](./open-method-ado-recordset.md) para restaurar posteriormente el **conjunto de registros** desde *Destination*.  
  
 Si la propiedad [Filter Property](./filter-property.md) está en vigor para el **conjunto de registros**, solo se guardan las filas a las que se puede tener acceso en el filtro. Si el **conjunto de registros** es jerárquico, se guardan el **conjunto de registros** secundario actual y sus elementos secundarios, incluido el conjunto de **registros** primario. Si se llama al método Save de un **conjunto de registros** secundario, se guardan el elemento secundario y todos sus elementos secundarios, pero el elemento primario no lo es.  
  
 La primera vez que se guarda el **conjunto de registros**, es opcional especificar el *destino*. Si omite *Destination*, se creará un nuevo archivo con un nombre establecido en el valor de la propiedad Source del conjunto de **registros**.  
  
 Omitir *destino* cuando se llama a **Save** después de la primera vez que se guarda o se produce un error en tiempo de ejecución. Si posteriormente llama a **Save** con un nuevo *destino*, el **conjunto de registros** se guarda en el nuevo destino. Sin embargo, el nuevo destino y el destino original se abrirán.  
  
 **Guardar** no cierra el **conjunto de registros** o el *destino*, por lo que puede continuar trabajando con el **conjunto de registros** y guardar los cambios más recientes. El *destino* permanece abierto hasta que se cierra el **conjunto de registros** .  
  
 Por motivos de seguridad, el método **Save** solo permite el uso de la configuración de seguridad baja y personalizada de un script ejecutado por Microsoft Internet Explorer.  
  
 Si se llama al método **Save** mientras una operación de captura, ejecución o actualización de **conjunto de registros** asincrónica está en curso, se **guardará** la espera hasta que se complete la operación asincrónica.  
  
 Los registros se guardan empezando por la primera fila del **conjunto de registros**. Cuando finaliza el método **Save** , la posición de la fila actual se mueve a la primera fila del **conjunto de registros**.  
  
 Para obtener los mejores resultados, establezca la propiedad [CursorLocation (propiedad de ADO)](./cursorlocation-property-ado.md) en **adUseClient** con **Guardar**. Si el proveedor no admite toda la funcionalidad necesaria para guardar objetos de **conjunto de registros** , el servicio de cursor proporcionará esa funcionalidad.  
  
 Cuando un **conjunto de registros** se mantiene con la propiedad **CursorLocation** establecida en **adUseServer**, la capacidad de actualización del **conjunto de registros** es limitada. Normalmente, solo se permiten actualizaciones de tabla única, inserciones y eliminaciones (dependiendo de la funcionalidad del proveedor). El método [Resync Method](./resync-method.md) también está disponible en esta configuración.  
  
> [!NOTE]
>  ADO no admite el guardado de un **conjunto de registros** con **campos** de tipo **adVariant**, **adIDispatch** o **adIUnknown** y puede producir resultados imprevisibles.  
  
 Solo los filtros en forma de cadenas de criterios (por ejemplo, OrderDate > ' 12/31/1999 ') afectan al contenido de un **conjunto de registros** persistente. Los filtros creados con una matriz de **marcadores** o usando un valor de [FilterGroupEnum](./filtergroupenum.md) no afectarán al contenido del **conjunto de registros** guardado. Estas reglas se aplican al **conjunto de registros** creado con cursores del lado cliente o del lado servidor.  
  
 Dado que el parámetro de *destino* puede aceptar cualquier objeto que admita la interfaz OLE DB IStream, puede guardar un **conjunto de registros** directamente en el objeto de respuesta de ASP. Para obtener más información, vea el **escenario de persistencia del conjunto de registros XML**.  
  
 También puede guardar un **conjunto de registros** en formato XML en una instancia de un objeto DOM MSXML, como se muestra en el siguiente código de Visual Basic:  
  
```  
Dim xDOM As New MSXML.DOMDocument  
Dim rsXML As New ADODB.Recordset  
Dim sSQL As String, sConn As String  
  
sSQL = "SELECT customerid, companyname, contactname FROM customers"  
sConn="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=Northwind.mdb"  
rsXML.Open sSQL, sConn  
rsXML.Save xDOM, adPersistXML   'Save Recordset directly into a DOM tree.  
...  
```  
  
> [!NOTE]
>  Al guardar conjuntos de registros jerárquicos (formas de datos) en formato XML, se aplican dos limitaciones. No se puede guardar en XML si el **conjunto de registros** jerárquico contiene actualizaciones pendientes y no se puede guardar un conjunto de **registros** jerárquico con parámetros.  
  
 Un **conjunto de registros** guardado en formato XML se guarda con el formato UTF-8. Cuando se carga un archivo de este tipo en una secuencia de ADO, el objeto de secuencia no intentará abrir un **conjunto de registros** desde la secuencia a menos que la propiedad charset de la secuencia esté establecida en el valor adecuado para el formato UTF-8.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto de secuencia (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de métodos Save y Open (VB)](./save-and-open-methods-example-vb.md)   
 [Ejemplo de métodos Save y Open (VC + +)](./save-and-open-methods-example-vc.md)   
 [Open (método) (conjunto de registros ADO)](./open-method-ado-recordset.md)   
 [Open (método, secuencia de ADO)](./open-method-ado-stream.md)   
 [Método SaveToFile](./savetofile-method.md)
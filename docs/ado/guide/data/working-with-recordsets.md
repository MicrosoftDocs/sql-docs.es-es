---
description: Trabajar con conjuntos de registros
title: Trabajar con conjuntos de registros | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset object [ADO]
ms.assetid: bdf9a56a-de4a-44de-9111-2f11ab7b16ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 4e64608adc461534a30630a6b8b8d29d0478544a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032257"
---
# <a name="working-with-recordsets"></a>Trabajar con conjuntos de registros
El objeto de **conjunto de registros** tiene características integradas que permiten reorganizar el orden de los datos en el conjunto de resultados, buscar un registro específico en función de los criterios proporcionados e incluso optimizar esas operaciones de búsqueda mediante índices. El uso de estas características depende del proveedor y, en algunos casos, como el de la propiedad de [Índice](../../../ado/reference/ado-api/index-property.md) , la estructura del propio origen de datos.  
  
## <a name="arranging-data"></a>Organizar datos  
 Con frecuencia, la manera más eficaz de ordenar los datos en el **conjunto de registros** es mediante la especificación de una cláusula ORDER BY en el comando SQL que se usa para devolver resultados. Sin embargo, es posible que tenga que cambiar el orden de los datos en un **conjunto de registros** que ya se ha creado. Puede utilizar la propiedad **Sort** para establecer el orden en el que se recorren las filas de un **conjunto de registros** . Además, la propiedad **Filter** determina las filas a las que se puede tener acceso al atravesar filas.  
  
 La propiedad **Sort** establece o devuelve un valor de **cadena** que indica los nombres de campo del **conjunto de registros** por los que se va a ordenar. Cada nombre está separado por una coma y, opcionalmente, sigue un espacio y la palabra clave **ASC** (que ordena el campo en orden ascendente) o **DESC** (que ordena el campo en orden descendente). De forma predeterminada, si no se especifica ninguna palabra clave, el campo se ordena en orden ascendente.  
  
 La operación de ordenación es eficaz porque los datos no se reorganizan físicamente pero se obtiene acceso a ellos en el orden especificado por un índice.  
  
 La propiedad **Sort** requiere que la propiedad [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) esté establecida en **adUseClient**. Se creará un índice temporal para cada campo especificado en la propiedad **Sort** si aún no existe un índice.  
  
 Si establece la propiedad **Sort** en una cadena vacía, se restablecerán las filas a su orden original y se eliminarán los índices temporales. Los índices existentes no se eliminarán.  
  
 Supongamos que un **conjunto de registros** contiene tres campos denominados *FirstName*, *middleInitial* y *LastName*. Establezca la propiedad **Sort** en la cadena " `lastName DESC, firstName ASC` ", que ordenará el **conjunto de registros** por apellido en orden descendente y luego por nombre en orden ascendente. Se omite la inicial del segundo nombre.  
  
 Ningún campo al que se haga referencia en una cadena de criterios de ordenación puede denominarse "ASC" o "DESC", ya que esos nombres entran en conflicto con las palabras clave **ASC** y **DESC**. Asigne un alias a un campo que tenga un nombre en conflicto mediante la palabra clave **as** en la consulta que devuelve el **conjunto de registros**.  
  
 Para obtener más información sobre el filtrado de **conjuntos de registros** , vea "filtrar los resultados" más adelante en este tema.  
  
## <a name="finding-a-specific-record"></a>Buscar un registro específico  
 ADO proporciona los métodos [Find](../../../ado/reference/ado-api/find-method-ado.md) y [Seek](../../../ado/reference/ado-api/seek-method.md) para buscar un registro determinado en un **conjunto de registros**. El método **Find** es compatible con varios proveedores, pero está limitado a un único criterio de búsqueda. El método **Seek** admite la búsqueda en varios criterios, pero no es compatible con muchos proveedores.  
  
 Los índices en los campos pueden mejorar en gran medida el rendimiento del método **Buscar** y las propiedades de **ordenación** y **filtrado** del objeto de **conjunto de registros** . Puede crear un índice interno para un objeto de **campo** estableciendo su propiedad [optimización](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md) dinámica. Esta propiedad dinámica se agrega a la colección **Properties** del objeto **Field** cuando se establece la propiedad [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) en **adUseClient**. Recuerde que este índice es interno de ADO: no puede obtener acceso a él ni usarlo para ningún otro propósito. Además, este índice es diferente de la propiedad de [Índice](../../../ado/reference/ado-api/index-property.md) del objeto de **conjunto de registros** .  
  
 El método **Find** busca rápidamente un valor dentro de una columna (campo) de un **conjunto de registros**. Con frecuencia, puede mejorar la velocidad del método **Find** en una columna mediante la propiedad **Optimize** para crear un índice en él.  
  
 El método **Find** limita la búsqueda al contenido de un campo. El método **Seek** requiere que tenga un índice y también tenga otras limitaciones. Si tiene que buscar en varios campos que no son la base de un índice o si el proveedor no admite índices, puede limitar los resultados mediante la propiedad **Filter** del objeto de **conjunto de registros** .  
  
### <a name="find"></a>Buscar  
 El método **Find** busca en un **conjunto de registros** la fila que cumple un criterio especificado. Opcionalmente, se puede especificar la dirección de la búsqueda, la fila inicial y el desplazamiento de la fila inicial. Si se cumple el criterio, la posición de la fila actual se establece en el registro encontrado; de lo contrario, la posición se establece en el final (o en el inicio) del **conjunto de registros**, en función de la dirección de búsqueda.  
  
 Solo se puede especificar un nombre de una sola columna para el criterio. En otras palabras, este método no admite búsquedas en varias columnas.  
  
 El operador de comparación para el criterio puede ser " **>** " (mayor que), "* * \<**" (less than), "=" (equal), "> =" (mayor o igual que), "<=" (menor o igual que), "<>" (no igual) o "like" (coincidencia de patrones).  
  
 El valor del criterio puede ser una cadena, un número de punto flotante o una fecha. Los valores de cadena se delimitan con comillas simples o "#" (signo de número) (por ejemplo, "State = ' WA '" o "State = #WA #"). Los valores de fecha se delimitan con marcas "#" (signo de número) (por ejemplo, "start_date > #7/22/97 #").  
  
 Si el operador de comparación es "like", el valor de cadena puede contener un asterisco (*) para buscar una o más apariciones de cualquier carácter o subcadena. Por ejemplo, "State like \* '" coincide con Maine y Massachusetts. También puede usar asteriscos iniciales y finales para buscar una subcadena incluida dentro de los valores. Por ejemplo, "State like ' \* as \* '" coincide con Alaska, Arkansas y Massachusetts.  
  
 Los asteriscos solo se pueden usar al final de una cadena de criterios o juntos al principio y al final de una cadena de criterios, como se mostró anteriormente. No se puede usar el asterisco como carácter comodín inicial (' * str ') o carácter comodín incrustado ( \* r '). Esto producirá un error.  
  
### <a name="seek-and-index"></a>Búsqueda e índice  
 Use el método de **búsqueda** junto con la propiedad de **Índice** si el proveedor subyacente admite índices en el objeto de **conjunto de registros** . Use el método [Supports](../../../ado/reference/ado-api/supports-method.md)**(adSeek)** para determinar si el proveedor subyacente admite **Seek** y el método **Supports (adIndex)** para determinar si el proveedor admite índices. (Por ejemplo, el [proveedor de OLE DB para Microsoft Jet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-microsoft-jet.md) admite **Seek** e **index**).  
  
 Si **Seek** no encuentra la fila deseada, no se produce ningún error y la fila se coloca al final del conjunto de **registros**. Establezca la propiedad **index** en el índice deseado antes de ejecutar este método.  
  
 Este método solo se admite con los cursores del lado servidor. No se admite Seek cuando el valor de la propiedad [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) del objeto de **conjunto de registros** es **adUseClient**.  
  
 Este método solo se puede usar cuando se ha abierto el objeto de **conjunto de registros** con el valor **adCmdTableDirect** de [commandtypeenum](../../../ado/reference/ado-api/commandtypeenum.md) .  
  
## <a name="filtering-the-results"></a>Filtrar los resultados  
 El método **Find** limita la búsqueda al contenido de un campo. El método **Seek** requiere que tenga un índice y también tenga otras limitaciones. Si tiene que buscar en varios campos que no son la base de un índice o si el proveedor no admite índices, puede limitar los resultados mediante la propiedad **Filter** del objeto de **conjunto de registros** .  
  
 Utilice la propiedad **Filter** para filtrar de forma selectiva los registros de un objeto de **conjunto de registros** . El conjunto de **registros** filtrado se convierte en el cursor actual, lo que significa que los registros que no cumplen los criterios de **filtro** no están disponibles en el **conjunto de registros** hasta que se quita el **filtro** . Otras propiedades que devuelven valores basados en el cursor actual se ven afectadas, como **AbsolutePosition**, **AbsolutePage**, **RecordCount** y **PageCount**. Esto se debe a que si se establece la propiedad **Filter** en un valor concreto, se moverá el registro actual al primer registro que cumpla el nuevo valor.  
  
 La propiedad **Filter** toma un argumento Variant. Este valor representa uno de los tres métodos para usar la propiedad **Filter** : una cadena de criterios, una constante **FilterGroupEnum** o una matriz de marcadores. Para obtener más información, vea filtrar con una cadena de criterios, filtrar con una constante y filtrar con marcadores más adelante en este tema.  
  
> [!NOTE]
>  Cuando conozca los datos que desea seleccionar, normalmente es más eficaz abrir un **conjunto de registros** con una instrucción SQL que filtre el conjunto de resultados, en lugar de confiar en la propiedad **Filter** .  
  
 Para quitar un filtro de un **conjunto de registros**, use la constante **adFilterNone** . Establecer la propiedad **Filter** en una cadena de longitud cero ("") tiene el mismo efecto que el uso de la constante **adFilterNone** .  
  
### <a name="filtering-with-a-criteria-string"></a>Filtrar con una cadena de criterios  
 La cadena de criterios consta de cláusulas con el formato de *operador de valor* de tipo (por ejemplo, `"LastName = 'Smith'"` ). Puede crear cláusulas compuestas mediante la concatenación de cláusulas individuales con **y** (por ejemplo, `"LastName = 'Smith' AND FirstName = 'John'"` ) y **o** (por ejemplo, `"LastName = 'Smith' OR LastName = 'Jones'"` ). Utilice las siguientes directrices para las cadenas de criterios:  
  
-   *FieldName* debe ser un nombre de campo válido del **conjunto de registros**. Si el nombre del campo contiene espacios, debe escribir el nombre entre corchetes.  
  
-   El *operador* debe ser uno de los siguientes: **\<**, **>** , **\<=**, **>=** , **<>** , **=** o **like**.  
  
-   *Value* es el valor con el que se comparan los valores de campo (por ejemplo,, `'Smith'` `#8/24/95#` , `12.345` o `$50.00` ). Use comillas simples (') con las cadenas y los signos de almohadilla ( `#` ) con las fechas. En el caso de los números, puede usar separadores decimales, signos de dólar y notación científica. Si el *operador* es **like**, *Value* puede usar caracteres comodín. Solo el asterisco ( \* ) y el signo de porcentaje (%) se permiten caracteres comodín y deben ser el último carácter de la cadena. El *valor* no puede ser null.  
  
    > [!NOTE]
    >  Para incluir comillas simples (') en el *valor* de filtro, utilice dos comillas simples para representar una. Por ejemplo, para filtrar por *O'Malley*, la cadena de criterios debe ser `"col1 = 'O''Malley'"` . Para incluir comillas simples al principio y al final del valor de filtro, incluya la cadena entre signos de almohadilla (#). Por ejemplo, para filtrar por *' 1 '*, la cadena de criterios debe ser `"col1 = #'1'#"` .  
  
 No hay ninguna prioridad entre **and** y **or**. Las cláusulas se pueden agrupar entre paréntesis. Sin embargo, no puede agrupar las cláusulas Unidas por **o** y, a continuación, unir el grupo a otra cláusula con y, como se indica a continuación.  
  
```  
(LastName = 'Smith' OR LastName = 'Jones') AND FirstName = 'John'  
```  
  
 En su lugar, crearía este filtro como se indica a continuación.  
  
```  
(LastName = 'Smith' AND FirstName = 'John') OR (LastName = 'Jones' AND FirstName = 'John')  
```  
  
 En una cláusula **like** , puede usar un carácter comodín al principio y al final del patrón (por ejemplo, `LastName Like '*mit*'` ) o solo al final del patrón (por ejemplo, `LastName Like 'Smit*'` ).  
  
### <a name="filtering-with-a-constant"></a>Filtrar con una constante  
 Las constantes siguientes están disponibles para filtrar **conjuntos de registros**.  
  
|Constante|Descripción|  
|--------------|-----------------|  
|**adFilterAffectedRecords**|Filtros para ver solo los registros afectados por la llamada de última **eliminación**, **resincronización**, **UpdateBatch** o **CancelBatch** .|  
|**adFilterConflictingRecords**|Filtros para ver los registros que dieron error en la última actualización por lotes.|  
|**adFilterFetchedRecords**|Filtra para ver los registros en la caché actual; es decir, los resultados de la última llamada para recuperar los registros de la base de datos.|  
|**adFilterNone**|Quita el filtro actual y restaura todos los registros para su visualización.|  
|**adFilterPendingRecords**|Filtros para ver solo los registros que han cambiado pero que todavía no se han enviado al servidor. Aplicable solo para el modo de actualización por lotes.|  
  
 Las constantes de filtro facilitan la resolución de conflictos de registros individuales durante el modo de actualización por lotes, ya que permiten ver, por ejemplo, solo los registros que se vieron afectados durante la última llamada al método **UpdateBatch** , tal como se muestra en el ejemplo siguiente.  
  
 `Attribute VB_Name = "modExaminingData"`  
  
### <a name="filtering-with-bookmarks"></a>Filtrar con marcadores  
 Por último, puede pasar una matriz variante de marcadores a la propiedad **Filter** . El cursor resultante contendrá solo los registros cuyo marcador se pasó a la propiedad. En el ejemplo de código siguiente se crea una matriz de marcadores a partir de los registros de un **conjunto de registros** que tienen una "B" en el campo *ProductName* . A continuación, pasa la matriz a la propiedad **Filter** y muestra información sobre el conjunto de **registros** filtrado resultante.  
  
```  
'BeginFilterBkmk  
Dim vBkmkArray() As Variant  
Dim i As Integer  
  
'Recordset created using "SELECT * FROM Products" as command.  
'So, we will check to see if ProductName has a capital B, and  
'if so, add to the array.  
i = 0  
Do While Not objRs.EOF  
    If InStr(1, objRs("ProductName"), "B") Then  
        ReDim Preserve vBkmkArray(i)  
        vBkmkArray(i) = objRs.Bookmark  
        i = i + 1  
        Debug.Print objRs("ProductName")  
    End If  
    objRs.MoveNext  
Loop  
  
'Filter using the array of bookmarks.  
objRs.Filter = vBkmkArray  
  
objRs.MoveFirst  
Do While Not objRs.EOF  
    Debug.Print objRs("ProductName")  
    objRs.MoveNext  
Loop  
'EndFilterBkmk  
```  
  
## <a name="creating-a-clone-of-a-recordset"></a>Crear un clon de un conjunto de registros  
 Utilice el método **Clone** para crear varios objetos **Recordset** duplicados, especialmente si desea mantener más de un registro actual en un conjunto determinado de registros. Usar el método **Clone** es más eficaz que crear y abrir un nuevo objeto de **conjunto de registros** con la misma definición que el original.  
  
 El registro actual de un clon recién creado se establece originalmente en el primer registro. El puntero de registro actual de un **conjunto de registros** clonado no está sincronizado con el original ni viceversa. Puede navegar de forma independiente en cada **conjunto de registros**.  
  
 Los cambios que realice en un objeto de **conjunto de registros** serán visibles en todos sus clones, independientemente del tipo de cursor. Sin embargo, después de ejecutar [Requery](../../../ado/reference/ado-api/requery-method.md) en el **conjunto de registros** original, los clones ya no se sincronizarán con el original.  
  
 Al cerrar el **conjunto de registros** original no se cierran sus copias, ni al cerrar una copia se cierra el original o cualquiera de las demás copias.  
  
 Puede clonar un objeto de **conjunto de registros** solo si admite marcadores. Los valores de marcador son intercambiables. es decir, una referencia de marcador de un objeto de **conjunto de registros** hace referencia al mismo registro en cualquiera de sus clones.

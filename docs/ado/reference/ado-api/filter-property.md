---
description: Propiedad Filter
title: Propiedad Filter | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Filter
helpviewer_keywords:
- Filter property
ms.assetid: 80263a7a-5d21-45d1-84fc-34b7a9be4c22
author: rothja
ms.author: jroth
ms.openlocfilehash: 7366ba67d36049f1c5df701bc633d76f56bb9af5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167327"
---
# <a name="filter-property"></a>Propiedad Filter
Indica un filtro para los datos de un [conjunto de registros](./recordset-object-ado.md).  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos

Establece o devuelve un valor **Variant** , que puede contener uno de los siguientes elementos:  
  
-   **Cadena de criterios:** Cadena formada por una o varias cláusulas individuales concatenadas con operadores **and** u **or** .  
  
-   **Matriz de marcadores:** Matriz de valores de marcador únicos que apuntan a los registros del objeto de **conjunto de registros** .  
  
-   Un valor de [FilterGroupEnum](./filtergroupenum.md) .  
  
## <a name="remarks"></a>Observaciones

Utilice la propiedad **Filter** para filtrar de forma selectiva los registros de un objeto de **conjunto de registros** . El conjunto de **registros** filtrado se convierte en el cursor actual. Otras propiedades que devuelven valores basados en el **cursor** actual se ven afectadas, como la [propiedad AbsolutePosition (ADO)](./absoluteposition-property-ado.md), la [propiedad AbsolutePage (ADO)](./absolutepage-property-ado.md), la [propiedad RecordCount (ADO)](./recordcount-property-ado.md)y la [propiedad PageCount (ADO)](./pagecount-property-ado.md). Al establecer la propiedad **Filter** en un valor nuevo específico, se mueve el registro actual al primer registro que satisface el nuevo valor.
  
La cadena de criterios se compone de cláusulas con el formato *FieldName-Operator-Value* (por ejemplo, `"LastName = 'Smith'"` ). Puede crear cláusulas compuestas mediante la concatenación de cláusulas individuales con **y** (por ejemplo, `"LastName = 'Smith' AND FirstName = 'John'"` ) o **o** (por ejemplo, `"LastName = 'Smith' OR LastName = 'Jones'"` ). En el caso de las cadenas de criterios, use las siguientes directrices:

-   *FieldName* debe ser un nombre de campo válido del **conjunto de registros**. Si el nombre del campo contiene espacios, debe escribir el nombre entre corchetes.  
  
-   El operador debe ser uno de los siguientes: \<, > , \<=, > =,  <>, = o **like**.  
  
-   Valor es el valor con el que se comparan los valores de campo (por ejemplo, ' Smith ', #8/24/95 #, 12,345 o $50,00). Use comillas simples con cadenas y signos de almohadilla (#) con fechas. En el caso de los números, puede usar separadores decimales, signos de dólar y notación científica. Si el operador es **like**, Value puede usar caracteres comodín. Solo el asterisco (*) y el signo de porcentaje (%) se permiten caracteres comodín y deben ser el último carácter de la cadena. Value cannot be null.  
  
> [!NOTE]
>  Para incluir comillas simples (') en el valor de filtro, utilice dos comillas simples para representar una. Por ejemplo, para filtrar por O'Malley, la cadena de criterios debe ser `"col1 = 'O''Malley'"` . Para incluir comillas simples al principio y al final del valor de filtro, incluya la cadena entre signos de almohadilla (#). Por ejemplo, para filtrar por ' 1 ', la cadena de criterios debe ser `"col1 = #'1'#"` .  
  
-   No hay ninguna prioridad entre AND y OR. Las cláusulas se pueden agrupar entre paréntesis. Sin embargo, no puede agrupar las cláusulas Unidas por o y, a continuación, unir el grupo a otra cláusula con y, como en el siguiente fragmento de código:  
 `(LastName = 'Smith' OR LastName = 'Jones') AND FirstName = 'John'`  
  
-   En su lugar, crearía este filtro como  
 `(LastName = 'Smith' AND FirstName = 'John') OR (LastName = 'Jones' AND FirstName = 'John')`  
  
-   En una cláusula **like** , puede usar un carácter comodín al principio y al final del patrón. Por ejemplo, puede usar `LastName Like '*mit*'`. O con **like** , solo puede usar un carácter comodín al final del patrón. Por ejemplo, `LastName Like 'Smit*'`.  
  
 Las constantes de filtro facilitan la resolución de conflictos de registros individuales durante el modo de actualización por lotes, ya que permiten ver, por ejemplo, solo los registros que se vieron afectados durante la última llamada al método del [método UpdateBatch](./updatebatch-method.md) .  
  
Puede producirse un error en la configuración de la propiedad de **filtro** debido a un conflicto con los datos subyacentes. Por ejemplo, este error puede producirse cuando otro usuario ya ha eliminado un registro. En tal caso, el proveedor devuelve advertencias a la colección de [errores (ADO)](./errors-collection-ado.md) , pero no detiene la ejecución del programa. Un error en tiempo de ejecución solo se produce si hay conflictos en todos los registros solicitados. Utilice la propiedad [status (conjunto de registros ADO)](./status-property-ado-recordset.md) para buscar registros con conflictos.  
  
Establecer la propiedad **Filter** en una cadena de longitud cero ("") tiene el mismo efecto que el uso de la constante **adFilterNone** .
  
Siempre que se establece la propiedad **Filter** , la posición del registro actual se mueve al primer registro del subconjunto filtrado de registros del **conjunto** de registros. Del mismo modo, cuando se borra la propiedad **Filter** , la posición del registro actual se mueve al primer registro del **conjunto de registros**.

Supongamos que un **conjunto de registros** se filtra en función de un campo de un tipo Variant, como el tipo sql_variant. Se produce un error (DISP_E_TYPEMISMATCH o 80020005) cuando no coinciden los subtipos del campo y los valores de filtro utilizados en la cadena de criterios. Por ejemplo, supongamos que:

- Un objeto de **conjunto de registros** (RS) contiene una columna (C) del tipo sql_variant.
- Y se ha asignado a un campo de esta columna el valor 1 del tipo I4. La cadena de criterios se establece en `rs.Filter = "C='A'"` en el campo.

Esta configuración genera el error durante el tiempo de ejecución. Sin embargo, si `rs.Filter = "C=2"` se aplica en el mismo campo, no se producirá ningún error. Y se filtra el campo del conjunto de registros actual.

Vea la propiedad [Bookmark (propiedad de ADO)](./bookmark-property-ado.md) para obtener una explicación de los valores de marcador desde los que puede crear una matriz que se va a usar con la propiedad Filter.

Solo los filtros en forma de cadenas de criterios afectan al contenido de un **conjunto de registros** guardado. Un ejemplo de una cadena de criterios es `OrderDate > '12/31/1999'` . Los filtros creados con una matriz de marcadores o el uso de un valor de **FilterGroupEnum** no afectan al contenido del **conjunto de registros** guardado. Estas reglas se aplican a los conjuntos de registros creados con cursores del lado cliente o del lado servidor.
  
> [!NOTE]
>  Al aplicar la marca adFilterPendingRecords a un **conjunto de registros** filtrado y modificado en el modo de actualización por lotes, el **conjunto de registros** resultante está vacío si el filtrado se basó en el campo clave de una tabla con una sola clave y la modificación se realizó en los valores del campo de clave. El conjunto de **registros** resultante no estará vacío si se cumple alguna de las siguientes instrucciones:  
  
-   El filtrado se basó en campos que no son de clave en una tabla con una sola clave.  
  
-   El filtrado se basó en cualquier campo de una tabla con varias claves.  
  
-   Las modificaciones se realizaron en campos que no son de clave en una tabla con una sola clave.  
  
-   Se han realizado modificaciones en los campos de una tabla con varias claves.  
  
En la tabla siguiente se resumen los efectos de **adFilterPendingRecords** en diferentes combinaciones de filtrado y modificaciones. En la columna izquierda se muestran las posibles modificaciones. Las modificaciones se pueden realizar en cualquiera de los campos sin clave, en el campo clave de una tabla con una sola clave o en cualquiera de los campos clave de una tabla con varias claves. La fila superior muestra el criterio de filtrado. El filtrado puede basarse en cualquiera de los campos sin clave, en el campo clave de una tabla con una sola clave o en cualquiera de los campos clave de una tabla con varias claves. Las celdas de intersección muestran los resultados. Un **+** signo más significa que la aplicación de **adFilterPendingRecords** da como resultado un **conjunto de registros** no vacío. Un **-** signo menos significa un **conjunto de registros** vacío.  
  
|Posibles|No claves|Clave única|Varias claves|
|-|--------------|----------------|-------------------|
|**No claves**|+|+|+|
|**Clave única**|+|-|No aplicable|
|**Varias claves**|+|No aplicable|+|
|||||
  
## <a name="applies-to"></a>Se aplica a

[Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también

[Ejemplo de las propiedades Filter y RecordCount (VB)](./filter-and-recordcount-properties-example-vb.md) 
 [Ejemplo de las propiedades Filter y RecordCount (VC + +)](./filter-and-recordcount-properties-example-vc.md) 
 [Clear (método) (ADO)](./clear-method-ado.md) 
 [Optimizar Property-Dynamic (ADO)](./optimize-property-dynamic-ado.md)
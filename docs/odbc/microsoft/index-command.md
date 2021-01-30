---
description: Comando de índice
title: Comando de índice | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- index command [ODBC]
ms.assetid: 694e8cf5-2f69-4001-9c1e-b735a4da3aff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ac225132e2d89f1edd4baab012b2383cd508588
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183389"
---
# <a name="index-command"></a>Comando de índice
Crea un archivo de índice para mostrar y obtener acceso a los registros de la tabla en un orden lógico.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
INDEX ON eExpression TO IDXFileName | TAG TagName [OF CDXFileName]  
   [FOR lExpression]  
   [COMPACT]  
   [ASCENDING | DESCENDING]  
   [UNIQUE | CANDIDATE]  
   [ADDITIVE]  
```  
  
## <a name="arguments"></a>Argumentos  
 *eExpression*  
 Especifica una expresión de índice que puede incluir el nombre de un campo o campos de la tabla actual. Una clave de índice basada en la expresión de índice se crea en el archivo de índice para cada registro de la tabla. Visual FoxPro usa estas claves para mostrar y obtener acceso a los registros de la tabla.  
  
> [!NOTE]  
>  Aunque no se recomienda, *eExpression* también puede ser una variable de memoria, un elemento de matriz o una expresión de campo o campo de una tabla de otro área de trabajo. Los campos memo no se pueden usar solos en expresiones de archivo de índice; deben combinarse con otras expresiones de caracteres. Si obtiene acceso a un índice que contiene una variable o un campo que ya no existe o no se encuentra, Visual FoxPro genera un mensaje de error.  
  
 Si intenta crear un índice con una clave que varíe de longitud, la clave se rellenará con espacios. En Visual FoxPro no se admiten claves de índice de longitud variable.  
  
 Es posible crear una clave de índice con una longitud de cero. Por ejemplo, se crea una clave de índice de longitud cero cuando la expresión de índice es una subcadena de un campo Memo vacío. Una clave de índice de longitud cero genera un mensaje de error. Cuando Visual FoxPro crea un índice, evalúa los campos del primer registro de la tabla. Si un campo está vacío, puede que sea necesario escribir algunos datos temporales en el campo del primer registro para evitar una clave de índice de longitud 0.  
  
 A *IDXFileName*  
 Crea un archivo de índice. idx. Al archivo de índice se le asigna la extensión predeterminada. idx.  
  
 ETIQUETA *tagName*[de *CDXFileName*]  
 Crea un archivo de índice compuesto. Un archivo de índice compuesto es un archivo de índice único que consta de cualquier número de etiquetas independientes (entradas de índice). Cada etiqueta se identifica mediante su nombre de etiqueta único. Los nombres de etiqueta deben empezar por una letra o un carácter de subrayado y pueden constar de cualquier combinación de hasta 10 letras, dígitos o caracteres de subrayado. El número de etiquetas de un archivo de índice compuesto solo está limitado por la memoria disponible y el espacio en disco.  
  
 Los archivos de índice compuesto de varias entradas siempre son compactos. No es necesario incluir COMPACT al crear un archivo de índice compuesto. A los nombres de los archivos de índice compuesto se les asigna una extensión. CDX.  
  
 Se pueden crear dos tipos de archivos de índice compuesto: estructural y no estructural.  
  
 **Archivos de índice compuesto estructural** Puede crear un archivo de índice compuesto estructural con el *tagName* de etiqueta excluyendo la cláusula opcional of *CDXFileName* . Un archivo de índice compuesto estructural siempre tiene el mismo nombre base que la tabla y se abre automáticamente cuando se abre la tabla.  
  
 **Archivos de índice compuesto no estructurales** Puede crear un archivo de índice compuesto no estructural mediante la inclusión de *CDXFileName* después de la etiqueta *tagName*. A diferencia de un archivo de índice compuesto estructural, un archivo de índice compuesto no estructural debe abrirse explícitamente con la cláusula INDEX en uso.  
  
 Si ya se ha creado y abierto un archivo de índice compuesto, la emisión del índice con la etiqueta *tagName* agrega una etiqueta al archivo de índice compuesto.  
  
 PARA *lExpression*  
 Especifica una condición por la que solo se pueden mostrar y obtener acceso a los registros que cumplen con la expresión de filtro *lExpression* . las claves de índice se crean en el archivo de índice solo para aquellos registros que coincidan con la expresión de filtro.  
  
 La tecnología Rushmore de Visual FoxPro optimiza un índice... PARA el comando *lExpression* si *lExpression* es una expresión optimizable. Para obtener el mejor rendimiento, utilice una expresión optimizable en la cláusula FOR.  
  
 UNIDAD  
 Crea un archivo. idx compacto.  
  
 ORDEN ascendente  
 Especifica un orden ascendente para el archivo. CDX. De forma predeterminada, las etiquetas. CDX se crean en orden ascendente. (Puede incluir el orden ascendente como recordatorio del pedido del archivo de índice). Una tabla se puede indexar en orden inverso incluyendo descendente.  
  
 DESCENDENTE  
 Especifica un orden descendente para el archivo. CDX. No se puede incluir en orden descendente al crear archivos de índice. idx.  
  
 UNIQUE  
 Especifica que solo el primer registro encontrado con un valor de clave de índice determinado se incluye en un archivo. idx o en una etiqueta. CDX. UNIQUE se puede usar para evitar la visualización o el acceso a registros duplicados. Todos los registros agregados con claves de índice duplicadas se excluyen del archivo de índice. El uso de la opción UNIQUE de INDEX es idéntico a ejecutar SET UNIQUE en antes de emitir INDEX o REINDEX.  
  
 Cuando se activa una etiqueta de índice o índice único y se cambia un registro duplicado de manera que cambia su clave de índice, se actualiza la etiqueta de índice o índice. Sin embargo, no se puede tener acceso al siguiente registro duplicado con la clave de índice original ni mostrarse hasta que se vuelva a indexar el archivo mediante REINDEX.  
  
 BUEN  
 Crea una etiqueta de índice estructural candidato. La palabra clave CANDIDAte solo se puede incluir al crear una etiqueta de índice estructural. de lo contrario, Visual FoxPro genera un mensaje de error.  
  
 Una etiqueta de índice candidata evita valores duplicados en el campo o combinación de campos especificados en la expresión de índice *eExpression*. El término *Candidate* hace referencia al tipo de índice; Dado que los índices candidatos evitan valores duplicados, se califican como un "candidato" como índice principal.  
  
 Visual FoxPro genera un error si crea una etiqueta de índice candidata para un campo o combinación de campos que ya contiene valores duplicados.  
  
 SUMA  
 Mantiene abiertos los archivos de índice abiertos previamente. Si se omite la cláusula Additive al crear un archivo de índice o archivos para una tabla con el índice, se cerrarán todos los archivos de índice abiertos previamente (excepto el índice compuesto estructural).  
  
## <a name="remarks"></a>Observaciones  
 Los registros de una tabla que tiene un archivo de índice se muestran y se obtiene acceso a ellos en el orden especificado por la expresión de índice. El orden físico de los registros de la tabla no cambia por un archivo de índice.  
  
## <a name="index-types"></a>Tipos de índice  
 Visual FoxPro permite crear dos tipos de archivos de índice:  
  
-   Archivos de índice. CDX compuestos que contienen varias entradas de índice denominadas etiquetas  
  
-   archivos de índice. idx que contienen una entrada de índice  
  
 También puede crear un archivo de índice compuesto estructural, que se abre automáticamente con la tabla.  
  
> [!NOTE]  
>  Dado que los archivos de índice compuesto estructural se abren automáticamente al abrir la tabla, son el tipo de índice preferido.  
  
 Incluya COMPACT para crear archivos de índice Compact. idx. Los archivos de índice compuesto siempre son compactos.  
  
## <a name="index-order-and-updating"></a>Orden y actualización de índices  
 Solo un archivo de índice (el archivo de índice principal) o la etiqueta (la etiqueta maestra) controla el orden en el que se muestra o se tiene acceso a la tabla. Algunos comandos (SEEK, por ejemplo) usan el archivo de índice maestro o la etiqueta para buscar registros. Sin embargo, todos los archivos de índice. idx y. CDX abiertos se actualizan a medida que se realizan cambios en la tabla.  
  
## <a name="user-defined-functions"></a>Funciones definidas por el usuario  
 Aunque una expresión de índice puede contener una función definida por el usuario, no debe utilizar funciones definidas por el usuario en una expresión de índice. Las funciones definidas por el usuario en una expresión de índice aumentan el tiempo necesario para crear o actualizar el índice. Además, es posible que las actualizaciones de índices no se produzcan cuando se usa una función definida por el usuario para una expresión de índice.  
  
 Si usa una función definida por el usuario en una expresión de índice, Visual FoxPro debe ser capaz de encontrar la función definida por el usuario. Cuando Visual FoxPro crea un índice, la expresión de índice se guarda en el archivo de índice, pero solo se incluye una referencia a la función definida por el usuario en la expresión de índice.  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE-comando SQL](../../odbc/microsoft/alter-table-sql-command.md)   
 [Comando eliminar etiqueta](../../odbc/microsoft/delete-tag-command.md)   
 [SET COLLAte (comando)](../../odbc/microsoft/set-collate-command.md)   
 [CONJUNTO único comando](../../odbc/microsoft/set-unique-command.md)

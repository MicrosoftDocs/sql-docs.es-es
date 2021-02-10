---
description: Registros y campos proporcionados por el proveedor
title: Registros y Provider-Supplied campos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- records-provided fields [ADO]
- provider-supplied fields [ADO]
ms.assetid: 77f95e0a-0cf2-411a-a792-593f77330fbd
author: rothja
ms.author: jroth
ms.openlocfilehash: a64168a8a1fec81d47c337978ea75664fec0a10c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037115"
---
# <a name="records-and-provider-supplied-fields"></a>Registros y campos proporcionados por el proveedor
Cuando se abre un objeto de [registro](../../../ado/reference/ado-api/record-object-ado.md) , su origen puede ser la fila actual de un [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md)abierto, una dirección URL absoluta o una dirección URL relativa junto con un objeto de [conexión](../../../ado/reference/ado-api/connection-object-ado.md) abierto.  
  
 Si el **registro** se abre desde un **conjunto de registros**, la colección de [campos](../../../ado/reference/ado-api/fields-collection-ado.md) de objeto de **registro** contendrá todos los campos del **conjunto de registros**, además de los campos agregados por el proveedor subyacente.  
  
 El proveedor puede insertar campos adicionales que sirvan como características complementarias del **registro**. Como resultado, un **registro** puede tener campos únicos que no están en el **conjunto de registros** como un todo o cualquier **registro** derivado de otra fila del **conjunto de registros**.  
  
 Por ejemplo, todas las filas de un **conjunto de registros** derivado de un origen de datos de correo electrónico podrían tener columnas como from, to y Subject. Un **registro** derivado de ese **conjunto de registros** tendrá los mismos campos. Sin embargo, el **registro** también puede tener otros campos exclusivos del mensaje determinado representado por ese **registro**, como datos adjuntos y CC (copia carbón).  
  
 Aunque el objeto de **registro** y la fila actual del **conjunto de registros** tienen los mismos campos, son diferentes porque los objetos **Record** y **Recordset** tienen diferentes métodos y propiedades.  
  
 Un campo mantenido en común por el **registro** y el **conjunto de registros** se puede modificar en cualquier objeto. Sin embargo, el campo no se puede eliminar en el objeto de **registro** , aunque el proveedor subyacente puede admitir el establecimiento del campo en NULL.  
  
 Una vez abierto el **registro** , puede agregar campos mediante programación. También puede eliminar los campos que ha agregado, pero no puede eliminar los campos del conjunto de **registros** original.  
  
 También puede abrir el objeto de **registro** directamente desde una dirección URL. En este caso, los campos agregados al **registro** dependen del proveedor subyacente. Actualmente, la mayoría de los proveedores agregan un conjunto de campos que describen la entidad representada por el **registro**. Si la entidad está compuesta de un flujo de bytes, como un archivo simple, un objeto de [flujo](../../../ado/reference/ado-api/stream-object-ado.md) normalmente se puede abrir desde el **registro**.  
  
## <a name="special-fields-for-document-source-providers"></a>Campos especiales para proveedores de origen de documentos  
 Una clase especial de proveedores, denominada *proveedores de origen de documento*, administra carpetas y documentos. Cuando un objeto de **registro** representa un documento o un objeto de **conjunto de registros** representa una carpeta de documentos, el proveedor de origen de documento rellena esos objetos con un conjunto de campos único que describen las características del documento en lugar del propio documento. Normalmente, un campo contiene una referencia a la **secuencia** que representa el documento.  
  
 Estos campos constituyen un **registro** de recursos o un **conjunto de registros** y se enumeran para los proveedores específicos que los admiten en el [apéndice a: proveedores](../../../ado/guide/appendixes/appendix-a-providers.md).  
  
 Dos constantes indizan la colección de **campos** de un **registro** de recursos o un **conjunto de registros** para recuperar un par de campos de uso frecuente. La propiedad [valor](../../../ado/reference/ado-api/value-property-ado.md) del objeto de **campo** devuelve el contenido deseado.  
  
-   El campo al que se obtiene acceso con la constante **adDefaultStream** contiene una secuencia predeterminada asociada al **registro** o al objeto de **conjunto de registros** . El proveedor asigna un flujo predeterminado a un objeto.  
  
-   El campo al que se obtiene acceso con la constante **adRecordURL** contiene la dirección URL absoluta que identifica el documento.  
  
 Un proveedor de origen de documento no admite la colección de [propiedades](../../../ado/reference/ado-api/properties-collection-ado.md) de los objetos de **registro** y de **campo** . El contenido de la colección **Properties** es null para estos objetos.  
  
 Un proveedor de origen de documento puede Agregar una propiedad específica del proveedor, como el **tipo DataSource** para identificar si es un proveedor de origen de documento. Para obtener más información sobre cómo determinar el tipo de proveedor, consulte la documentación del proveedor.  
  
## <a name="resource-recordset-columns"></a>Columnas de conjunto de registros de recursos  
 Un *conjunto de registros de recursos* consta de las siguientes columnas.  
  
|Nombre de la columna|Tipo|Descripción|  
|-----------------|----------|-----------------|  
|RESOURCE_PARSENAME|AdVarWChar|Solo lectura. Indica la dirección URL del recurso.|  
|RESOURCE_PARENTNAME|AdVarWChar|Solo lectura. Indica la dirección URL absoluta del Registro primario.|  
|RESOURCE_ABSOLUTEPARSENAME|AdVarWChar|Solo lectura. Indica la dirección URL absoluta del recurso, que es la concatenación de PARENTNAME y PARSENAME.|  
|RESOURCE_ISHIDDEN|AdBoolean|True si el recurso está oculto. No se devolverá ninguna fila a menos que el comando que crea el conjunto de filas seleccione explícitamente las filas en las que RESOURCE_ISHIDDEN sea true.|  
|RESOURCE_ISREADONLY|AdBoolean|True si el recurso es de solo lectura. Intenta abrir este recurso con DBBINDFLAG_WRITE y producirá un error con DB_E_READONLY. Esta propiedad se puede editar incluso cuando el recurso solo se ha abierto para lectura.|  
|RESOURCE_CONTENTTYPE|AdVarWChar|Indica el uso probable del documento (por ejemplo, la breve de un abogado). Esto puede corresponder a la plantilla de Office que se usó para crear el documento.|  
|RESOURCE_CONTENTCLASS|AdVarWChar|Indica el tipo MIME del documento, que indica el formato, por ejemplo, " `text/html` ".|  
|RESOURCE_CONTENTLANGUAGE|AdVarWChar|Indica el idioma en el que se almacena el contenido.|  
|RESOURCE_CREATIONTIME|adFileTime|Solo lectura. Indica una estructura FILETIME que contiene la hora a la que se creó el recurso. La hora se registra en formato de hora universal coordinada (UTC).|  
|RESOURCE_LASTACCESSTIME|AdFileTime|Solo lectura. Indica una estructura FILETIME que contiene la hora a la que se produjo el último acceso al recurso. La hora está en formato UTC. Los miembros FILETIME son cero si el proveedor no admite este miembro de hora.|  
|RESOURCE_LASTWRITETIME|AdFileTime|Solo lectura. Indica una estructura FILETIME que contiene la hora en que se escribió por última vez en el recurso. La hora está en formato UTC. Los miembros FILETIME son cero si el proveedor no admite este miembro de hora.|  
|RESOURCE_STREAMSIZE|asUnsignedBigInt|Solo lectura. Indica el tamaño de la secuencia predeterminada del recurso, en bytes.|  
|RESOURCE_ISCOLLECTION|AdBoolean|Solo lectura. True si el recurso es una colección, como un directorio. False si el recurso es un archivo simple.|  
|RESOURCE_ISSTRUCTUREDDOCUMENT|AdBoolean|True si el recurso es un documento estructurado. False si el recurso no es un documento estructurado. Podría ser una colección o un archivo simple.|  
|DEFAULT_DOCUMENT|AdVarWChar|Solo lectura. Indica que este recurso contiene una dirección URL para el documento simple predeterminado de una carpeta o un documento estructurado. Se usa cuando se solicita la secuencia predeterminada de un recurso. Esta propiedad está en blanco para un archivo simple.|  
|CHAPTERED_CHILDREN|AdChapter|Solo lectura. Opcional. Indica el capítulo del conjunto de filas que contiene los elementos secundarios del recurso. (El *proveedor de OLE DB para la publicación en Internet* no usa esta columna).|  
|RESOURCE_DISPLAYNAME|AdVarWChar|Solo lectura. Indica el nombre para mostrar del recurso.|  
|RESOURCE_ISROOT|AdBoolean|Solo lectura. True si el recurso es la raíz de una colección o un documento estructurado.|  
  
## <a name="see-also"></a>Consulte también  
 [Objeto Record (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Apéndice A: Proveedores](../../../ado/guide/appendixes/appendix-a-providers.md)

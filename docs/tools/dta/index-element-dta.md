---
title: Index, elemento (DTA)
description: En la utilidad DTA, el elemento Index contiene información sobre un índice de una configuración especificada por el usuario que se quiere crear o quitar.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Index element (DTA)
ms.assetid: 447d3964-b387-40f6-9189-71386774c29e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: e6d7337e2215a95979f86cfc94cb7105587e6947
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340587"
---
# <a name="index-element-dta"></a>Index, elemento (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contiene información acerca de un índice de una configuración especificada por el usuario que se desea crear o quitar.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<Recommendation>  
  <Create>  
    <Index [Clustered | Unique | Online | IndexSizeInMB | NumberOfRows             | QUOTED_IDENTIFIER | ARITHABORT | CONCAT_NULL_YIELDS_NULL             | ANSI_NULLS | ANSI_PADDING | ANSI_WARNINGS  
            | NUMERIC_ROUNDABORT]  
     ...code removed here...  
    </Index>  
```  
  
## <a name="element-attributes"></a>Atributos del elemento  
  
|Atributo Index|Tipo de datos|Descripción|  
|---------------------|---------------|-----------------|  
|**Agrupado**|**boolean**|Opcional. Especifica un índice clúster. Se establece en "true" o "false", por ejemplo:<br /><br /> `<Index Clustered="true">`<br /><br /> De forma predeterminada, este atributo está establecido en "false".|  
|**Único**|**boolean**|Opcional. Especifica un índice único. Se establece en "true" o "false", por ejemplo:<br /><br /> `<Index Unique="true">`<br /><br /> De forma predeterminada, este atributo está establecido en "false".|  
|**En línea**|**boolean**|Opcional. Especifica un índice que puede realizar operaciones mientras el servidor está en línea, lo que exige espacio temporal en disco. Se establece en "true" o "false", por ejemplo:<br /><br /> `<Index Online="true">`<br /><br /> De forma predeterminada, este atributo está establecido en "false".<br /><br /> Para más información, consulte [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).|  
|**IndexSizeInMB**|**double**|Opcional. Especifica el tamaño máximo del índice en megabytes, por ejemplo:<br /><br /> `<Index IndexSizeInMB="873.75">`<br /><br /> Sin valor predeterminado.|  
|**NumberOfRows**|**integer**|Opcional. Simula diferentes tamaños de índice, lo que simula de forma eficaz diferentes tamaños de tabla, por ejemplo:<br /><br /> `<Index NumberOfRows="3000">`<br /><br /> Sin valor predeterminado.|  
|**QUOTED_IDENTIFIER**|**boolean**|Opcional. Hace que [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siga las reglas de ISO en cuanto a las comillas delimitadoras de identificadores y cadenas literales. Si el índice se encuentra en una columna o una vista calculada, este atributo debe estar activado. Por ejemplo, la sintaxis siguiente activa este atributo:<br /><br /> `<Index QUOTED_IDENTIFIER [...]>`<br /><br /> De forma predeterminada, este atributo está desactivado.<br /><br /> Para obtener más información, vea [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).|  
|**ARITHABORT**|**boolean**|Opcional. Cancela una consulta cuando se produce un error de desbordamiento o división por cero durante su ejecución. Si el índice se encuentra en una columna o una vista calculada, este atributo debe estar activado. Por ejemplo, la sintaxis siguiente activa este atributo:<br /><br /> `<Index ARITHABORT [...]>`<br /><br /> De forma predeterminada, este atributo está desactivado.<br /><br /> Para obtener más información, vea [SET ARITHABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-arithabort-transact-sql.md).|  
|**CONCAT_NULL_YIELDS_**<br /><br /> **NULL**|**boolean**|Opcional. Determina si los resultados de la concatenación se tratan como valor NULL o como cadena vacía. Si el índice se encuentra en una columna o una vista calculada, este atributo debe estar activado. Por ejemplo, la sintaxis siguiente activa este atributo:<br /><br /> `<Index CONCAT_NULL_YIELDS_NULL [...]>`<br /><br /> De forma predeterminada, este atributo está desactivado.<br /><br /> Para obtener más información, vea [SET CONCAT_NULL_YIELDS_NULL &#40;Transact-SQL&#41;](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).|  
|**ANSI_NULLS**|**boolean**|Opcional. Especifica el comportamiento conforme a ISO de los operadores de comparación Es igual a (=) y No es igual a (<>) cuando se utilizan con valores NULL. Si el índice se encuentra en una columna o una vista calculada, este atributo debe estar activado. Por ejemplo, la sintaxis siguiente activa este atributo:<br /><br /> `<Index ANSI_NULLS [...]>`<br /><br /> De forma predeterminada, este atributo está desactivado.<br /><br /> Para obtener más información, vea [SET ANSI_NULLS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-nulls-transact-sql.md).|  
|**ANSI_PADDING**|**boolean**|Opcional. Controla el modo en que una columna almacena valores inferiores a su tamaño definido. Si el índice se encuentra en una columna o una vista calculada, este atributo debe estar activado. Por ejemplo, la sintaxis siguiente activa este atributo:<br /><br /> `<Index ANSI_PADDING [...]>`<br /><br /> De forma predeterminada, este atributo está desactivado.<br /><br /> Para obtener más información, vea [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md).|  
|**ANSI_WARNINGS**|**boolean**|Opcional. Especifica el comportamiento estándar de ISO para diversas condiciones de error. Si el índice se encuentra en una columna o una vista calculada, este atributo debe estar activado. Por ejemplo, la sintaxis siguiente activa este atributo:<br /><br /> `<Index ANSI_WARNING [...]>`<br /><br /> De forma predeterminada, este atributo está desactivado.<br /><br /> Para obtener más información, vea [SET ANSI_WARNINGS &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-warnings-transact-sql.md).|  
|**NUMERIC_ROUNDABORT**|**boolean**|Opcional. Especifica el nivel de informes de error generados cuando el redondeo en una expresión produce pérdida de precisión. Si el índice se encuentra en una columna o una vista calculada, este atributo debe estar desactivado.<br /><br /> La sintaxis siguiente activa este atributo:<br /><br /> `<Index ANSI_WARNING [...]>`<br /><br /> De forma predeterminada, este atributo está desactivado.<br /><br /> Para obtener más información, vea [SET NUMERIC_ROUNDABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-numeric-roundabort-transact-sql.md).|  
  
## <a name="element-characteristics"></a>Características de los elementos  
  
|Característica|Descripción|  
|--------------------|-----------------|  
|**Tipo y longitud de los datos**|Ninguno.|  
|**Valor predeterminado**|Ninguno.|  
|**Repetición**|Una obligatoria por cada elemento **Create** o **Drop** si no se especifica otra estructura de diseño físico con el elemento **Statistics** o **Heap** .|  
  
## <a name="element-relationships"></a>Relaciones del elemento  
  
|Relación|Elementos|  
|------------------|--------------|  
|**Elemento primario**|[Create &#40;DTA, elemento&#41;](../../tools/dta/create-element-dta.md)<br /><br /> Elemento **Drop** . Para obtener más información, vea el esquema XML del Asistente para la optimización de motor de base de datos.|  
|**Elementos secundarios**|[Name &#40;DTA, elemento de Index&#41;](../../tools/dta/name-element-for-index-dta.md)<br /><br /> [Column &#40;DTA, elemento de Index&#41;](../../tools/dta/column-element-for-index-dta.md)<br /><br /> Elemento **PartitionScheme** . Para obtener más información, vea el esquema XML del Asistente para la optimización de motor de base de datos.<br /><br /> Elemento **PartitionColumn** . Para obtener más información, vea el esquema XML del Asistente para la optimización de motor de base de datos.<br /><br /> [Filegroup &#40;DTA. elemento de Index&#41;](../../tools/dta/filegroup-element-for-index-dta.md)<br /><br /> Elemento **NumberOfReferences** . Para obtener más información, vea el esquema XML del Asistente para la optimización de motor de base de datos.<br /><br /> Elemento **PercentUsage** . Para obtener más información, vea el esquema XML del Asistente para la optimización de motor de base de datos.|  
  
## <a name="example"></a>Ejemplo  
 Para obtener un ejemplo de uso de este elemento, vea [Ejemplo de archivo de entrada XML con configuración especificada por el usuario &#40;DTA&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md).  
  
## <a name="see-also"></a>Consulte también  
 [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  

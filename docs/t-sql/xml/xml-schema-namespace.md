---
description: xml_schema_namespace
title: xml_schema_namespace (Transact-SQL)
ms.custom: ''
ms.date: 07/27/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- xml_schema_namespace_TSQL
- xml_schema_namespace
dev_langs:
- TSQL
helpviewer_keywords:
- XML schema collections [SQL Server], reconstructing schemas
- xml_schema_namespace function
- reconstructing schemas
- schemas [SQL Server], XML
- schema collections [SQL Server], reconstructing schemas
ms.assetid: ee9873d8-dd3a-4bff-a10c-68bbadbdf1a6
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 865f2548293fc572a56c90b0813bb83aa2776cbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181697"
---
# <a name="xml_schema_namespace"></a>xml_schema_namespace
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Reconstruye todos los esquemas o un esquema determinado en la colección de esquemas XML especificada. La función devuelve una instancia de tipo de datos **xml** .  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
xml_schema_namespace( Relational_schema , XML_schema_collection_name , [ Namespace ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *Relational_schema*  
 Es el nombre del esquema relacional. *Relational_schema* es **sysname**.  
  
 *XML_schema_collection_name*  
 Es el nombre de la colección de esquemas XML que se va a reconstruir. *XML_schema_collection_name* es **sysname**.  
  
 *Espacio de nombres*  
 Es el URI de espacio de nombres del esquema XML que desea reconstruir. Tiene un límite de 1.000 caracteres. Si no se proporciona ningún URI de espacio de nombres, se reconstruye toda la colección de esquemas XML. *Namespace* es **nvarchar(4000)** .  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **xml**  
  
## <a name="remarks"></a>Observaciones  
 Al importar componentes de esquema XML en la base de datos mediante [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md) o [ALTER XML SCHEMA COLLECTION](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md), se mantienen los aspectos del esquema usados para la validación. Por lo tanto, el esquema reconstruido puede no ser léxicamente el mismo que el documento del esquema original. De forma específica, se pierden comentarios, espacios en blanco y anotaciones; asimismo, la información implícita se hace explícita. Por ejemplo, \<xs:element name="e1" /> se convierte en \<xs:element name="e1" type="xs:anyType"/>. Los prefijos de los espacios de nombres no se mantienen.  
  
 Si especifica un parámetro de espacio de nombres, el documento del esquema resultante contendrá definiciones para todos los componentes del esquema en ese espacio de nombres, incluso si se han agregado en diferentes documentos de esquema o pasos de DDL, o ambos.  
  
 No puede usar esta función para construir documentos de esquema XML desde la colección de esquemas XML **sys.sys**.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente recupera la colección de esquemas XML `ProductDescriptionSchemaCollection` desde el esquema relacional de producción en la base de datos `AdventureWorks`.  
  
```sql
USE AdventureWorks;  
GO  
SELECT xml_schema_namespace(N'production',N'ProductDescriptionSchemaCollection');  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Ver una colección de esquemas XML almacenada](../../relational-databases/xml/view-a-stored-xml-schema-collection.md)   
 [Colecciones de esquemas XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  

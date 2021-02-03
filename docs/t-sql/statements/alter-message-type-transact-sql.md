---
description: ALTER MESSAGE TYPE (Transact-SQL)
title: ALTER MESSAGE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_MESSAGE_TYPE_TSQL
- ALTER MESSAGE TYPE
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER MESSAGE TYPE statement
- modifying message types
- message types [Service Broker], modifying
ms.assetid: 98c94176-2bdf-4725-b4bc-d33b6b14817d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 400ba882af76481c272a240cabedadc6a03f4202
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204154"
---
# <a name="alter-message-type-transact-sql"></a>ALTER MESSAGE TYPE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cambia las propiedades de un tipo de mensaje.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql 
ALTER MESSAGE TYPE message_type_name  
   VALIDATION =  
    {  NONE   
     | EMPTY   
     | WELL_FORMED_XML   
     | VALID_XML WITH SCHEMA COLLECTION schema_collection_name }  
[ ; ]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *message_type_name*  
 Es el nombre del tipo de mensaje que se va a cambiar. No se pueden especificar nombres de servidor, base de datos o esquema.  
  
 VALIDATION   
 Especifica cómo [!INCLUDE[ssSB](../../includes/sssb-md.md)] valida el cuerpo del mensaje para mensajes de este tipo.  
  
 Ninguno  
 No se realiza ninguna validación. El cuerpo del mensaje puede contener cualquier dato o tener un valor NULL.  
  
 EMPTY  
 El cuerpo del mensaje debe tener un valor NULL.  
  
 WELL_FORMED_XML   
 El cuerpo del mensaje debe contener XML correcto.  
  
 VALID_XML_WITH_SCHEMA = *schema_collection_name*  
 El cuerpo del mensaje debe contener XML que cumpla con el esquema de la colección de esquemas especificada. *schema_collection_name* debe ser el nombre de una colección de esquemas XML existente.  
  
## <a name="remarks"></a>Observaciones  
 El cambio de la validación de un tipo de mensaje no afecta a los mensajes que ya se han entregado a una cola.  
  
 Para modificar AUTHORIZATION para un tipo de mensaje, utilice la instrucción ALTER AUTHORIZATION.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, se concede permiso para modificar un tipo de mensaje al propietario del tipo de mensaje, a los miembros de los roles fijos de base de datos **db_ddladmin** o **db_owner**, y a los miembros del rol fijo de servidor **sysadmin**.  
  
 Si la instrucción ALTER MESSAGE TYPE especifica una colección de esquemas, el usuario que ejecuta la instrucción debe tener el permiso REFERENCES en la colección de esquemas especificada.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se cambia el tipo de mensaje `//Adventure-Works.com/Expenses/SubmitExpense` para que requiera que el cuerpo del mensaje contenga un documento XML correcto.  
  
```sql  
ALTER MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = WELL_FORMED_XML ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [DROP MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-message-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

---
description: CREATE MESSAGE TYPE (Transact-SQL)
title: CREATE MESSAGE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_MESSAGE_TSQL
- MESSAGE_TSQL
- MESSAGE
- CREATE MESSAGE
- CREATE_MESSAGE_TYPE_TSQL
- MESSAGE TYPE
- MESSAGE_TYPE_TSQL
- CREATE MESSAGE TYPE
dev_langs:
- TSQL
helpviewer_keywords:
- XML [Service Broker]
- validation [Service Broker]
- message types [Service Broker], creating
- empty messages [SQL Server]
- binary [SQL Server], message types
- CREATE MESSAGE TYPE statement
ms.assetid: 98fe0fff-1a2e-4ca2-b37f-83a06fdf098e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 476d527279c83d7b67babeb6f405c320a7299d6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188581"
---
# <a name="create-message-type-transact-sql"></a>CREATE MESSAGE TYPE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un tipo de mensaje nuevo. El tipo de mensaje define el nombre de un mensaje y la validación que [!INCLUDE[ssSB](../../includes/sssb-md.md)] realiza para los mensajes que tienen dicho nombre. Las dos partes de una conversación deben definir los mismos tipos de mensajes.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE MESSAGE TYPE message_type_name  
    [ AUTHORIZATION owner_name ]  
    [ VALIDATION = {  NONE  
                    | EMPTY   
                    | WELL_FORMED_XML  
                    | VALID_XML WITH SCHEMA COLLECTION schema_collection_name  
                   } ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *message_type_name*  
 Es el nombre del tipo de mensaje que se va a crear. Se crea un tipo de mensaje nuevo en la base de datos actual, el cual pertenece a la entidad de seguridad especificada en la cláusula AUTHORIZATION. No se pueden especificar nombres de servidor, base de datos o esquema. *message_type_name* puede tener hasta 128 caracteres.  
  
 AUTHORIZATION *owner_name*  
 Establece el propietario del tipo de mensaje en el usuario o el rol de base de datos que se ha especificado. Cuando el usuario actual es **dbo** o **sa**, *owner_name* puede ser el nombre de cualquier usuario o rol válidos. En caso contrario, *owner_name* debe ser el nombre del usuario actual, el nombre de un usuario para el que el usuario actual tiene permiso IMPERSONATE o el nombre de un rol al que pertenece el usuario actual. Si se omite esta cláusula, el tipo de mensaje pertenece al usuario actual.  
  
 VALIDATION   
 Especifica cómo [!INCLUDE[ssSB](../../includes/sssb-md.md)] valida el cuerpo del mensaje para mensajes de este tipo. Si no se especifica esta cláusula, el valor predeterminado de la validación es NONE.  
  
 Ninguno  
 Especifica que no se realiza ninguna validación. El cuerpo del mensaje puede contener datos o tener un valor NULL.  
  
 EMPTY  
 Especifica que el cuerpo del mensaje debe ser NULL.  
  
 WELL_FORMED_XML   
 Especifica que el cuerpo del mensaje debe contener XML correcto.  
  
 VALID_XML WITH SCHEMA COLLECTION *schema_collection_name*  
 Especifica que el cuerpo del mensaje debe contener XML que se ajuste a un esquema de la colección de esquemas especificada. *schema_collection_name* debe ser el nombre de una colección de esquemas XML existente.  
  
## <a name="remarks"></a>Observaciones  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] valida los mensajes entrantes. Si un mensaje contiene un cuerpo de mensaje que no se ajusta al tipo de validación especificado, [!INCLUDE[ssSB](../../includes/sssb-md.md)] descarta el mensaje no válido y devuelve un mensaje de error al servicio que ha enviado el mensaje.  
  
 Las dos partes de una conversación deben definir el mismo nombre para un tipo de mensaje. Para facilitar la solución de problemas, las dos partes de una conversación suelen especificar la misma validación para el tipo de mensaje, aunque [!INCLUDE[ssSB](../../includes/sssb-md.md)] no necesita que ambas partes de la conversación utilicen la misma validación.  
  
 Un tipo de mensaje no puede ser un objeto temporal. Se permiten los nombres de tipo de mensaje que empiezan por **#** , aunque se trata de objetos permanentes.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, tienen permiso para crear un tipo de mensaje los miembros de los roles fijos de base de datos **db_ddladmin** o **db_owner** y el rol fijo de servidor **sysadmin**.  
  
 El permiso REFERENCES para un tipo de mensaje se concede de forma predeterminada al propietario del tipo de mensaje, a los miembros del rol fijo de base de datos **db_owner** y a los miembros del rol fijo de servidor **sysadmin**.  
  
 Si la instrucción CREATE MESSAGE TYPE especifica una colección de esquemas, el usuario que ejecuta la instrucción debe tener el permiso REFERENCES en la colección de esquemas especificada.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-message-type-containing-well-formed-xml"></a>A. Crear un tipo de mensaje que contenga XML correcto  
 En el siguiente ejemplo se crea un tipo de mensaje que contiene XML correcto.  
  
```sql  
CREATE MESSAGE TYPE  
  [//Adventure-Works.com/Expenses/SubmitExpense]  
  VALIDATION = WELL_FORMED_XML ;     
```  
  
### <a name="b-creating-a-message-type-containing-typed-xml"></a>B. Crear un tipo de mensaje que contenga XML con tipo  
 En el siguiente ejemplo se crea un tipo de mensaje para un informe de gastos codificado en XML. En el ejemplo se crea una colección de esquemas XML que contiene el esquema de un informe de gastos sencillo. En el ejemplo se crea a continuación un tipo de mensaje nuevo que valida los mensajes en relación con el esquema.  
  
```sql  
CREATE XML SCHEMA COLLECTION ExpenseReportSchema AS  
N'<?xml version="1.0" encoding="UTF-16" ?>  
  <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
     targetNamespace="https://Adventure-Works.com/schemas/expenseReport"  
     xmlns:expense="https://Adventure-Works.com/schemas/expenseReport"  
     elementFormDefault="qualified"  
   >   
    <xsd:complexType name="expenseReportType">  
       <xsd:sequence>  
         <xsd:element name="EmployeeName" type="xsd:string"/>  
         <xsd:element name="EmployeeID" type="xsd:string"/>  
         <xsd:element name="ItemDetail"  
           type="expense:ItemDetailType" maxOccurs="unbounded"/>  
      </xsd:sequence>  
    </xsd:complexType>  
  
    <xsd:complexType name="ItemDetailType">  
      <xsd:sequence>  
        <xsd:element name="Date" type="xsd:date"/>  
        <xsd:element name="CostCenter" type="xsd:string"/>  
        <xsd:element name="Total" type="xsd:decimal"/>  
        <xsd:element name="Currency" type="xsd:string"/>  
        <xsd:element name="Description" type="xsd:string"/>  
      </xsd:sequence>  
    </xsd:complexType>  
  
    <xsd:element name="ExpenseReport" type="expense:expenseReportType"/>  
  
  </xsd:schema>' ;  
  
  CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = VALID_XML WITH SCHEMA COLLECTION ExpenseReportSchema ;  
```  
  
### <a name="c-creating-a-message-type-for-an-empty-message"></a>C. Crear un tipo de mensaje para un mensaje vacío  
 En el siguiente ejemplo se crea un tipo de mensaje con codificación vacía.  
  
```sql  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = EMPTY ;  
```  
  
### <a name="d-creating-a-message-type-containing-binary-data"></a>D. Crear un tipo de mensaje que contenga datos binarios  
 En el siguiente ejemplo se crea un tipo de mensaje para incluir datos binarios. Dado que el mensaje contiene datos que no son XML, el tipo de mensaje especifica el tipo de validación `NONE`. En este caso, debe tener en cuenta que la aplicación que recibe un mensaje de este tipo debe comprobar que el mensaje contiene datos y que dichos datos son del tipo previsto.  
  
```sql  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/ReceiptImage]  
    VALIDATION = NONE ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-message-type-transact-sql.md)   
 [DROP MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-message-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

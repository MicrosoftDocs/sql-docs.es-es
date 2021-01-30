---
description: sp_syscollector_update_collector_type (Transact-SQL)
title: sp_syscollector_update_collector_type (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_update_collector_type_TSQL
- sp_syscollector_update_collector_type
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_update_collector_type
- data collector [SQL Server], stored procedures
ms.assetid: 3c414dfd-d9ca-4320-81aa-949465b967bf
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ad2fefb206f417c4759f3e85d34a63f320e34d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201871"
---
# <a name="sp_syscollector_update_collector_type-transact-sql"></a>sp_syscollector_update_collector_type (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza un tipo de recopilador para un elemento de recopilación. Dado el nombre y GUID de un tipo de recopilador, actualiza la configuración del tipo de recopilador, incluida la colección y el paquete de carga, el esquema de parámetros y el esquema del formateador de parámetros.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_update_collector_type [ @collector_type_uid = ] 'collector_type_uid' OUTPUT  
          , [ @name = ] 'name'  
          , [ @parameter_schema = ] 'parameter_schema'  
          , [ @collection_package_id = ] collection_package_id  
          , [ @upload_package_id = ] upload_package_id  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @collector_type_uid = ] 'collector_type_uid'` Es el GUID del tipo de recopilador. *collector_type_uid* es de tipo **uniqueidentifier** y si es null, se creará automáticamente y se devolverá como salida.  
  
`[ @name = ] 'name'` Es el nombre del tipo de recopilador. *Name* es **sysname** y debe especificarse.  
  
`[ @parameter_schema = ] 'parameter_schema'` Es el esquema XML para este tipo de recopilador. *parameter_schema* es **XML** y puede ser necesario para determinados tipos de recopilador. Si no es necesario, este argumento puede ser NULL.  
  
`[ @collection_package_id = ] collection_package_id` Es un identificador local único que apunta al paquete de [!INCLUDE[ssIS](../../includes/ssis-md.md)] colección utilizado por el conjunto de recopilación. *collection_package_id* es **uniqueidentifier** y es obligatorio. Para obtener el valor de *collection_package_id*, consulte la vista del sistema collector_collector_types dbo.sysen la base de datos msdb.  
  
`[ @upload_package_id = ] upload_package_id` Es un identificador local único que apunta al paquete de [!INCLUDE[ssIS](../../includes/ssis-md.md)] carga usado por el conjunto de recopilación. *upload_package_id* es **uniqueidentifier** y es obligatorio. Para obtener el valor de *upload_package_id*, consulte la vista del sistema collector_collector_types dbo.sysen la base de datos msdb.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **dc_admin** (con permiso Execute).  
  
## <a name="example"></a>Ejemplo  
 Este ejemplo actualiza el tipo de recopilador Consultas T-SQL genérico. (En el ejemplo se usa el esquema predeterminado para el tipo de recopilador Consultas T-SQL genérico).  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_update_collector_type  
@collector_type_uid = '302E93D1-3424-4BE7-AA8E-84813ECF2419',  
@name = 'Generic T-SQL Query Collector Type',  
@parameter_schema = '<?xml version="1.0" encoding="utf-8"?>  
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" targetNamespace="DataCollectorType">  
  <xs:element name="TSQLQueryCollector">  
<xs:complexType>  
  <xs:sequence>  
<xs:element name="Query" minOccurs="1" maxOccurs="unbounded">  
  <xs:complexType>  
<xs:sequence>  
  <xs:element name="Value" type="xs:string" />  
  <xs:element name="OutputTable" type="xs:string" />  
</xs:sequence>  
  </xs:complexType>  
</xs:element>  
<xs:element name="Databases" minOccurs="0" maxOccurs="1">  
  <xs:complexType>  
<xs:sequence>  
  <xs:element name="Database" minOccurs="0" maxOccurs="unbounded" type="xs:string" />  
</xs:sequence>  
<xs:attribute name="UseSystemDatabases" type="xs:boolean" use="optional" />  
<xs:attribute name="UseUserDatabases" type="xs:boolean" use="optional" />  
  </xs:complexType>  
</xs:element>  
  </xs:sequence>  
</xs:complexType>  
  </xs:element>  
</xs:schema>',  
@collection_package_id = '292B1476-0F46-4490-A9B7-6DB724DE3C0B',  
@upload_package_id = '6EB73801-39CF-489C-B682-497350C939F0';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)  
  
  

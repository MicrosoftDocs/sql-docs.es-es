---
description: sp_syscollector_create_collector_type (Transact-SQL)
title: sp_syscollector_create_collector_type (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_create_collector_type
- sp_syscollector_create_collector_type_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_create_collector_type
- data collector [SQL Server], stored procedures
ms.assetid: 568e9119-b9b0-4284-9cef-3878c691de5f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5431d45716aa0c1c685f303c3ee4b5d3cec67a13
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200518"
---
# <a name="sp_syscollector_create_collector_type-transact-sql"></a>sp_syscollector_create_collector_type (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un tipo de recopilador para el recopilador de datos. Un tipo de recopilador es un contenedor lógico alrededor de los [!INCLUDE[ssIS](../../includes/ssis-md.md)] paquetes que proporcionan el mecanismo real para recopilar datos y cargarlos en el almacén de administración de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_create_collector_type   
    [ [@collector_type_uid = ] 'collector_type_uid' OUTPUT ]  
    , [ @name = ] 'name'  
    , [ [ @parameter_schema = ] 'parameter_schema' ]  
    , [ [ @parameter_formatter = ] 'parameter_formatter' ]  
    , [ @collection_package_id = ] 'collection_package_id'  
    , [ @upload_package_id = ] 'upload_package_id'  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @collector_type_uid =] '*collector_type_uid*'  
 Es el GUID para el tipo de recopilador. *collector_type_uid* es de tipo **uniqueidentifier** y si es null, se creará automáticamente y se devolverá como salida.  
  
 [ @name =] '*nombre*'  
 Es el nombre del tipo de recopilador. *Name* es **sysname** y debe especificarse.  
  
 [ @parameter_schema =] '*parameter_schema*'  
 Es el esquema XML para este tipo de recopilador. *parameter_schema* es **XML** con un valor predeterminado de NULL.  
  
 [ @parameter_formatter =] '*parameter_formatter*'  
 Es la plantilla que debe usarse para transformar el XML a fin de usarlo en la página de propiedades del conjunto de recopilación. *parameter_formatter* es **XML** con un valor predeterminado de NULL.  
  
 [ @collection_package_id =] *collection_package_id*  
 Es un identificador local único que apunta al paquete de recopilaciones de [!INCLUDE[ssIS](../../includes/ssis-md.md)] usado por el conjunto de recopilaciones. *collection_package_id* es **uniqueidentifier** y es obligatorio.  
  
 [ @upload_package_id =] *upload_package_id*  
 Es un identificador local único que apunta al paquete de carga de [!INCLUDE[ssIS](../../includes/ssis-md.md)] usado por el conjunto de recopilaciones. *upload_package_id* es **uniqueidentifier** y es obligatorio.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de base de datos dc_admin (con permiso EXECUTE) para ejecutar este procedimiento.  
  
## <a name="example"></a>Ejemplo  
 Así se crea el tipo de recopilador de consultas T-SQL genérico.  
  
```  
EXEC sp_syscollector_create_collector_type  
@collector_type_uid = '302E93D1-3424-4be7-AA8E-84813ECF2419',  
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
  
  

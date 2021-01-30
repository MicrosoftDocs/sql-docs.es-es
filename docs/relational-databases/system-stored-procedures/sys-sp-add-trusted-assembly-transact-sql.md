---
description: sys.sp_add_trusted_assembly (Transact-SQL)
title: sys.sp_add_trusted_assembly (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/14/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_trusted_assembly_TSQL
- sp_add_trusted_assembly
- sys.sp_add_trusted_assembly_TSQL
- sys.sp_add_trusted_assembly
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_add_trusted_assembly
ms.assetid: ''
author: VanMSFT
ms.author: vanto
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3ae54693fcefff29423092a08feafd12587d4c1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190535"
---
# <a name="syssp_add_trusted_assembly-transact-sql"></a>sys.sp_add_trusted_assembly (Transact-SQL)  
[!INCLUDE[tsql-appliesto-ss2017-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2017-asdbmi-xxxx-xxx-md.md)]

Agrega un ensamblado a la lista de ensamblados de confianza para el servidor.

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  


## <a name="syntax"></a>Sintaxis
```  
sp_add_trusted_assembly 
    [ @hash = ] 'value'
    [ , [ @description = ] 'description' ]
```  

## <a name="remarks"></a>Observaciones  

Este procedimiento agrega un ensamblado a  [Sys.trusted_assemblies](../../relational-databases/system-catalog-views/sys-trusted-assemblies-transact-sql.md).

## <a name="arguments"></a>Argumentos

[ @hash =] '*valor*'  
Valor hash SHA2_512 del ensamblado que se va a agregar a la lista de ensamblados de confianza para el servidor. Los ensamblados de confianza pueden cargarse cuando se habilita [CLR STRICT Security](../../database-engine/configure-windows/clr-strict-security.md) , incluso si el ensamblado está sin firmar o si la base de datos no está marcada como de confianza.

[ @description =] '*Descripción*'  
Descripción opcional definida por el usuario del ensamblado. Microsoft recomienda usar el nombre canónico que codifica el nombre simple, el número de versión, la referencia cultural, la clave pública y la arquitectura del ensamblado para confiar. Este valor identifica de forma única el ensamblado en el lado Common Language Runtime (CLR) y es el mismo que el valor clr_name de sys. Assemblies. 

## <a name="permissions"></a>Permisos

Requiere la pertenencia al `sysadmin` rol fijo de servidor o al `CONTROL SERVER` permiso.

## <a name="examples"></a>Ejemplos  

En el ejemplo siguiente se agrega un ensamblado denominado `pointudt` a la lista de ensamblados de confianza para el servidor. Estos valores están disponibles en  [Sys. Assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md).     

```  
EXEC sp_add_trusted_assembly 0x8893AD6D78D14EE43DF482E2EAD44123E3A0B684A8873C3F7BF3B5E8D8F09503F3E62370CE742BBC96FE3394477214B84C7C1B0F7A04DCC788FA99C2C09DFCCC, 
N'pointudt, version=0.0.0.0, culture=neutral, publickeytoken=null, processorarchitecture=msil';
```  

## <a name="see-also"></a>Consulte también  
  [sys.sp_drop_trusted_assembly](sys-sp-drop-trusted-assembly-transact-sql.md)  
  [sys.trusted_assemblies](../../relational-databases/system-catalog-views/sys-trusted-assemblies-transact-sql.md)  
  [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)  
  [CLR strict security](../../database-engine/configure-windows/clr-strict-security.md)  
  [sys.assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md)  
  [sys.dm_clr_loaded_assemblies](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)  


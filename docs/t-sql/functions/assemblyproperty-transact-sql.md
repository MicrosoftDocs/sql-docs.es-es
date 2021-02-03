---
description: ASSEMBLYPROPERTY (Transact-SQL)
title: ASSEMBLYPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASSEMBLYPROPERTY_TSQL
- ASSEMBLYPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- ASSEMBLYPROPERTY statement
- assemblies [CLR integration], properties
ms.assetid: cf03d1b1-724c-48bf-a8df-3fe2586b150a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5a5acce18561975c29a3e5f22f74227e079f422f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210006"
---
# <a name="assemblyproperty-transact-sql"></a>ASSEMBLYPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Esta función devuelve información sobre una propiedad de un ensamblado.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ASSEMBLYPROPERTY('assembly_name', 'property_name')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*assembly_name*  
Nombre del ensamblado.
  
*property_name*  
El nombre de una propiedad de la que se va a recuperar información. *property_name* puede tener uno de los valores siguientes:
  
|Value|Descripción|  
|---|---|
|**CultureInfo**|Configuración regional del ensamblado.|  
|**PublicKey**|Clave pública o símbolo (token) de clave pública del ensamblado.|  
|**MvID**|Número completo de identificación de versión del ensamblado generado por el compilador.|  
|**VersionMajor**|Componente principal (primera parte) del número de identificación de la versión de cuatro partes del ensamblado.|  
|**VersionMinor**|Componente secundario (segunda parte) del número de identificación de la versión de cuatro partes del ensamblado.|  
|**VersionBuild**|Componente de generación (tercera parte) del número de identificación de la versión de cuatro partes del ensamblado.|  
|**VersionRevision**|Componente de revisión (cuarta parte) del número de identificación de la versión de cuatro partes del ensamblado.|  
|**SimpleName**|Nombre sencillo del ensamblado.|  
|**Architecture**|Arquitectura del procesador del ensamblado.|  
|**CLRName**|Cadena canónica que codifica el nombre sencillo, número de versión, referencia cultural, clave pública y arquitectura del ensamblado. Este valor identifica de forma única el ensamblado en Common Language Runtime (CLR).|  
  
## <a name="return-type"></a>Tipo de valor devuelto
**sql_variant**
  
## <a name="examples"></a>Ejemplos  
En este ejemplo, se supone que se registra un ensamblado de `HelloWorld` en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Para más información, vea [Ejemplo de Hola a todos](/previous-versions/sql/sql-server-2016/ff878250(v=sql.130)).
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ASSEMBLYPROPERTY ('HelloWorld' , 'PublicKey');  
```  
  
## <a name="see-also"></a>Consulte también
[CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)  
[DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)
  

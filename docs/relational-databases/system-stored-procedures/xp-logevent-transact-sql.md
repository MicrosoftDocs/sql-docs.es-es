---
description: xp_logevent (Transact-SQL)
title: xp_logevent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_logevent
- xp_logevent_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_logevent
ms.assetid: 7b379ad0-5b12-4d2e-9c52-62465df1fdbd
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 2a3a1aa4f71f05860975e9eadc60fa49c65a1951
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99124627"
---
# <a name="xp_logevent-transact-sql"></a>xp_logevent (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Registra un mensaje definido por el usuario en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] archivo de registro y en el visor de eventos de Windows. xp_logevent se puede utilizar para enviar una alerta sin enviar un mensaje al cliente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
xp_logevent { error_number , 'message' } [ , 'severity' ]  
```  
  
## <a name="arguments"></a>Argumentos  
 *error_number*  
 Es un número de error definido por el usuario mayor que 50.000. El valor máximo es 2147483647 (2^31 - 1).  
  
 **'** *mensaje* **'**  
 Es una cadena con un máximo de 2048 caracteres.  
  
 **'** *Severity* **'**  
 Es una de las tres cadenas de caracteres: INFORMATIONAL, WARNING, o ERROR. la *gravedad* es opcional y su valor predeterminado es informativo.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 xp_logevent devuelve el siguiente mensaje de error para el ejemplo de código incluido:  
  
 `The command(s) completed successfully.`  
  
## <a name="remarks"></a>Observaciones  
 Al enviar mensajes desde [!INCLUDE[tsql](../../includes/tsql-md.md)] procedimientos, desencadenadores, lotes, etc., utilice la instrucción RAISERROR en lugar de xp_logevent. xp_logevent no llama a un controlador de mensajes de un cliente ni establece @ @ERROR . Para escribir mensajes en el Visor de eventos de Windows y en el archivo del registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dentro de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ejecute la instrucción RAISERROR.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de servidor db_owner o al rol fijo de base de datos maestra o al rol fijo de servidor sysadmin.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se registra el mensaje (con las variables pasadas al mensaje) en el Visor de eventos de Windows.  
  
```  
DECLARE @@TABNAME varchar(30), @@USERNAME varchar(30), @@MESSAGE varchar(255);  
SET @@TABNAME = 'customers';  
SET @@USERNAME = USER_NAME();  
SELECT @@MESSAGE = 'The table ' + @@TABNAME + ' is not owned by the user   
   ' + @@USERNAME + '.';  
  
USE master;  
EXEC xp_logevent 60000, @@MESSAGE, informational;  
```  
  
## <a name="see-also"></a>Consulte también  
 [PRINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/print-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados extendidos generales &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)  
  
  

---
description: sqlsrv_configure
title: sqlsrv_configure | Microsoft Docs
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_configure
apitype: NA
helpviewer_keywords:
- sqlsrv_configure
- API Reference, sqlsrv_configure
ms.assetid: 9393f975-a4ef-4c50-b4dd-14892fc55cc9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d54957aa83af94e4c139c52a59a31f542b4e3e77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158203"
---
# <a name="sqlsrv_configure"></a>sqlsrv_configure
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Cambia la configuración para el control de errores y las opciones de registro.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sqlsrv_configure( string $setting, mixed $value )  
```  
  
#### <a name="parameters"></a>Parámetros  
*$setting*: el nombre del valor que se va a configurar. Consulte la siguiente tabla, en la que se incluye una lista de opciones de configuración.  
  
*$value*: el valor que se va a aplicar a la configuración especificada en el parámetro *$setting* . Los valores posibles para este parámetro dependen de la configuración especificada. En la siguiente tabla se incluyen las posibles combinaciones:  
  
|Setting|Posibles valores para el parámetro $value (equivalente entero entre paréntesis)|Valor predeterminado|  
|-----------|------------------------------------------------------------------------------|-----------------|  
|ClientBufferMaxKBSize<sup>1</sup>|Un número no negativo hasta el límite de memoria PHP.<br /><br />No se permiten ni el cero ni los números negativos.|10240 KB|  
|LogSeverity<sup>2</sup>|SQLSRV_LOG_SEVERITY_ALL (-1)<br /><br />SQLSRV_LOG_SEVERITY_ERROR (1)<br /><br />SQLSRV_LOG_SEVERITY_NOTICE (4)<br /><br />SQLSRV_LOG_SEVERITY_WARNING (2)|SQLSRV_LOG_SEVERITY_ERROR (1)|  
|LogSubsystems<sup>2</sup>|SQLSRV_LOG_SYSTEM_ALL (-1)<br /><br />SQLSRV_LOG_SYSTEM_CONN (2)<br /><br />SQLSRV_LOG_SYSTEM_INIT (1)<br /><br />SQLSRV_LOG_SYSTEM_OFF (0)<br /><br />SQLSRV_LOG_SYSTEM_STMT (4)<br /><br />SQLSRV_LOG_SYSTEM_UTIL (8)|SQLSRV_LOG_SYSTEM_OFF (0)|  
|WarningsReturnAsErrors<sup>3</sup>|**true** (1) o **false** (0)|**true** (1)|  
  
## <a name="return-value"></a>Valor devuelto  
Si se llama a **sqlsrv_configure** con un valor o una configuración no compatibles, la función devuelve el valor **False**. De lo contrario, devuelve **True**.  
  
## <a name="remarks"></a>Observaciones  
1) Para obtener más información sobre las consultas de cliente, vea [Cursor Types &#1;SQLSRV Driver&#40;](../../connect/php/cursor-types-sqlsrv-driver.md) (Tipos de cursor &#40;Controlador SQLSRV&#41;).  
  
2\) Para obtener más información sobre la actividad de registro, consulte [Logging Activity](../../connect/php/logging-activity.md) (Actividad de registro).  
  
(3) Para más información sobre cómo configurar el control de errores y advertencias, consulte [Procedimientos: Configuración del control de errores y advertencias con el controlador SQLSRV](../../connect/php/how-to-configure-error-and-warning-handling-using-the-sqlsrv-driver.md).  
  
## <a name="see-also"></a>Consulte también  
[Referencia de API del controlador SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)

[Guía de programación para los controladores de Microsoft para PHP para SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md) 
  

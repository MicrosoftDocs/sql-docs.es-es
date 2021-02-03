---
description: RECONFIGURE (Transact-SQL)
title: RECONFIGURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/20/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RECONFIGURE
- RECONFIGURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- reconfiguring configuration options
- configuration options [SQL Server], reconfiguring
- updating configuration options
- RECONFIGURE, RECONFIGURE statement
- RECONFIGURE
- RECONFIGURE, WITH OVERRIDE statement
ms.assetid: 2e6e4eeb-b70b-4f45-a253-28ac4e595d75
author: cawrites
ms.author: chadam
ms.openlocfilehash: b920a123f104e02b2906d6f55d181bdedbdd1f37
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182258"
---
# <a name="reconfigure-transact-sql"></a>RECONFIGURE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza el valor configurado actualmente (la columna **config_value** del conjunto de resultados **sp_configure**) de una opción de configuración cambiada con el procedimiento almacenado del sistema **sp_configure**. Debido a que, con algunas opciones de configuración es necesario detener y reiniciar el servidor para actualizar el valor en ejecución, RECONFIGURE no siempre actualiza este valor (la columna **run_value** del conjunto de resultados **sp_configure**) para un valor de configuración modificado.    
    
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>Sintaxis    
    
```syntaxsql
RECONFIGURE [ WITH OVERRIDE ]    
```    
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 RECONFIGURE    
 Especifica que, si para el valor de la configuración no es necesario que se detenga y reinicie el servidor, se debe actualizar el valor actual en ejecución. RECONFIGURE también comprueba los valores de configuración nuevos de los valores que no son válidos (por ejemplo, un criterio de ordenación que no existe en **syscharsets**) o que no se recomiendan. En el caso de las opciones de configuración que no requieren que se detenga y reinicie el servidor, el valor en ejecución y los valores configurados actualmente para la opción de configuración deben tener el mismo valor después de especificar RECONFIGURE.    
    
 WITH OVERRIDE    
 Deshabilita la comprobación de los valores de configuración (para los valores que no son válidos o que no se recomiendan) de la opción de configuración avanzada **Intervalo de recuperación**.    
    
 Es posible volver a configurar prácticamente cualquier opción de configuración mediante la opción WITH OVERRIDE, pero pueden evitarse algunos errores irrecuperables. Por ejemplo, la opción de configuración **Memoria de servidor mínima** se podría configurar con un valor mayor que el especificado en la opción de configuración **Memoria de servidor máxima**.
      
## <a name="remarks"></a>Observaciones    
 **sp_configure** no acepta nuevos valores de opciones de configuración que no se encuentren dentro de los intervalos válidos documentados para cada opción de configuración.    
    
 RECONFIGURE no se permite en una transacción implícita o explícita. Al reconfigurar varias opciones al mismo tiempo, si una de las operaciones de reconfiguración genera un error, ninguna de estas operaciones surtirá efecto.    
    
 Al volver a configurar Resource Governor, vea la opción RECONFIGURE de [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md).    
    
## <a name="permissions"></a>Permisos    
 De manera predeterminada, los permisos de RECONFIGURE corresponden a los beneficiarios del permiso ALTER SETTINGS. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen este permiso de manera implícita.    
    
## <a name="examples"></a>Ejemplos    
 En el ejemplo siguiente se establece el límite superior de la opción de configuración `recovery interval` en `75` minutos y se utiliza `RECONFIGURE WITH OVERRIDE` para instalarlo. No se recomiendan intervalos de recuperación superiores a 60 minutos y, por ello, no se admiten de manera predeterminada. No obstante, al especificar la opción `WITH OVERRIDE`, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no comprueba si el valor especificado (`75`) es un valor válido para la opción de configuración `recovery interval`.    
    
```sql    
EXEC sp_configure 'recovery interval', 75    
RECONFIGURE WITH OVERRIDE;    
GO    
```    
    
## <a name="see-also"></a>Consulte también    
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)     
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)    
    
  

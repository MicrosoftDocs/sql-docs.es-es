---
title: Cambio del tiempo de recuperación de destino de una base de datos
description: Obtenga información sobre cómo establecer o cambiar el tiempo de recuperación de destino de una base de datos de SQL Server en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: e466419a-d8a4-48f7-8d97-13a903ad6b15
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: d5762a71ab0bd2425abb0ee59c997f1f30fb10b4
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489369"
---
# <a name="change-the-target-recovery-time-of-a-database-sql-server"></a>Cambiar el tiempo de recuperación de destino de una base de datos (SQL Server)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema, se describe cómo establecer o cambiar el tiempo de recuperación de destino de una base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. De forma predeterminada, el tiempo de recuperación de destino es de 60 segundos y la base de datos usa *puntos de comprobación indirectos*. El tiempo de recuperación de destino establece un límite superior en el tiempo de recuperación para esta base de datos.  
  
> [!NOTE]  
>  El límite superior especificado para una base de datos determinada por el valor de tiempo de recuperación de destino se puede superar si una transacción de larga duración provoca tiempos de UNDO excesivos.  
  
-   **Antes de empezar:**  [Limitaciones y restricciones](#Restrictions), [Seguridad](#Security)  
  
-   **Para cambiar el tiempo de recuperación de destino con:**  [SQL Server Management Studio](#SSMSProcedure) o [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones 
  
> [!CAUTION]  
>  Una carga de trabajo transaccional en línea en una base de datos que esté configurada para puntos de comprobación indirectos podría experimentar un deterioro del rendimiento. Los puntos de comprobación indirectos garantizan que el número de páginas desfasadas se encuentra por debajo de un umbral determinado para que la recuperación de la base de datos finalice en el tiempo de recuperación de destino. La opción de configuración intervalo de recuperación usa el número de transacciones para determinar el tiempo de recuperación, a diferencia de los puntos de comprobación indirectos, que usan el número de páginas desfasadas. Cuando se habilitan los puntos de comprobación indirectos en una base de datos que recibe un gran número de operaciones de DML, el escritor en segundo plano puede iniciar agresivamente el vaciado de búferes desfasados en el disco para asegurarse de que el tiempo necesario para realizar la recuperación se encuentra dentro del tiempo de recuperación de destino establecido en la base de datos. Esto puede provocar actividad de E/S adicional en determinados sistemas, lo que puede contribuir a un cuello de botella de rendimiento si el subsistema del disco está funcionando por encima del umbral de E/S o cerca de él.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la base de datos.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para cambiar el tiempo de recuperación de destino**  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]y expándala.  
  
2.  Expanda el contenedor **Bases de datos**, haga clic con el botón derecho en la base de datos que quiera cambiar y haga clic en el comando **Propiedades**.  
  
3.  En el cuadro de diálogo **Propiedades de la base de datos** , haga clic en la página **Opciones** .  
  
4.  En el panel **Recuperación**, en el campo **Tiempo de recuperación de destino (segundos)** , especifique el número de segundos que quiera como límite superior para el tiempo de recuperación de esta base de datos.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para cambiar el tiempo de recuperación de destino**  
  
1.  Conéctese a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde reside la base de datos.  
  
2.  Use la siguiente instrucción [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) del siguiente modo:  
  
     TARGET_RECOVERY_TIME **=** _target_recovery_time_ { SECONDS | MINUTES }  
  
     *target_recovery_time*  
     Empezando por [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)], el valor predeterminado es 1 minuto. Cuando el valor es mayor que 0 (valor predeterminado para versiones más antiguas), especifica el límite superior para el tiempo de recuperación de la base de datos especificada en caso de bloqueo.  
  
     SECONDS  
     Indica que *target_recovery_time* se expresa como el número de segundos.  
  
     MINUTES  
     Indica que *target_recovery_time* se expresa como el número de minutos.  
  
     El ejemplo siguiente se establece el tiempo de recuperación de destino de la base de datos de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en `60` segundos.  
  
    ```  
    ALTER DATABASE AdventureWorks2012 SET TARGET_RECOVERY_TIME = 60 SECONDS;  
    ```  
  
## <a name="see-also"></a>Consulte también  
 [Puntos de comprobación de base de datos &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)  
  
  

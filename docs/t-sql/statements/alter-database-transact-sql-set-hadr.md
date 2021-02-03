---
description: ALTER DATABASE SET HADR (Transact-SQL)
title: ALTER DATABASE SET HADR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SET HADR
- SET_HADR_TSQL
- HADR_TSQL
- HADR
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER DATABASE statement, AlwaysOn Availability Group
- ALTER DATABASE statement, SET HADR options
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], Transact-SQL statements
- Availability Groups [SQL Server], databases
ms.assetid: 20e6e803-d6d5-48d5-b626-d1e0a73d174c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7d62a25ccfdf4abb6231f226e7a846418680a155
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184522"
---
# <a name="alter-database-transact-sql-set-hadr"></a>ALTER DATABASE (Transact-SQL) SET HADR 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe la sintaxis de ALTER DATABASE para configurar las opciones de [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] en bases de datos secundarias. Solo se permite una opción SET HADR por instrucción ALTER DATABASE. Estas opciones solo se admiten en réplicas secundarias.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER DATABASE database_name  
   SET HADR   
   {  
        { AVAILABILITY GROUP = group_name | OFF }  
   | { SUSPEND | RESUME }  
   }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *database_name*  
 Es el nombre de la base de datos secundaria que se va a modificar.  
  
 SET HADR  
 Ejecuta el comando de [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] especificado en la base de datos indicada.  
  
 { AVAILABILITY GROUP **=** _group_name_ | OFF }  
 Une o quita la base de datos de disponibilidad del grupo de disponibilidad especificado, de la manera siguiente:  
  
 *group_name*  
 Une la base de datos especificada de la réplica secundaria hospedada por la instancia del servidor en la que se ejecuta el comando al grupo de disponibilidad especificado por group_name.  
  
 Los requisitos previos para esta operación son los siguientes:  
  
-   La base de datos debe haberse agregado al grupo de disponibilidad de la réplica principal.  
  
-   La réplica principal debe estar activa. Para obtener información sobre cómo solucionar los problemas relativos a una réplica principal inactiva, vea [Solución de problemas de configuración de grupos de disponibilidad AlwaysOn (SQL Server)](/previous-versions/sql/sql-server-2012/ff878308(v=sql.110)).  
  
-   La réplica principal debe estar en línea y la réplica secundaria debe estar conectada a la réplica principal.  
  
-   La base de datos secundaria debe haberse restaurado mediante WITH NORECOVERY desde las copias de seguridad recientes de bases de datos y de registros de la base de datos principal, finalizando con una copia de seguridad de registros que sea suficientemente reciente como para permitir que la base de datos secundaria se ponga al día con la base de datos principal.  
  
    > [!NOTE]  
    >  Para agregar una base de datos al grupo de disponibilidad, conéctese a la instancia del servidor que hospeda la réplica principal y use la instrucción [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)*group_name* ADD DATABASE *database_name*.  
  
 Para obtener más información, vea [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
 Apagado  
 Quita la base de datos secundaria especificada del grupo de disponibilidad.  
  
 Quitar una base de datos secundaria puede ser útil si se ha retrasado con respecto a la base de datos principal y no desea esperar hasta que la base de datos secundaria se ponga al día. Después de quitar la base de datos secundaria, puede actualizarla restaurando una secuencia de copias de seguridad que termina con una copia de seguridad de registros reciente (mediante RESTORE… WITH NORECOVERY).  
  
> [!IMPORTANT]  
>  Para quitar completamente una base de datos de disponibilidad de un grupo de disponibilidad, conéctese a la instancia del servidor que hospeda la réplica de disponibilidad principal y use la instrucción [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md)*group_name* REMOVE DATABASE *availability_database_name*. Para obtener más información, vea [Eliminación de una base de datos principal de un grupo de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).  
  
 SUSPEND  
 Suspende el movimiento de datos en una base de datos secundaria. Un comando SUSPEND realiza la devolución en cuanto haya sido aceptado por la réplica que hospeda la base de datos de destino, pero la suspensión real de la base de datos se produce de forma asincrónica.  
  
 El ámbito del impacto depende de dónde se ejecute la instrucción ALTER DATABASE:  
  
-   Si suspende una base de datos secundaria en una réplica secundaria, solo se suspende la base de datos secundaria local. Las conexiones existentes en la secundaria legible siguen estando utilizables. Las nuevas conexiones a la base de datos suspendida en la secundaria legible no se permiten hasta que se reanude el movimiento de datos.  
  
-   Si suspende una base de datos en la réplica principal, el movimiento de datos se suspende en las bases de datos secundarias correspondientes en cada réplica secundaria. Las conexiones existentes en una réplica secundaria legible permanecen utilizables y las nuevas conexiones de intención de lectura no podrán conectarse a réplicas secundarias legibles.  
  
-   Cuando se suspende el movimiento de datos debido a una conmutación por error manual forzada, no se permiten las conexiones a la nueva réplica secundaria mientras el movimiento de datos esté suspendido.  
  
 Cuando se suspende una base de datos en una réplica secundaria, tanto la base de datos como la réplica quedan desincronizadas y se marcan como NOT SYNCHRONIZED.  
  
> [!IMPORTANT]  
>  Mientras se suspende una base de datos secundaria, la cola de envío de la base de datos principal correspondiente acumulará registros de transacciones sin enviar. Las conexiones a la réplica secundaria devuelven los datos que estaban disponibles en el momento en que suspendió el movimiento de datos.  
  
> [!NOTE]  
>  La suspensión y reanudación de una base de datos secundaria AlwaysOn no afecta directamente a la disponibilidad de la base de datos principal, aunque la suspensión de una base de datos secundaria puede afectar a las funciones de conmutación por error y redundancia para la base de datos principal, hasta que se reanude la base de datos secundaria suspendida. Esto se diferencia del reflejo de base de datos, en el que el estado de reflejo se suspende tanto en la base de datos reflejada como en la base de datos principal hasta que el reflejo se reanuda. Al suspender una base de datos principal AlwaysOn, se suspende el movimiento de datos en todas las bases de datos secundarias y las capacidades de conmutación por error y redundancia cesan para esa base de datos hasta que la base de datos principal se reanuda.  
  
 Para obtener más información, vea [Suspensión de una base de datos de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/suspend-an-availability-database-sql-server.md).  
  
 RESUME  
 Reanuda el movimiento de datos suspendido en la base de datos secundaria especificada. Un comando RESUME realiza la devolución en cuanto haya sido aceptado por la réplica que hospeda la base de datos de destino, pero la reanudación real de la base de datos se produce de forma asincrónica.  
  
 El ámbito del impacto depende de dónde se ejecute la instrucción ALTER DATABASE:  
  
-   Si reanuda una base de datos secundaria en una réplica secundaria, solo se reanuda la base de datos secundaria local. El movimiento de datos se reanuda a menos que la base de datos también se haya suspendido en la réplica principal.  
  
-   Si reanuda una base de datos en la réplica principal, el movimiento de datos se reanuda en cada réplica secundaria en la que la base de datos secundaria correspondiente tampoco se haya suspendido localmente. Para reanudar una base de datos secundaria que se suspendió individualmente en una réplica secundaria, conéctese a la instancia del servidor que hospeda la réplica secundaria y reanude la base de datos allí.  
  
     En el modo de confirmación sincrónica, el estado de la base de datos cambia a SYNCHRONIZING. Si no hay ninguna otra base de datos suspendida actualmente, el estado de la réplica también cambia a SYNCHRONIZING.  
  
     Para obtener más información, vea [Reanudar una base de datos de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md).  
  
## <a name="database-states"></a>Estados de base de datos  
 Cuando una base de datos secundaria se une a un grupo de disponibilidad, la réplica secundaria local cambia el estado de la base de datos secundaria de RESTORING a ONLINE. Si se quita una base de datos secundaria del grupo de disponibilidad, la réplica secundaria local vuelve a establecerse al estado RESTORING. Esto permite aplicar copias de seguridad de registros subsiguientes de la base de datos principal a la base de datos secundaria.  
  
## <a name="restrictions"></a>Restricciones  
 Ejecute las instrucciones ALTER DATABASE fuera de transacciones y de lotes.  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER en la base de datos. Combinar una base de datos con un grupo de disponibilidad requiere ser miembro del rol fijo de base de datos **db_owner**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se une la base de datos secundaria `AccountsDb1` a la réplica secundaria local del grupo de disponibilidad `AccountsAG`.  
  
```sql  
ALTER DATABASE AccountsDb1 SET HADR AVAILABILITY GROUP = AccountsAG;  
```  
  
> [!NOTE]  
>  Para ver esta instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] usada en contexto, vea [Crear un grupo de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) [Solucionar problemas de configuración de grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md) 
  

---
title: Configuración de transacciones distribuidas para un grupo de disponibilidad
description: 'Se describe cómo configurar transacciones distribuidas para bases de datos dentro de un grupo de disponibilidad Always On. '
ms.custom: seodec18
ms.date: 02/06/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: da3071f14be97a4bbbd9ac909926f210c49504d4
ms.sourcegitcommit: 62c7b972db0ac28e3ae457ce44a4566ebd3bbdee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231523"
---
# <a name="configure-distributed-transactions-for-an-always-on-availability-group"></a>Configuración de transacciones distribuidas para un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] admite todas las transacciones distribuidas, incluidas las bases de datos de un grupo de disponibilidad. En este artículo se explica cómo configurar un grupo de disponibilidad para las transacciones distribuidas.  

A fin de garantizar que las transacciones distribuidas sucedan, el grupo de disponibilidad debe estar configurado para registrar las bases de datos como administradores de recursos de transacción distribuida.  

>[!NOTE]
>[!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] Service Pack 2 y versiones posteriores proporcionan compatibilidad total con las transacciones distribuidas en los grupos de disponibilidad. En las versiones de [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] anteriores al Service Pack 2, las transacciones distribuidas entre bases de datos (es decir, las transacciones que usan bases de datos en la misma instancia de SQL Server) que implican una base de datos en un grupo de disponibilidad no son compatibles. [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] no presenta esta limitación. 
>
>En [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)], los pasos de configuración son los mismos que en [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)].

En una transacción distribuida, las aplicaciones cliente funcionan con Microsoft DTC (Coordinador de transacciones distribuidas) para mantener la coherencia de las transacciones en múltiples orígenes de datos. DTC es un servicio disponible en los sistemas operativos basados en Windows Server compatibles. En una transacción distribuida, DTC es el *coordinador de transacciones*. Una instancia de SQL Server suele ser el *administrador de recursos*. Cuando hay una base de datos en un grupo de disponibilidad, cada base de datos debe ser su propio administrador de recursos. 

[!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] no impide las transacciones distribuidas de bases de datos en un grupo de disponibilidad, aun cuando el grupo de disponibilidad no esté configurado para transacciones distribuidas. Pero cuando un grupo de disponibilidad no está configurado para transacciones distribuidas, hay ocasiones en las que la conmutación por error puede que no se efectúe correctamente. En concreto, es posible que la nueva instancia de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] de réplica principal no pueda obtener el resultado de la transacción de DTC. Para que la instancia de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] pueda obtener el resultado de las transacciones dudosas de DTC tras la conmutación por error, configure el grupo de disponibilidad para que admita transacciones distribuidas. 

DTC no interviene en el procesamiento de grupos de disponibilidad a menos que una base de datos también sea miembro de un clúster de conmutación por error. Dentro de un grupo de disponibilidad, la coherencia entre las réplicas se mantiene mediante la lógica del grupo de disponibilidad: la principal no completará la confirmación y reconocerá la confirmación al autor de la llamada hasta que la secundaria haya confirmado que ha guardado las entradas del registro en el almacenamiento duradero. Solo entonces la principal declarará que la transacción se ha completado. En el modo asincrónico, no se espera el reconocimiento por parte de la réplica secundaria, y hay una posibilidad explícita de que se pierda una pequeña cantidad de datos.

## <a name="prerequisites"></a>Requisitos previos

Antes de configurar un grupo de disponibilidad que admita transacciones distribuidas, debe cumplir los siguientes requisitos previos:

* Todas las instancias de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] que participan en la transacción distribuida deben pertenecer a la versión [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] o posterior.

* Los grupos de disponibilidad deben ejecutarse en Windows Server 2016 o Windows Server 2012 R2. Para Windows Server 2012 R2, debe instalar la actualización de KB3090973 disponible en [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973).  

## <a name="create-an-availability-group-for-distributed-transactions"></a>Crear un grupo de disponibilidad que admita transacciones distribuidas

Configure un grupo de disponibilidad que admita transacciones distribuidas. Establezca el grupo de disponibilidad para permitir que cada base de datos se registre como un administrador de recursos. En este artículo se explica cómo configurar un grupo de disponibilidad de manera que cada base de datos sea un administrador de recursos en DTC.



Puede crear un grupo de disponibilidad que admita transacciones distribuidas en [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] o una versión posterior. Para crear un grupo de disponibilidad que admita transacciones distribuidas, incluya `DTC_SUPPORT = PER_DB` en la definición de dicho grupo de disponibilidad. Con el siguiente script se crea un grupo de disponibilidad que admite transacciones distribuidas. 

```sql
CREATE AVAILABILITY GROUP MyAG
   WITH (
      DTC_SUPPORT = PER_DB  
      )
   FOR DATABASE DB1, DB2
   REPLICA ON
      'Server1' WITH (
         ENDPOINT_URL = 'TCP://SERVER1.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         ),
      'Server2' WITH (
         ENDPOINT_URL = 'TCP://SERVER2.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         )
```

>[!NOTE]
>El script anterior es un ejemplo sencillo de un grupo de disponibilidad y no está pensado para ningún entorno de producción en concreto. 

## <a name="alter-an-availability-group-for-distributed-transactions"></a>Modificar un grupo de disponibilidad para que admita transacciones distribuidas

Puede modificar un grupo de disponibilidad para que admita transacciones distribuidas en [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] o una versión posterior. Para modificar un grupo de disponibilidad para que admita transacciones distribuidas, incluya `DTC_SUPPORT = PER_DB` en el script `ALTER AVAILABILITY GROUP`. Con este script de ejemplo se cambia el grupo de disponibilidad de forma que admita transacciones distribuidas. 

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = PER_DB  
      );
```

>[!NOTE]
>A partir de [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] Service Pack 2, puede modificar un grupo de disponibilidad para transacciones distribuidas. Para las versiones de [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] anteriores al Service Pack 2, necesita descartar y recrear el grupo de disponibilidad con la configuración `DTC_SUPPORT = PER_DB`. 

Para deshabilitar las transacciones distribuidas, utilice el siguiente comando de Transact-SQL:

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = NONE  
      );
```

## <a name="distributed-transactions---technical-concepts"></a><a name="distTran"/>Transacciones distribuidas: conceptos técnicos

Una transacción distribuida abarca dos o más bases de datos. Como administrador de transacciones, DTC coordina la transacción entre las instancias de SQL Server y otros orígenes de datos. Cada instancia del motor de base de datos de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] puede funcionar como un administrador de recursos. Cuando un grupo de disponibilidad se configura con `DTC_SUPPORT = PER_DB`, las bases de datos pueden actuar como administradores de recursos. Para obtener más información, consulte la documentación de MS DTC.

Una transacción con dos o más bases de datos en una única instancia del motor de base de datos es una transacción distribuida de facto. La instancia administra la transacción distribuida internamente; para el usuario funciona como una transacción local. [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] promueve todas las transacciones entre bases de datos a DTC cuando las bases de datos están en un grupo de disponibilidad configurado con `DTC_SUPPORT = PER_DB`, incluso en una sola instancia de SQL Server. 

En la aplicación, una transacción distribuida se administra de forma muy parecida a una transacción local. Al final de la transacción, la aplicación solicita que se confirme o se revierta la transacción. El administrador de transacciones debe administrar una confirmación distribuida de forma diferente para reducir al mínimo el riesgo de que, si se produce un error en la red, algunos administradores de recursos realicen confirmaciones mientras los demás revierten la transacción. Esto se consigue administrando el proceso de confirmación en dos fases (la fase de preparación y la fase de confirmación), lo que se conoce como confirmación en dos fases.

- **Fase de preparación**
   
   Cuando el administrador de transacciones recibe una solicitud de confirmación, envía un comando de preparación a todos los administradores de recursos implicados en la transacción. Cada administrador de recursos hace lo necesario para que la transacción sea duradera y todos los búferes que contienen imágenes del registro de la transacción se pasan a disco. A medida que cada administrador de recursos completa la fase de preparación, notifica si la preparación ha tenido éxito o no al administrador de transacciones.

- **Fase de confirmación**
   
   Si el administrador de transacciones recibe la notificación de que todas las preparaciones son correctas por parte de todos los administradores de recursos, envía comandos de confirmación a cada administrador de recursos. A continuación, los administradores de recursos pueden completar la confirmación. Si todos los administradores de recursos indican que la confirmación ha sido correcta, el administrador de transacciones envía una notificación de éxito a la aplicación. Si algún administrador de recursos informó de un error al realizar la preparación, el administrador de transacciones envía un comando para revertir la transacción a cada administrador de recursos e indica a la aplicación que se ha producido un error de confirmación.

### <a name="detailed-steps"></a>Pasos detallados

Con los siguientes puntos se explica cómo funciona la aplicación con DTC para completar las transacciones distribuidas.

1. La instancia de SQL Server se da de alta en la transacción de DTC. Esto puede ocurrir cuando hay más de un administrador de recursos en la transacción, o bien si el cliente solicita que una transacción se promueva al nivel de transacción de DTC.
2. El cliente realiza algunas funciones en la instancia de SQL Server de la transacción de DTC.
3. El cliente emite si la transacción de DTC se confirma o se anula.
    - Si el cliente emite una anulación, la transacción se anula inmediatamente.
    - Si el cliente emite una confirmación, DTC inicia el protocolo de confirmación en dos fases, para lo cual solicita a todos los administradores de recursos de la transacción que preparen la transacción.
4. DTC indica a todos los administradores de recursos que confirmen la transacción después de que todos ellos hayan confirmado correctamente la fase de preparación. Si algo impide que puedan confirmar la transacción correctamente, DTC la anula. 

### <a name="effects-of-configuring-an-availability-group-for-distributed-transactions"></a>Consecuencias de configurar un grupo de disponibilidad para que admita transacciones distribuidas

Cada entidad que participa en una transacción distribuida se conoce como administrador de recursos. Ejemplos de administradores de recursos:

* Instancia de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]. 
* Una base de datos en un grupo de disponibilidad que se ha configurado para admitir transacciones distribuidas.
* Servicio DTC: también puede ser un administrador de transacciones.
* Otros orígenes de datos. 

Para poder participar en las transacciones distribuidas, una instancia de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] se da de alta en DTC. Normalmente, la instancia de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] se da de alta en DTC en el servidor local. Cada instancia de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] crea un administrador de recursos con un identificador de administrador de recursos (RMID) único y lo registra con DTC. En la configuración predeterminada, todas las bases de datos de una instancia de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] usan el mismo RMID. 

Cuando una base de datos está en un grupo de disponibilidad, la copia de lectura y escritura de la base de datos (o réplica principal) se puede trasladar a otra instancia de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]. Para admitir transacciones distribuidas durante este traslado, cada base de datos debe actuar como un administrador de recursos independiente y tener un RMID único. Cuando un grupo de disponibilidad tiene `DTC_SUPPORT = PER_DB`, [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] crea un administrador de recursos para cada base de datos y se registra con DTC por medio de un RMID único. En esta configuración, la base de datos es un administrador de recursos en las transacciones de DTC.

>[!IMPORTANT]
>Tenga en cuenta que DTC tiene un límite de 32 inscripciones por transacción distribuida. Dado que cada base de datos de un grupo de disponibilidad se inscribe con DTC por separado, si la transacción incluye más de 32 bases de datos, es posible que reciba el siguiente error cuando [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] intente inscribir la 33.ª base de datos:
>
>`Enlist operation failed: 0x8004d101(XACT_E_TOOMANY_ENLISTMENTS). SQL Server could not register with Microsoft Distributed Transaction Coordinator (MS DTC) as a resource manager for this transaction. The transaction may have been stopped by the client or the resource manager.`

Para más información sobre las transacciones distribuidas en [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)], vea [Transacciones distribuidas](#distTran).

## <a name="manage-unresolved-transactions"></a>Administrar las transacciones sin resolver

Después de una conmutación por error no se puede recuperar el resultado de las transacciones activas que había durante el cambio de RMID. Esto se debe a que el RMID que SQL Server usó para dar de alta no es el mismo que el RMID que usó para recuperar el resultado. El cambio de RMID puede ocurrir en los siguientes casos:

* Cuando se cambia `DTC_SUPPORT` en un grupo de disponibilidad. 
* Cuando se agrega o quita una base de datos de un grupo de disponibilidad. 
* Cuando se elimina un grupo de disponibilidad.

En los casos anteriores, si la réplica principal se conmuta por error a una nueva instancia de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)], dicha instancia intenta ponerse en contacto con DTC para identificar el resultado de la transacción. DTC no puede devolver el resultado, porque el RMID que la base de datos está usando para obtener el resultado de las transacciones dudosas durante la recuperación no está dado de alta. Por lo tanto, la base de datos pasa a un estado SOSPECHOSO.

El nuevo registro de errores de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] contiene una entrada similar al siguiente ejemplo:

```
Microsoft Distributed Transaction Coordinator (MS DTC) 
failed to reenlist citing that the database RMID does 
not match the RMID [xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx] 
associated with the transaction.  Please manually resolve
the transaction.
    
SQL Server detected a DTC/KTM in-doubt transaction with UOW 
{yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy}.Please resolve it 
following the guideline for Troubleshooting DTC Transactions.
```

El ejemplo anterior refleja que DTC no podría volver a dar de alta la base de datos de la nueva réplica principal en la transacción que se creó después de la conmutación por error. La instancia de [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] no puede averiguar el resultado de la transacción distribuida, por lo que marca la base de datos como sospechosa. La transacción se marca como una unidad de trabajo (UOW) y se hace referencia a ella con un GUID. Para recuperar la base de datos, confirme o revierta la transacción manualmente. 

>[!WARNING]
>Cuando una transacción se confirma o revierte manualmente, ello puede repercutir en una aplicación. Confirme que la acción de confirmación o reversión es coherente con los requisitos de la aplicación. 

Ejecute solo uno de los siguientes scripts:

   * Para confirmar la transacción, actualice y ejecute el siguiente script (sustituya `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` por la UOW de la transacción dudosa reflejada en el mensaje de error anterior) y ejecute lo siguiente:

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH COMMIT
   ```

   * Para revertir la transacción, actualice y ejecute el siguiente script (sustituya `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` por la UOW de la transacción dudosa reflejada en el mensaje de error anterior) y ejecute lo siguiente:

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH ROLLBACK
   ```

Después de confirmar o revertir la transacción, puede usar `ALTER DATABASE` para establecer la base de datos en línea. Actualice y ejecute el siguiente script (indique el nombre de base de datos de la base de datos sospechosa):

   ```sql
   ALTER DATABASE [DB1] SET ONLINE
   ```

Para más información sobre cómo resolver transacciones dudosas, vea [Resolve Transactions Manually](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754134(v=ws.10)) (Resolver transacciones manualmente).

## <a name="next-steps"></a>Pasos siguientes  

[Transacciones distribuidas](/dotnet/framework/data/adonet/distributed-transactions)

[Grupos de disponibilidad Always On: interoperabilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
[Transactions - Always On Availability Groups and Database Mirroring](transactions-always-on-availability-and-database-mirroring.md) (Transacciones: grupos de disponibilidad AlwaysOn y creación de reflejo de la base de datos)  

[Supporting XA Transactions](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753563(v=ws.10)) (Compatibilidad con las transacciones XA)

[How It Works: Session/SPID (-2) for DTC Transactions](/archive/blogs/bobsql/how-it-works-sessionspid-2-for-dtc-transactions) (Funcionamiento de Session/SPID [-2] en las transacciones de DTC)

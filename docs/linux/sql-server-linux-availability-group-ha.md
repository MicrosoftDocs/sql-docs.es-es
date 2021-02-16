---
title: 'Patrones de implementación de grupo de disponibilidad: SQL Server en Linux'
description: Obtenga información sobre las configuraciones de implementación admitidas para los grupos de disponibilidad Always On de SQL Server en servidores Linux.
ms.custom: seo-lt-2019
ms.date: 04/17/2019
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
ms.assetid: edd75f68-dc62-4479-a596-57ce8ad632e5
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: edeb7ab3de19b50f78f2b4b5bab9c0f8afe80780
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347787"
---
# <a name="high-availability-and-data-protection-for-availability-group-configurations"></a>Alta disponibilidad y protección de datos para las configuraciones de grupo de disponibilidad

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se presentan las configuraciones de implementación admitidas para los grupos de disponibilidad Always On de SQL Server en servidores Linux. Un grupo de disponibilidad admite alta disponibilidad y protección de datos. La detección automática de errores, la conmutación automática por error y la reconexión transparente después de la conmutación por error proporcionan alta disponibilidad. Las réplicas sincronizadas proporcionan la protección de datos. 

En un clúster de conmutación por error de Windows Server (WSFC), una configuración común para la alta disponibilidad usa dos réplicas sincrónicas y un tercer servidor o recurso compartido de archivos para proporcionar el cuórum. El testigo de recurso compartido de archivos valida la configuración del grupo de disponibilidad: estado de sincronización y el rol de la réplica, por ejemplo. Esta configuración garantiza que la réplica secundaria elegida como destino de la conmutación por error tenga los cambios de configuración del grupo de disponibilidad y los datos más recientes. 

El WSFC sincroniza los metadatos de configuración para el arbitraje de conmutación por error entre las réplicas del grupo de disponibilidad y el testigo del recurso compartido de archivos. Cuando un grupo de disponibilidad no está en un WSFC, las instancias de SQL Server almacenan los metadatos de configuración en la base de datos maestra.

Por ejemplo, un grupo de disponibilidad en un clúster de Linux tiene `CLUSTER_TYPE = EXTERNAL`. No hay ningún WSFC para arbitrar la conmutación por error. En este caso, las instancias de SQL Server administran y mantienen los metadatos de configuración. Dado que no hay ningún servidor testigo en este clúster, se requiere una tercera instancia de SQL Server para almacenar los metadatos de estado de configuración. Las tres instancias de SQL Server juntas proporcionan almacenamiento de metadatos distribuido para el clúster. 

El administrador de clústeres puede consultar las instancias de SQL Server del grupo de disponibilidad y coordinar la conmutación por error para mantener una alta disponibilidad. En un clúster de Linux, Pacemaker es el administrador de clústeres. 

SQL Server 2017 CU 1 permite una alta disponibilidad para un grupo de disponibilidad con `CLUSTER_TYPE = EXTERNAL` para dos réplicas sincrónicas más una réplica de solo configuración. La réplica de solo configuración se puede hospedar en cualquier edición de SQL Server 2017 CU1 o posterior, incluida la edición SQL Server Express. La réplica de solo configuración mantiene información de configuración sobre el grupo de disponibilidad en la base de datos maestra, pero no contiene las bases de datos de usuario del grupo de disponibilidad. 

## <a name="how-the-configuration-affects-default-resource-settings"></a>Cómo afecta la configuración a la configuración de recursos predeterminada

SQL Server 2017 presenta la configuración del recurso de clúster `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT`. Esta configuración garantiza que el número especificado de réplicas secundarias escriba los datos de la transacción que se registrarán antes de que la réplica principal confirme cada transacción. Cuando se usa un administrador de clústeres externo, esta configuración afecta a la alta disponibilidad y a la protección de datos. El valor predeterminado de la configuración depende de la arquitectura en el momento en que se crea el recurso de clúster. Al instalar el agente de recursos de SQL Server (`mssql-server-ha`) y crear un recurso de clúster para el grupo de disponibilidad, el administrador de clústeres detecta la configuración del grupo de disponibilidad y establece `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` en consecuencia. 

Si es compatible con la configuración, el parámetro de agente de recurso `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` se establece en el valor que proporciona alta disponibilidad y protección de datos. Para obtener más información, consulte [Información sobre el agente de recursos de SQL Server para Pacemaker](#pacemakerNotify).

En las siguientes secciones se explica el comportamiento predeterminado para el recurso de clúster. 

Elija un diseño de grupo de disponibilidad para satisfacer los requisitos empresariales específicos de alta disponibilidad, protección de datos y escalado de lectura.

Las siguientes configuraciones describen los patrones de diseño del grupo de disponibilidad y las capacidades de cada patrón. Estos patrones de diseño se aplican a los grupos de disponibilidad con `CLUSTER_TYPE = EXTERNAL` para soluciones de alta disponibilidad. 

- **Tres réplicas sincrónicas**
- **Dos réplicas sincrónicas**
- **Dos réplicas sincrónicas y una réplica de solo configuración**

<a name="threeSynch"></a>

## <a name="three-synchronous-replicas"></a>Tres réplicas sincrónicas

Esta configuración consta de tres réplicas sincrónicas. De forma predeterminada, proporciona alta disponibilidad y protección de datos. También puede proporcionar escalado de lectura.

![Tres réplicas][3]

Un grupo de disponibilidad con tres réplicas sincrónicas puede proporcionar protección de datos, alta disponibilidad y escalado de lectura. En la tabla siguiente se describe la disponibilidad de este comportamiento. 

|Comportamiento de la disponibilidad |Escalado de lectura|Alta disponibilidad y </br> protección de datos | Protección de los datos|
|:---|---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 |1<sup>\*</sup>|2|
|Interrupción principal |Conmutación por error automática. La nueva réplica principal es de L/E. |Conmutación por error automática. La nueva réplica principal es de L/E. |Conmutación por error automática. La nueva réplica principal no está disponible para todas las transacciones de usuario hasta que la réplica principal anterior se recupere y una al grupo de disponibilidad como réplica secundaria. |
|Interrupción de réplica secundaria  | La réplica principal es de L/E. No se produce una conmutación automática por error si la réplica principal presenta errores. |La réplica principal es de L/E. No se produce una conmutación automática por error si la réplica principal también presenta errores. | La réplica principal no está disponible para transacciones de usuario. |

<sup>\*</sup> Valor predeterminado

<a name="twoSynch"></a>

## <a name="two-synchronous-replicas"></a>Dos réplicas sincrónicas

Esta configuración habilita la protección de datos. Al igual que las demás configuraciones de grupo de disponibilidad, puede habilitar el escalado de lectura. La configuración de dos réplicas sincrónicas no proporciona alta disponibilidad automática. Una configuración de dos réplicas solo es aplicable a SQL Server 2017 RTM y ya no se admite con versiones posteriores (CU1 y posteriores) de SQL Server 2017.

![Dos réplicas sincrónicas][1]

Un grupo de disponibilidad con dos réplicas sincrónicas proporciona protección de datos y escalado de lectura. En la tabla siguiente se describe la disponibilidad de este comportamiento. 

|Comportamiento de la disponibilidad |Escalado de lectura |Protección de los datos|
|:---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>\*</sup>|1|
|Interrupción principal | Conmutación por error manual. Es posible que haya pérdida de datos. La nueva réplica principal es de L/E.| Conmutación por error automática. La nueva réplica principal no está disponible para todas las transacciones de usuario hasta que la réplica principal anterior se recupere y una al grupo de disponibilidad como réplica secundaria.|
|Interrupción de réplica secundaria  |La réplica principal es de L/E, la ejecución se expone a pérdida de datos. |La réplica principal no está disponible para transacciones de usuario hasta que se recupere la réplica secundaria.|

<sup>\*</sup> Valor predeterminado

<a name = "configOnly"></a>

## <a name="two-synchronous-replicas-and-a-configuration-only-replica"></a>Dos réplicas sincrónicas y una réplica de solo configuración

Un grupo de disponibilidad con dos (o más) réplicas sincrónicas y una réplica de solo configuración proporciona protección de datos y también puede proporcionar alta disponibilidad. El diagrama siguiente representa esta arquitectura:

![Grupo de disponibilidad de solo configuración][2]

1. Replicación sincrónica de datos de usuario en la réplica secundaria. También incluye los metadatos de configuración del grupo de disponibilidad.
2. Replicación sincrónica de los metadatos de configuración del grupo de disponibilidad. No incluye los datos de usuario.

En el diagrama de grupo de disponibilidad, una réplica principal envía los datos de configuración a la réplica secundaria y a la réplica de solo configuración. La réplica secundaria también recibe datos de usuario. La réplica de solo configuración no recibe datos de usuario. La réplica secundaria está en modo de disponibilidad sincrónica. La réplica de solo configuración no contiene las bases de datos de los metadatos del grupo de disponibilidad, solo metadatos sobre el grupo de disponibilidad. Los datos de configuración de la réplica de solo configuración se confirman sincrónicamente.

> [!NOTE]
> Un grupo de disponibilidad con una réplica de solo configuración es nuevo para SQL Server 2017 CU1. Todas las instancias de SQL Server en el grupo de disponibilidad deben ser de SQL Server 2017 CU1 o posterior. 

El valor predeterminado para `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` es 0. En la tabla siguiente se describe la disponibilidad de este comportamiento. 

|Comportamiento de la disponibilidad |Alta disponibilidad y </br> protección de datos | Protección de los datos|
|:---|---|---|
|`REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT=`|0 <sup>\*</sup>|1|
|Interrupción principal | Conmutación por error automática. La nueva réplica principal es de L/E. | Conmutación por error automática. La nueva réplica principal no está disponible para transacciones de usuario. |
|Interrupción de la réplica secundaria | La réplica principal es de L/E, la ejecución se expone a pérdida de datos (si se produce un error en la réplica principal y no se puede recuperar). No se produce una conmutación automática por error si la réplica principal también presenta errores. | La réplica principal no está disponible para transacciones de usuario. No hay ninguna réplica a la que realizar la conmutación por error si también se produce un error en la réplica principal. |
|Interrupción de la réplica de solo configuración | La réplica principal es de L/E. No se produce una conmutación automática por error si la réplica principal también presenta errores. | La réplica principal es de L/E. No se produce una conmutación automática por error si la réplica principal también presenta errores. |
|Interrupción de réplicas secundarias y de solo configuración de replicación sincrónica| La réplica principal no está disponible para transacciones de usuario. Sin conmutación automática por error. | La réplica principal no está disponible para transacciones de usuario. No se produce ninguna réplica en la conmutación por error en caso de que sea la réplica principal. |

<sup>\*</sup> Valor predeterminado

> [!NOTE]
> La instancia de SQL Server que hospeda la réplica de solo configuración también puede hospedar otras bases de datos. También puede participar como una base de datos solo de configuración para más de un grupo de disponibilidad. 

## <a name="requirements"></a>Requisitos

- Todas las réplicas de un grupo de disponibilidad con una réplica de solo configuración deben ser SQL Server 2017 CU 1 o posterior.
- Cualquier edición de SQL Server puede hospedar una réplica de solo configuración, incluida SQL Server Express. 
- El grupo de disponibilidad necesita al menos una réplica secundaria, además de la réplica principal.
- Las réplicas de solo configuración no cuentan para el número máximo de réplicas por instancia de SQL Server. La edición estándar de SQL Server permite hasta tres réplicas, la edición de SQL Server Enterprise permite hasta 9.

## <a name="considerations"></a>Consideraciones

- No hay más de una réplica de solo configuración en cada grupo de disponibilidad. 
- Una réplica de solo configuración no puede ser una réplica principal.
- No se puede modificar el modo de disponibilidad de una réplica de solo configuración. Para cambiar de una réplica de solo configuración a una réplica secundaria sincrónica o asincrónica, quite la réplica de solo configuración y agregue una réplica secundaria con el modo de disponibilidad requerido. 
- Una réplica de solo configuración es sincrónica con los metadatos del grupo de disponibilidad. No existen datos de usuario. 
- Un grupo de disponibilidad con una réplica principal y una réplica de solo configuración, pero ninguna réplica secundaria no es válido. 
- No se puede crear un grupo de disponibilidad en una instancia de la edición SQL Server Express. 

<a name="pacemakerNotify"></a>

## <a name="understand-sql-server-resource-agent-for-pacemaker"></a>Información sobre el agente de recursos de SQL Server para Pacemaker

SQL Server 2017 CTP 1.4 agrega `sequence_number` a `sys.availability_groups` para permitir que Pacemaker identifique las réplicas secundarias actualizadas con la réplica principal. `sequence_number` es un valor BIGINT con una progresión continua que representa el grado de actualización de la réplica del grupo de disponibilidad local. Pacemaker actualiza el `sequence_number` con cada cambio de configuración del grupo de disponibilidad. Algunos ejemplos de cambios de configuración son la conmutación por error, la adición de réplica o la eliminación. El número se actualiza en la réplica principal y, después, se inserta en las réplicas secundarias. Por tanto, una réplica secundaria que esté actualizada tendrá el mismo número de secuencia que la réplica principal. 

Cuando Pacemaker decide promover una réplica a principal, en primer lugar envía una notificación *previa a la promoción* a todas las réplicas. Las réplicas devuelven el número de secuencia. Después, cuando Pacemaker intenta realmente promover una réplica a principal, la réplica solo se promueve a sí misma si su número de secuencia es el más alto de todos los números de secuencia. Si su propio número de secuencia no coincide con el número de secuencia más alto, la réplica rechaza la operación de promoción. De esta manera, solo se puede promover a principal la réplica con el número de secuencia más alto, lo que garantiza que no se producirá una pérdida de datos. 

Este proceso requiere al menos una réplica disponible para la promoción con el mismo número de secuencia que la réplica principal anterior. El agente de recursos de Pacemaker establece `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` de forma que al menos una réplica secundaria sincrónica esté actualizada y disponible para ser el destino de una conmutación automática por error de forma predeterminada. Con cada acción de supervisión, el valor de `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` se calcula (y actualiza, si es necesario). El valor `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` es el “número de réplicas sincrónicas” dividido entre 2. En el momento de la conmutación por error, el agente de recursos requiere (réplicas `total number of replicas` - `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT`) para responder a la notificación previa a la promoción. La réplica con el valor de `sequence_number` más alto se promueve a principal. 

Por ejemplo, un grupo de disponibilidad con tres réplicas sincrónicas: una réplica principal y dos réplicas secundarias sincrónicas.

- `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` es 1 (3/2-> 1).

- El número necesario de réplicas para responder a la acción previa a la promoción es de 2 (3 - 1 = 2). 

En este escenario, dos réplicas tienen que responder para que se desencadene la conmutación por error. Para una conmutación automática por error correcta después de una interrupción de la réplica principal, ambas réplicas secundarias deben estar actualizadas y responder a la notificación previa a la promoción. Si están en línea y son sincrónicas, tienen el mismo número de secuencia. El grupo de disponibilidad promueve una de ellas. Si solo una de las réplicas secundarias responde a la acción previa a la promoción, el agente de recursos no puede garantizar que la réplica secundaria que ha respondido tenga el valor de sequence_number más alto y no se desencadena una conmutación por error.

> [!IMPORTANT]
> Cuando `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` es 0, existe riesgo de pérdida de datos. Durante una interrupción de la réplica principal, el agente de recursos no desencadena automáticamente una conmutación por error. Puede esperar a que el servidor principal se recupere o conmutar por error manualmente con `FORCE_FAILOVER_ALLOW_DATA_LOSS`.

Puede elegir invalidar el comportamiento predeterminado e impedir que el recurso de grupo de disponibilidad se establezca `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` automáticamente.

El siguiente script establece `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` en 0 en un grupo de disponibilidad denominado `<**ag1**>`. Antes de efectuar la ejecución, reemplace `<**ag1**>` por el nombre del grupo de disponibilidad.

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=0
```

Para revertir al valor predeterminado, en función de la configuración del grupo de disponibilidad, ejecute:

```bash
sudo pcs resource update <**ag1**> required_synchronized_secondaries_to_commit=
```

> [!NOTE]
> Al ejecutar los comandos anteriores, la réplica principal se degrada temporalmente a secundaria y luego se promueve de nuevo. La actualización de recursos hace que todas las réplicas se detengan y reinicien. El nuevo valor de `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` solo se establece una vez que se reinician las réplicas, no de forma instantánea.

## <a name="see-also"></a>Consulte también

[Grupos de disponibilidad en Linux](sql-server-linux-availability-group-overview.md)

<!--Image references-->
[1]: ./media/sql-server-linux-availability-group-ha/1-read-scale-out.png
[2]: ./media/sql-server-linux-availability-group-ha/2-configuration-only.png
[3]: ./media/sql-server-linux-availability-group-ha/3-three-replica.png
[4]: ./media/sql-server-linux-availability-group-ha/configuration-only-example.png

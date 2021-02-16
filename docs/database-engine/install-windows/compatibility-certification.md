---
title: Certificación de compatibilidad | Microsoft Docs
description: La certificación de compatibilidad elimina cualquier riesgo relativo a la compatibilidad de la aplicación, lo cual permite actualizar una base de datos de SQL Server local y en la nube.
ms.custom: ''
ms.date: 08/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- compatibility [SQL Server], databases
- compatibility levels [SQL Server], after upgrade
- Database Engine [SQL Server], upgrading
- Databases [SQL Server], upgrading
- Database Engine [SQL Server], upgrading
- compatibility [SQL Server], certification
- compatibility level [SQL Server], upgrades
ms.assetid: 3c036813-36cf-4415-a0c9-248d0a433856
author: pmasl
ms.author: pelopes
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c9a4b07a3e6db3da84845f4689ecd373b30e7306
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353847"
---
# <a name="compatibility-certification"></a>Certificación de compatibilidad

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

La certificación de compatibilidad permite que las empresas actualicen y modernicen una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el entorno local, en la nube y en el perímetro, lo cual elimina los riesgos relativos a la compatibilidad de las aplicaciones. 

El mismo [!INCLUDE[ssde_md](../../includes/ssde_md.md)] activa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], incluida la Instancia administrada. Este recurso compartido [!INCLUDE[ssde_md](../../includes/ssde_md.md)] significa que una base de datos de usuario puede moverse sin problemas entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el entorno local y [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], mientras que el código de aplicación que se ejecuta en la base de datos como [!INCLUDE[tsql](../../includes/tsql-md.md)] sigue funcionando como en su sistema de origen.

En cada versión nueva de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el nivel de compatibilidad predeterminado se establece en la versión de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Sin embargo, el nivel de compatibilidad de las versiones anteriores se conserva para mantener la compatibilidad continua de las aplicaciones existentes. Esta matriz de compatibilidad se puede ver [aquí](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#supported-dbcompats).
Por lo tanto, una aplicación que estuviera certificada para trabajar con una versión determinada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **en realidad estaba certificada para funcionar en el nivel de compatibilidad predeterminado de esa versión**.

Por ejemplo, el nivel de compatibilidad de la base de datos 130 era el valor predeterminado en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Dado que los niveles de compatibilidad fuerzan determinados comportamientos funcionales y de optimización de consultas de [!INCLUDE[tsql](../../includes/tsql-md.md)], **una base de datos certificada para funcionar en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] se ha certificado implícitamente en el nivel de compatibilidad 130 de la base de datos**. Esta base de datos puede funcionar tal cual en una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (como [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]) y [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], siempre que el nivel de compatibilidad de la base de datos se mantenga en 130. 

Este es un principio fundamental para el modelo de operación de integración continua de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)]. El [!INCLUDE[ssde_md](../../includes/ssde_md.md)] se mejora y actualiza continuamente en Azure, pero dado que las bases de datos existentes mantienen su nivel de compatibilidad actual, estas continúan funcionando como se han diseñado incluso después de las actualizaciones al [!INCLUDE[ssde_md](../../includes/ssde_md.md)]subyacente. 

Así es también como SharePoint Server 2016 y SharePoint Server 2019 certifican en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)], lo que le permite implementar cualquier [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que pueda usar los niveles de compatibilidad de base de datos admitidos para esas versiones de SharePoint Server. Para más información, consulte [Requisitos de hardware y software para SharePoint Server 2016](/sharepoint/install/hardware-and-software-requirements#minimum-requirements-for-a-database-server-in-a-farm) y [Requisitos de hardware y software para SharePoint Server 2019](/sharepoint/install/hardware-and-software-requirements-2019#minimum-requirements-for-a-database-server-in-a-farm).

## <a name="managing-upgrade-risk-with-compatibility-certification"></a>Administración del riesgo de actualización con la certificación de compatibilidad
El uso de la certificación de compatibilidad es un enfoque valioso para la modernización de las bases de datos. Mediante la certificación basada en el nivel de compatibilidad, los desarrolladores establecen los requisitos técnicos para que una aplicación se admita en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], pero desacoplan el ciclo de vida de la aplicación del de la plataforma de las bases de datos. Esto permite a las empresas mantener el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] actualizado según sea necesario de acuerdo con las directivas del ciclo de vida, así como aprovechar las nuevas mejoras de escalabilidad y rendimiento que no dependen del código y conectar aplicaciones para **mantener su estado funcional** a través de actualizaciones.

La posibilidad de afectar negativamente a la funcionalidad y el rendimiento es el principal factor de riesgo de cualquier actualización. La certificación de compatibilidad representa una tranquilidad en cuanto a la administración de estos riesgos de actualización:

-  En lo que se refiere al comportamiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], cualquier cambio significa que una aplicación debe volver a certificarse para que sea correcta. Sin embargo, la configuración del [nivel de compatibilidad de la base de datos](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) proporciona compatibilidad con versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo para la base de datos especificada, no para todo el servidor. Mantener el nivel de compatibilidad de la base de datos tal y como está garantiza que las consultas de las aplicaciones existentes sigan mostrando el mismo comportamiento antes y después de una actualización de [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Para obtener más información sobre los niveles de compatibilidad y comportamiento de [!INCLUDE[tsql](../../includes/tsql-md.md)], consulte [Usar el nivel de compatibilidad para la compatibilidad con versiones anteriores](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#backwardCompat).

-  En lo que se refiere al rendimiento, dado que las mejoras en el optimizador de consultas se introducen con cada versión, se podrían esperar diferencias en los planes de consulta entre distintas versiones de [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Las diferencias de los planes de consulta en el ámbito de una actualización normalmente se traducen en un riesgo cuando es posible que algunos cambios puedan ser perjudiciales para determinada consulta o carga de trabajo. A su vez, este riesgo es una motivación para la recertificación, que puede retrasar las actualizaciones y plantear problemas relativos al ciclo de vida y el soporte técnico. 
   La mitigación de los riesgos de actualización es el motivo por el que las mejoras del optimizador de consultas se canalizan en el nivel de compatibilidad predeterminado de una nueva versión (en otras palabras, el nivel de compatibilidad más elevado disponible para cualquier versión nueva). La certificación de compatibilidad incluye la **protección de las formas de los planes de consulta**: el principio de mantener el nivel de compatibilidad de la base de datos tal cual inmediatamente después de una actualización de [!INCLUDE[ssde_md](../../includes/ssde_md.md)] se traduce en el uso del mismo modelo de optimización de consultas en la versión nueva, como ya se hacía antes de la actualización, y que la forma del plan de consulta no debe cambiar. 
   Para obtener más información, consulte la sección [¿Por qué es necesaria la forma del plan de consulta?](#queryplan_shape) de este artículo.
   
Para obtener más información sobre los niveles de compatibilidad, consulte [Usar el nivel de compatibilidad para la compatibilidad con versiones anteriores](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#backwardCompat).
   
> [!IMPORTANT]
> Para una aplicación existente que ya estaba certificada para un nivel de compatibilidad determinado, actualice el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] y **mantenga** el nivel de compatibilidad de base de datos anterior. No es necesario volver a certificar una aplicación en este escenario. Para obtener más información, consulte [Actualizaciones del motor de base de datos y niveles de compatibilidad](#compatibility-levels-and-database-engine-upgrades), más adelante en este artículo.
>
> Para los nuevos trabajos de desarrollo, o en el caso de que una aplicación existente requiera el uso de características nuevas, como el [Procesamiento de consultas inteligentes](../../relational-databases/performance/intelligent-query-processing.md), así como algunos [!INCLUDE[tsql](../../includes/tsql-md.md)] nuevos, plantéese actualizar el nivel de compatibilidad de base de datos a la versión más reciente disponible en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y vuelva a certificar la aplicación para que funcione con ese nivel de compatibilidad. Para obtener más información sobre cómo actualizar el nivel de compatibilidad de base de datos, consulte [Prácticas recomendadas para actualizar el nivel de compatibilidad de base de datos](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#best-practices-for-upgrading-database-compatibility-level).
   
### <a name="why-query-plan-shape"></a><a name="queryplan_shape"></a> ¿Por qué es necesaria la forma del plan de consulta?      
La forma del plan de consulta hace referencia a la representación visual de los distintos operadores que componen un plan de consulta. Esto incluye operadores como búsquedas, exámenes, combinaciones y ordenaciones, así como las conexiones entre ellos que indican el flujo de datos y el orden de las operaciones que se deben ejecutar para conseguir el conjunto de resultados previsto. El optimizador de consultas determina la forma del plan de consulta.

Para que el rendimiento de las consultas sea predecible durante una actualización, uno de los objetivos fundamentales es asegurarse de que se usa la misma forma del plan de consulta. Esto se puede lograr si no se cambia el nivel de compatibilidad de la base de datos inmediatamente después de una actualización, aunque el [!INCLUDE[ssde_md](../../includes/ssde_md.md)] subyacente tenga versiones diferentes. Si no ha cambiado nada en el ecosistema de ejecución de consultas, como cambios significativos en los recursos disponibles o la distribución de datos en los datos subyacentes, el rendimiento de una consulta debe ser inalterable. 

Sin embargo, mantener la forma de un plan de consulta no es el único factor que puede afectar al rendimiento después de una actualización. Si mueve la base de datos a un [!INCLUDE[ssde_md](../../includes/ssde_md.md)] más reciente y también realiza cambios de entorno, es posible que esté introduciendo factores que influirán inmediatamente en el rendimiento de una consulta, incluso si el plan de consulta conserva la misma forma en las distintas versiones. Estos cambios ambientales pueden incluir el hecho de que el nuevo [!INCLUDE[ssde_md](../../includes/ssde_md.md)] tenga más o menos recursos de memoria y CPU disponibles, cambios en las opciones de configuración del servidor o de la base de datos, o cambios en la distribución de datos que afectan al modo en que se crea un plan de consulta. Esta es la razón por la que es importante comprender que mantener el nivel de compatibilidad de la base de datos protege frente a los cambios en la **forma** del plan de consulta, pero no ofrece protección contra otros aspectos ambientales que influyen en el rendimiento de las consultas, algunos de los cuales son cambios que el usuario inicia.

Para más información, vea la [Guía de arquitectura de procesamiento de consulta](../../relational-databases/query-processing-architecture-guide.md#optimizing-select-statements).
   
## <a name="compatibility-certification-benefits"></a>Ventajas de la certificación de compatibilidad
El uso de la certificación de bases de datos como enfoque basado en la compatibilidad, en lugar de un enfoque de versión con nombre, ofrece varias ventajas inmediatas:

-  **Desacople la certificación de la aplicación de la plataforma**. Debido a su recurso compartido [!INCLUDE[ssde_md](../../includes/ssde_md.md)], en el caso de las aplicaciones que solo necesitan ejecutar consultas de [!INCLUDE[tsql](../../includes/tsql-md.md)], no es necesario mantener procesos de certificación independientes para Azure y el entorno local.
-  **Reduzca los riesgos de las actualizaciones**, ya que, durante la modernización de la plataforma de las bases de datos, los ciclos de actualización del nivel de plataforma de las bases datos y la aplicación pueden separarse con menos interrupciones y una administración de cambios mejorada.
-  **Realice actualizaciones sin cambios en el código**. La actualización a una nueva versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] puede realizarse sin cambios en el código, manteniendo el mismo nivel de compatibilidad que el sistema de origen y sin la necesidad inmediata de volver a realizar la certificación hasta que la aplicación necesite sacar provecho de las mejoras que solo estén disponibles en un nivel de compatibilidad superior de la base de datos.
- **Mejore la capacidad de administración y escalabilidad** sin necesidad de realizar cambios en la aplicación, con mejoras que no se canalicen mediante el nivel de compatibilidad de la base de datos. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se incluyen, por ejemplo: 
  - Una supervisión enriquecida y mejoras en la solución de problemas, con nuevas [vistas de administración dinámica del sistema](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md), [eventos extendidos](../../relational-databases/extended-events/extended-events.md) y [ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md). 
  - Escalabilidad mejorada, por ejemplo, con [Soft-NUMA automática](../../database-engine/configure-windows/soft-numa-sql-server.md#automatic-soft-numa), [Recuperación acelerada de la base de datos](../../relational-databases/accelerated-database-recovery-concepts.md) o [Metadatos de tempdb optimizados para memoria](../../relational-databases/in-memory-database.md#memory-optimized-tempdb-metadata).

Las nuevas bases de datos todavía se establecen en el nivel de compatibilidad predeterminado de la versión de [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Sin embargo, cuando se mueve una base de datos de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a una nueva versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], la base de datos mantiene su nivel de compatibilidad existente. 

> [!IMPORTANT]
> Antes de mover una base de datos a una nueva versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], compruebe si todavía se admite el nivel de compatibilidad de la base de datos. La matriz de compatibilidad del nivel de compatibilidad de la base de datos se puede ver [aquí](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#arguments). 
>
> Actualizar una base de datos con un nivel de compatibilidad inferior al nivel permitido (por ejemplo, 90, que era el valor predeterminado en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]) hace que la base de datos se establezca automáticamente en el nivel de compatibilidad más bajo permitido (100).
>
> Para averiguar el nivel de compatibilidad actual, consulte la columna **compatibility_level** de [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

## <a name="compatibility-levels-and-database-engine-upgrades"></a>Actualizaciones del motor de base de datos y niveles de compatibilidad
Para actualizar [!INCLUDE[ssde_md](../../includes/ssde_md.md)] a la versión más reciente y mantener al mismo tiempo el nivel de compatibilidad de base de datos que existía antes de la actualización y su estado de compatibilidad, se recomienda realizar la **validación del área de superficie funcional estática** del código de aplicación **de la base de datos** (objetos de programación, como procedimientos almacenados, funciones, desencadenadores y demás) y **de la aplicación** (mediante un seguimiento de la carga de trabajo que captura el código dinámico enviado por la aplicación).

Esto se puede hacer fácilmente mediante el uso de la herramienta [Microsoft Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA). La ausencia de errores en la salida de la herramienta DMA, sobre funcionalidades incompatibles o ausentes, protege la aplicación de cualquier regresión funcional en la nueva versión de destino. Si se requieren cambios para asegurarse de que la base de datos funcione en la nueva versión, DMA le permitirá identificar dónde se necesitan los cambios y qué soluciones alternativas hay. Para obtener más información, consulte [Información general de Data Migration Assistant](../../dma/dma-overview.md).   

> [!TIP]
> Esta validación funcional es especialmente importante cuando se mueve una base de datos de una versión heredada (como [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] o [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) a una nueva versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], ya que el código de la aplicación puede estar usando una instancia de [!INCLUDE[tsql](../../includes/tsql-md.md)] descontinuada que no está protegida por el nivel de compatibilidad de base de datos. Pero, al pasar de una versión más reciente (como [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) a [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] o [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], no hay ninguna instancia de [!INCLUDE[tsql](../../includes/tsql-md.md)] descontinuada de la que preocuparse. Para obtener más información sobre las instancias de [!INCLUDE[tsql](../../includes/tsql-md.md)] descontinuadas, consulte [Uso del nivel de compatibilidad para la compatibilidad con versiones anteriores](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#backwardCompat).

> [!NOTE]
> DMA admite el nivel de compatibilidad de base de datos 100 y posterior. Queda excluido [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] como versión de origen.   

> [!IMPORTANT]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] recomienda que se lleven a cabo algunas pruebas mínimas para validar el éxito de una actualización, mientras se mantiene el nivel de compatibilidad de base de datos anterior. Debe determinar qué comprenderían estas pruebas mínimas para su aplicación y su escenario.   

> [!IMPORTANT]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] ofrece protección de formas de los planes de consulta cuando:
>
> - La nueva versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (destino) se ejecuta en un hardware comparable al hardware en el que se ejecutaba la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anterior (origen).
> - Se usa el mismo [nivel de compatibilidad de base de datos admitido](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#supported-dbcompats) tanto en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino como en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de origen.
> - La **misma** base de datos y la carga de trabajo se usan en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de origen. 
>
> Se corregirán todas las regresiones de forma de los planes de consulta (en comparación con el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de origen) que se produzcan en las condiciones anteriores. Póngase en contacto con el servicio de atención al cliente de Microsoft si es el caso.
  
## <a name="see-also"></a>Consulte también 
[Nivel de compatibilidad de ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)       
[Ver o cambiar el nivel de compatibilidad de una base de datos](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)       
[Prácticas recomendadas para actualizar el nivel de compatibilidad de base de datos](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#best-practices-for-upgrading-database-compatibility-level)
---
title: Opciones de Fin del soporte técnico
description: Obtenga información sobre las distintas opciones disponibles para los productos de SQL Server que han llegado al final del soporte técnico, como SQL Server 2005, SQL Server 2008 y SQL Server 2008 R2.
ms.date: 08/12/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c7560fbda04ef5a14c73ead8a7bbc8337e3e38ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352898"
---
# <a name="sql-server-end-of-support-options"></a>Opciones de Fin del soporte técnico de SQL Server 
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

En este artículo se explican las opciones que tiene para tratar con los productos de SQL Server que llegaron al final del soporte técnico.

## <a name="understanding-the-sql-server-lifecycle"></a>Descripción del ciclo de vida de SQL Server

Cada versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta con el respaldo de un soporte técnico de 10 años como mínimo, que incluye cinco años de soporte técnico estándar y cinco años de soporte técnico extendido:
-  El **soporte técnico estándar** incluye actualizaciones funcionales, de rendimiento, de escalabilidad y de seguridad. 
-  El **soporte técnico extendido** solo incluye actualizaciones de seguridad. 

El **Fin del soporte técnico** (que también se conoce como el fin de la vida útil) indica que un producto alcanzó el final de su ciclo de vida y que el servicio y el soporte técnico ya no están disponibles para él. Para más información sobre el ciclo de vida de Microsoft, consulte la [directiva del ciclo de vida de Microsoft](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy).



## <a name="options"></a>Opciones

Una vez que su [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] haya llegado a la fase de finalización del soporte técnico, puede elegir:
- Actualice a una versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Compre una [suscripción a las Actualizaciones de seguridad extendidas](https://www.microsoft.com/cloud-platform/extended-security-updates). 
- Migre su carga de trabajo a una máquina virtual de Azure tal como está para obtener las [Actualizaciones de seguridad extendidas gratuitas](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support).
- Migre la carga de trabajo a un [servicio de Azure SQL Database](/azure/sql-database/sql-database-paas-vs-sql-server-iaas). 

Si quiere más información, orientación y herramientas para planear y automatizar la actualización o migración, consulte [Fin del soporte de SQL Server 2005](https://www.microsoft.com/sql-server/sql-server-2005) y [Fin del soporte de SQL Server 2008](https://www.microsoft.com/cloud-platform/windows-sql-server-2008).  

![Opciones de Fin del soporte técnico](media/sql-server-end-of-life-overview/sql-server-upgrade-paths.png)

En este artículo se describen las ventajas y consideraciones de cada enfoque, así como los recursos adicionales que pueden ayudar a guiar el proceso de toma de decisiones.

## <a name="upgrade-sql-server"></a>Actualizar SQL Server

Una vez que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha alcanzado el final del soporte técnico, puede optar por actualizar a una versión más reciente y compatible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esto le ofrece coherencia con el entorno, le permite usar el conjunto de características más reciente y adopta el ciclo de vida de soporte técnico de la versión nueva.

### <a name="benefits"></a>Ventajas
- **La tecnología más reciente**: las versiones nuevas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] presentan innovaciones que incluyen características de rendimiento, escalabilidad y alta disponibilidad, además de una seguridad mejorada. 
- **Control**: tiene el máximo control sobre las características y la escalabilidad, ya que administra tanto hardware como software.
- **Entorno familiar**: si va a actualizar desde una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], este es el entorno más parecido.
- **Aplicabilidad amplia**: se aplica a aplicaciones de bases de datos de cualquier tipo, incluidos los sistemas OLTP y el almacenamiento de datos.
- **Riesgo bajo para las aplicaciones de bases de datos**: al mantener la compatibilidad con la base de datos en el mismo nivel que el sistema heredado, las aplicaciones de bases de datos existentes están protegidas contra cambios funcionales y de rendimiento que pueden tener efectos perjudiciales. Una aplicación solo necesita que se vuelva a certificar completamente cuando necesita usar características que se han validado con una configuración de compatibilidad de base de datos más reciente. Para obtener más información, vea [Certificación de compatibilidad](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Consideraciones

- **Costo**: este enfoque requiere la mayor inversión inicial y la administración más continua. Tiene que comprar, mantener y administrar su propio hardware y software.
- **Tiempo de inactividad**: podría haber tiempo de inactividad en función de la estrategia de actualización. También hay un riesgo inherente de encontrarse con problemas durante un proceso de actualización local.
- **Complejidad**: si ejecuta Windows Server 2008 o [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)], también deberá actualizar el sistema operativo porque es posible que las versiones más recientes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no sean compatibles en esas versiones de Windows. Existe un riesgo adicional durante el proceso de actualización del sistema operativo, por lo que realizar una migración en paralelo puede ser el enfoque más prudente, pero más costoso. No se admiten las actualizaciones locales del sistema operativo en instancias de clúster de conmutación por error para Windows Server 2008 o [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)]. 

  > [!NOTE]
  > Las actualizaciones graduales del sistema operativo de clúster están disponibles a partir de Windows°Server 2016.

### <a name="resources"></a>Recursos

[Soporte de instalación](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm)   
[Actualización de SQL Server con el Asistente para instalación](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

Novedades de:
- [SQL Server 2016](../what-s-new-in-sql-server-2016.md)
- [SQL Server 2017](../what-s-new-in-sql-server-2017.md) 
- [SQL Server 2019](../what-s-new-in-sql-server-ver15.md)   

Requisitos de hardware:
- [SQL Server 2017 y versiones anteriores](../install/hardware-and-software-requirements-for-installing-sql-server.md)  
- [SQL Server 2019](../install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)    

Actualizaciones de ediciones y versiones admitidas:
- [SQL Server 2016](../../database-engine/install-windows/supported-version-and-edition-upgrades.md?view=sql-server-2016&preserve-view=true) 
- [SQL Server 2017](../../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md)
- [SQL Server 2019](../../database-engine/install-windows/supported-version-and-edition-upgrades-version-15.md)

Herramientas:
-  El [Asistente para experimentación con bases de datos](../../dea/database-experimentation-assistant-overview.md) puede ayudar a evaluar la versión de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para una carga de trabajo específica. 
-  [Data Migration Assistant](../../dma/dma-overview.md) puede ayudar a detectar problemas de compatibilidad que pueden afectar la funcionalidad de la base de datos en la versión nueva de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
-  El [Asistente para la optimización de consultas](../../relational-databases/performance/upgrade-dbcompat-using-qta.md) puede ayudar a optimizar las cargas de trabajo que pueden experimentar efectos negativos al actualizar la compatibilidad de la base de datos.

En la imagen siguiente se ofrece un ejemplo de innovación sobre las distintas versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a lo largo de los años: 

![25 años de innovación de SQL Server](media/sql-server-end-of-life-overview/sql-server-version-improvements.png)

## <a name="extend-support"></a>Soporte técnico extendido 

Si no está listo para la actualización ni para migrar a la nube, tiene la posibilidad de comprar una suscripción a las Actualizaciones de seguridad extendidas para recibir actualizaciones de seguridad **críticas** hasta por tres años a partir de la fecha de finalización del soporte técnico.  

### <a name="benefits"></a>Ventajas 

- **Compatibilidad con la aplicación**: esta es la mejor opción si la aplicación requiere una certificación nueva en una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esto es común para las aplicaciones que no usan la [certificación de compatibilidad](../../database-engine/install-windows/compatibility-certification.md). 
- **Infraestructura coherente**: no es necesario cambiar la infraestructura de ninguna manera. 
- **Soporte técnico**: si tiene Software Assurance u otro plan de soporte técnico, puede seguir recibiendo soporte técnico de [!INCLUDE[msCoName](../../includes/msconame-md.md)] para el producto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el que llegó el fin del soporte técnico. Esta es la única manera de obtener soporte técnico para [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]. 
- **Time**: esta opción está disponible durante tres años, lo que ofrece un tiempo adicional para certificar sus aplicaciones. 

### <a name="considerations"></a>Consideraciones 

- **Disponibilidad limitada**: esta opción solo está disponible para los clientes que tienen Software Assurance o licencias de suscripción. 
- **Costo**: esta opción puede resultar costosa, ya que las Actualizaciones de seguridad extendidas representan aproximadamente el 75 % del costo de licencias locales cada año.
- **Período limitado**: esta opción solo está disponible durante tres años, por lo que tendrá que actualizar o migrar al final del período de tres años si quiere garantizar la seguridad y el cumplimiento.
- **Sin correcciones de errores**: si encuentra un error que no es de seguridad con el producto, [!INCLUDE[msCoName](../../includes/msconame-md.md)] no publicará ninguna reparación para él. 
- **Compatibilidad limitada**: las Actualizaciones de seguridad extendidas no incluyen características nuevas, mejoras funcionales ni correcciones solicitadas por el cliente. Las correcciones de seguridad se limitan a las clasificadas como críticas por el [Centro de respuestas de seguridad de Microsoft (MSRC)](https://portal.msrc.microsoft.com/).

### <a name="resources"></a>Recursos

[Información general sobre las Actualizaciones de seguridad extendidas (ESU)](sql-server-extended-security-updates.md)       
[Preguntas más frecuentes detalladas sobre las Actualizaciones de seguridad extendidas](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[Soporte técnico extendido gratis mediante la migración tal cual a una máquina virtual de Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)       
[Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default)       

## <a name="azure-virtual-machine"></a>Máquina virtual de Azure

Otra opción consiste en migrar la carga de trabajo a una [máquina virtual de Azure que ejecute SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview). Puede migrar el sistema tal cual y mantener el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de fin del soporte técnico, o bien puede actualizar a una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta es la mejor opción para las migraciones y las aplicaciones que requieren acceso a nivel de sistema operativo. Las máquinas virtuales [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] están listas para las migraciones mediante lift-and-shift para aplicaciones existentes que requieren una rápida migración a la nube con mínimos cambios o ninguno. 

### <a name="benefits"></a>Ventajas

- **Actualizaciones de seguridad extendidas gratuitas**: si elige mantener [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tal cual con el uso de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] o [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)], puede obtener las Actualizaciones de seguridad extendidas gratis durante tres años a contar de la fecha de finalización del soporte técnico, incluso si no tiene Software Assurance. 
- **Ahorro de costos**: puede ahorrar el costo de software de servidor y hardware, porque solo paga el uso por hora. 
- **Migración mediante lift-and-shift**: puede migrar mediante lift-and-shift [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y la infraestructura de la aplicación a la nube con un mínimo de cambios o sin cambio alguno. 
- **Entorno hospedado**: obtendrá las ventajas de un entorno hospedado, como la descarga de hardware y el mantenimiento de software. 
- **Automatización**: si usa [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)] y versiones posteriores, tendrá la ventaja de aplicar revisiones de manera automatizada, además de crear copias de seguridad automatizadas. 
- **Control del sistema operativo**: tiene control sobre el entorno del sistema operativo, pero con el conjunto de características conocido de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Implementación rápida**: puede realizar la implementación rápidamente desde una biblioteca de imágenes de máquina virtual. 
- **Movilidad de licencias**: puede llevar su licencia, lo que le permite reducir el costo operativo. 
- **Alta disponibilidad**: no solo se beneficia de la disponibilidad de máquinas virtuales integradas en la infraestructura de Azure que incluye una disponibilidad del 99,99 %, sino que también puede aprovechar las ventajas de las opciones de alta disponibilidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], como las instancias de clúster de conmutación por error y los grupos de disponibilidad Always On. 
- **Riesgo bajo para las aplicaciones de bases de datos**: al mantener la compatibilidad con la base de datos en el mismo nivel que las bases de datos heredadas, las aplicaciones de bases de datos existentes están protegidas contra cambios funcionales y de rendimiento que pueden tener efectos perjudiciales. Una aplicación solo necesita que se vuelva a certificar completamente cuando necesita usar características que se han validado con una configuración de compatibilidad de base de datos más reciente. Para obtener más información, vea [Certificación de compatibilidad](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Consideraciones

- **Capacidad de administración**: seguirá teniendo que administrar tanto el software del sistema operativo como [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Redes**: debe configurar la máquina virtual para que se integre con la infraestructura de Active Directory y red, lo que agrega un nivel de complejidad adicional. 
- **FCI de almacenamiento compartido**: las máquinas virtuales de Azure solo admiten instancias de clúster de conmutación por error mediante Espacios de almacenamiento directo o Recursos compartidos de archivos Premium y no admiten una instancia de clúster de conmutación por error que use almacenamiento compartido. Por lo tanto, las máquinas virtuales de Azure solo admiten instancias de clúster de conmutación por error cuando se usa Windows Server 2012 o posterior.
- **Tiempo de inactividad de escalabilidad**: hay tiempo de inactividad al cambiar los recursos de CPU y de almacenamiento. 
- **Limitación del tamaño**: si bien la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede admitir tantas bases de datos como sea necesario, el total acumulado de todas las bases de datos para una instancia única de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es 256 TB en lugar de 524 PB para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] local. 

### <a name="resources"></a>Recursos

[Información general sobre VM con SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)       
[Elección de la opción de Azure SQL](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Migración de SQL Server a una máquina virtual de Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql)       
[Actualizaciones de seguridad extendidas (ESU) gratuitas para migrar a Azure tal cual](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)       
[Información general sobre las Actualizaciones de seguridad extendidas (ESU)](sql-server-extended-security-updates.md)       
[Preguntas más frecuentes detalladas sobre las Actualizaciones de seguridad extendidas](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[Aplicación de revisiones automatizada en máquinas virtuales de SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)       
[Copia de seguridad automatizada de máquinas virtuales de SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2)       
[Alta disponibilidad de máquinas virtuales de SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr)       
[Preguntas más frecuentes sobre máquinas virtuales de SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-faq)       

## <a name="azure-sql-database-single-database"></a>Base de datos única de Azure SQL Database

Si desea descargar el mantenimiento, reducir los costos y eliminar la necesidad de actualizaciones en el futuro, puede trasladar la carga de trabajo a una [base de datos única de Azure SQL Database](/azure/sql-database/sql-database-single-database). Esta es la mejor opción para las aplicaciones en la nube modernas que pretenden usar las características estables de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] más recientes y que tienen restricciones de tiempo en las actividades de desarrollo y marketing. 

### <a name="benefits"></a>Ventajas

- **Costo**: la base de datos única puede ser rentable, ya que se descargan hardware, software y el mantenimiento y es posible pagar el uso por segundo o por hora. 
- **Flexibilidad**: la base de datos única se adapta perfectamente a las aplicaciones diseñadas para la nube cuando la productividad del desarrollador y un tiempo de comercialización rápido para las soluciones son críticos, o que deben exigir acceso externo.  
- **Características comunes**: están disponibles las características de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] más usadas, pero no tantas como para SQL Managed Instance.  
- **Implementación rápida**: puede implementar rápidamente una base de datos única. 
- **Escalabilidad**: puede escalar y reducir verticalmente de manera rápida y sencilla según sea necesario para su negocio, lo que proporciona ventajas adicionales para el ahorro de costos. 
- **Disponibilidad**: el costo del servicio incluye almacenamiento y alta disponibilidad, con una disponibilidad garantizada del 99,995 %.  
- **Automatización**: la aplicación de revisiones y las copias de seguridad se producen automáticamente, lo que ahorra tiempo de mantenimiento valioso.  
- **Intelligent Insights**: obtenga información sobre el rendimiento de la base de datos con el análisis de inteligencia integrado.  
- **Sin versión**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no tiene versión, lo que significa que siempre tiene la versión más reciente y no se tiene que preocupar nunca por las actualizaciones o el tiempo de inactividad. Además, siempre está totalmente al día y las características estables más recientes se publican primero en la nube.
- **Riesgo bajo para las aplicaciones de bases de datos**: al mantener la compatibilidad con la base de datos en el mismo nivel que la base de datos local, las aplicaciones de existentes están protegidas contra cambios funcionales y de rendimiento que pueden tener efectos perjudiciales. Una aplicación solo necesita que se vuelva a certificar completamente cuando necesita usar características que se han validado con una configuración de compatibilidad de base de datos más reciente. Para obtener más información, vea [Certificación de compatibilidad](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Consideraciones

- **Opciones limitadas de migración**:  solo se puede migrar una base de datos única a la vez, en lugar de una instancia completa.   
- **Limitación de características**:  aunque están disponibles las características de Azure SQL Database más utilizadas, el conjunto de características para una base de datos única no es tan completo como para Azure SQL Managed Instance o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Diferencias de Transact-SQL**:  hay algunas diferencias de [!INCLUDE[tsql](../../includes/tsql-md.md)] (T-SQL) entre una base de datos única y una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] local. 
- **Limitaciones de tamaño**:  una base de datos única tiene un tamaño máximo de base de datos de 100 TB, en comparación con un tamaño de 524 PB para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Tiempo de mantenimiento**: no hay ninguna garantía con respecto al tiempo de mantenimiento exacto, aunque es casi transparente. 

### <a name="resources"></a>Recursos

[Información general sobre Azure SQL Database](/azure/sql-database/sql-database-technical-overview)       
[Elección de la opción de Azure SQL](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Comparación de las características de SQL Database](/azure/sql-database/sql-database-features)       
[Migración de SQL Server a una base de datos única](/azure/sql-database/sql-database-single-database-migrate)       
[Proceso de migración más amplio](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)       
[Diferencias de T-SQL de la base de datos única](/azure/sql-database/sql-database-transact-sql-information)       
Límites de recursos de [núcleo virtual](/azure/sql-database/sql-database-vcore-resource-limits-single-databases) y [DTU](/azure/sql-database/sql-database-dtu-resource-limits-single-databases)       
[Intelligent Insights](/azure/sql-database/sql-database-intelligent-insights)       

Herramientas:
- [Data Migration Assistant](../../dma/dma-overview.md)
- [Database Migration Service](/azure/dms/dms-overview)

## <a name="sql-managed-instance"></a>Instancia administrada de SQL

Si quiere aprovechar las ventajas de reducir el mantenimiento y el costo, pero encuentra que el conjunto de características de una única base de datos de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] es demasiado restrictivo, puede migrar a [SQL Managed Instance](/azure/sql-database/sql-database-managed-instance). Una instancia administrada es muy similar a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] local, sin tener que preocuparse de cosas como los errores de hardware o la aplicación de revisiones. Managed Instance es una colección de bases de datos de usuario y del sistema con un conjunto compartido de recursos que está listo para migraciones mediante lift-and-shift y se puede usar para la mayoría de las migraciones a la nube. Esta es la mejor opción para aplicaciones nuevas o aplicaciones locales existentes que pretenden usar las características estables de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] más recientes y que se han migrado a la nube con cambios mínimos. 

### <a name="benefits"></a>Ventajas

- **Costo**: puede ahorrar costos si descarga el mantenimiento de software y el hardware.  
- **Migración mediante lift-and-shift**: puede migrar mediante lift-and-shift toda su instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] local a una instancia administrada, incluidas todas las bases de datos con un mínimo o nada de cambios en la base de datos. 
- **Características**: el conjunto de características de una instancia administrada coincide estrechamente con el de una instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], como las consultas entre bases de datos, la publicación y distribución de la replicación transaccional, la programación de trabajos de SQL y la compatibilidad con CLR. 
- **Escalabilidad**: todas las bases de datos de una instancia administrada comparten recursos y es posible escalar y reducir verticalmente en cualquier momento sin tiempo de inactividad.   
- **Automatización**: la aplicación de revisiones y las copias de seguridad se producen automáticamente, lo que ahorra tiempo de mantenimiento valioso.  
- **Disponibilidad**: el costo del servicio incluye almacenamiento y alta disponibilidad, con una disponibilidad garantizada del 99,99 %.  
- **Intelligent Insights**: obtenga información sobre el rendimiento de las bases de datos con el análisis de inteligencia integrado.  
- **Sin versión**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no tiene versión, lo que significa que siempre tiene la versión más reciente y no se tiene que preocupar nunca por las actualizaciones o el tiempo de inactividad. Además, siempre está totalmente al día y las características estables más recientes se publican primero en la nube.
- **Riesgo bajo para las aplicaciones de bases de datos**: al mantener la compatibilidad con la base de datos en el mismo nivel que las bases de datos locales, las aplicaciones de bases de datos existentes están protegidas contra cambios funcionales y de rendimiento que pueden tener efectos perjudiciales. Una aplicación solo necesita que se vuelva a certificar completamente cuando necesita usar características que se han validado con una configuración de compatibilidad de base de datos más reciente. Para obtener más información, vea [Certificación de compatibilidad](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Consideraciones

- **Costo**: La opción de instancia administrada puede ser más costosa que la opción de base de datos única.  
- **Diferencias de Transact-SQL**: hay algunas diferencias de [!INCLUDE[tsql](../../includes/tsql-md.md)] (T-SQL) entre una base de datos única y una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] local.  
- **Implementación**:  la implementación de una instancia administrada puede tardar más tiempo que una base de datos única.  
- **Limitación de características**: aunque una instancia administrada comparte la mayoría de las características con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sigue habiendo algunas características que no son compatibles. 
- **Limitación del tamaño**: el tamaño de almacenamiento combinado de todas las bases de datos dentro de una instancia administrada se limita a 8 TB, en lugar de 524 PB para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] local.  
- **Redes**: Los requisitos de red para una instancia administrada agregan una capa de complejidad adicional a la infraestructura y requieren Azure ExpressRoute o VPN Gateway.
- **Tiempo de mantenimiento**: no hay ninguna garantía con respecto al tiempo de mantenimiento exacto, aunque es casi transparente. 

### <a name="resources"></a>Recursos

[Introducción a SQL Managed Instance](/azure/sql-database/sql-database-managed-instance)       
[Elección de la opción de Azure SQL](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Comparación de las características de SQL Database](/azure/sql-database/sql-database-features)       
[Migración de SQL Server a Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-migrate)       
[Proceso de migración más amplio](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)       

Herramientas:
- [Data Migration Assistant](../../dma/dma-overview.md)
- [Database Migration Service](/azure/dms/dms-overview)

## <a name="non-sql-options"></a>Opciones que no son de SQL

En el caso de ciertos tipos de aplicaciones, es posible que también considere la posibilidad de usar una solución no relacional o NoSQL, como Azure Cosmos DB o Azure Table Storage.

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Considere Azure Cosmos DB para aplicaciones web, móviles, modernas y escalables que usan datos JSON y requieren una combinación de consultas sólidas y procesamiento de datos transaccionales. Para más información, vea [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Para más información sobre cómo importar datos, vea [Importación de datos en Azure Cosmos DB](/azure/cosmos-db/import-data/).

Azure Cosmos DB tiene estas ventajas:
- Los documentos se indizan y se puede usar la conocida sintaxis SQL para consultarlos.
- La base de datos no tiene esquema.
- Puede agregar propiedades a los documentos sin tener que volver a generar índices.
- Obtiene compatibilidad con JSON y JavaScript directamente del motor de la base de datos.
- Obtiene soporte nativo para datos geoespaciales e integración con otros servicios de Azure, incluidas la Búsqueda de Azure, HDInsight y la Factoría de datos.
- Obtiene una latencia baja y almacenamiento de alto rendimiento con niveles de rendimiento reservados.

### <a name="azure-table-storage"></a>Almacenamiento de tablas de Azure

Tenga en cuenta Azure Table Storage para almacenar petabytes de datos semiestructurados en una solución rentable. Para obtener más información, consulte [Almacenamiento de tablas](https://azure.microsoft.com/services/storage/tables/).

Azure Table Storage tiene estas ventajas:
- Pueden desarrollar las aplicaciones y el esquema de tabla sin tener que desconectar los datos.
- Puede escalar sin tener que realizar un particionamiento del conjunto de datos.
- Obtiene almacenamiento con redundancia geográfica que replica los datos en varias regiones.

## <a name="lifecycle-dates"></a>Fechas del ciclo de vida

En la tabla siguiente se proporciona una aproximación de las fechas del ciclo de vida de los productos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más detalles y precisión, consulte la página de la [directiva del ciclo de vida de Microsoft](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy). 

| **Versión**     | **Año de la versión** | **Año final del soporte técnico estándar** | **Año final del soporte técnico extendido** |
| :---------------| :--------------- | :------------------------------ | :---------------------------- |
| [SQL Server 2019](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202019) | 2019 | 2025 | 2030 |
| [SQL Server 2017](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202017) | 2017 | 2022 | 2027 |
| [SQL Server 2016](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202016) | 2016 | 2021 | 2026 |
| [SQL Server 2014](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202014) | 2014 | 2019 | 2024 |
| [SQL Server 2012](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202012) | 2012 | 2017 | 2022 |
| [SQL Server 2008 R2](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008%20R2) | 2010 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2008](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008) | 2008 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2005](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202005) | 2006 | 2011 | [2016](https://www.microsoft.com/sql-server/sql-server-2005) |
| [SQL Server 2000](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202000) | 2000 | 2005 | [2013](/archive/blogs/cdnitmanagers/sql-server-2000-end-of-support-april-2013) |

> [!IMPORTANT]
> Si existe alguna discrepancia entre esta tabla y la página del ciclo de vida de [!INCLUDE[msCoName](../../includes/msconame-md.md)], el ciclo de vida de [!INCLUDE[msCoName](../../includes/msconame-md.md)] sustituye esta tabla, porque está pensada para usarla como referencia aproximada.  

## <a name="next-steps"></a>Pasos siguientes  
[SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019)   
[Finalización del soporte de SQL Server 2005](https://www.microsoft.com/sql-server/sql-server-2005)   
[Fin del soporte técnico de SQL Server 2008](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)   
[Información general sobre las Actualizaciones de seguridad extendidas (ESU)](sql-server-extended-security-updates.md)   
[Actualizaciones de seguridad extendidas (ESU) gratuitas para migrar a Azure tal cual](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)   
[Información general sobre VM con SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)   
[Información general sobre Azure SQL Database](/azure/sql-database/sql-database-technical-overview)
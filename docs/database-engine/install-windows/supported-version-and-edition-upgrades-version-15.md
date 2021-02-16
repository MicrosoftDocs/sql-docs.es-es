---
title: Actualizaciones de ediciones y versiones admitidas (SQL Server 2019)
description: Las actualizaciones de ediciones y versiones admitidas de SQL Server 2019.
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server], adding to existing installations
- versions [SQL Server], upgrading
- upgrading SQL Server, upgrades supported
- cross-language support
ms.assetid: 702359c4-6ca9-42a8-860c-a95a802898a1
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: f2ba32f7f3c8defa21fdb8b035fc96e64a023159
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341812"
---
# <a name="supported-version--edition-upgrades-sql-server-2019"></a>Actualizaciones de ediciones y versiones admitidas (SQL Server 2019)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  Puede actualizar de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]y [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]. En este artículo se enumeran las rutas de actualización admitidas desde estas versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y las actualizaciones de edición admitidas para [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)].  
  
## <a name="pre-upgrade-checklist"></a>Lista de comprobación previa a la actualización  

- Antes de actualizar una edición de [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)], compruebe que las funciones que utiliza se admiten en la edición a la que va a cambiar.  
- Compruebe la compatibilidad de [hardware y software](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md).
- Antes de actualizar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], habilite la autenticación de Windows para el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y compruebe la configuración predeterminada: la cuenta de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe ser miembro del grupo sysadmin de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Para actualizar a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)], debe estar ejecutando un sistema operativo admitido. Para más información, vea [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md).  
- La actualización se bloqueará si hay un reinicio pendiente.  
- La actualización se bloqueará si el servicio de Windows Installer no se está ejecutando.

## <a name="unsupported-scenarios"></a>Escenarios no admitidos

- No se admiten instancias de [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] de distintas versiones. Los números de versión de los componentes de [!INCLUDE[ssDE](../../includes/ssde-md.md)] deben ser los mismos en una instancia de [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)].  
  
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] solo está disponible para plataformas de 64 bits. No se admite la actualización entre plataformas. No puede actualizar una instancia de 32 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a 64 bits nativo con el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sin embargo, puede realizar copias de seguridad o separar bases de datos de una instancia de 32 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, después, si no están publicadas en replicación, restaurarlas o anexarlas a una nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (64 bits). Debe volver a crear los inicios de sesión y los demás objetos de usuario en las bases de datos del sistema maestra, msdb y modelo.  
  
- No puede agregar características nuevas durante la actualización de una instancia existente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Después de actualizar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)], puede agregar características mediante el programa de instalación de [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]. Para más información, vea [Agregar características a una instancia de SQL Server &#40;programa de instalación&#41;](./add-features-to-an-instance-of-sql-server-setup.md).  
 
## <a name="upgrades-from-earlier-versions-to-sssql19-md"></a>Actualizaciones de versiones anteriores a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]  
 
[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] admite la actualización de las siguientes versiones de SQL Server:

- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 o posterior
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP3 o posterior
- [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] SP2 o posterior
- [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]

En la siguiente tabla se indican los escenarios de actualización de versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]admitidos.  
  
|Actualización de|Ruta de actualización admitida|  
|:------|:------|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Enterprise|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Developer|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/>[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Standard|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Web|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Express |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express <br/> <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Business Intelligence|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4 Evaluation|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Enterprise|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Developer|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Standard|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Web|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Express |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Business Intelligence|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 Evaluation|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Enterprise|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Developer|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Standard|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Web|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Express |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Business Intelligence|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 13.0.1601.5 Evaluation|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Enterprise|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Developer|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Standard|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Web|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Express |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Business Intelligence|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/> |  
|[!INCLUDE[sssql17](../../includes/sssql17-md.md)] Evaluation|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Evaluation <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer|
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] versión candidata para lanzamiento* |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise |  
|[!INCLUDE[sssqlv15_md](../../includes/sssql19-md.md)] Developer |[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise | 

 \* La compatibilidad de Microsoft para actualizar del software de la versión candidata para lanzamiento está destinado específicamente a los clientes que han participado en el Programa de usuarios pioneros.

## <a name="migrate-to-sql-server-2019"></a>Migración a SQL Server 2019

Puede migrar bases de datos de versiones anteriores. Por ejemplo, puede migrar las bases de datos de [!INCLUDE [sskilimanjaro-md](../../includes/sskilimanjaro-md.md)] a SQL Server 2019.

Para obtener más información, consulte la [guía de migración de Azure Database](https://datamigration.microsoft.com/scenario/sql-to-sqlserver).

Los consejos y herramientas siguientes le ayudarán a planear e implementar la migración.

- Herramientas de migración: la migración se admite a través de [Data Migration Assistant (DMA)](../../dma/dma-overview.md).
- Copias de seguridad y restauración: las copias de seguridad realizadas en SQL Server 2008 o SQL Server 2008 R2 se pueden restaurar en SQL Server 2019.
- Trasvase de registros: se admite el trasvase de registros si la instancia principal ejecuta SQL Server 2008 SP3 o una versión posterior, o bien SQL Server 2008 R2 SP2 o una versión posterior, y si la instancia secundaria ejecuta SQL Server 2019. 

   > [!WARNING]
   > Si se produce una conmutación por error manual o automática y la instancia de SQL Server 2019 se convierte en la principal, SQL Server 2008 o SQL Server 2008 R2 se convierte en la instancia secundaria y no puede recibir los cambios de la principal.

- Carga masiva: Las tablas se pueden copiar de forma masiva de SQL Server 2008 o SQL Server 2008 R2 a SQL Server 2019.

## <a name="sssql19-md-edition-upgrade"></a>Actualización de la edición de [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] 

En la tabla siguiente se enumeran los escenarios de actualización de la edición de [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] admitidos.  

Para obtener instrucciones paso a paso sobre cómo realizar una actualización de la edición, vea [Actualizar a una edición diferente de SQL Server &#40;programa de instalación&#41;](../../database-engine/install-windows/upgrade-to-a-different-edition-of-sql-server-setup.md).  
  
|Actualización de|Actualización a|  
|------------------|----------------|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (Server+CAL y Core)**|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise |  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Evaluation Enterprise**|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Server+CAL o Core) <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> La actualización de Evaluation (una edición gratuita) a cualquiera de las ediciones de pago se admite para las instalaciones independientes, pero no para las instalaciones en clúster. Esta limitación no se aplica a las instancias independientes instaladas en un clúster de conmutación por error de Windows que participa en un grupo de disponibilidad. |  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard**|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Server+CAL o Core)|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer**|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Server+CAL o Core) <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Server+CAL o Core) <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express*|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Server+CAL o Core) <br/><br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard <br/> <br/> [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Web|  
  
 Además, también puede realizar una actualización de la edición entre [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Server+CAL) y [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Core):  
  
|Actualización de la edición desde|Actualización de la edición a|  
|--------------------------|------------------------|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Server+CAL)**|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Core)|  
|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Core)|[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise (licencia Server+CAL)|  
  
 \* También se aplica a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express with Tools y [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Express con Advanced Services.  
  
 ** El cambio de la edición de una instancia en clúster de [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] está limitado. Los escenarios siguientes no se admiten en los clústeres de conmutación por error de [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]:  
  
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Enterprise a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer, Standard o Evaluation.  
  
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Developer a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard o Evaluation.  
  
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Evaluation.  
  
- [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Evaluation a [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] Standard.  
  
## <a name="see-also"></a>Consulte también  

 [Ediciones y características admitidas de [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)]](../../sql-server/editions-and-components-of-sql-server-version-15.md)

 [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)

 [Actualizar SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)
---
title: Actualización del trasvase de registros a SQL Server 2016 y versiones posteriores
description: Conozca el orden adecuado para conservar la solución de recuperación ante desastres de trasvase de registros al actualizar a SQL Server 2016 y versiones posteriores desde una versión anterior.
ms.custom: seo-lt-2019
ms.date: 02/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], upgrading
ms.assetid: b1289cc3-f5be-40bb-8801-0e3eed40336e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8682cc0d93a279c9a14a04d767a52af7b66ed955
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346919"
---
# <a name="upgrading-log-shipping-to-sql-server-2016-transact-sql"></a>Actualización del trasvase de registros a SQL Server 2016 (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Para conservar la solución de recuperación ante desastres de trasvase de registros, actualice o aplique actualizaciones de servicio en el orden adecuado. Las actualizaciones de servicio incluyen Service Pack o actualizaciones acumulativas.  
  
> [!NOTE]  
> La [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md) se incluyó en [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)]. Una configuración de trasvase de registros actualizada usa la opción de configuración de nivel de seguridad **Compresión de copia de seguridad predeterminada** para controlar si se emplea la compresión de copia de seguridad para los archivos de copia de seguridad del registro de transacciones. El comportamiento de la compresión de las copias de seguridad de registros se puede especificar para cada configuración de trasvase de registros. Para obtener más información, vea [Configurar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md).  
  
 **En este tema:**  
  
-   [Requisitos previos](#Prerequisites)  
  
-   [Proteger los datos antes de la actualización](#ProtectData)  
  
-   [Actualizar la instancia del servidor de supervisión](#UpgradeMonitor)  
  
-   [Actualización de instancias del servidor secundario](#UpgradeSecondaries)  
  
-   [Actualización de la instancia principal](#UpgradePrimary)  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
 Antes de empezar, revise la siguiente información importante:  
  
-   [Actualizaciones de ediciones y versiones admitidas](../../database-engine/install-windows/supported-version-and-edition-upgrades.md): compruebe que puede actualizar a SQL Server 2016 desde su versión del sistema operativo Windows y la versión de SQL Server. Por ejemplo, no puede actualizar directamente desde una instancia de SQL Server 2005 a [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)].  
  
-   [Elegir un método de actualización del motor de base de datos](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md): seleccione el método y los pasos de actualización adecuados en función de la revisión de versiones admitidas y actualizaciones de ediciones, y también teniendo en cuenta otros componentes instalados en el entorno con el fin de actualizar los componentes en el orden correcto.  
  
-   [Planeación y prueba del plan de actualización del motor de base de datos](../../database-engine/install-windows/plan-and-test-the-database-engine-upgrade-plan.md): revise las notas de la versión y los problemas conocidos de actualización, así como la lista de comprobación previa a la actualización, y desarrolle y pruebe el plan de actualización.  
  
-   [Requisitos de hardware y software para instalar SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md):  revise los requisitos de software para instalar [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Si se requiere software adicional, puede instalarlo en cada nodo antes de comenzar el proceso de actualización para reducir los posibles tiempos de inactividad.  
  
##  <a name="protect-your-data-before-the-upgrade"></a><a name="ProtectData"></a> Proteger los datos antes de la actualización  
 Como práctica recomendada, es aconsejable que proteja sus datos antes de realizar una actualización del trasvase de registros.  
  
 **Para proteger los datos**  
  
1.  Realice una copia de seguridad completa de cada base de datos principal.  
  
     Para obtener más información, vea [Crear una copia de seguridad completa de base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
2.  Ejecute el comando [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) en cada base de datos principal.  
  
> [!IMPORTANT]  
>  Asegúrese de que haya espacio suficiente en el servidor principal para almacenar las copias de seguridad del registro durante el tiempo previsto que vaya a durar la actualización de los servidores secundarios.  Si conmuta por error a un elemento secundario, este punto se aplica también al secundario (el nuevo elemento principal).  
  
##  <a name="upgrading-the-optional-monitor-server-instance"></a><a name="UpgradeMonitor"></a> Actualización de la instancia del servidor de supervisión (opcional)  
 La instancia del servidor de supervisión, si existe, se puede actualizar en cualquier momento. Sin embargo, no es necesario actualizar el servidor de supervisión opcional al actualizar los servidores principal y secundarios.  
  
 Mientras se actualiza el servidor de supervisión, la configuración de trasvase de registros continúa funcionando, pero su estado no se registra en las tablas del monitor. Cualquier alerta que se haya configurado no se desencadenará mientras el servidor de supervisión se esté actualizando. Después de la actualización, puede actualizar la información de las tablas del monitor ejecutando el procedimiento almacenado del sistema [sp_refresh_log_shipping_monitor](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md).   Para obtener más información sobre el servidor de supervisión, vea [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md).  
  
##  <a name="upgrading-the-secondary-server-instances"></a><a name="UpgradeSecondaries"></a> Actualización de instancias del servidor secundario  

El proceso de actualización implica actualizar las instancias de servidor secundario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antes de actualizar la instancia de servidor principal. Actualice siempre las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] secundario en primer lugar. El trasvase de registros continúa a lo largo del proceso de actualización porque las instancias de servidor secundario actualizadas continúan restaurando las copias de seguridad de registros a partir de la instancia de servidor principal. Si la instancia del servidor principal se actualizara antes que la instancia de un servidor secundario, se produciría un error en el trasvase de registros porque una copia de seguridad creada en una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se puede restaurar en una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Puede actualizar las instancias secundarias de forma simultánea o en serie, pero todas las instancias secundarias deben actualizarse antes de actualizar la instancia principal a fin de evitar un error de trasvase de registros.  
  
Mientras se actualiza la instancia del servidor secundario, no se ejecutan los trabajos de copia y restauración del trasvase de registros. Esto significa que las copias de seguridad del registro de transacciones sin restaurar se acumularán en el servidor principal y que debe tener espacio suficiente para alojar estas copias de seguridad sin restaurar. La cantidad de acumulación depende de la frecuencia de la copia de seguridad programada en la instancia de servidor principal y de la secuencia en que se actualicen las instancias secundarias. Además, si se ha configurado un servidor de supervisión independiente, se podrían generar alertas que indiquen que no se han realizado restauraciones durante más tiempo que el intervalo configurado.  
  
 Una vez que las instancias del servidor secundario se han actualizado, los trabajos de los agentes de trasvase de registros se reanudan y siguen copiando y restaurando las copias de seguridad de registros a partir de la instancia del servidor principal hasta las instancias de servidores secundarios. La cantidad de tiempo necesaria para que las instancias del servidor secundario actualicen la base de datos secundaria varía en función del tiempo empleado en actualizar la instancia del servidor secundario y de la frecuencia de las copias de seguridad en el servidor principal.  
  
> [!NOTE]  
> Durante la actualización del servidor, la propia base de datos secundaria no se actualiza a una nueva versión. Solo se actualizará si se conecta al iniciar una conmutación por error de la base de datos de trasvase de registros. En teoría, esta condición podría mantenerse indefinidamente. La cantidad de tiempo para actualizar los metadatos de la base de datos cuando se inicia una conmutación por error es pequeña.  
  
> [!IMPORTANT]  
>  La opción RESTORE WITH STANDBY no se admite para una base de datos que requiere actualizarse. Si una base de datos secundaria actualizada se ha configurado utilizando RESTORE WITH STANDBY, los registros de transacciones ya no se pueden restaurar después de la actualización. Para reanudar el trasvase de registros en esa base de datos secundaria, tendrá que configurarlo de nuevo en ese servidor de reserva. Para obtener más información sobre la opción STANDBY, vea [Restaurar una copia de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md).  
  
##  <a name="upgrading-the-primary-server-instance"></a><a name="UpgradePrimary"></a> Actualizar la instancia del servidor principal  
 Puesto que el trasvase de registros es principalmente una solución de recuperación ante desastres, el escenario más sencillo y más común consiste en actualizar la instancia principal en contexto y la base de datos simplemente no está disponible durante la actualización. Una vez actualizado el servidor, la base de datos se vuelve a poner en línea automáticamente, lo que hace que se actualice. Una vez actualizada la base de datos, los trabajos de trasvase de registros se reanudan.  
  
> [!NOTE]  
>  El trasvase de registros también admite la opción [Conmutar por error a una base de datos secundaria de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md) y, opcionalmente, [Cambiar los roles entre el servidor de trasvase de registros primario y secundario &#40;SQL Server&#41;](../../database-engine/log-shipping/change-roles-between-primary-and-secondary-log-shipping-servers-sql-server.md). Sin embargo, dado que el trasvase de registros ya rara vez se configura como una solución de alta disponibilidad (las opciones más recientes son mucho más sólidas), la conmutación por error generalmente no minimiza el tiempo de inactividad porque no se sincronizarán los objetos de base de datos del sistema y porque puede ser una odisea permitir que los clientes busquen un elemento secundario promovido y se conecten a él fácilmente.  
  
## <a name="see-also"></a>Consulte también  
 [Actualización a SQL Server 2016 mediante el Asistente para instalación &#40;programa de instalación&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)   
 [Instalar SQL Server 2016 desde el símbolo del sistema](../install-windows/install-sql-server-from-the-command-prompt.md)   
 [Configurar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)   
 [Supervisar el trasvase de registros &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)   
 [Tablas y procedimientos almacenados de trasvase de registros](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  

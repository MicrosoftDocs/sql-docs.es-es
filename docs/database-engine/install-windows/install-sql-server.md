---
title: Guía de instalación de SQL Server
description: Índice de contenido que permite instalar SQL Server y los componentes asociados mediante opciones como el Asistente para la instalación, el símbolo del sistema o sysprep.
ms.custom: ''
ms.date: 03/05/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- AdventureWorks sample database
- installing SQL Server, preparing to install
- installation [SQL Server]
ms.assetid: 0300e777-d56b-4d10-9c33-c9ebd2489ee5
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 4e433c16ba070d59c963589c8bceec371238ca38
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464279"
---
# <a name="sql-server-installation-guide"></a>Guía de instalación de SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Este artículo es un índice de contenido que proporciona instrucciones para instalar SQL Server en Windows.

Para otros escenarios de implementación, consulte:

- [Linux](../../linux/sql-server-linux-setup.md)
- [Contenedores de Docker](../../linux/sql-server-linux-docker-container-deployment.md)
- [Kubernetes: clústeres de macrodatos](../../big-data-cluster/deploy-get-started.md)

A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] solo está disponible como una aplicación de 64 bits. Aquí encontrará información importante acerca de cómo obtener SQL Server y cómo instalarlo.

## <a name="getting-started"></a>Introducción
  
* **Ediciones y características**: Revise las características admitidas en las distintas ediciones y versiones de SQL Server para determinar cuáles son las que mejor se adaptan a sus necesidades empresariales. 
    - [[!INCLUDE[ss2019](../../includes/sssql19-md.md)]](~/sql-server/editions-and-components-of-sql-server-version-15.md).  
    - [[!INCLUDE[ss2017](../../includes/sssql17-md.md)]](~/sql-server/editions-and-components-of-sql-server-2017.md).  
    - [[!INCLUDE[ss2016](../../includes/sssql16-md.md)]](~/sql-server/editions-and-components-of-sql-server-2016.md).  
    - [[!INCLUDE[ss2014](../../includes/sssql14-md.md)]](https://docs.microsoft.com/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014)

*  **Requisitos**: revise los requisitos de instalación de hardware y software para [SQL Server 2016 y 2017](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md), [SQL Server 2019](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) o [SQL Server en Linux](../../linux/sql-server-linux-setup.md), además de las comprobaciones de la configuración del sistema y las consideraciones de seguridad en [Planeación de una instalación de SQL Server](../../sql-server/install/planning-a-sql-server-installation.md) 


  
* **Bases de datos de ejemplo y código de ejemplo**: 
    * No se instalan como parte de la configuración de SQL Server de forma predeterminada pero se pueden encontrar. 
    * Para instalarlos para ediciones distintas de Express de SQL Server, vea [Ejemplos de SQL](../../samples/sql-samples-where-are.md).
    

## <a name="installation-media"></a>Medios de instalación

La ubicación de descarga de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] depende de la edición:

* **Las ediciones Enterprise, Standard y Express de SQL Server** tienen licencia para su uso en producción. En el caso de las ediciones Enterprise y Standard, póngase en contacto con su proveedor de software para obtener los soportes de instalación. Encontrará información sobre la adquisición y un directorio de asociados de Microsoft en el [página de licencias de Microsoft](https://www.microsoft.com/licensing/product-licensing/sql-server).
* Si tiene un contrato de licencias por volumen, por ejemplo, un [Contrato Enterprise](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise), puede descargar software del [centro de servicios de licencias por volumen (VLSC)](https://www.microsoft.com/licensing/servicecenter/default.aspx).
* [Versión gratuita: más reciente](https://www.microsoft.com/sql-server/sql-server-downloads).
* [Versión gratuita: otras](https://www.microsoft.com/evalcenter/evaluate-sql-server).

Encontrará otros componentes de SQL Server aquí:

* [Todas las actualizaciones acumulativas](https://sqlserverbuilds.blogspot.com/)
* [SQL Server Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122). 
* [SQL Server Management Studio](https://aka.ms/ssmsfullsetup)
* [Azure Data Studio](https://go.microsoft.com/fwlink/?linkid=2109256)


## <a name="considerations"></a>Consideraciones

-   La instalación no se lleva a cabo si el programa de instalación se inicia a través de una Conexión a Escritorio remoto con los medios de un recurso local en el cliente RDC. Para realizar la instalación de forma remota, los medios deben estar en un recurso compartido de red o localmente en una máquina virtual o física. Los medios de instalación de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden estar en un recurso compartido de red, en una unidad asignada, en una unidad local o como una imagen ISO de una máquina virtual.  
  
  
-   El programa de instalación de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instala los siguientes componentes de software que el producto necesita:  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client    
    -   Archivos auxiliares del programa de instalación de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

## <a name="sql-server-installation"></a>Instalación de SQL Server


|Artículo|Descripción|  
|-----------|-----------------|  
|[Asistente para la instalación](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)|Instale SQL Server mediante la GUI del Asistente para instalación que se inicia desde el medio de instalación setup.exe. |  
|[Símbolo del sistema](./install-sql-server-from-the-command-prompt.md)|Sintaxis de ejemplo y parámetros de instalación para ejecutar una instalación de SQL Server desde el símbolo del sistema. | 
|[Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md)|Instale [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en Windows Server Core.|  
|[Comprobar los parámetros del Comprobador de configuración del sistema](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)|Describe la función del Comprobador de la configuración del sistema (SCC).|   
|[Archivo de configuración](./install-sql-server-using-a-configuration-file.md)|Parámetros de instalación y sintaxis de ejemplo para ejecutar una instalación mediante un archivo de configuración.|  
|[SysPrep](../../database-engine/install-windows/install-sql-server-using-sysprep.md)|Parámetros de instalación y sintaxis de ejemplo para ejecutar una instalación mediante SysPrep.|
|[Adición de características en una instancia](./add-features-to-an-instance-of-sql-server-setup.md)|Actualice los componentes de una instancia existente de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].|  
|[Instalación de clúster de conmutación por error de SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)| Instale una instancia del clúster de conmutación por error de SQL Server.  | 
|[Reparar una instalación de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] con errores](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)|Repare una instalación dañada de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].|  
|[Cambio del nombre de un equipo con SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)|Actualice los metadatos del sistema que se almacenan en sys.servers después de cambiar el nombre de host de un equipo que hospeda una instancia independiente de SQL Server. |  
|[Instalar actualizaciones de servicio de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/install-sql-server-servicing-updates.md)|Instale las actualizaciones de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].|  
|[Archivos de registro del programa de instalación](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)| Vea y lea los errores en los archivos de registro de instalación de SQL Server. |  
|[Validación de una instalación](../../database-engine/install-windows/validate-a-sql-server-installation.md)|Revise el uso del informe de SQL Discovery para comprobar la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instaladas en el equipo.|  
  
  
## <a name="individual-component-installation"></a>Instalación de componentes individuales 
  
|Artículo|Descripción|  
|-----------|-----------------|  
|[Motor de base de datos de SQL Server](../../database-engine/install-windows/install-sql-server-database-engine.md)|Instale y configure [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].|  
|[Replicación de SQL Server](../../database-engine/install-windows/install-sql-server-replication.md)|Instale y configure la replicación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Distributed Replay](../../tools/distributed-replay/install-distributed-replay-overview.md)|Se enumeran los artículos para instalar la característica Distributed Replay.|  
|[Herramientas de administración de SQL Server con SSMS](../../ssms/download-sql-server-management-studio-ssms.md)|Instale y configure las herramientas de administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[SQL Server PowerShell](../../database-engine/install-windows/install-sql-server-powershell.md)|Consideraciones para instalar los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell.|  
  

## <a name="sql-server-configuration"></a>Configuración de SQL Server 
  
|Artículo|Descripción|  
|-----------|-----------------|  
|[Configuración de las reglas de Firewall de Windows (SQL Server)](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)|Información general sobre la configuración del firewall y el procedimiento para configurar Firewall de Windows para permitir el acceso a SQL Server.|  
|[Configuración del Firewall de Windows (SSAS)](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)|Configure los valores de puerto y firewall para permitir el acceso a [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] o [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint.|  
|[Configuración de un equipo de hosts múltiples](../../sql-server/install/configure-a-multi-homed-computer-for-sql-server-access.md)|Configure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y Firewall de Windows con seguridad avanzada para proporcionar las conexiones de red a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un entorno de host múltiple.|  

 
## <a name="see-also"></a>Consulte también  

[Actualizar [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../database-engine/install-windows/upgrade-sql-server.md)   
[Desinstalar [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]](../../sql-server/install/uninstall-sql-server.md)   
[Instalación de SQL Server Reporting Services (SSRS)](../../reporting-services/install-windows/install-reporting-services.md)   
[Instalación de SQL Server Analysis Services (SSAS)](/analysis-services/instances/install-windows/install-analysis-services)   
[Instalación de características de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Business Intelligence](../../sql-server/install/install-sql-server-business-intelligence-features.md)   
[Soluciones de alta disponibilidad &#40;SQL Server&#41;](../sql-server-business-continuity-dr.md)
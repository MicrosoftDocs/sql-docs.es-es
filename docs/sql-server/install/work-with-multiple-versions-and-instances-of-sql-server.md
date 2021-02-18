---
title: Uso de varias versiones e instancias
description: Puede instalar varias instancias de SQL Server, o bien instalar SQL Server en un equipo donde ya estén instaladas versiones anteriores de SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- concurrent installations [SQL Server]
- versions [SQL Server], multiple
- side-by-side installations [SQL Server]
- multiple SQL Server component versions
- installing SQL Server, side-by-side installations
- Setup [SQL Server], side-by-side installations
- 64-bit edition [SQL Server]
- 32-bit edition [SQL Server]
- editions [SQL Server], side-by-side installations
ms.assetid: 93acefa8-bb41-4ccc-b763-7801f51134e0
author: cawrites
ms.author: chadam
ms.openlocfilehash: c5290748f7a3a99909f124e17011e2d652557682
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336034"
---
# <a name="work-with-multiple-versions-and-instances-of-sql-server"></a>Uso de varias versiones e instancias de SQL Server

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Puede instalar varias instancias de SQL Server, o instalar SQL Server en un equipo donde ya estén instaladas versiones anteriores de SQL Server.

Los siguientes elementos relacionados con SQL Server son compatibles con la instalación de varias instancias en el mismo equipo:

- Motor de base de datos

- Analysis Services

- Reporting Services (en SQL Server 2016 y anteriores). A partir de SQL Server 2016. SQL Server Reporting Services (SSRS) tiene una instalación independiente. 


Puede actualizar versiones anteriores de SQL Server en un equipo donde ya estén instaladas otras versiones de SQL Server. Para conocer escenarios de actualización admitidos, vea [Actualizaciones de ediciones y versiones admitidas](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).
  
## <a name="version-components-and-numbering"></a>Componentes de versión y numeración

 Los siguientes conceptos resultan útiles para entender el comportamiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para instancias en paralelo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
 El formato de versión de producto estándar para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es MM.nn.bbbb.rr, donde cada segmento se define del siguiente modo:
  
 PP: versión principal  
  
 ss: versión secundaria  
  
 cccc: número de compilación  
  
 rr: número de revisión de la compilación  
  
 En cada lanzamiento principal o secundario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], hay un incremento del número de versión para diferenciarlo de las versiones anteriores. Este cambio de la versión se usa para numerosos propósitos. Entre otros, se incluye la presentación de información de la versión en la interfaz de usuario, el control del modo en que los archivos se reemplazan durante la actualización, la aplicación de Service Pack y también como un mecanismo para la diferenciación funcional entre las versiones sucesivas.
  
### <a name="components-shared-by-all-versions-of-ssnoversion"></a>Componentes compartidos por todas las versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

 Determinados componentes se comparten en todas las instancias de todas las versiones instaladas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Al instalar diferentes versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en paralelo en el mismo equipo, estos componentes se actualizan automáticamente a la versión más reciente. Dichos componentes normalmente se desinstalan de forma automática cuando se desinstala la última instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
 Ejemplos: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser y Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] VSS Writer.
  
### <a name="components-shared-across-all-instances-of-the-same-major-version-of-ssnoversion"></a>Componentes compartidos por todas las instancias de la misma versión principal de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que tienen la misma versión principal comparten algunos componentes en todas las instancias. Si los componentes compartidos se seleccionan durante la actualización, los componentes existentes se actualizan a la última versión.
  
Ejemplos: [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)], [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]y Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
### <a name="components-shared-across-minor-versions"></a>Componentes compartidos por las versiones secundarias

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que tienen la misma versión principal.secundaria comparten algunos componentes.
  
Ejemplo: .
  
### <a name="components-specific-to-an-instance-of-ssnoversion"></a>Componentes específicos de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Algunos componentes o servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] son específicos de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. También se denominan dependientes de la instancia. Comparten la misma versión que la instancia que los hospeda y se usan exclusivamente para dicha instancia.
  
Ejemplos: [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]y [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
### <a name="components-that-are-independent-of-the-ssnoversion-versions"></a>Componentes que son independientes de las versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Determinados componentes se instalan durante la instalación [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , pero son independientes de las versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se pueden compartir en las versiones principales o en todas las versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

Ejemplos: Microsoft Sync Framework, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
Para obtener más información sobre la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact, vea [Instalación de SQL Server 2016 desde el Asistente para la instalación &#40;programa de instalación&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md). Para obtener más información sobre cómo desinstalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact, vea [Desinstalar una instancia existente de SQL Server &#40;programa de instalación&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md).  
  
## <a name="using-ssnoversion-side-by-side-with-previous-versions-of-ssnoversion"></a>Usar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en paralelo con versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Puede instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un equipo que ya ejecute instancias de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si ya existe una instancia predeterminada en el equipo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se debe instalar como una instancia con nombre.  

En la tabla siguiente se muestra la compatibilidad en paralelo para cada versión de SQL Server en las versiones de Windows que se admiten normalmente con las versiones requeridas de .NET instaladas:

| Instancia existente | Compatibilidad en paralelo| 
|-------------------|----------------------------|
| SQL Server 2019 | De SQL Server 2008 a SQL Server 2017| 
| SQL Server 2017 | De SQL Server 2008 a SQL Server 2016| 
| SQL Server 2016 | De SQL Server 2008 a SQL Server 2014| 

Para obtener más información, vea [Usar SQL Server en Windows 8 y versiones posteriores del sistema operativo Windows](https://support.microsoft.com/help/2681562/using-sql-server-in-windows-8-and-later-versions-of-windows-operating). 

  
> [!CAUTION]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no admite la instalación en paralelo de las instancias preparadas de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] con versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el mismo equipo. Sin embargo, puede instalar varias instancias preparadas de la misma versión principal de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en paralelo en el mismo equipo. Para obtener más información, vea [Considerations for Installing SQL Server Using SysPrep](../../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).  
>
> SQL Server 2016 y versiones posteriores no se puede instalar en paralelo con versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un equipo que ejecute Windows Server 2008 R2 Server Core SP1. Para obtener más información sobre cómo llevar a cabo una instalación Server Core, vea [Instalación de SQL Server 2016 en Server Core](../../database-engine/install-windows/install-sql-server-on-server-core.md).  
  


## <a name="preventing-ip-address-conflicts"></a>Evitar conflictos de direcciones IP

Al instalar una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en paralelo con una instancia independiente de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], tenga cuidado para evitar conflictos de números de puerto TCP en las direcciones IP. Los conflictos suelen suceder cuando se configuran dos instancias de [!INCLUDE[ssDE](../../includes/ssde-md.md)] para usar el puerto TCP (1433). Para evitar conflictos, configure una instancia para que utilice un puerto fijo predeterminado. La configuración de un puerto fijo es normalmente lo más sencillo en el caso de la instancia independiente. La configuración de [!INCLUDE[ssDE](../../includes/ssde-md.md)] para usar puertos diferentes evitará un conflicto inesperado entre el puerto TCP y la dirección IP que impide que se inicie la instancia cuando una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] produce un error en el nodo en espera.
  
## <a name="see-also"></a>Consulte también

* [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
* [Instalación de SQL Server desde el asistente para instalación &#40;programa de instalación&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md).
* [Actualizaciones de ediciones y versiones admitidas](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)
* [Actualizar SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)
* [Ediciones y características admitidas de SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md) 
* [Ediciones y las características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md)
* [Ediciones y las características admitidas de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)
* [Compatibilidad con versiones anteriores](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))
---
title: Instalar SQL Server con SysPrep
description: SQL Server SysPrep permite preparar una instancia independiente de SQL Server en un equipo y completar la configuración más adelante.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: e1792eeb-2874-4653-b20e-3063f4eb4e5d
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: e5eeb7446542613eac7e1d3a78144f3ca165a47b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336366"
---
# <a name="considerations-for-installing-sql-server-using-sysprep"></a>Consideraciones acerca de la instalación de SQL Server con SysPrep

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep permite preparar una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un equipo y completar la configuración en un momento posterior. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep emplea un proceso de dos pasos para obtener una instancia independiente configurada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los pasos son los siguientes:  
  
- [Preparar imagen](#BKMK_PrepareImage)  
  
    Este paso detiene el proceso de instalación una vez instalados los archivos binarios del producto, sin configurar la información específica del equipo, la red o la cuenta para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se está preparando.  
  
- [Completar imagen](#BKMK_CompleteImage)  
  
    Este paso permite completar la configuración de una instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Durante este paso, puede proporcionar el equipo, la red y la información específica de cuenta.  
  
Para más información sobre cómo instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con SysPrep, vea [Instalar SQL Server mediante SysPrep](../../database-engine/install-windows/install-sql-server-using-sysprep.md).  
  
## <a name="common-uses-for-ssnoversion-sysprep"></a>Usos comunes de SysPrep de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
Puede usar la funcionalidad de SysPrep de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de cualquiera de las maneras siguientes:  
  
- Si realiza el paso Preparar imagen, puede preparar una o varias instancias sin configurar de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el mismo equipo. Puede configurar estas instancias preparadas efectuando el paso Completar imagen en el mismo equipo.  
  
- Puede capturar el archivo de configuración de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la instancia preparada y usarlo para preparar instancias adicionales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sin configurar en varios equipos para su configuración posterior.  
  
- En combinación con la herramienta de preparación del sistema de Windows (también denominada Windows SysPrep), puede crear una imagen del sistema operativo, incluidas las instancias preparadas no configuradas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el equipo de origen. Puede implementar la imagen del sistema operativo en varios equipos. Después de completar la configuración del sistema operativo, puede configurar las instancias preparadas con el paso Completar imagen del programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    La herramienta SysPrep de Windows se usa para preparar las imágenes del sistema operativo Windows. Se usa para capturar una imagen personalizada del sistema operativo para su implementación en toda la organización. Para más información sobre SysPrep y sus usos, vea [SysPrep](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview).  
  
## <a name="installation-media-considerations"></a>Consideraciones acerca de los medios de instalación  
 Si usa una versión completa de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], considere los siguientes aspectos:  
  
- Ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]que no son Express:  
  
    - El proceso de preparación de imagen usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Evaluation Edition para instalar los archivos binarios del producto. Cuando la instancia está completada, la edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] depende del identificador del producto proporcionado durante el paso de compleción de imagen.  
  
    - Si proporciona un identificador de producto de Evaluation Edition, el período de evaluación se configurará para expirar al cabo de 180 días, después de que se complete la instancia preparada.  
  
- Ediciones Express de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
    - Para preparar una instancia de las ediciones Express de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , use los medios de instalación de Express.  
  
    - No puede especificar los identificadores de producto de una instancia preparada de las ediciones Express de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="supported-ssnoversion-installations"></a>Instalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compatibles  
SysPrep de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] admite para todas las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], incluidas las herramientas.  
  
Puede preparar varias instancias para instalaciones en paralelo de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] o versiones anteriores. Las características de estas instancias deben admitir SysPrep.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client se instala y completa automáticamente al final del paso de preparación de imagen.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Writer se preparan automáticamente al preparar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se completan al completar la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el paso Completar imagen.  
  
Para obtener más información sobre las ediciones admitidas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consulte [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) y [Ediciones y características admitidas de SQL Server 2019 (15.x)](../../sql-server/editions-and-components-of-sql-server-version-15.md).  
  
Puede realizar una actualización de la edición si configura una instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta opción no se admite para las ediciones Express de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], SysPrep de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite instalaciones de clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde la línea de comandos.  
  
## <a name="ssnoversion-sysprep-limitations"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Limitaciones de SysPrep  
No se admite la reparación de una instancia preparada. Si se produce un error del programa de instalación durante el paso de preparación o de compleción de la imagen, debe ejecutar la desinstalación.  
  
##  <a name="prepare-image"></a><a name="BKMK_PrepareImage"></a> Preparar imagen  
El paso Preparar imagen instala el producto y las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , pero no configura la instalación.  
  
Las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se van a instalar y la ubicación de instalación de los archivos de instalación del producto [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se pueden especificar durante este paso. Puede preparar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la opción de **preparación de la imagen de una instancia independiente para la implementación de SysPrep** en la página **Avanzadas** del **Centro de instalación** o desde el símbolo del sistema.  
  
- Puede preparar varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el mismo equipo que se pueden completar posteriormente.  
  
- Puede agregar o quitar características que se admiten para las instalaciones de SysPrep de las instancias preparadas existentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Después de preparar la instancia, dispone de un acceso directo en el menú **Inicio** para completar la configuración de la instancia preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="complete-image"></a><a name="BKMK_CompleteImage"></a> Completar imagen  
Puede completar las instancias preparadas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante cualquiera de los métodos siguientes:  
  
- Usar el acceso directo del menú Inicio.  
  
- Obtener acceso al paso **Completar imagen de una instancia independiente preparada de SQL Server** de la página **Avanzadas** del **Centro de instalación**.  
  
## <a name="see-also"></a>Consulte también  
[Planear una instalación de SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)
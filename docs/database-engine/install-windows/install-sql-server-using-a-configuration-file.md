---
title: Instalar SQL Server mediante un archivo de configuración | Microsoft Docs
description: Puede usar el programa de instalación de SQL Server para generar un archivo de configuración a fin de implementar SQL Server en la organización mediante una configuración uniforme.
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: a832153a-6775-4bed-83f0-55790766d885
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b5b38250065d1c7474d5f500284a082b421332c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348345"
---
# <a name="install-sql-server-using-a-configuration-file"></a>Instalar SQL Server mediante un archivo de configuración

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
 
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El programa de instalación permite generar un archivo de configuración basado en las entradas de tiempo de ejecución y en la configuración predeterminada del sistema. Puede usar el archivo de configuración para implementar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en toda la empresa con la misma configuración. También puede normalizar las instalaciones manuales en toda la empresa mediante la creación de un archivo por lotes que inicie Setup.exe. 
 
Este artículo se mantiene para SQL Server 2016 y versiones posteriores. En cuanto a las versiones anteriores de SQL Server, vea [Instalar SQL Server 2014 mediante un archivo de configuración](/previous-versions/sql/2014/database-engine/install-windows/install-sql-server-using-a-configuration-file?view=sql-server-2014&preserve-view=true).
 
El programa de instalación admite el uso del archivo de configuración solamente a través del símbolo del sistema. A continuación se indica el orden de procesamiento de los parámetros cuando se usa el archivo de configuración:  
  
-  El archivo de configuración sobrescribe los valores predeterminados de un paquete.  
  
-   Los valores de línea de comandos sobrescriben los valores del archivo de configuración.  
  
 El archivo de configuración se puede usar para realizar el seguimiento de los parámetros y valores de cada instalación. De este modo, el archivo de configuración es útil para comprobar y auditar las instalaciones. 
  
## <a name="configuration-file-structure"></a>Estructura de los archivos de configuración  
 El archivo ConfigurationFile.ini es un archivo de texto con parámetros (pares de nombre/valor) y comentarios descriptivos. 
  
 A continuación se muestra un ejemplo de archivo ConfigurationFile.ini:  
  
```  
; Microsoft SQL Server Configuration file  
[OPTIONS]  
; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE.  
; This is a required parameter.  
ACTION="Install"  
; Specifies features to install, uninstall, or upgrade.  
; The list of top-level features include SQL, AS, RS, IS, and Tools.  
; The SQL feature will install the database engine, replication, and full-text.  
; The Tools feature will install Management Tools, Books online,   
; SQL Server Data Tools, and other shared components.  
FEATURES=SQL,Tools  
```  
  
#### <a name="how-to-generate-a-configuration-file"></a>Cómo generar un archivo de configuración  
  
1. Inserte el medio de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Desde la carpeta raíz, haga doble clic en Setup.exe. Para realizar la instalación desde un recurso compartido de red, localice la carpeta raíz de dicho recurso y, a continuación, haga doble clic en Setup.exe. 
  
    > [!NOTE]  
    >  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express Edition no crea automáticamente un archivo de configuración. El siguiente comando iniciará el programa de instalación y creará un archivo de configuración. 
    >   
    >  SETUP.exe /UIMODE=Normal /ACTION=INSTALL  
  
2. Siga los pasos del asistente hasta llegar a la página **Listo para instalar** . La ruta de acceso al archivo de configuración se especifica en la sección que así lo indica en la página **Listo para instalar** . Para más información sobre cómo instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Instalar SQL Server desde el Asistente para la instalación &#40;programa de instalación&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md). 
  
3. Cancele la instalación sin completarla realmente para generar el archivo INI. 
  
    > [!NOTE]  
    >  La infraestructura de instalación escribe todos los parámetros correspondientes a las acciones que se ejecutaron, con la excepción de la información confidencial, como las contraseñas. El parámetro /IAcceptSQLServerLicenseTerms tampoco se escribe en el archivo de configuración, y para incluirlo es necesario modificar dicho archivo o proporcionar un valor en el símbolo del sistema. Para más información, consulte [Instalar SQL Server 2016 desde el símbolo del sistema](./install-sql-server-from-the-command-prompt.md). Además se incluye un valor para los parámetros booleanos, ya que normalmente no se proporciona a través del símbolo del sistema. 
  
## <a name="using-the-configuration-file-to-install-ssnoversion"></a>Usar el archivo de configuración para instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

El archivo de configuración solamente se usa en instalaciones de línea de comandos. 
  
> [!NOTE]  
> Si necesita realizar cambios en el archivo de configuración, se recomienda hacer una copia y trabajar con ella. 
  
### <a name="how-to-use-a-configuration-file-to-install-a-stand-alone-ssnoversion-instance"></a>Cómo usar un archivo de configuración para instalar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] independiente  
  
-   Realice la instalación a través del símbolo del sistema y proporcione el archivo ConfigurationFile.ini mediante el parámetro *ConfigurationFile* . 
  
### <a name="how-to-use-a-configuration-file-to-prepare-and-complete-an-image-of-a-stand-alone-ssnoversion-instance-sysprep"></a>Utilizar un archivo de configuración para preparar y completar una imagen de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] independiente (SysPrep)  
  
1. Para preparar una o más instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y configurarlas en el mismo equipo. 
  
    - Ejecute **Preparar imagen de una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** en la página **Avanzadas** del Centro de instalación y capture el archivo de configuración de preparación de imagen. 
  
    - Utilice el mismo archivo de configuración de preparación de imagen como plantilla para preparar más instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
    - Ejecute **Completar imagen de una instancia independiente preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** en la página **Avanzadas** del Centro de instalación para configurar una instancia preparada en el equipo. 
  
2. Para preparar una imagen del sistema operativo que incluya una instancia preparada no configurada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mediante la herramienta Windows SysPrep. 
  
    -   Ejecute **Preparar imagen de una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** en la página Avanzadas del Centro de instalación y capture el archivo de configuración de preparación de imagen. 
  
    -   Ejecute **Completar imagen de una instancia independiente preparada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** en la página **Avanzadas** del Centro de instalación, pero cancele el proceso en la página **Listo para completar la imagen** después de capturar el archivo de configuración completo. 
  
    -   El archivo de configuración de imagen completo se puede almacenar con la imagen de Windows para automatizar la configuración de las instancias preparadas. 
  
### <a name="how-to-install-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>Cómo instalar un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el archivo de configuración  
  
1. Opción de instalación integrada (crear un clúster de conmutación por error de nodo único en un nodo y ejecutar AddNode en los demás nodos):  
  
    -   Ejecute la opción de instalación de clúster de conmutación por error y capture el archivo de configuración que enumera todos los valores de configuración de la instalación. 
  
    -   Ejecute la instalación del clúster de conmutación por error de línea de comandos proporcionando el parámetro *ConfigurationFile* . 
  
    -   En un nodo adicional que vaya a agregarse, ejecute AddNode para capturar el archivo ConfigurationFile.ini aplicable al clúster de conmutación por error existente. 
  
    -   Ejecute AddNode en la línea de comandos en todos los demás nodos que se unirán al clúster de conmutación por error; proporcione el mismo archivo de configuración mediante el parámetro ConfigurationFile. 
  
2. Opción de instalación avanzada (preparar el clúster de conmutación por error en todos los nodos de clúster de conmutación por error y, a continuación, después de preparar todos los nodos, ejecutar "complete" en el nodo donde se encuentra el disco compartido):  
  
    -   Ejecute **Prepare** en uno de los nodos y capture el archivo ConfigurationFile.ini. 
  
    -   Proporcione al programa de instalación el mismo archivo ConfigurationFile.ini en todos los nodos que se prepararán para el clúster de conmutación por error. 
  
    -   Una vez preparados todos los nodos, ejecute una operación para completar el clúster de conmutación por error en el nodo que posee el disco compartido y capture el archivo ConfigurationFile.ini. 
  
    -   A continuación, puede proporcionar este archivo ConfigurationFile.ini para completar el clúster de conmutación por error. 
  
### <a name="how-to-add-or-remove-a-node-to-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>Cómo agregar o quitar un nodo en un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el archivo de configuración  
  
-   Si tiene un archivo de configuración que ya se usó previamente para agregar o quitar un nodo en un clúster de conmutación por error, puede volver a usar ese mismo archivo para agregar o quitar nodos adicionales. 
  
### <a name="how-to-upgrade-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>Cómo actualizar un clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el archivo de configuración  
  
1. Ejecute la actualización en el nodo pasivo y capture el archivo ConfigurationFile.ini. Para ello, puede realizar la actualización real o salir al final sin llegar a realizarla. 
  
2. En todos los nodos adicionales que se van a actualizar, proporcione el archivo ConfigurationFile.ini para completar el proceso. 
  
## <a name="sample-syntax"></a>Sintaxis de ejemplo  
 A continuación se ofrecen algunos ejemplos de uso del archivo de configuración:  
  
-   Para especificar el archivo de configuración en el símbolo del sistema:  
  
```  
Setup.exe /ConfigurationFile=MyConfigurationFile.INI  
```  
  
-   Para especificar las contraseñas en el símbolo del sistema en lugar de hacerlo en el archivo de configuración:  
  
```  
Setup.exe /SQLSVCPASSWORD="************" /AGTSVCPASSWORD="************" /ASSVCPASSWORD="************" /ISSVCPASSWORD="************" /RSSVCPASSWORD="************" /ConfigurationFile=MyConfigurationFile.INI  
```  
  
## <a name="see-also"></a>Consulte también  
 [Instalar SQL Server desde el símbolo del sistema](./install-sql-server-from-the-command-prompt.md)   
 [Instalación de clúster de conmutación por error de SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)   
 [Actualización de una instancia del clúster de conmutación por error de SQL Server](../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md).  
  

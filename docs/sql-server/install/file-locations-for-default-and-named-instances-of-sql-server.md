---
title: Ubicación de archivos
description: Una instancia de SQL Server tiene sus propios archivos de programa y de datos. Puede compartir archivos comunes con otras instancias de SQL Server. En este artículo se incluyen las ubicaciones de archivo.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 463c570e-9f75-4653-b3b8-4d61753b0013
author: cawrites
ms.author: chadam
ms.openlocfilehash: 053f8519ab8d75659dc9073c11fcc940cacdc1d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341140"
---
# <a name="file-locations-for-default-and-named-instances-of-sql-server"></a>Ubicaciones de archivos para las instancias predeterminadas y con nombre de SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Una instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se compone de una o más instancias independientes. Una instancia, ya sea predeterminada o con nombre, tiene su propio conjunto de archivos de programa y de datos, así como un conjunto de archivos comunes compartidos entre todas las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del equipo.  
  
 En una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que incluya el [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]y [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], cada componente tiene un conjunto completo de datos y de archivos ejecutables, así como de archivos comunes compartidos por todos los componentes.  
  
 Para aislar las ubicaciones de instalación de cada componente, se generan identificadores de instancia únicos para cada componente de una determinada instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
>  Los archivos de programa y los archivos de datos no se pueden instalar en una unidad de disco extraíble, en un sistema de archivos que use compresión, en un directorio en el que haya ubicados archivos del sistema ni en unidades compartidas en una instancia en clúster de conmutación por error.  
>  
>  Es posible que necesite configurar el software de detección, como aplicaciones antivirus y antispyware, para excluir las carpetas y los tipos de archivo de SQL Server. Revise este artículo de soporte técnico para obtener más información: [Elegir software antivirus para equipos que ejecutan SQL Server](https://support.microsoft.com/kb/309422).
> 
>  Las bases de datos del sistema (maestra, modelo, MSDB y tempdb) y las bases de datos de usuario del [!INCLUDE[ssDE](../../includes/ssde-md.md)] se pueden instalar con el servidor de archivos del Bloque de mensajes del servidor (SMB) como opción de almacenamiento. Esto se aplica tanto a las instalaciones independientes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como a las instalaciones de clústeres de conmutación por error (FCI) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para más información, consulte [Instalar SQL Server con el recurso compartido de archivos SMB como opción de almacenamiento](../../database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option.md).  
>   
>  No elimine ninguno de los directorios siguientes ni su contenido: Binn, Data, Ftdata, HTML o 1033. Si fuera necesario, puede eliminar otros directorios; no obstante, es posible que no pueda recuperar algunas de las funciones o datos perdidos sin tener que desinstalar y volver a instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No elimine ni modifique ninguno de los archivos .htm del directorio HTML. Son necesarios para que las herramientas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] funcionen correctamente.  
  
## <a name="shared-files-for-all-instances-of-ssnoversion"></a>Archivos compartidos para todas las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Los archivos comunes que usan todas las instancias en un único equipo se instalan en la carpeta [!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]. \<*drive*> es la letra de la unidad en la que se instalarán los componentes. El valor predeterminado es normalmente la unidad C. _nnn_ identifica la versión. En la tabla siguiente se identifican las versiones de las rutas de acceso. \{nn} es el valor de versión usado en el identificador de instancia y la ruta de acceso del registro. 

|Versión|\*nnn*|{nn}|
|-----|-----|--------|
|[!INCLUDE[ssqlv15](../../includes/sssql19-md.md)]| 150| 15| 
|[!INCLUDE[ssqlv14](../../includes/sssql17-md.md)]| 140| 14| 
|[!INCLUDE[ssqlv13](../../includes/sssql16-md.md)]| 130| 13 | 
|[!INCLUDE[ssqlv12](../../includes/sssql14-md.md)]  | 120|12 | 
|[!INCLUDE[sssql11](../../includes/sssql11-md.md)] | 110|11 | 
  
## <a name="file-locations-and-registry-mapping"></a>Ubicaciones de archivos y asignaciones del Registro  
 Durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se genera un identificador de instancia en cada componente de servidor. Los componentes de servidor de esta versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] son el [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]y [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 El identificador de instancia predeterminado se construye con el formato siguiente:  
  
-   MSSQL para el [!INCLUDE[ssDE](../../includes/ssde-md.md)], seguido del número de versión principal, un guión bajo y la versión secundaria cuando proceda, un punto y, a continuación, el nombre de instancia.  
  
-   MSAS para [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], seguido del número de versión principal, un guión bajo y la versión secundaria cuando proceda, un punto y, a continuación, el nombre de instancia.  
  
-   MSRS para [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], seguido del número de versión principal, un guión bajo y la versión secundaria cuando proceda, un punto y, a continuación, el nombre de instancia.  
  
 A continuación se enumeran algunos ejemplos de identificadores de instancia predeterminados de esta versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   MSSQL\{nn}.MSSQLSERVER para una instancia predeterminada de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
-   MSAS\{nn}.MSSQLSERVER para una instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Analysis Services.  
  
-   MSSQL\{nn}.MyInstance para una instancia con nombre de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] denominada "MyInstance."  
  

 La estructura de directorios para una instancia con nombre de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] que incluyera el [!INCLUDE[ssDE](../../includes/ssde-md.md)] y [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], se denominara "MyInstance" y estuviera instalada en los directorios predeterminados sería como sigue:  
  
-   C:\Archivos de programa\Microsoft SQL Server\MSSQL\{nn}.MyInstance\  
  
-   C:\Archivos de programa\Microsoft SQL Server\MSAS\{nn}.MyInstance\  
  
 Puede especificar cualquier valor para el identificador de instancia, pero evite los caracteres especiales y las palabras clave reservadas.  
  
 Puede especificar un identificador de instancia no predeterminado durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . En lugar de \\{Archivos de programa}\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se usa una \<custom path>\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si el usuario decide cambiar el directorio de instalación predeterminado. Tenga en cuenta que no se admiten identificadores de instancia que comienzan por un subrayado (_) o que contienen el signo de almohadilla (#) o el signo de dólar ($).  
  
> [!NOTE]  
>  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y los componentes de cliente no reconocen las instancias y, por consiguiente, no tienen asignado un identificador de instancia. De forma predeterminada, los componentes que no reconocen las instancias se instalan en un único directorio: [!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]. Si cambia la ruta de instalación de un componente compartido, cambiará también la de los demás componentes compartidos. Las instalaciones posteriores instalan componentes que no reconocen instancias en el mismo directorio que la instalación original.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] es el único componente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que permite cambiar el nombre de las instancias después de la instalación. Si se cambia el nombre una instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , el identificador de la instancia no cambiará. Después de completarse el cambio de nombre de la instancia, los directorios y claves del Registro continuarán utilizando el identificador de instancia creado durante la instalación.  
  
 El subárbol del Registro se crea en HKLM\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\<*Instance_ID*> para los componentes dependientes de la instancia. Por ejemplo,  
  
-   HKLM\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL\{nn}.MyInstance  
  
-   HKLM\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSAS\{nn}.MyInstance  
  
-   HKLM\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSRS\{nn}.MyInstance  
  
 El Registro también mantiene una asignación de identificador de instancia a nombre de instancia. La asignación de identificador de instancia a nombre de instancia se mantiene de la siguiente forma:  
  
-   [HKEY_LOCAL_MACHINE\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\Instance Names\SQL] "\<InstanceName>"="MSSQL\{nn}"  
  
-   [HKEY_LOCAL_MACHINE\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\Instance Names\OLAP] "\<InstanceName>"="MSAS\{nn}"  
  
-   [HKEY_LOCAL_MACHINE\Software\\[!INCLUDE[msCoName](../../includes/msconame-md.md)]\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\Instance Names\RS] "\<InstanceName>"="MSRS\{nn}"  
  
## <a name="specifying-file-paths"></a>Especificar rutas de acceso a los archivos  
 Durante la instalación, puede cambiar la ruta de instalación de las siguientes características:  
  
 La ruta de instalación solo aparece para las características cuya carpeta de destino puede configurar el usuario:  
  
|Componente|Ruta de acceso predeterminada|Ruta de acceso configurable o fija|  
|---------------|------------------|--------------------------------|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] componentes de servidor|\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL\{nn}.\<InstanceID>\ |Configurable|  
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] archivos de datos|\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL\{nn}.\<InstanceID>\ |Configurable|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] servidores|\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSAS\{nn}.\<InstanceID>\ |Configurable|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] archivos de datos|\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSAS\{nn}.\<InstanceID>\ |Configurable|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] servidor de informes|\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSRS\{nn}.\<InstanceID>\Reporting Services\ReportServer\Bin\ |Configurable|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] administrador de informes|\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSRS\{nn}.\<InstanceID>\Reporting Services\ReportManager\ |Ruta de acceso fija|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|\<Install Directory>\nnn\DTS\\ <sup>1</sup> |Configurable |  
|Componentes cliente (excepto bcp.exe y sqlcmd.exe)|\<Install Directory>\nnn\Tools\\ <sup>1</sup> |Configurable |  
|Componentes cliente (bcp.exe y sqlcmd.exe)|\<Install Directory>\Client SDK\ODBC\nnn\Tools\Binn|Ruta de acceso fija|  
|Objetos COM del servidor y la replicación|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]COM\\ <sup>2</sup> |Ruta de acceso fija|  
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] DLL de componentes del motor en tiempo de ejecución de transformación de datos, el motor de canalización de transformación de datos y la utilidad de símbolo del sistema **dtexec**|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Binn|Ruta de acceso fija|  
|DLL que proporcionan compatibilidad con la conexión administrada para [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Connections|Ruta de acceso fija|  
|DLL para cada tipo de enumeración que admita [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\ForEachEnumerators|Ruta de acceso fija|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , proveedores WMI|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Shared\\ |Ruta de acceso fija|  
|Componentes que se comparten entre todas las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Shared\\ |Ruta de acceso fija|  
  
> [!WARNING]
> Asegúrese de que la carpeta \Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\ esté protegida con permisos limitados.  
  
Tenga en cuenta que la unidad predeterminada para las ubicaciones de archivo es *systemdrive*, normalmente la unidad C. Las rutas de acceso de instalación para las características secundarias vienen determinadas por la ruta de instalación de la característica primaria.  
  
<sup>1</sup>Una sola ruta de acceso de instalación se comparte entre [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y los componentes de cliente. Si cambia la ruta de instalación de un componente, cambiará también la de los otros componentes. Las instalaciones posteriores instalan los componentes en la misma ubicación que la instalación original.  
  
<sup>2</sup>Este directorio lo usan todas las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un equipo. Si aplica una actualización a alguna de las instancias del equipo, los cambios en los archivos de esta carpeta afectarán a todas las instancias en el equipo. Cuando agregue características a una instalación existente, no podrá cambiar la ubicación de una característica instalada anteriormente, ni especificar dicha ubicación para una característica nueva. Debe instalar características adicionales en los directorios ya establecidos por el programa de instalación, o desinstalar y volver a instalar el producto.  
  
> [!NOTE]  
>  En configuraciones en clúster, deberá seleccionar una unidad local que esté disponible en cada nodo del clúster.  
  
 Cuando especifique una ruta de instalación durante la instalación de los componentes de servidor o de los archivos de datos, el programa de instalación utilizará el identificador de instancia además de la ubicación especificada para el programa y los archivos de datos. El programa de instalación no utiliza el identificador de instancia para las herramientas y otros archivos compartidos. Tampoco utiliza ningún identificador de instancia para el programa y los archivos de datos de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , aunque lo use para el depósito de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
 Si establece una ruta de instalación para la característica [!INCLUDE[ssDE](../../includes/ssde-md.md)] , el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizará dicha ruta como directorio raíz de todas las carpetas específicas de la instancia en dicha instalación, incluido SQL Data Files. En este caso, si el directorio raíz se establece en "C:\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL\{nn}.\<InstanceName>\MSSQL\\", los directorios específicos de la instancia se agregarán al final de esa ruta de acceso.  
  
 Los clientes que decidan usar la funcionalidad de actualización de USESYSDB en el Asistente para la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (modo de UI del programa de instalación) pueden llegar con facilidad a una situación en la que el producto se instale en una estructura de carpetas recursiva. Por ejemplo, \<*SQLProgramFiles*>\MSSQL14\MSSQL\MSSQL10_50\MSSQL\Data\\. En su lugar, para usar la característica USESYSDB, establezca una ruta de instalación para la característica de archivos de datos de SQL (SQL Data Files) y no para la característica [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
> [!NOTE]
>  Los archivos de datos deberían encontrarse en un directorio secundario denominado Data. Por ejemplo, especifique C:\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL\{nn}.\<InstanceName>\ para especificar la ruta de acceso raíz al directorio de datos de las bases de datos del sistema durante la actualización si los archivos de datos se encuentran en C:\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL\{nn}.\<InstanceName>\MSSQL\Data.  
  
## <a name="see-also"></a>Consulte también  
 [Configuración del motor de base de datos - Directorios de datos](../../database-engine/install-windows/install-sql-server.md)   
 [Configuración de Analysis Services - Directorios de datos](../../database-engine/install-windows/install-sql-server.md)  
  

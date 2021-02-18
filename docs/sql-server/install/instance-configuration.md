---
title: Ayuda del Asistente para instalación | Microsoft Docs
description: Especifique si quiere crear una instancia predeterminada o una instancia con nombre de SQL Server mediante Configuración de instancia en el Asistente para la instalación de SQL Server.
ms.custom: ''
ms.date: 08/16/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
f1_keywords:
- instance configuration, Setup
helpviewer_keywords:
- Instance Name page [SQL Server Installation Wizard]
- SQL Server Installation Wizard, Instance Name page
ms.assetid: 5bf822fc-6dec-4806-a153-e200af28e9a5
author: cawrites
ms.author: chadam
robots: noindex,nofollow
ms.openlocfilehash: 7b678ac25174b75db198739e0e9bc191d29f18d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336168"
---
# <a name="installation-wizard-help"></a>Ayuda del Asistente para instalación

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este artículo se describen algunas de las páginas de configuración del Asistente para instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="instance-configuration-page"></a>Página Configuración de instancia

Use la página **Configuración de instancia** del Asistente para instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el fin de especificar si quiere crear una instancia predeterminada o una instancia con nombre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si aún no hay instalada ninguna instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se crea una predeterminada, a menos que especifique una instancia con nombre.  
  
Cada instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consta de un conjunto de servicios distinto con una configuración específica para intercalaciones y otras opciones. La estructura de directorios, la estructura del Registro y los nombres de los servicios reflejan todos el nombre de instancia y un identificador de instancia específico que se crearon durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
La instancia puede ser una instancia predeterminada o una instancia con nombre. El nombre de instancia predeterminado es MSSQLSERVER. Para realizar una conexión, no es necesario que un cliente especifique el nombre de la instancia. La instancia con nombre queda determinada por el usuario durante la instalación. Puede instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como una instancia con nombre sin instalar primero la instancia predeterminada. Solo una instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], independientemente de la versión, puede ser la instancia predeterminada.  
  
> [!NOTE]  
> Con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep, puede especificar el nombre de instancia cuando complete una instancia preparada en la página **Configuración de instancia**. Puede configurar la instancia preparada que está completando como una instancia predeterminada si no existe una instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la máquina.  
  
### <a name="multiple-instances"></a>Varias instancias
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un solo servidor o procesador, pero solo una puede ser la predeterminada. Todas las demás deben ser instancias con nombre. Un equipo puede ejecutar varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] simultáneamente y cada instancia se ejecuta independientemente de las otras instancias.  
  
 Para obtener más información, consulte [Especificaciones de capacidad máxima para SQL Server](../maximum-capacity-specifications-for-sql-server.md).  
  
### <a name="options"></a>Opciones

**Solo instancias de clúster de conmutación por error**: Especifique el nombre de red del clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Este nombre identifica la instancia en clúster de conmutación por error en la red.  
  
**Decidir entre un valor predeterminado o una instancia con nombre**: Tenga presente la información siguiente a la hora de decidir entre instalar una instancia predeterminada o una instancia con nombre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
* Si piensa instalar una única instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un servidor de base de datos, debe ser una instancia predeterminada.  
  
* Use una instancia con nombre para aquellas situaciones en las que piensa tener varias instancias en el mismo equipo. Un servidor solo puede alojar una instancia predeterminada.  
* Cualquier aplicación que instale [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] debe instalarla como instancia con nombre. Esta práctica reduce los conflictos en situaciones en las que se instalan varias aplicaciones en el mismo equipo.
  
**Instancia predeterminada**: Seleccione esta opción para instalar una instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un equipo solo puede hospedar una instancia predeterminada; todas las demás instancias deben ser instancias con nombre. No obstante, si tiene instalada una instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], podrá agregar una instancia predeterminada de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] al mismo equipo.  
  
**Instancia con nombre**: Seleccione esta opción para crear una instancia con nombre nueva. Cuando asigne un nombre a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tenga en cuenta la siguiente información:  
  
* Los nombres de instancia no distinguen mayúsculas de minúsculas.  
  
* Los nombres de instancia no pueden comenzar ni terminar con un guion bajo (_).  
  
* Los nombres de instancia no pueden contener el término "Default" ni otras palabras clave reservadas. Si se utiliza una palabra clave reservada en un nombre de instancia, se producirá un error en el programa de instalación. Para obtener más información, consulte [Palabras clave reservadas &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md).  
  
* Si especifica MSSQLSERVER como nombre de instancia, se crea una instancia predeterminada.  
  
* Una instalación de [!INCLUDE[ssGeminiLong](../../includes/ssgeminilong-md.md)] siempre se instala como una instancia con nombre de "[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]". No puede especificar un nombre de instancia diferente para este rol de característica.  
  
* Los nombres de instancias están limitados a 16 caracteres.  
  
* El primer carácter del nombre de la instancia debe ser una letra. Las letras aceptables son las que define el estándar Unicode 2.0. Estas letras incluyen los caracteres latinos, a-z, A-Z y los caracteres alfabéticos de otros idiomas.  
  
* Los siguientes caracteres pueden ser letras definidas por el estándar Unicode 2.0, números decimales del alfabeto Latín básico y de otros alfabetos nacionales, el signo de dólar ($) o un carácter de subrayado (_).  
  
* No se permiten espacios insertados ni otros caracteres especiales en los nombres de instancia. Tampoco se permiten los caracteres de barra diagonal inversa (\\), coma (,), dos puntos (:), punto y coma (;), comillas sencillas ('), y comercial (&), guion (-) y la arroba (@).  
  
  En los nombres de instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] solo se pueden usar caracteres que figuren como válidos en la página actual de códigos de Windows. Si se utiliza un carácter Unicode no admitido, se producirá un error en el programa de instalación.  
  
**Instancias y características detectadas**: Vea una lista de las instancias y los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instalados en el equipo en el que se ejecuta el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
**Id. de instancia**: de forma predeterminada, el nombre de instancia se usa como identificador de la instancia. Este identificador se usa para identificar los directorios de instalación y las claves del Registro para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El mismo comportamiento se produce para las instancias predeterminadas y las instancias con nombre. Para las instancias predeterminadas, el nombre y el identificador son MSSQLSERVER. Para usar un identificador de instancia no predeterminado, especifíquelo en el campo **Id. de instancia**.  
  
> [!IMPORTANT]  
>  Con SysPrep de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el identificador de instancia que se muestra en la página **Configuración de instancia** es el identificador de instancia especificado durante el paso de preparación de la imagen del proceso SysPrep de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No podrá especificar un identificador de instancia diferente durante el paso para completar la imagen.

> [!NOTE]  
>  No se admiten identificadores de instancia que comiencen con un guion bajo (_) o que contengan el signo de almohadilla (#) o el signo de dólar ($).  
  
Para obtener más información sobre los directorios, las ubicaciones de archivos y los nombres de identificador de instancia, consulte [Ubicaciones de archivos para las instancias predeterminadas y con nombre de SQL Server](file-locations-for-default-and-named-instances-of-sql-server.md).  
  
Todos los componentes de una instancia determinada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se administran como una unidad. Todos los Service Pack y actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se aplican a cada componente de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Todos los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que comparten el mismo nombre de instancia deben cumplir los siguientes criterios:  
  
* La misma versión
* La misma edición
* La misma configuración de idioma
* El mismo estado de clúster
* El mismo sistema operativo  
  
> [!NOTE]  
> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no detecta los clústeres.  

## <a name="analysis-services-configuration---account-provisioning-page"></a>Página Configuración de Analysis Services - Aprovisionamiento de cuentas
  
Use esta página para establecer el modo de servidor y para conceder permisos administrativos a los usuarios o servicios que requieran acceso sin restricciones a [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. El programa de instalación no agrega automáticamente el grupo Windows local BUILTIN\Administrators al rol de administrador del servidor de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de la instancia que está instalando. Si desea agregar el grupo local de administradores al rol de administrador del servidor, debe especificar ese grupo explícitamente.  
  
Si está instalando [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)], asegúrese de conceder permisos administrativos a los administradores de la granja de SharePoint o a los administradores de servicios que sean responsables de una implementación del servidor de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] en una granja de [!INCLUDE[SPS2010](../../includes/sps2010-md.md)].  
  
### <a name="options"></a>Opciones

**Modo de servidor**: el modo de servidor especifica el tipo de bases de datos de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] que se pueden implementar en el servidor. Los modos de servidor se determinan durante la instalación y no se pueden modificar posteriormente. Cada modo es mutuamente exclusivo, lo que significa que se necesitarán dos instancias de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], cada una configurada para un modo diferente, para admitir soluciones de modelo de procesamiento analítico en línea (OLAP) clásico y tabular.  
  
**Especificar administradores**: debe especificar al menos un administrador de servidor para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los usuarios o grupos que especifique serán miembros del rol de administrador del servidor de la instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] que está instalando. Estos miembros deben tener cuentas de usuario de dominio de Windows en el mismo dominio que el del equipo en el que se instala el software.  
  
> [!NOTE]  
> Control de cuentas de usuario (UAC) es una característica de seguridad de Windows que requiere que un administrador apruebe específicamente acciones administrativas o aplicaciones antes de que se puedan ejecutar. Dado que UAC está activado de forma predeterminada, se le solicitará que permita las operaciones concretas que requieren privilegios elevados. Puede configurar UAC para cambiar el comportamiento predeterminado o personalizarlo para programas concretos. Para más información sobre UAC y la configuración de UAC, consulte [Guía paso a paso de Control de cuentas de usuario](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc709691(v=ws.10)) y el tema sobre [control de cuentas de usuario (Wikipedia)](https://go.microsoft.com/fwlink/?linkid=196351).  
  
### <a name="see-also"></a>Consulte también
  
* [Configurar las cuentas de servicio &#40;Analysis Services&#41;](/analysis-services/instances/configure-service-accounts-analysis-services)
* [Configuración de permisos y cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  

## <a name="analysis-services-configuration---data-directories-page"></a>Página Configuración de Analysis Services - Directorios de datos

Los directorios predeterminados de la tabla siguiente los puede configurar el usuario durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los permisos para acceder a estos archivos se conceden a los administradores locales y a los miembros del grupo de seguridad SQLServerMSASUser$\<instance> que se crea y aprovisiona durante la instalación.  
  
### <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario  
  
|Descripción|Directorio predeterminado|Recomendaciones|  
|-----------------|-----------------------|---------------------|  
|**Directorio raíz de datos**|\<Drive:>\Archivos de programa\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Data\ |Asegúrese de que la carpeta \Archivos de programa\Microsoft SQL Server\ está protegida con permisos limitados. El rendimiento de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] depende, en muchas configuraciones, del rendimiento del espacio de almacenamiento en el que se ubica el directorio de datos. Coloque este directorio en el almacenamiento de más alto rendimiento asociado al sistema. En las instalaciones del clúster de conmutación por error, asegúrese de que los directorios de datos están ubicados en el disco compartido.|  
|**Directorio de archivos de registro**|\<Drive:>\Archivos de programa\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Log\ |Este es el directorio para los archivos de registro de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] e incluye el registro FlightRecorder. Si incrementa la duración de la Caja negra SQL, asegúrese de que el directorio del registro tenga espacio suficiente.|  
|**Directorio temporal**|\<Drive:>\Archivos de programa\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Temp\ |Coloque el directorio Temp en el subsistema de almacenamiento de alto rendimiento.|  
|**Directorio de copia de seguridad**|\<Drive:>\Archivos de programa\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Backup\ |Este directorio es para los archivos de copia de seguridad predeterminados de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. En las instalaciones de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint, este directorio es también donde los Servicios del sistema de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] almacenan en caché los archivos de datos de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)].<br /><br /> Asegúrese de que se han establecido los permisos adecuados para evitar la pérdida de datos y de que el grupo de usuarios del [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tiene los permisos apropiados para escribir en el directorio de copia de seguridad. No se permite usar una unidad asignada para los directorios de copia de seguridad.|  
  
### <a name="considerations"></a>Consideraciones  
  
* Cuando se implementan instancias de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en una granja de SharePoint, almacenan archivos de aplicación, archivos de datos y propiedades de bases de datos de contenido y de aplicación de servicio.  
  
* Cuando agregue características a una instalación existente, no podrá cambiar la ubicación de una característica instalada anteriormente, ni especificar dicha ubicación para una característica nueva.  

* Es posible que necesite configurar el software de detección, como aplicaciones antivirus y antispyware, para excluir las carpetas y los tipos de archivo de SQL Server. Revise este artículo de soporte técnico para obtener más información: [Elegir software antivirus para equipos que ejecutan SQL Server](https://support.microsoft.com/kb/309422).
  
* Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No comparta directorios en este cuadro de diálogo con los de otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Instale también los componentes de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en directorios independientes.  
  
* Los archivos de programa y los archivos de datos no se pueden instalar en las situaciones siguientes:  
  
  * En una unidad de disco extraíble  
  * En un sistema de archivos que usa compresión  
  * En un directorio que contiene archivos del sistema  
  
### <a name="see-also"></a>Consulte también

Para obtener más información sobre los directorios, las ubicaciones de archivos y los nombres de identificador de instancia, consulte [Ubicaciones de archivos para las instancias predeterminadas y con nombre de SQL Server](file-locations-for-default-and-named-instances-of-sql-server.md).  
  
### <a name="analysis-services-configuration---data-directories-page"></a>Página Configuración de Analysis Services - Directorios de datos

Los directorios predeterminados de la tabla siguiente los puede configurar el usuario durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los permisos para acceder a estos archivos se conceden a los administradores locales y a los miembros del grupo de seguridad SQLServerMSASUser$\<instance> que se crea y aprovisiona durante la instalación.  
  
#### <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario
  
|Descripción|Directorio predeterminado|Recomendaciones|  
|-----------------|-----------------------|---------------------|  
|**Directorio raíz de datos** |\<Drive:>\Archivos de programa\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Data |Asegúrese de que la carpeta \Archivos de programa\Microsoft SQL Server\ está protegida con permisos limitados. El rendimiento de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] depende, en muchas configuraciones, del rendimiento del espacio de almacenamiento en el que se ubica el directorio de datos. Coloque este directorio en el almacenamiento de más alto rendimiento asociado al sistema. En las instalaciones del clúster de conmutación por error, asegúrese de que los directorios de datos están ubicados en el disco compartido.|  
|**Directorio de archivos de registro**|\<Drive:>\Archivos de programa\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Log |Este es el directorio para los archivos de registro de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] e incluye el registro FlightRecorder. Si incrementa la duración de la Caja negra SQL, asegúrese de que el directorio del registro tenga espacio suficiente.|  
|**Directorio temporal**|\<Drive:>\Archivos de programa\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Temp |Coloque el directorio Temp en el subsistema de almacenamiento de alto rendimiento.|  
|**Directorio de copia de seguridad**|\<Drive:>\Archivos de programa\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Backup |Este directorio es para los archivos de copia de seguridad predeterminados de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. En las instalaciones de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] para SharePoint, es también donde los Servicios del sistema de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] almacenan en caché los archivos de datos de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)].<br /><br /> Asegúrese de que se han establecido los permisos adecuados para evitar la pérdida de datos y de que el grupo de usuarios del servicio [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tiene los permisos apropiados para escribir en el directorio de copia de seguridad. No se permite usar una unidad asignada para los directorios de copia de seguridad.|  
  
#### <a name="considerations"></a>Consideraciones
  
* Cuando se implementan instancias de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en una granja de SharePoint, almacenan archivos de aplicación, archivos de datos y propiedades de bases de datos de contenido y de aplicación de servicio.  
  
* Cuando agregue características a una instalación existente, no podrá cambiar la ubicación de una característica instalada anteriormente, ni especificar dicha ubicación para una característica nueva.  

* Es posible que necesite configurar el software de detección, como aplicaciones antivirus y antispyware, para excluir las carpetas y los tipos de archivo de SQL Server. Para obtener más información, consulte [Software antivirus en equipos que ejecutan SQL Server](https://support.microsoft.com/kb/309422).
  
* Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No comparta ninguno de los directorios de este cuadro de diálogo con los de otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Instale también los componentes de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en directorios independientes.  
  
* Los archivos de programa y los archivos de datos no se pueden instalar en las situaciones siguientes:  
  
  * En una unidad de disco extraíble  
  * En un sistema de archivos que usa compresión  
  * En un directorio que contiene archivos del sistema  
  
#### <a name="see-also"></a>Consulte también

* Para obtener más información sobre los directorios, las ubicaciones de archivos y los nombres de identificador de instancia, consulte [Ubicaciones de archivos para las instancias predeterminadas y con nombre de SQL Server](file-locations-for-default-and-named-instances-of-sql-server.md).  
* [Share and NTFS permissions on a file server](/iis/web-hosting/configuring-servers-in-the-windows-web-platform/configuring-share-and-ntfs-permissions) (Permisos NTFS y de uso compartido en un servidor de archivos)

## <a name="database-engine-configuration---server-configuration-page"></a><a name="serverconfig"></a> Página Configuración del Motor de base de datos - Configuración del servidor

Use esta página para establecer el modo de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y para agregar grupos o usuarios de Windows como administradores de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
### <a name="considerations-for-running-sscurrent"></a>Consideraciones para ejecutar [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]

En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se aprovisionaba el grupo BUILTIN\Administrators como inicio de sesión de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y los miembros del grupo local Administradores podían iniciar sesión con sus credenciales de administrador. Sin embargo, el uso de permisos elevados no es un procedimiento recomendado.

En [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], el grupo BUILTIN\Administrators no se aprovisiona como inicio de sesión. Asegúrese de crear un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para cada usuario administrativo y agregarlo al rol fijo de servidor **sysadmin** durante la instalación de una nueva instancia de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Haga lo mismo en el caso de cuentas de Windows que ejecuten trabajos de agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], incluidos los trabajos del agente de replicación.  
  
### <a name="options"></a>Opciones

**Modo de seguridad**: seleccione **Autenticación de Windows** o **Autenticación de modo mixto** para la instalación.  
  
**Aprovisionamiento de entidad de seguridad de Windows**: En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el grupo local BUILTIN\Administrators de Windows estaba ubicado en el rol de servidor **sysadmin** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], con lo que se concedía a los administradores de Windows acceso a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], el grupo BUILTIN\Administrators o se aprovisiona en el rol de servidor **sysadmin**. En su lugar, debería aprovisionar explícitamente administradores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para las nuevas instalaciones durante la instalación.  
  
> [!IMPORTANT]  
> Debe aprovisionar explícitamente administradores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para las nuevas instalaciones durante la instalación. El programa de instalación no le permitirá continuar hasta que complete este paso.
  
**Especificar los administradores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : debe especificar al menos una entidad de seguridad de Windows para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para agregar la cuenta en la que se ejecuta el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], seleccione el botón **Agregar usuario actual**. Para agregar o quitar cuentas de la lista de administradores del sistema, seleccione **Agregar** o **Quitar** y, luego, modifique la lista de usuarios, grupos o equipos que tendrán privilegios de administrador para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Cuando termine de modificar la lista, haga clic en **Aceptar** y, luego, compruebe la lista de administradores en el cuadro de diálogo de configuración. Si la lista esté completa, seleccione **Siguiente**.  
  
Si selecciona **Autenticación de modo mixto**, debe proporcionar credenciales de inicio de sesión para la cuenta de administrador del sistema (**sa**) integrada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
**Modo de autenticación de Windows**: cuando un usuario se conecta mediante una cuenta de usuario de Windows, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valida el nombre de cuenta y la contraseña con el token de la entidad de seguridad de Windows del sistema operativo. La autenticación de Windows es el modo de autenticación predeterminado y es mucho más seguro que la autenticación de modo mixto. La autenticación de Windows utiliza el protocolo de seguridad Kerberos, cumple la directiva de contraseñas en lo que respecta a la validación de la complejidad de las contraseñas seguras, y permite el bloqueo de las cuentas y la expiración de las contraseñas.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  

> [!IMPORTANT]  
> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] No establezca nunca una contraseña de **sa** en blanco o no segura.  
  
**Modo mixto (autenticación de Windows o autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])** : Permite a los usuarios conectarse con la autenticación de Windows o la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los usuarios que se conectan mediante una cuenta de usuario de Windows pueden usar conexiones de confianza validadas por Windows.  
  
Si tiene que elegir el modo de autenticación mixto y necesita utilizar inicios de sesión de SQL para incluir aplicaciones heredadas, debe establecer contraseñas seguras para todas las cuentas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
> La autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se proporciona únicamente por motivos de compatibilidad con versiones anteriores. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
**Escribir contraseña**: Escriba y confirme el inicio de sesión del administrador del sistema (**sa**). Las contraseñas son la primera línea de defensa contra los intrusos, por lo que establecer contraseñas seguras es esencial para la seguridad del sistema. No establezca nunca una contraseña de **sa** en blanco o no segura.  
  
> [!NOTE]  
> Las contraseñas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden contener de 1 a 128 caracteres, incluida cualquier combinación de letras, símbolos y números. Si elige la autenticación de modo mixto, debe escribir una contraseña de **sa** segura antes de continuar con la siguiente página del Asistente para la instalación.  
  
#### <a name="strong-password-guidelines"></a>Directrices para contraseñas seguras
  
Las contraseñas seguras no pueden ser adivinadas con facilidad y tampoco son fácilmente vulnerables en caso de utilizar un programa informático. Las contraseñas seguras no pueden utilizar los siguientes términos o condiciones prohibidos:  
  
* Una condición en blanco o NULL
* "Password"
* "Admin"
* "Administrator"
* "sa"
* "sysadmin"

Una contraseña segura no puede ser ninguno de los siguientes términos asociados al equipo de instalación:  
  
* El nombre del usuario que haya iniciado actualmente la sesión en la máquina.
* Nombre del equipo  
  
Una contraseña segura debe tener más de 8 caracteres de longitud y satisfacer al menos tres de los siguientes criterios:  
  
* Debe contener letras mayúsculas.
* Debe contener letras minúsculas.  
* Debe contener números.
* Debe contener caracteres no alfanuméricos; por ejemplo, #, %, o ^.  
  
Las contraseñas que se escriben en esta página deben cumplir los requisitos de las directivas de contraseñas seguras. Si tiene alguna automatización que use la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], asegúrese de que la contraseña cumpla los requisitos de las directivas de contraseñas seguras.  
  
### <a name="see-also"></a>Consulte también

Para obtener más información sobre elegir la autenticación de Windows en lugar de la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea el tema [Elegir un modo de autenticación](../../relational-databases/security/choose-an-authentication-mode.md).  

Para obtener más información sobre cómo elegir una cuenta para ejecutar [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], consulte el tema [Configurar los permisos y las cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

## <a name="database-engine-configuration---data-directories-page"></a><a name ="datadir"></a> Configuración del Motor de base de datos - Directorios de datos

Use esta página para especificar la ubicación de instalación de los archivos de programa y datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Según el tipo de instalación, es posible que el almacenamiento compatible incluya un disco local, almacenamiento compartido o un servidor de archivos SMB.  
  
Para especificar un recurso compartido de archivos SMB como directorio, deberá escribir manualmente la ruta UNC admitida. No se admite el desplazamiento a un recurso compartido de archivos de SMB. En el ejemplo siguiente se muestra el formato de la ruta de acceso UNC admitida de un recurso compartido de archivos de SMB:

`\\<ServerName>\<ShareName>\...`

### <a name="standalone-instance-of-ssnoversion"></a>Instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
  
Para instancias independientes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en la tabla siguiente se enumeran los tipos de almacenamiento admitidos y los directorios predeterminados que puede configurar durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
### <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario
  
|Descripción|Tipos de almacenamiento compatibles|Directorio predeterminado|Recomendaciones|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**Directorio raíz de datos**|Disco local, servidor de archivos SMB y almacenamiento compartido* |\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\ |El programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configura listas de control de acceso (ACL) para directorios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e interrumpe la herencia como parte de la configuración.|  
|**Directorio de base de datos de usuario**|Disco local, servidor de archivos SMB y almacenamiento compartido*|\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data |Las prácticas recomendadas para los directorios de datos del usuario dependen de los requisitos de carga de trabajo y rendimiento.|  
|**Directorio de registro de base de datos de usuario**|Disco local, servidor de archivos SMB y almacenamiento compartido*|\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|Asegúrese de que el directorio de registro tenga espacio suficiente.|  
|**Directorio de copia de seguridad**|Disco local, servidor de archivos SMB y almacenamiento compartido*|\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Backup|Establezca los permisos adecuados para evitar la pérdida de datos y asegúrese de que la cuenta de usuario para el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tenga los permisos adecuados para escribir en el directorio de copia de seguridad. No se permite usar una unidad asignada para los directorios de copia de seguridad.|  
  
\* Aunque se admiten discos compartidos, no se recomienda su uso con una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="failover-cluster-instance-of-ssnoversion"></a>Instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Para instancias de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en la tabla siguiente se indican los tipos de almacenamiento admitidos y los directorios predeterminados que puede configurar durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Descripción|Tipos de almacenamiento compatibles|Directorio predeterminado|Recomendaciones|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**Directorio raíz de datos**|Almacenamiento compartido, servidor de archivos de SMB|\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\<br /><br /> **Sugerencia**: Si selecciona un **disco compartido** en la página **Selección de disco de clúster**, el valor predeterminado será el primer disco compartido. El valor predeterminado de este campo será en blanco si no se realiza ninguna selección en la página **Selección de disco de clúster**.|El programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configura las ACL para los directorios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e interrumpe la herencia como parte de la configuración.|  
|**Directorio de base de datos de usuario**|Almacenamiento compartido, servidor de archivos de SMB|\<Drive:>Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **Sugerencia**: Si selecciona un **disco compartido** en la página **Selección de disco de clúster**, el valor predeterminado será el primer disco compartido. El valor predeterminado de este campo será en blanco si no se realiza ninguna selección en la página **Selección de disco de clúster**.|Las prácticas recomendadas para los directorios de datos del usuario dependen de los requisitos de carga de trabajo y rendimiento.|  
|**Directorio de registro de base de datos de usuario**|Almacenamiento compartido, servidor de archivos de SMB|\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **Sugerencia**: Si selecciona un **disco compartido** en la página **Selección de disco de clúster**, el valor predeterminado será el primer disco compartido. El valor predeterminado de este campo será en blanco si no se realiza ninguna selección en la página **Selección de disco de clúster**.|Asegúrese de que el directorio de registro tenga espacio suficiente.|  
|**Directorio de copia de seguridad**|Disco local, almacenamiento compartido y servidor de archivos SMB|\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Backup<br /><br /> **Sugerencia**: Si selecciona un **disco compartido** en la página **Selección de disco de clúster**, el valor predeterminado será el primer disco compartido. El valor predeterminado de este campo será en blanco si no se realiza ninguna selección en la página **Selección de disco de clúster**.|Establezca los permisos apropiados para evitar la pérdida de datos y asegúrese de que la cuenta de usuario para el servicio de SQL Server tenga los permisos adecuados para escribir en el directorio de copia de seguridad. No se permite usar una unidad asignada para los directorios de copia de seguridad.|  
  
### <a name="security-considerations"></a>Consideraciones sobre la seguridad
  
El programa de instalación configurará las ACL para los directorios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y anulará la herencia como parte de la configuración.  
  
Las siguientes recomendaciones se aplican a los servidores de archivos de SMB:  
  
* Si se usa un servidor de archivos SMB, la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe ser una cuenta de dominio.  
  
* La cuenta que se usa para instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe los tener permisos NTFS de control total en la carpeta de recurso compartido de archivos de SMB que se usa como directorio de datos.  
  
* Al a cuenta que se usa para instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se le debe conceder los privilegios SeSecurityPrivilege en el servidor de archivos SMB. Para ello, use la consola Directiva de seguridad local del servidor de archivos para agregar la cuenta de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la directiva **Administrar registro de seguridad y auditoría**. Esta opción está disponible en la sección **Asignaciones de derechos de usuario** en **Directivas locales** de la consola Directiva de seguridad local.

### <a name="considerations"></a>Consideraciones
  
* Cuando agregue características a una instalación existente, no puede cambiar la ubicación de una característica instalada anteriormente ni especificar dicha ubicación para una característica nueva.  
  
* Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No comparta ninguno de los directorios de este cuadro de diálogo con los de otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Instale también los componentes de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en directorios independientes.  
  
* Los archivos de programa y los archivos de datos no se pueden instalar en las situaciones siguientes:  
  
  * En una unidad de disco extraíble
  * En un sistema de archivos que usa compresión
  * En un directorio que contiene archivos del sistema
  * En una unidad de red asignada en una instancia de clúster de conmutación por error  
  
## <a name="a-nametempdb-database-engine-configuration---tempdb-page"></a><a name="tempdb"><a/> Página Configuración del Motor de base de datos - TempDB

Use esta página para especificar la ubicación de los archivos de registro y datos de **tempdb**, su tamaño, su configuración de crecimiento y su número de archivos para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Según el tipo de instalación, es posible que el almacenamiento compatible incluya un disco local, almacenamiento compartido o un servidor de archivos SMB.  
  
Para especificar un recurso compartido de archivos SMB como directorio, deberá escribir manualmente la ruta UNC admitida. No se admite el desplazamiento a un recurso compartido de archivos de SMB. En el ejemplo siguiente se muestra el formato de la ruta de acceso UNC admitida de un recurso compartido de archivos de SMB:

`\\<ServerName>\<ShareName>\....`
  
### <a name="data-and-log-directories-for-a-standalone-instance-of-ssnoversion"></a>Directorios de datos y de registro para una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Para instancias independientes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en la tabla siguiente se indican los tipos de almacenamiento admitidos y los directorios predeterminados que puede configurar durante la instalación.  
  
|Descripción|Tipo de almacenamiento compatible|Directorio predeterminado|Recomendaciones|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**Directorios de datos**|Disco local, servidor de archivos SMB y almacenamiento compartido* |\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|El programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configura las ACL para los directorios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e interrumpe la herencia como parte de la configuración.<br /><br /> Los procedimientos recomendados para los directorios **tempdb** dependen de los requisitos de carga de trabajo y rendimiento. Para distribuir los archivos de datos entre varios volúmenes, especifique varias carpetas o unidades.|  
|**Directorio de registro**|Disco local, servidor de archivos SMB y almacenamiento compartido*|\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|Asegúrese de que el directorio de registro tenga espacio suficiente.|  
  
\* Aunque se admiten discos compartidos, no se recomienda su uso con una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="data-and-log-directories-for-a-failover-cluster-instance-of-ssnoversion"></a>Directorios de datos y registro para una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Para instancias de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en la tabla siguiente se indican los tipos de almacenamiento admitidos y los directorios predeterminados que puede configurar durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Descripción|Tipo de almacenamiento compatible|Directorio predeterminado|Recomendaciones|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**Directorio de datos de tempdb**|Disco local, almacenamiento compartido y servidor de archivos SMB|\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\Data<br /><br /> **Sugerencia**: Si selecciona un **disco compartido** en la página **Selección de disco de clúster**, el valor predeterminado será el primer disco compartido. El valor predeterminado de este campo será en blanco si no se realiza ninguna selección en la página **Selección de disco de clúster**.|El programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configura las ACL para los directorios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e interrumpe la herencia como parte de la configuración.<br /><br /> Asegúrese de que el directorio o los directorios (en caso de indicarse varios archivos) especificados son válidos para todos los nodos del clúster. Durante la conmutación por error, si los directorios **tempdb** no están disponibles en el nodo de destino de la conmutación por error, el recurso de SQL Server no podrá ponerse en línea.|  
|**Directorio del registro de tempdb**|Disco local, almacenamiento compartido y servidor de archivos SMB|\<Drive:>\Archivos de programa\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **Sugerencia**: Si selecciona un **disco compartido** en la página **Selección de disco de clúster**, el valor predeterminado será el primer disco compartido. El valor predeterminado de este campo será en blanco si no se realiza ninguna selección en la página **Selección de disco de clúster**.|Las prácticas recomendadas para los directorios de datos del usuario dependen de los requisitos de carga de trabajo y rendimiento.<br /><br /> Asegúrese de que el directorio especificado sea válido para todos los nodos de clúster. Durante la conmutación por error, si los directorios **tempdb** no están disponibles en el nodo de destino de la conmutación por error, el recurso de SQL Server no podrá ponerse en línea.<br /><br /> Asegúrese de que el directorio de registro tenga espacio suficiente.|  
  
### <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario

Configure las opciones de **tempdb** según sus requisitos y la carga de trabajo. Las siguientes opciones se aplican a los archivos de datos de **tempdb** :  
  
* El valor de **Número de archivos** equivale a la cantidad total de archivos de datos de **tempdb**. El valor predeterminado es por debajo de 8 o el número de núcleos lógicos detectados durante la instalación. Como regla general, si el número de procesadores lógicos es menor o igual a 8, use el mismo número de archivos de datos que procesadores lógicos. Si el número de procesadores lógicos es superior a 8, utilice 8 archivos de datos. Si se produce la contención, aumente el número de archivos de datos en múltiplos de 4 (hasta el número de procesadores lógicos) hasta que la contención se encuentre dentro de niveles aceptables, o realice cambios en la carga de trabajo o el código.
  
* El **tamaño inicial (MB)** equivale al tamaño inicial en megabytes para cada archivo de datos de **tempdb**. El valor predeterminado es de 8 MB (o 4 MB para [!INCLUDE[ssexpress](../../includes/ssexpress_md.md)]). [!INCLUDE[sssql17](../../includes/sssql17-md.md)] presenta un tamaño máximo de archivo inicial de 262 144 MB (256 GB). [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] tenía un tamaño máximo de archivo inicial de 1024 MB. Todos los archivos de datos de **tempdb** tienen el mismo tamaño inicial. Como **tempdb** se vuelve a crear cada vez que SQL Server se inicia o conmuta por error, debe especificar un valor parecido al tamaño requerido por la carga de trabajo para ofrecer un funcionamiento normal. Para optimizar aún más la creación de **tempdb** durante el inicio, habilite la [inicialización instantánea de archivos de base de datos](../../relational-databases/databases/database-instant-file-initialization.md).  
  
* El **tamaño inicial total (MB)** es el tamaño acumulado de todos los archivos de datos de **tempdb**.  
  
* **Crecimiento automático (MB)** es la cantidad de espacio en megabytes en la que cada archivo de datos de **tempdb** aumentará automáticamente cuando se quede sin espacio. En [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, todos los archivos de datos aumentarán su tamaño al mismo tiempo en la cantidad especificada en esta configuración.  
  
* **Crecimiento automático total (MB)** es el tamaño acumulado de cada evento de crecimiento automático.  
* **Directorios de datos** muestra todos los directorios que contienen archivos de datos de **tempdb**. Cuando hay varios directorios, los archivos de datos se colocan en ellos mediante un mecanismo round robin. Por ejemplo, si crea 3 directorios y especifica 8 archivos de datos, los archivos de datos 1, 4 y 7 se crean en el primer directorio. En el segundo se crean los archivos de datos 2, 5 y 8. Los archivos de datos 3 y 6 están en el tercer directorio.  
  
* Para agregar directorios, seleccione **Agregar...** .  
  
* Para quitar un directorio, seleccione el que desee y haga clic en **Quitar**.  
  
**TempDB log file** (Archivo de registro de TempDB) es el nombre del archivo de registro. Este archivo se crea automáticamente. Las siguientes opciones se aplican únicamente a los archivos de registro de **tempdb** :  
  
* **Tamaño inicial (MB)** es el tamaño inicial del archivo de registro de **tempdb** . El valor predeterminado es de 8 MB (o 4 MB para [!INCLUDE[ssexpress](../../includes/ssexpress_md.md)]). [!INCLUDE[sssql17](../../includes/sssql17-md.md)] presenta un tamaño máximo de archivo inicial de 262 144 MB (256 GB). [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] tenía un tamaño máximo de archivo inicial de 1024 MB. Como **tempdb** se vuelve a crear cada vez que SQL Server se inicia o conmuta por error, debe especificar un valor parecido al tamaño requerido por la carga de trabajo para ofrecer un funcionamiento normal. Para optimizar aún más la creación de **tempdb** durante el inicio, habilite la [inicialización instantánea de archivos de base de datos](../../relational-databases/databases/database-instant-file-initialization.md).  
  
  > [!NOTE]
  > **Tempdb** usa el registro mínimo. No se puede hacer una copia de seguridad del archivo de registro de **tempdb**. Se vuelve a crear cada vez que SQL Server se inicia o cuando una instancia del clúster conmuta por error.

* **Crecimiento automático (MB)** equivale al valor en megabytes en el que aumentará el tamaño del registro de **tempdb** .  Con el valor predeterminado de 64 MB, se crea el número óptimo de archivos de registro virtuales durante la inicialización.  

  > [!NOTE]
  > Los archivos de registro de **Tempdb** no usan la inicialización instantánea de archivos de base de datos.
  
* **Directorio de registro** es el directorio en el que se crean los archivos de registro de **tempdb** . Solo hay un directorio de registro de **tempdb**.  
  
### <a name="security-considerations"></a>Consideraciones sobre la seguridad
  
El programa de instalación configura las ACL para los directorios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e interrumpe la herencia como parte de la configuración.  

Al servidor de archivos SMB se aplican las recomendaciones siguientes:  
  
* Si se usa un servidor de archivos SMB, la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe ser una cuenta de dominio.  
  
* La cuenta que se usa para instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe los tener permisos NTFS de **control total** en la carpeta de recurso compartido de archivos de SMB que se usa como directorio de datos.  
  
* Al a cuenta que se usa para instalar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se le debe conceder los privilegios SeSecurityPrivilege en el servidor de archivos SMB. Para ello, use la consola Directiva de seguridad local del servidor de archivos para agregar la cuenta de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la directiva **Administrar registro de seguridad y auditoría**. Esta opción está disponible en la sección **Asignaciones de derechos de usuario** en **Directivas locales** de la consola Directiva de seguridad local.  
  
> [!NOTE]
> Si especifica directorios de instalación no predeterminados, asegúrese de que las carpetas de instalación sean únicas para esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ninguno de los directorios de este cuadro de diálogo se debe compartir con los de otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los componentes de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también deben instalarse en directorios independientes.
  
### <a name="see-also"></a>Consulte también

* [Configuración de permisos y cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)
* [Share and NTFS permissions on a file server](/iis/web-hosting/configuring-servers-in-the-windows-web-platform/configuring-share-and-ntfs-permissions) (Permisos NTFS y de uso compartido en un servidor de archivos)  

<!--
The MaxDOP setting applies only to SQL Server 2019 and later.
-->

::: moniker range=">=sql-server-ver15"

## <a name="a-namemaxdop-database-engine-configuration---maxdop-page"></a><a name="maxdop"><a/> Página Configuración del Motor de base de datos: MaxDOP

El **grado máximo de paralelismo (MaxDOP)** determina el número máximo de procesadores que puede usar una sola instrucción. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduce la posibilidad de configurar esta opción durante la instalación. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] detecta también automáticamente la configuración de MaxDOP recomendada para el servidor en función del número de núcleos.  

Si se omite esta página durante la instalación, el valor predeterminado de MaxDOP es el valor recomendado que se muestra en esta página, en lugar del valor de [!INCLUDE[ssde_md](../../includes/ssde_md.md)] predeterminado para las versiones anteriores (0). También puede configurar manualmente esta opción en esta página y modificarla tras la instalación. 

### <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario

* El **grado máximo de paralelismo (MaxDOP)** es el valor del número máximo de procesadores que se van a usar durante la ejecución en paralelo de una única instrucción. El valor predeterminado se alineará con las directrices sobre el grado máximo de paralelismo en [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines).

## <a name="a-namememory-database-engine-configuration---memory-page"></a><a name="memory"><a/> Página Configuración del Motor de base de datos: memoria

El valor de **Memoria de servidor mínima** determina el límite inferior de memoria que usará [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] para el grupo de búferes y otras cachés. El valor predeterminado es 0, así como el valor recomendado. Para obtener más información sobre los efectos de la **memoria de servidor mínima**, vea la [Guía de arquitectura de administración de memoria](../../relational-databases/memory-management-architecture-guide.md#effects-of-min-and-max-server-memory).

El valor de **Memoria de servidor máxima** determina el límite superior de memoria que [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] usará para el grupo de búferes y otras cachés. El valor predeterminado es 2 147 483 647 megabytes (MB) y los valores recomendados calculados se alinearán con las directrices de configuración de memoria en las [opciones de configuración de memoria del servidor](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually) para una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] independiente, en función de la memoria del sistema existente. Para obtener más información sobre los efectos de la **memoria de servidor máxima**, vea la [Guía de arquitectura de administración de memoria](../../relational-databases/memory-management-architecture-guide.md#effects-of-min-and-max-server-memory).

Si se omite esta página durante la instalación, el valor de **memoria de servidor máxima** que se usa es el valor predeterminado de [!INCLUDE[ssde_md](../../includes/ssde_md.md)] (2 147 483 647 megabytes). Puede configurar manualmente estas opciones en esta página si selecciona el botón de radio **Recomendado**. También puede modificar esta configuración después de la instalación. Para obtener más información, vea [Opciones de configuración de memoria del servidor](../../database-engine/configure-windows/server-memory-server-configuration-options.md).

### <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario
  
**Valor predeterminado**: Este botón de radio está seleccionado de forma predeterminada y establece la configuración de la **memoria de servidor mínima** y la **memoria de servidor máxima** en los valores predeterminados de [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. 

**Recomendado**: este botón de radio debe estar seleccionado para aceptar los valores recomendados calculados o para cambiar los valores calculados a los valores configurados por el usuario.  
  
**Memoria de servidor mínima (MB)** : si va a cambiar el valor recomendado calculado a un valor configurado por el usuario, escriba el valor de **Memoria de servidor mínima**.  
  
**Memoria de servidor máxima (MB)** : si va a cambiar el valor recomendado calculado a un valor configurado por el usuario, escriba el valor de **Memoria de servidor máxima**.  

**Haga clic aquí para aceptar las configuraciones de memoria recomendadas para el Motor de base de datos de SQL Server**: Active esta casilla para aceptar las configuraciones de memoria recomendadas calculadas en este servidor. Si se ha seleccionado el botón de radio **Recomendado**, el programa de instalación no puede continuar sin que se active esta casilla.

::: moniker-end

## <a name="database-engine-configuration---filestream-page"></a>Página Configuración del Motor de base de datos - FILESTREAM

Utilice esta página para habilitar FILESTREAM para esta instalación de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. FILESTREAM integra [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] con un sistema de archivos NTFS mediante el almacenamiento de los datos de objetos binarios grandes (BLOB) **varbinary(max)** como archivos del sistema de archivos. [!INCLUDE[tsql](../../includes/tsql-md.md)] pueden insertar, actualizar, consultar, buscar y realizar copias de seguridad de los datos FILESTREAM. Las interfaces del sistema de archivos Win32 de Microsoft proporcionan acceso en streaming a los datos. 
  
### <a name="ui-element-list"></a>Lista de elementos de la interfaz de usuario
  
**Habilitar FILESTREAM para acceso Transact-SQL**: Seleccione esta opción para habilitar FILESTREAM para el acceso a [!INCLUDE[tsql](../../includes/tsql-md.md)] . Debe activar esta casilla para que las demás opciones estén disponibles.  
  
**Habilitar FILESTREAM para el acceso de transmisión por secuencias de E/S de archivos**: Seleccione esta opción para habilitar el acceso de transmisión por secuencias de Win32 para FILESTREAM.  
  
**Nombre de recurso compartido de Windows**: especifique el nombre del recurso compartido de Windows en el que se almacenarán los datos de FILESTREAM.  
  
**Permitir que los clientes remotos tengan acceso de transmisión por secuencias a los datos de FILESTREAM**: active esta casilla para permitir que los clientes remotos tengan acceso a los datos de FILESTREAM en este servidor.  
  
### <a name="see-also"></a>Consulte también

* [Habilitar y configurar FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md)
* [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  

## <a name="database-engine-configuration---user-instance-page"></a>Página Configuración del Motor de base de datos - Instancia de usuario

Use la página **Instancia de usuario** para:

* Generar una instancia independiente de [!INCLUDE[ssDE](../../includes/ssde-md.md)] para usuarios sin permisos de administrador
* Agregar usuarios al rol de administrador  
  
### <a name="options"></a>Opciones
  
**Habilitar instancias de usuario**: el valor predeterminado es activado. Para desactivar la capacidad de habilitar instancias de usuario, desactive la casilla.  
  
La instancia de usuario, también denominada instancia secundaria o cliente, es una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generada por la instancia primaria (la instancia principal que se ejecuta como servicio, como [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]) en nombre de un usuario. La instancia de usuario se ejecuta como un proceso de usuario en el contexto de seguridad de dicho usuario. La instancia de usuario está aislada de la instancia principal y de las demás instancias de usuario que se ejecutan en el equipo. La característica de instancias de usuario también se denomina "Ejecutar como usuario normal" (RANU).  
  
> [!NOTE]  
> Los inicios de sesión aprovisionados como miembros del rol fijo de servidor **sysadmin** durante la instalación se aprovisionan como administradores en la base de datos de plantilla. Son miembros del rol fijo de servidor **sysadmin** en la instancia de usuario a menos que se quiten.  
  
**Agregar usuario al rol Administrador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** :  El valor predeterminado es desactivado. Para agregar el usuario del programa de instalación actual al rol Administrador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], active la casilla.  
  
Los usuarios de [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] que sean miembros de BUILTIN\Administrators no se agregan automáticamente al rol fijo de servidor **sysadmin** al conectarse a [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]. Solo los usuarios de [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] que se hayan agregado de forma explícita a un rol Administrador de servidor pueden administrar [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]. Los miembros del grupo Built-In\Users pueden conectarse a la instancia de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], pero tendrán permisos limitados para hacer tareas de base de datos. Por este motivo, a los usuarios que hereden los privilegios de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] de los grupos BUILTIN\Administrators y Built-In\Users de versiones anteriores de Windows se les debe conceder de forma explícita privilegios administrativos a las instancias de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] que se ejecuten en [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)].  
  
Para realizar cambios en los roles de usuario después de que finalice el programa de instalación, use [SQL Server Management Studio](../../relational-databases/security/authentication-access/join-a-role.md) o [Transact-SQL](../../t-sql/statements/alter-role-transact-sql.md).
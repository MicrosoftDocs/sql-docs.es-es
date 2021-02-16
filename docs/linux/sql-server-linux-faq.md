---
title: Preguntas más frecuentes sobre SQL Server en Linux
description: En este artículo se ofrecen respuestas a las preguntas más frecuentes (P+F) sobre SQL Server en Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/23/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 68f135dfbc2b03cfb27c7ab8b14345f0ffcc32db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348132"
---
# <a name="sql-server-on-linux-frequently-asked-questions-faq"></a>Preguntas más frecuentes sobre SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En las secciones siguientes se proporcionan preguntas y respuestas comunes sobre SQL Server en Linux.

## <a name="general-questions"></a>Preguntas generales

1. **¿Qué plataformas Linux se admiten?**

   SQL Server actualmente se admite en Red Hat Enterprise Server, SUSE Linux Enterprise Server y Ubuntu. También se puede ejecutar en un contenedor con Docker. Para obtener la información más reciente sobre las versiones admitidas, consulte [Plataformas compatibles](sql-server-linux-setup.md#supportedplatforms).

1. **¿SQL Server en Linux funcionará en otras plataformas?**

   SQL Server se ha probado y se admite en Linux en las distribuciones indicadas anteriormente. Existen otras distribuciones de Linux muy relacionadas en las que podría ejecutarse SQL Server (por ejemplo, CentOS está estrechamente ligado a Red Hat Enterprise Server). Si decide instalar SQL Server en un sistema operativo no compatible, revise la sección **Directiva de soporte técnico** de la [Directiva de soporte técnico de Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) a fin de comprender las repercusiones para el soporte técnico. Tenga en cuenta también que algunas distribuciones de Linux mantenidas por la comunidad no tienen una manera formal de recibir soporte técnico si el sistema operativo subyacente es el problema.

1. **¿Es SQL Server en Linux igual que en Windows?**

   El Motor de base de datos principal de SQL Server es el mismo en Linux que en Windows. Aun así, algunas características no se admiten actualmente en Linux. Para obtener una lista de las características que no se admiten en Linux, consulte las [características y servicios no admitidos](sql-server-linux-editions-and-components-2019.md#Unsupported). Revise también los [problemas conocidos](sql-server-linux-release-notes.md#known-issues). A menos que se especifique en estas listas, se admiten en Linux otras características y servicios de SQL Server.

1. **¿Cuál es la directiva de soporte técnico de SQL Server?**

   Para conocer la directiva de soporte técnico, revise la [Directiva de soporte técnico de SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

1. **Estoy acostumbrado a usar SQL Server en Windows. ¿Hay recursos que me puedan ayudar a aprender a usar SQL Server en Linux?**

   Las [guías de inicio rápido](sql-server-linux-setup.md#platforms) proporcionan instrucciones paso a paso sobre cómo instalar SQL Server en Linux y ejecutar consultas de Transact-SQL. Otros tutoriales contienen instrucciones adicionales sobre el uso de SQL Server en Linux. Para obtener una lista de sugerencias de terceros, vea la [lista de MSSQLTIPS de sugerencias sobre SQL Server en Linux](https://www.mssqltips.com/sql-server-tip-category/226/sql-server-on-linux/).

## <a name="licensing"></a>Licencias

1. **¿Cómo funciona la concesión de licencias en Linux?**

   La licencia de SQL Server se concede de la misma manera para Windows y Linux. De hecho, primero se concede la licencia para SQL Server y, luego, se puede optar por usarla en la plataforma que se prefiera. Para obtener más información, vea [Cómo obtener una licencia de SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

1. **¿Qué edición de SQL Server debo elegir después de comprarlo?**

   Cuando ejecute el programa de configuración mssql-conf, se le mostrarán las siguientes opciones:
   
   ```
   Choose an edition of SQL Server:
      1. Evaluation (free, no production use rights, 180-day limit)
      2. Developer (free, no production use rights)
      3. Express (free)
      4. Web (PAID)
      5. Standard (PAID)
      6. Enterprise (PAID)
      7. Enterprise Core (PAID)
      8. I bought a license through a retail sales channel and have a product key to enter.
   ```
     
   Si ha obtenido la licencia a través del programa de licencias por volumen como parte de un Contrato Enterprise o a través de su suscripción a MSDN, debe seleccionar las opciones de 4 a 7. En este paso no se le pide que especifique la licencia, pero debe haber adquirido previamente la licencia adecuada para la configuración. Si ha comprado la edición Standard a través de un canal de venta directa, seleccione la opción 8. Esta opción le pedirá que especifique una clave. 

1. **¿Cómo puedo comprobar la versión instalada y la edición de SQL Server en Linux?**

   Conéctese a la instancia de SQL Server con una herramienta de cliente como **sqlcmd**, **mssql-cli** o Visual Studio Code. Después, ejecute la siguiente consulta de Transact-SQL para comprobar la versión y la edición de SQL Server que está ejecutando: 

   ```sql
   SELECT @@VERSION
   SELECT SERVERPROPERTY('Edition')
   ```

## <a name="installation"></a>Instalación

1. **¿Cómo puedo instalar SQL Server en mis servidores Linux?**

   Microsoft mantiene repositorios de paquetes para instalar SQL Server y admite la instalación a través de administradores de paquetes nativos como yum, zypper y apt. Para instalarlo rápidamente, consulte una de las [guías de inicio rápido](sql-server-linux-setup.md#platforms).

1. **¿Puedo instalar SQL Server en el subsistema de Linux para Windows 10?**

   No. Linux en ejecución en Windows 10 no es actualmente una plataforma compatible con SQL Server y herramientas relacionadas.

1. **¿Qué sistemas de archivos de Linux puede usar SQL Server para los archivos de datos?**

   Actualmente SQL Server en Linux admite ext4 y XFS. En el futuro se agregará compatibilidad con otros sistemas de archivos según sea necesario.

1. **¿Puedo descargar los paquetes de instalación para instalar SQL Server sin conexión?**

   Sí. Para obtener más información, vea los vínculos de descarga de los paquetes en las [notas de la versión](sql-server-linux-release-notes.md). Revise también las [instrucciones para instalaciones sin conexión](sql-server-linux-setup.md#offline).

1. **¿Puedo realizar una instalación desatendida de SQL Server en Linux?**

   Sí. Para obtener una explicación sobre la instalación desatendida, consulte la [guía de instalación de SQL Server en Linux](sql-server-linux-setup.md#unattended). Vea los scripts de ejemplo para [Red Hat](sample-unattended-install-redhat.md), [SUSE Linux Enterprise Server](sample-unattended-install-suse.md) y [Ubuntu](sample-unattended-install-ubuntu.md). También puede consultar [este script de ejemplo](/archive/blogs/sqlcat/unattended-install-and-configuration-for-sql-server-2017-on-linux) que ha creado el equipo de asesoramiento al cliente de SQL Server.

## <a name="tools"></a>Herramientas

1. **¿Puedo usar el cliente de SQL Server Management Studio en Windows para acceder a SQL Server en Linux?**

   Sí, puede usar todas las herramientas existentes que se ejecutan en Windows para acceder a SQL Server en Linux. Entre estas se incluyen herramientas de Microsoft, como SQL Server Management Studio (SSMS), SQL Server Data Tools (SSDT) y OSS, así como herramientas de terceros.

1. **¿Hay alguna herramienta como SSMS que se ejecute en Linux?**

   Azure Data Studio es una nueva herramienta multiplataforma para administrar SQL Server. Para obtener más información, vea [¿Qué es Azure Data Studio?](../azure-data-studio/what-is-azure-data-studio.md)

1. **¿Hay disponibles en Linux comandos como sqlcmd y bcp?**

   Sí, [sqlcmd y bcp](sql-server-linux-setup-tools.md) están disponibles de forma nativa en Linux, macOS y Windows. Además, puede usar la nueva herramienta de línea de comandos [mssql-scripter](https://github.com/Microsoft/mssql-scripter) en Linux, macOS o Windows para generar scripts de T-SQL para una base de datos SQL que se ejecute en cualquier lugar. Consulte también la versión preliminar de [mssql-cli](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/).

1. **¿Es posible ver el Monitor de actividad cuando se conecta a través de SSMS en Windows para una instancia que se ejecuta en Linux?**

   Sí, puede usar SSMS en Windows para conectarse de forma remota y usar herramientas o características como comandos del Monitor de actividad en una instancia de Linux.

1. **¿Qué herramientas hay disponibles para supervisar el rendimiento de SQL Server en Linux?**

   Puede usar [vistas de administración dinámica (DMV) del sistema](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) para recopilar diversos tipos de información sobre SQL Server, incluida información de procesos de Linux. Puede usar el [Almacén de consultas](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md) para mejorar el rendimiento de las consultas. Otras herramientas, como el [Panel de rendimiento](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in) integrado, funcionan de forma remota en SQL Server Management Studio (SSMS) desde Windows.

   > [!TIP]
   > Una manera de mejorar el rendimiento consiste en configurar correctamente el sistema operativo Linux y la instancia de SQL Server. Para obtener más información, consulte [Procedimientos recomendados e instrucciones de configuración de SQL Server en Linux](sql-server-linux-performance-best-practices.md).

## <a name="administration"></a>Administración

1. **¿Ha creado Microsoft alguna aplicación como el Administrador de configuración de SQL Server en Linux?**

   Sí, hay una herramienta de configuración para SQL Server en Linux: [mssql-conf](sql-server-linux-configure-mssql-conf.md).

1. **¿Admite SQL Server en Linux varias instancias en el mismo host?**

   Se recomienda ejecutar varios contenedores en un host para tener varias instancias distintas. Esto se consigue fácilmente con Docker, pero cada contenedor debe escuchar en un puerto diferente. Para obtener más información, vea [Ejecutar varios contenedores de SQL Server](./sql-server-linux-docker-container-deployment.md#multiple).

1. **¿Se admite la autenticación de Active Directory en Linux?**

   Sí. Para obtener más información, vea [Usar la autenticación de Active Directory con SQL Server en Linux](sql-server-linux-active-directory-authentication.md).

1. **¿Se admiten Always On y la agrupación en clústeres en Linux?**

   Los clústeres de conmutación por error y la alta disponibilidad en Linux se logran mediante Pacemaker en Linux. Para obtener más información, consulte [Continuidad empresarial y recuperación de bases de datos: SQL Server en Linux](sql-server-linux-business-continuity-dr.md).

1. **¿Es posible configurar la replicación de Linux a Windows y viceversa?**

   Las réplicas de escalado de lectura se pueden usar entre Windows y Linux para la replicación de datos unidireccional.

1. **¿Es posible migrar de Windows a Linux bases de datos existentes en versiones anteriores de SQL Server?**

   Sí, hay [varios métodos](sql-server-linux-migrate-overview.md) para conseguirlo.

1. **¿Puedo migrar mis datos de Oracle y otros motores de base de datos a SQL Server en Linux?**

   Sí. SSMA admite la migración desde varios tipos de motores de base de datos: Microsoft Access, DB2, MySQL, Oracle y SAP ASE (anteriormente conocido como SAP Sybase ASE). Para obtener un ejemplo de cómo se usa SSMA, vea [Migración de un esquema de Oracle a SQL Server en Linux con SQL Server Migration Assistant](../ssma/oracle/sql-server-linux-convert-from-oracle.md?toc=/sql/toc/toc.json).

1. **¿Qué permisos son necesarios para los archivos de SQL Server?**

   Todos los archivos de la carpeta de archivos `/var/opt/mssql` deben ser propiedad del usuario **mssql** y pertenecer al grupo **mssql**. Tanto el usuario como el grupo **mssql** deben tener permisos de lectura y escritura para todos los archivos y directorios. Tenga en cuenta los siguientes escenarios especiales que implican permisos de archivo y directorio:

   * Los permisos para el propietario y el grupo mssql son necesarios para los recursos compartidos de red montados que se usan para almacenar archivos de SQL Server.
   * Si encuentra archivos de base de datos o copias de seguridad en un directorio que no sea el predeterminado, también debe establecer permisos para ese directorio.
   * Si cambia la umask raíz predeterminada (0022), se produce un error en la configuración de SQL Server después de la instalación. Después, debe aplicar manualmente los permisos necesarios para la cuenta de inicio de SQL Server.

1. **¿Se puede cambiar la propiedad de los archivos y los directorios de SQL Server de la cuenta y el grupo de mssql instalados?**

   No permite cambiar la propiedad del directorio y los archivos de SQL Server de la instalación predeterminada. La cuenta y el grupo de mssql se usan específicamente para SQL Server y no tienen acceso de inicio de sesión interactivo.
   
 1. **¿Se admiten vínculos simbólicos para directorios de registro y los datos de SQL Server?** 
    
    No, no se admiten vínculos simbólicos para directorios de registro y los datos de SQL Server. Para cambiar los directorios de registros y datos predeterminados, consulte [Cambio de la ubicación predeterminada del directorio de registro o los datos](sql-server-linux-configure-mssql-conf.md#datadir).
    
[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]
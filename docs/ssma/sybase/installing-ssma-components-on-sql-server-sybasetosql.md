---
description: Instalación de componentes de SSMA en SQL Server (SybaseToSQL)
title: Instalación de componentes de SSMA en SQL Server (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/14/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5ad9e12c-2cdb-4dd2-8703-05a23242d19d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 82f5d7eef228326f0dba9b194b87bffce4bb5eff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078239"
---
# <a name="installing-ssma-components-on-sql-server-sybasetosql"></a>Instalación de componentes de SSMA en SQL Server (SybaseToSQL)

Además de instalar SSMA, para usar la migración de datos del lado servidor, también debe instalar los componentes en el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Estos componentes incluyen el paquete de extensión SSMA, que admite la migración de datos y los proveedores de Sybase para habilitar la conectividad de servidor a servidor.

## <a name="ssma-for-sybase-extension-pack"></a>SSMA para el paquete de extensiones de Sybase

El paquete de extensión SSMA agrega las bases de datos, **sysdb** y **ssmatesterdb_syb**, a la instancia especificada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La base de datos **sysdb** contiene las tablas y los procedimientos almacenados necesarios para migrar los datos. La base de datos de **ssmatester_syb** contiene el esquema **ssma_sybase_utilities**, en el que se crean los objetos (tablas, desencadenadores, vistas) utilizados por el componente de evaluador SSMA.

Además, al migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crea [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trabajos del agente cuando se usa el motor de migración de datos del lado servidor para migrar los datos.

### <a name="prerequisites"></a>Prerrequisitos

Antes de instalar los componentes de servidor de SSMA para Sybase en, asegúrese de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que el sistema cumple los requisitos siguientes:

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la instancia de está instalada.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o una versión posterior.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] versión 4.7.2 o una versión posterior. Puede obtenerlo en el [Centro para desarrolladores de .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- El proveedor de Sybase OLE DB/ADO.Net/ODBC y la conectividad con el servidor de base de datos de SAP ASE que contiene las bases de datos que desea migrar. Puede instalar proveedores desde los medios del producto de SAP ASE. Para obtener información sobre la conectividad, consulte [conexión a Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).
- El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio Browser debe estar en ejecución durante la instalación. Se utiliza para rellenar una lista de las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el Asistente para la instalación. Puede deshabilitar el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio Browser después de la instalación.

  > [!NOTE]
  > Si el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio explorador de se está ejecutando, pero todavía no ve una lista de instancias en el programa de instalación, debe desbloquear el puerto UDP 1434. Puede usar el Firewall de Windows para desbloquear temporalmente el puerto o puede deshabilitar temporalmente el Firewall de Windows. Es posible que también tenga que deshabilitar temporalmente el software antivirus. Asegúrese de habilitar los firewalls y el software antivirus después de la instalación.

### <a name="installing-the-extension-pack"></a>Instalación del paquete de extensión

Puede instalar el paquete de extensión en cualquier momento antes de migrar los datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

> [!IMPORTANT]
> Para instalar el paquete de extensión, debe ser miembro del rol de servidor sysadmin en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Para instalar el paquete de extensión:

1. Copie **SSMAforSybaseExtensionPack_ *n*. msi**, donde *n* es el número de compilación, en el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
2. Haga doble clic en **SSMAforSybaseExtensionPack_ *n*. msi**.
3. En la página **principal**, haga clic en **Siguiente**.
4. En la página contrato de licencia para el **usuario final** , lea el contrato de licencia. Si está de acuerdo, seleccione la opción acepto **el contrato** y, a continuación, haga clic en **siguiente**.
5. En la página **elegir tipo de instalación** , haga clic en **típica**.
6. En la página **Preparado para instalar**, haga clic en **Instalar**.
7. En la página **finalización del primer paso de la instalación** , haga clic en **siguiente**.

   Aparecerá un nuevo cuadro de diálogo en el que podrá seleccionar la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la instalación del paquete de extensión.

8. Seleccione la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde va a migrar las bases de datos de SAP ase y, a continuación, haga clic en **siguiente**.

   La instancia predeterminada tiene el mismo nombre que el equipo. Las instancias con nombre van seguidas de una barra diagonal inversa y el nombre de la instancia.

9. En la página conexión, seleccione el método de autenticación y, a continuación, haga clic en **siguiente**.

   La autenticación de Windows utilizará las credenciales de Windows para intentar iniciar sesión en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si selecciona autenticación de servidor, debe especificar un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nombre de inicio de sesión y una contraseña.

10. El paso siguiente requiere que establezca la contraseña de una clave maestra que se usará para cifrar los datos confidenciales almacenados en la base de datos del paquete de extensión durante la migración de datos del lado servidor. Proporcione una contraseña segura y haga clic en **siguiente**.

11. En la página siguiente, seleccione **instalar las utilidades de la base de datos *n* e instalar las bibliotecas de paquetes de extensiones**, donde *n* es el número de versión. Si piensa usar la característica de evaluador, active la casilla **instalar base de datos de prueba** y, a continuación, seleccione **siguiente**.

    La base de datos **sysdb** se crea con las tablas y los procedimientos almacenados necesarios para la migración de datos (mediante el motor de migración de datos del servidor) se crean en esta base de datos.

    Si la opción **instalar base de datos de prueba** está activada, se creará la base de datos **ssmatesterdb_syb** .

12. Una vez completada la instalación, aparecerá un mensaje preguntándole si desea instalar la base de datos de utilidades en otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , seleccione **sí** y, a continuación, haga clic en **siguiente**, o bien para salir del asistente, seleccione **no** y, a continuación, **salir**.

### <a name="sql-server-database-objects"></a>SQL Server objetos de base de datos

Después de instalar el paquete de extensión, verá una tabla de **_migration_packages de ssma_syb. BCP** en la base de datos **sysdb** . También verá los siguientes procedimientos almacenados:

- `bcp_clean_migration_data`
- `bcp_ensure_message_table`
- `bcp_insert_new_message`
- `bcp_post_process`
- `bcp_read_new_migration_messages`
- `bcp_save_migration_package`
- `bcp_smart_truncate`
- `bcp_start_migration_process`
- `get_jobstep_info`
- `stop_agent_process`

Cada vez que se migran datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crea un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trabajo del agente. Estos trabajos se denominan **ssma_syb paquete de migración de datos {GUID}** y se pueden ver en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nodo agente de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en la carpeta trabajos.  

## <a name="sybase-providers"></a>Proveedores de Sybase

Cuando se usa la migración de datos del lado servidor para trasladar datos de SAP ASE a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , los datos se migran directamente entre SAP ase y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . No pasa por SSMA, ya que esto ralentizaría la migración de datos.

### <a name="installing-the-sybase-providers"></a>Instalación de los proveedores de Sybase

Las instrucciones siguientes proporcionan los pasos de instalación básica para instalar proveedores de Sybase. Las instrucciones exactas variarán en función de la versión del programa de instalación de Sybase.

> [!IMPORTANT]
> Antes de ejecutar el programa de instalación, compruebe que no está infringiendo los contratos de licencia.

1. Ejecute el programa de instalación de Sybase ASE.
2. Seleccione instalación personalizada.
3. En la página selección de características, seleccione los proveedores de datos ODBC, OLE DB y ADO.NET.
4. Compruebe las características seleccionadas y, a continuación, haga clic en **Finalizar** para instalar el proveedor de datos.

## <a name="see-also"></a>Consulte también

- [Instalación de SSMA para el cliente de Sybase](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)
- [Migración de bases de datos de Sybase ASE a SQL Server-Azure SQL Database](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)

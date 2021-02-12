---
title: Instalación de componentes de SSMA en SQL Server (MySQLToSql) | Microsoft Docs
description: Instale los componentes en el servidor que ejecuta SQL Server para admitir la conversión de la base de datos MySQL con SSMA, incluidos el paquete de extensiones de SSMA y los proveedores de MySQL.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- SSMA extension pack, Installation
ms.assetid: 6772d0c5-258f-4d7b-afb0-b5f810e71af1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 48c111ab342d7410031851a5635fa73fe143b96b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070200"
---
# <a name="installing-ssma-components-on-sql-server-mysqltosql"></a>Instalación de componentes de SSMA en SQL Server (MySQLToSql)

Además de instalar SSMA, también debe instalar los componentes en el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Estos componentes incluyen el paquete de extensión SSMA, que admite la migración de datos y los proveedores de MySQL para habilitar la conectividad de servidor a servidor.

## <a name="ssma-for-mysql-extension-pack"></a>SSMA para el paquete de extensiones de MySQL

El paquete de extensión SSMA agrega una base de datos, **sysdb**, a la instancia especificada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Esta base de datos contiene las tablas y los procedimientos almacenados necesarios para migrar los datos.

Además, al migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crea [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trabajos del agente, cuando el motor de migración de datos del lado servidor se usa para migrar los datos.

### <a name="prerequisites"></a>Prerrequisitos

Antes de instalar los componentes de SSMA para MySQL Server en, asegúrese de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que el equipo cumple los requisitos siguientes:

- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o una versión posterior.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] versión 4.7.2 o una versión posterior. Puede obtenerlo en el [Centro para desarrolladores de .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- El proveedor de cliente de MySQL y la conectividad a la base de datos MySQL que desea migrar. Puede instalar proveedores en el sitio web de MySQL o en el soporte del producto de MySQL.
- El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio Browser debe estar en ejecución durante la instalación. Se utiliza para rellenar una lista de las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el Asistente para la instalación. Puede deshabilitar el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio Browser después de la instalación.  

  > [!NOTE]
  > Si el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio explorador de se está ejecutando, pero todavía no ve una lista de instancias en el programa de instalación, debe desbloquear el puerto UDP 1434. Puede usar el Firewall de Windows para desbloquear temporalmente el puerto o puede deshabilitar temporalmente el Firewall de Windows. Es posible que también tenga que deshabilitar temporalmente el software antivirus. Asegúrese de habilitar los firewalls y el software antivirus después de la instalación.

### <a name="installing-the-extension-pack"></a>Instalación del paquete de extensión

Puede instalar el paquete de extensión en cualquier momento antes de migrar los datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

> [!IMPORTANT]
> Para instalar el paquete de extensión, debe ser miembro del rol de servidor **sysadmin** en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Para instalar el paquete de extensión:

1. Copie SSMA para **SSMAforMySQLExtensionPack_ *n*. msi**, donde *n* es el número de compilación, en el equipo que ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
2. Haga doble clic en **SSMAforMySQLExtensionPack_ *n*. msi**.
3. En el cuadro de diálogo de **bienvenida** , haga clic en **siguiente**.
4. En el cuadro de diálogo contrato de licencia para el **usuario final** , lea el contrato de licencia. Si está de acuerdo, seleccione la opción acepto **el contrato** y, a continuación, haga clic en **siguiente**.
5. En el cuadro de diálogo **elegir tipo de instalación** , haga clic en **típica**.
6. En el cuadro de diálogo **listo para instalar** , haga clic en **instalar**.
7. En el cuadro **de diálogo finalización del primer paso de la instalación** , haga clic en **siguiente**.

   Aparecerá un nuevo cuadro de diálogo en el que podrá seleccionar la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la instalación del paquete de extensión.
  
8. Seleccione la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde va a migrar los esquemas de MySQL y, a continuación, haga clic en **siguiente**.
  
   La instancia predeterminada tiene el mismo nombre que el equipo. Las instancias con nombre van seguidas de una barra diagonal inversa y el nombre de la instancia.

9. En la página conexión, seleccione el método de autenticación y, a continuación, haga clic en **siguiente**.
  
    La autenticación de Windows utilizará las credenciales de Windows para intentar iniciar sesión en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si selecciona autenticación de servidor, debe especificar un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nombre de inicio de sesión y una contraseña.

10. El paso siguiente requiere que establezca la contraseña de una clave maestra que se usará para cifrar los datos confidenciales almacenados en la base de datos del paquete de extensión durante la migración de datos del lado servidor. Proporcione una contraseña segura y haga clic en **siguiente**.

11. En el siguiente cuadro de diálogo, seleccione instalar las utilidades de la **base de datos *n* e instalar bibliotecas de paquetes de extensiones**, donde *n* es el número de versión y, a continuación, haga clic en **siguiente**.

    La base de datos **sysdb** se crea con las tablas y los procedimientos almacenados necesarios para la migración de datos (mediante el motor de migración de datos del servidor) se crean en esta base de datos.

12. Para instalar las utilidades en otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , seleccione **sí** y, a continuación, haga clic en **siguiente**. O bien, para salir del asistente, haga clic en **no**.

## <a name="see-also"></a>Consulte también

- [Instalación de SSMA para el cliente MySQL](../../ssma/mysql/installing-ssma-for-mysql-client-mysqltosql.md)
- [Migración de bases de datos de MySQL a SQL Server-Azure SQL Database](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)

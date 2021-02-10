---
title: Conexión a SAP ASE (SybaseToSQL) | Microsoft Docs
description: Obtenga información sobre cómo conectarse a un servidor adaptable para migrar una base de datos SAP Adaptive Server Enterprise (ASE) a SQL Server o Azure SQL Database.
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to Sybase
ms.assetid: a45a2330-9175-4c9e-af38-ef920e350614
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7afb893d194b874da4cbb6482df4ba8a16e8c0a7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056890"
---
# <a name="connecting-to-sap-ase-sybasetosql"></a>Conexión a SAP ASE (SybaseToSQL)

Para migrar las bases de datos de SAP Adaptive Server Enterprise (ASE) a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, debe conectarse al servidor adaptable que contiene las bases de datos que desea migrar. Cuando se conecta, SSMA obtiene los metadatos de todas las bases de datos en el servidor adaptable y muestra los metadatos de la base de datos en el panel Explorador de metadatos de Sybase. SSMA almacena información sobre el servidor de base de datos, pero no almacena contraseñas.  
  
La conexión a ASE permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse a ASE si desea una conexión activa con el servidor.  
  
Los metadatos sobre el servidor adaptable no se actualizan automáticamente. En su lugar, si desea actualizar los metadatos en el explorador de metadatos de Sybase, debe actualizar manualmente los metadatos, como se describe en la sección "actualización de metadatos de Sybase ASE" más adelante en este tema.  
  
## <a name="required-ase-permissions"></a>Permisos de ASE necesarios

La cuenta que se usa para conectarse a ASE debe tener al menos acceso **público** a la base de datos maestra y a cualquier base de datos de origen que se va a migrar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Además, para seleccionar los permisos en las tablas que se van a migrar, el usuario debe tener permisos SELECT en las siguientes tablas del sistema:  
  
- [source_db] .dbo.sysobjetos  
- [source_db] .dbo.syscolumnas  
- [source_db] .dbo.sysusuarios  
- [source_db] tipos de .dbo.sys  
- [source_db] restricciones de .dbo.sys  
- [source_db] .dbo.syscomentarios  
- [source_db] índices de .dbo.sys  
- [source_db] referencias de .dbo.sys  
- Bases de datos de master.dbo.sys  
  
## <a name="establishing-a-connection-to-ase"></a>Establecer una conexión con ASE

Cuando se conecta a un servidor adaptable, SSMA Lee los metadatos de la base de datos en el servidor de base de datos y, a continuación, agrega estos metadatos al archivo de proyecto. SSMA utiliza estos metadatos cuando convierte los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sintaxis o SQL Azure, y cuando migra datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Puede examinar estos metadatos en el panel Explorador de metadatos de Sybase y revisar las propiedades de los objetos de base de datos individuales.  
  
> [!IMPORTANT]  
> Antes de intentar conectarse al servidor de base de datos, asegúrese de que el servidor de base de datos se está ejecutando y de que puede aceptar conexiones.  
  
**Para conectarse a Sybase ASE**
  
1. En el menú **archivo** , seleccione **conectarse a Sybase**.  
  
   Si anteriormente se conectó a Sybase, el nombre del comando se volverá **a conectar a Sybase**.  
  
2. En el cuadro **proveedor** , seleccione cualquiera de los proveedores instalados en el equipo para conectarse al servidor de Sybase.  
  
3. En el cuadro **modo** , seleccione **modo estándar** o **modo avanzado**.  
  
   Utilice el modo estándar para especificar el nombre del servidor, el puerto, el nombre de usuario y la contraseña. Use el modo avanzado para proporcionar una cadena de conexión. Este modo se usa normalmente solo para solucionar problemas o para trabajar con el soporte técnico.  
  
4. Si selecciona el **modo estándar**, proporcione los valores siguientes:  
  
    1. En el cuadro **nombre del servidor** , escriba o seleccione el nombre o la dirección IP del servidor de base de datos.  
    2. Si el servidor de base de datos no está configurado para aceptar conexiones en el puerto predeterminado (5000), escriba el número de puerto que se utiliza para las conexiones de Sybase en el cuadro **Puerto del servidor** .  
    3. En el cuadro **nombre de usuario** , escriba una cuenta de Sybase que tenga los permisos necesarios.  
    4. En el cuadro **contraseña** , escriba la contraseña del nombre de usuario especificado.  
  
5. Si selecciona el **modo avanzado**, proporcione una cadena de conexión en el cuadro **cadena de conexión** .  
  
    A continuación se muestran ejemplos de cadenas de conexión diferentes:  
  
    1. **Cadenas de conexión para el proveedor de OLE DB de Sybase:**  
  
        En el caso de Sybase ASE OLE DB 12,5, una cadena de conexión de ejemplo es la siguiente:  
  
        `Server Name=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider=Sybase.ASEOLEDBProvider;`  
  
        En el caso de Sybase ASE OLE DB 15, una cadena de conexión de ejemplo es la siguiente:  
  
        `Server=sybserver;User ID=MyUserID;Password=MyP@$$word;Provider= ASEOLEDB;Port=5000;`  
  
    2. **Cadena de conexión para el proveedor ODBC de Sybase:**  
  
       `Driver=Adaptive Server Enterprise;Server=sybserver;uid=MyUserID;pwd=MyP@$$word;Port=5000;`  
  
    3. **Cadena de conexión para el proveedor de ADO.NET de Sybase:**  
  
       `Server=sybserver;Port=5000;uid=MyUserID;pwd=MyP@$$word;`  
  
    Para obtener más información, consulte [conexión a Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/connect-to-sybase-sybasetosql.md).  
  
## <a name="reconnecting-to-sybase-ase"></a>Volver a conectar a Sybase ASE

La conexión al servidor de base de datos permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse si desea una conexión activa con el servidor adaptable. Puede trabajar sin conexión hasta que desee actualizar los metadatos, cargar los objetos de base de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure y migrar los datos.  
  
## <a name="refreshing-sybase-ase-metadata"></a>Actualización de metadatos de Sybase ASE

Los metadatos de las bases de datos de ASE no se actualizan automáticamente. Los metadatos de Sybase Metadata Explorer son una instantánea de los metadatos la primera vez que se conectó al servidor adaptable o la última vez que se actualizaron los metadatos manualmente. Puede actualizar manualmente los metadatos de una sola base de datos, un esquema de base de datos único o todas las bases de datos.  
  
**Para actualizar los metadatos**
  
1. Asegúrese de que está conectado al servidor adaptable.  
  
2. En el explorador de metadatos de Sybase, active la casilla situada junto a la base de datos o el esquema de la base de datos que desea actualizar.  
  
3. Haga clic con el botón derecho en bases de datos o en el esquema de base de datos o base de datos individual y seleccione **actualizar desde base de datos**.  
  
4. Si se le pide que compruebe el objeto actual, haga clic en **sí**.  
  
## <a name="next-step"></a>siguiente paso  
  
- El siguiente paso del proceso de migración consiste en [conectarse a una instancia de SQL Server](connecting-to-sql-server-sybasetosql.md)  /  [conectarse a una instancia de SQL Azure](connecting-to-azure-sql-db-sybasetosql.md)  
  
## <a name="see-also"></a>Consulte también

[Migración de bases de datos de Sybase ASE a SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  

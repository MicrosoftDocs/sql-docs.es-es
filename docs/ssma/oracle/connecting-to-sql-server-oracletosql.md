---
title: Conexión a SQL Server (OracleToSQL) | Microsoft Docs
description: Obtenga información acerca de cómo conectarse a SQL Server para migrar una base de datos de Oracle. SSMA obtiene y muestra los metadatos de las bases de datos en SQL Server.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to SQL Server,Synchronizing SQL Server Metadata
ms.assetid: 1b2a8059-1829-4904-a82f-9c06de1e245f
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: e9dbff4115cf58723d186f1024f06d39c2eab1b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058830"
---
# <a name="connecting-to-sql-server-oracletosql"></a>Conexión a SQL Server (OracleToSQL)

Para migrar bases de datos de Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe conectarse a la instancia de de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cuando se conecta, SSMA obtiene los metadatos de todas las bases de datos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y muestra los metadatos de la base de datos en el **Explorador de metadatos de SQL Server**. SSMA almacena información acerca de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que está conectado, pero no almacena contraseñas.

La conexión a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si desea una conexión activa con el servidor. Puede trabajar sin conexión hasta que cargue los objetos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de base de datos en y migre los datos.

Los metadatos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se sincronizan automáticamente. En su lugar, para actualizar los metadatos en **SQL Server explorador de metadatos**, debe actualizar manualmente los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatos. Para obtener más información, vea la sección "sincronizar metadatos de SQL Server" más adelante en este tema.

## <a name="required-sql-server-permissions"></a>Permisos SQL Server necesarios

La cuenta que se usa para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere permisos diferentes en función de las acciones que realiza la cuenta:

- Para convertir objetos de Oracle en [!INCLUDE[tsql](../../includes/tsql-md.md)] sintaxis, actualizar metadatos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o guardar la sintaxis convertida en scripts de, la cuenta debe tener permiso para iniciar sesión en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- Para cargar los objetos de base de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la cuenta debe ser miembro del rol de base de datos **db_ddladmin** .

- Para migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la cuenta debe:
  - Miembro del rol de base de datos **db_owner** , si se usa el motor de migración de datos del lado cliente.
  - Miembro del rol de servidor **sysadmin** , si usa el motor de migración de datos del lado servidor. Esto es necesario para crear el `CmdExec` [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] paso de trabajo del agente durante la migración de datos para ejecutar la herramienta de copia masiva de SSMA.
    > [!NOTE]
    > [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] La migración de datos del servidor no admite las cuentas de proxy del agente.

- Para ejecutar el código generado por SSMA, la cuenta debe tener `EXECUTE` permisos para todas las funciones definidas por el usuario en el esquema de **ssma_oracle** de la base de datos de destino. Estas funciones proporcionan una funcionalidad equivalente de las funciones del sistema de Oracle y las usan los objetos convertidos.

## <a name="establishing-a-sql-server-connection"></a>Establecer una conexión SQL Server

Antes de convertir los objetos de base de datos de Oracle a la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sintaxis de, debe establecer una conexión con la instancia de en la que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desea migrar la base de datos o las bases de datos de Oracle.

Al definir las propiedades de conexión, también se especifica la base de datos en la que se migrarán los objetos y los datos. Puede personalizar esta asignación en el nivel de esquema de Oracle después de conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, vea [asignar esquemas de Oracle a esquemas de SQL Server &#40;&#41;OracleToSQL ](../../ssma/oracle/mapping-oracle-schemas-to-sql-server-schemas-oracletosql.md).

> [!IMPORTANT]
> Antes de intentar conectarse a, asegúrese de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando y puede aceptar conexiones.

Para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

1. En el menú **archivo** , seleccione **conectarse a SQL Server**.
   Si anteriormente se conectó a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el nombre del comando se volverá **a conectar a SQL Server**.

2. En el cuadro de diálogo conexión, escriba o seleccione el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   - Si se está conectando a la instancia predeterminada en el equipo local, puede escribir `localhost` o un punto ( `.` ).
   - Si se va a conectar a la instancia predeterminada en otro equipo, escriba el nombre del equipo.
   - Si se va a conectar a una instancia con nombre en otro equipo, escriba el nombre del equipo seguido de una barra diagonal inversa y, a continuación, el nombre de la instancia, por ejemplo, `MyServer\MyInstance` .

3. Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está configurada para aceptar conexiones en un puerto no predeterminado, escriba el número de puerto que se utiliza para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] las conexiones en el cuadro **Puerto del servidor** . Para la instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el número de puerto predeterminado es 1433. En el caso de las instancias con nombre, SSMA intentará obtener el número de puerto del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio explorador.

4. En el cuadro **base de datos** , escriba el nombre de la base de datos de destino.
   Esta opción no está disponible cuando se vuelve a conectar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

5. En el cuadro **autenticación** , seleccione el tipo de autenticación que se utilizará para la conexión. Para usar la cuenta de Windows actual, seleccione **autenticación de Windows**. Para usar un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión, seleccione **SQL Server autenticación** y proporcione el nombre de inicio de sesión y la contraseña.

6. Para una conexión segura, se agregan dos controles, las casillas de verificación **cifrar conexión** y **TrustServerCertificate** . Solo cuando se activa **cifrar conexión** , la casilla **TrustServerCertificate** está visible. Cuando se activa la opción **cifrar conexión** (true) y **TrustServerCertificate** está desactivado (false), se validará el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] certificado SSL. Validar el certificado de servidor es una parte del protocolo de enlace de SSL y asegurarse de que el servidor es el apropiado al que hay que conectarse. Para garantizar esto, se debe instalar un certificado en el lado cliente y en el lado servidor.

7. Haga clic en **Conectar**.

> [!IMPORTANT]
> Aunque puede conectarse a una versión posterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , en comparación con la versión elegida cuando se creó el proyecto de migración, la conversión de los objetos de base de datos viene determinada por la versión de destino del proyecto y no por la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que está conectado.

## <a name="synchronizing-sql-server-metadata"></a>Sincronizar metadatos de SQL Server

Los metadatos acerca de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] las bases de datos no se actualizan automáticamente. Los metadatos de **SQL Server explorador de metadatos** es una instantánea de los metadatos al conectarse por primera vez a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o la última vez que se actualizaron los metadatos manualmente. Puede actualizar manualmente los metadatos de todas las bases de datos o de cualquier base de datos o objeto de base de datos. Para sincronizar los metadatos:

1. Asegúrese de que está conectado a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

2. En **SQL Server explorador de metadatos**, active la casilla situada junto a la base de datos o el esquema de la base de datos que desea actualizar.
   Por ejemplo, para actualizar los metadatos de todas las bases de datos, active la casilla situada junto a **bases** de datos.

3. Haga clic con el botón derecho en **bases** de datos o en el esquema de base de datos o de base de datos individual y seleccione **sincronizar con base de datos**.
  
## <a name="next-step"></a>siguiente paso

El siguiente paso de la migración depende de las necesidades del proyecto:
  
- Para personalizar la asignación entre esquemas y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de datos de Oracle, vea [asignar esquemas de Oracle a esquemas de SQL Server &#40;&#41;OracleToSQL ](../../ssma/oracle/mapping-oracle-schemas-to-sql-server-schemas-oracletosql.md).
- Para personalizar las opciones de configuración de los proyectos, vea [establecer opciones de proyecto &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md).
- Para personalizar la asignación de los tipos de datos de origen y de destino, vea [asignar tipos de datos de Oracle y SQL Server &#40;&#41;OracleToSQL ](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md).
- Si no tiene que realizar ninguna de estas tareas, puede convertir las definiciones de objetos de base de datos de Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definiciones de objeto. Para obtener más información, vea [convertir esquemas de Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md).
  
## <a name="see-also"></a>Consulte también

[Migrar bases de datos de Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)

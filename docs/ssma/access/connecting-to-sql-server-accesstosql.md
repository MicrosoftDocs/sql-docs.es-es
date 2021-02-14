---
title: Conexión a SQL Server (AccessToSQL) | Microsoft Docs
description: Obtenga información sobre cómo conectarse a una instancia de destino de SQL Database para migrar bases de datos de Access. SSMA obtiene metadatos acerca de las bases de datos en SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- authentication
- instance of SQL Server
- metadata, refreshing
- ports
- refreshing metadata
- spaces in database names
- special characters
- SQL Server
- SQL Server, connecting
- SQL Server, connecting to
- SQL Server, reconnecting
ms.assetid: f84cf007-ddf1-4396-a07c-3e0729abc769
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 59583057641bb167aae197dd9fae3bf6e5833302
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076206"
---
# <a name="connecting-to-sql-server-accesstosql"></a>Conexión a SQL Server (AccessToSQL)

Para migrar bases de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe conectarse a la instancia de de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cuando se conecta, SSMA obtiene los metadatos de las bases de datos en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y muestra los metadatos de la base de datos en **SQL Server explorador de metadatos**. SSMA almacena información acerca de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que está conectado, pero no almacena contraseñas.

La conexión a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si desea una conexión activa con el servidor. Puede trabajar sin conexión hasta que cargue los objetos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de base de datos en y migre los datos.

Los metadatos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se sincronizan automáticamente. En su lugar, para actualizar los metadatos en SQL Server explorador de metadatos, debe actualizar manualmente los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatos. Para obtener más información, vea la sección "sincronizar metadatos de SQL Server" más adelante en este tema.

## <a name="required-sql-server-permissions"></a>Permisos SQL Server necesarios

La cuenta que se usa para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere permisos diferentes en función de las acciones que realiza la cuenta:

- Para convertir objetos de Access en [!INCLUDE[tsql](../../includes/tsql-md.md)] sintaxis, actualizar metadatos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o guardar la sintaxis convertida en scripts de, la cuenta debe tener permiso para iniciar sesión en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- Para cargar los objetos de base de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la cuenta debe ser miembro del rol de base de datos **db_ddladmin** .

- Para migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la cuenta debe ser miembro del rol de base de datos **db_owner** .

## <a name="establishing-a-sql-server-connection"></a>Establecer una conexión SQL Server

Antes de convertir los objetos de base [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de datos de Access a la sintaxis de, debe establecer una conexión con la instancia de en la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que desea migrar las bases de datos de Access.

Al definir las propiedades de conexión, también se especifica la base de datos en la que se migrarán los objetos y los datos. Puede personalizar esta asignación en el nivel de base de datos de Access después de conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, vea [asignación de bases de datos de origen y de destino](mapping-source-and-target-databases-accesstosql.md).

> [!IMPORTANT]
> Antes de conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , asegúrese de que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando y puede aceptar conexiones.

Para conectarse al [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:

1. En el menú **archivo** , seleccione **conectarse a SQL Server**.
   Si anteriormente se conectó a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el nombre del comando se volverá **a conectar a SQL Server**.

2. En el cuadro **nombre del servidor** , escriba o seleccione el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   - Si se está conectando a la instancia predeterminada en el equipo local, puede escribir `localhost` o un punto ( `.` ).
   - Si se va a conectar a la instancia predeterminada en otro equipo, escriba el nombre del equipo.
   - Si se va a conectar a una instancia con nombre, escriba el nombre del equipo, una barra diagonal inversa y el nombre de la instancia. Por ejemplo: `MyServer\MyInstance`.
   - Para conectarse a una instancia de usuario activa de [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)] , conéctese mediante el protocolo de canalizaciones con nombre y especifique el nombre de la canalización, como `\\.\pipe\sql\query` . Para obtener más información, consulte la documentación de [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)] .

3. Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está configurada para aceptar conexiones en un puerto no predeterminado, escriba el número de puerto que se utiliza para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] las conexiones en el cuadro **Puerto del servidor** . Para la instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , el número de puerto predeterminado es 1433. En el caso de las instancias con nombre, SSMA intentará obtener el número de puerto del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio explorador.

4. En el cuadro **base** de datos, escriba el nombre de la base de datos de destino para la migración de datos y objetos.
   Esta opción no está disponible cuando se vuelve a conectar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   El nombre de la base de datos de destino no puede contener espacios ni caracteres especiales. Por ejemplo, puede migrar las bases de datos de Access a una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de datos denominada `abc` . Sin embargo, no puede migrar las bases de datos de Access a una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de datos denominada `a b-c` .
   Puede personalizar esta asignación por base de datos después de conectarse. Para obtener más información, consulte [asignación de bases de datos de origen y de destino](mapping-source-and-target-databases-accesstosql.md) .

5. En el menú desplegable **autenticación** , seleccione el tipo de autenticación que se utilizará para la conexión. Para usar la cuenta de Windows actual, seleccione **autenticación de Windows**. Para usar un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión, seleccione **SQL Server autenticación** y proporcione un nombre de usuario y una contraseña.

6. Para una conexión segura, se agregan dos controles, la casilla **cifrar conexión** y la casilla **TrustServerCertificate** . Solo cuando la casilla **cifrar conexión** está activada la casilla de verificación **TrustServerCertificate** está visible. Cuando se activa la opción **cifrar conexión** (true) y **TrustServerCertificate** está desactivado (false), validará el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] certificado SSL. Validar el certificado de servidor es una parte del protocolo de enlace de SSL y asegurarse de que el servidor es el apropiado al que hay que conectarse. Para garantizar esto, se debe instalar un certificado en el lado cliente y en el lado servidor.

7. Haga clic en **Conectar**.

> [!IMPORTANT]
> Aunque puede conectarse a una versión posterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , en comparación con la versión elegida cuando se creó el proyecto de migración, la conversión de los objetos de base de datos viene determinada por la versión de destino del proyecto y no por la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que está conectado.

## <a name="synchronizing-sql-server-metadata"></a>Sincronizar metadatos de SQL Server

Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] los esquemas cambian después de conectarse, puede sincronizar los metadatos con el servidor de.

Para sincronizar metadatos de SQL Server, **SQL Server el explorador de metadatos**, haga clic con el botón derecho en **bases** de datos y seleccione **sincronizar con base de** datos.

## <a name="reconnecting-to-sql-server"></a>Volver a conectar a SQL Server

La conexión a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si desea una conexión activa con el servidor. Puede trabajar sin conexión hasta que cargue los objetos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de base de datos en y migre los datos.

El procedimiento para volver a conectar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es el mismo que el procedimiento para establecer una conexión.

## <a name="next-steps"></a>Pasos siguientes

Si desea personalizar la asignación entre las bases de datos de origen y de destino, consulte [asignación de bases de datos de origen y de destino](mapping-source-and-target-databases-accesstosql.md) ; en caso contrario, el siguiente paso consiste en convertir los objetos de base de datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis mediante la conversión de objetos de base de [datos](converting-access-database-objects-accesstosql.md).

## <a name="see-also"></a>Consulte también

[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)

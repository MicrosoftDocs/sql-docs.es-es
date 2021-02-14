---
description: Conexión a SQL Server (DB2ToSQL)
title: Conexión a SQL Server (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: b59803cb-3cc6-41cc-8553-faf90851410e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1109bd083476bf9be2421b06637751c34fc6405c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081656"
---
# <a name="connecting-to-sql-server-db2tosql"></a>Conexión a SQL Server (DB2ToSQL)

Para migrar las bases de datos de DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe conectarse a la instancia de de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cuando se conecta, SSMA obtiene los metadatos de todas las bases de datos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y muestra los metadatos de la base de datos en el **Explorador de metadatos de SQL Server**. SSMA almacena información acerca de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que está conectado, pero no almacena contraseñas.

La conexión a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si desea una conexión activa con el servidor. Puede trabajar sin conexión hasta que cargue los objetos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de base de datos en y migre los datos.

Los metadatos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se sincronizan automáticamente. En su lugar, para actualizar los metadatos en **SQL Server explorador de metadatos**, debe actualizar manualmente los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatos. Para obtener más información, vea la sección "sincronizar metadatos de SQL Server" más adelante en este tema.

## <a name="required-sql-server-permissions"></a>Permisos SQL Server necesarios

La cuenta que se usa para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere permisos diferentes en función de las acciones que realiza la cuenta:

- Para convertir objetos DB2 en [!INCLUDE[tsql](../../includes/tsql-md.md)] sintaxis, actualizar metadatos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o guardar la sintaxis convertida en scripts de, la cuenta debe tener permiso para iniciar sesión en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- Para cargar los objetos de base de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la cuenta debe ser miembro del rol de servidor **db_ddladmin** .

- Para migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la cuenta debe ser miembro del rol de base de datos **db_owner** .

- Para ejecutar el código generado por SSMA, la cuenta debe tener `EXECUTE` permisos para todas las funciones definidas por el usuario en el esquema de **ssma_db2** de la base de datos de destino. Estas funciones proporcionan una funcionalidad equivalente de las funciones del sistema DB2 y las usan los objetos convertidos.

## <a name="establishing-a-sql-server-connection"></a>Establecer una conexión SQL Server

Antes de convertir los objetos de base de datos DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis, debe establecer una conexión con la instancia de en la que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desea migrar la base de datos o bases de datos DB2.

Al definir las propiedades de conexión, también se especifica la base de datos en la que se migrarán los objetos y los datos. Puede personalizar esta asignación en el nivel de esquema de DB2 después de conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, vea [asignar esquemas DB2 a esquemas de SQL Server &#40;&#41;DB2ToSQL ](../../ssma/db2/mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).

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

6. Para una conexión segura, se agregan dos controles, las casillas de verificación **cifrar conexión** y **TrustServerCertificate** . Solo cuando se activa **cifrar conexión** , la casilla **TrustServerCertificate** está visible. Cuando se activa la opción **cifrar conexión** (true) y **TrustServerCertificate** está desactivado (false), se validará el certificado SSL SQL Server. Validar el certificado de servidor es una parte del protocolo de enlace de SSL y asegurarse de que el servidor es el apropiado al que hay que conectarse. Para garantizar esto, se debe instalar un certificado en el lado cliente y en el lado servidor.

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

- Para personalizar la asignación entre esquemas y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de datos de DB2, consulte asignación de [esquemas DB2 a esquemas de SQL Server &#40;&#41;DB2ToSQL ](../../ssma/db2/mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).
- Para personalizar las opciones de configuración de los proyectos, vea [configuración del proyecto &#40;conversión&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md) y secciones relacionadas.
- Para personalizar la asignación de los tipos de datos de origen y de destino, consulte [asignación de tipos de datos de DB2 y SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).
- Si no tiene que realizar ninguna de estas tareas, puede convertir las definiciones de objetos de base de datos DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definiciones de objeto. Para obtener más información, vea [convertir esquemas DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md).

## <a name="see-also"></a>Consulte también

[Migrar bases de datos DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)

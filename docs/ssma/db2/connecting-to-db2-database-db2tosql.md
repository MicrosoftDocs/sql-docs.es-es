---
title: Conectar con la base de datos DB2 (DB2ToSQL) | Microsoft Docs
description: Obtenga información acerca de cómo conectarse a una instancia de destino de la base de datos DB2 para migrar bases de datos DB2. SSMA obtiene metadatos acerca de todos los esquemas DB2.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5eb5801d-f0c3-4127-97c0-0b1ef49f4844
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b7a2bd0434e865e3748861b94b21d44615e07ed5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081627"
---
# <a name="connecting-to-db2-database-db2tosql"></a>Conectar con la base de datos DB2 (DB2ToSQL)

Para migrar las bases de datos de DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe conectarse a la base de datos DB2 que desea migrar. Cuando se conecta, SSMA obtiene los metadatos de todos los esquemas DB2 y, a continuación, los muestra en el panel Explorador de metadatos DB2. SSMA almacena información sobre el servidor de base de datos, pero no almacena contraseñas.

La conexión a la base de datos permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse si desea una conexión activa a la base de datos.

Los metadatos de la base de datos DB2 no se actualizan automáticamente. En su lugar, si desea actualizar los metadatos en el explorador de metadatos de DB2, debe actualizarlos manualmente. Para obtener más información, vea la sección "actualizar metadatos de DB2" más adelante en este tema.

## <a name="required-db2-permissions"></a>Permisos de DB2 necesarios

La autorización de usuario define la lista de comandos y objetos que están disponibles para un usuario. Así, esta lista controla las acciones del usuario. En DB2, existen grupos de privilegios predeterminados para la autorización, tanto en el nivel de instancia como en el nivel de una base de datos DB2. Esto permite a SSMA obtener los metadatos de los esquemas que pertenecen al usuario que se conecta. Para obtener los metadatos de los objetos de otros esquemas y, a continuación, convertir los objetos en esos esquemas, la cuenta debe tener los permisos siguientes:

- Normalmente, el acceso al esquema para la migración de esquemas se concede a PUBLIC, a menos que se use la palabra clave RESTRICT en CREATE
- El acceso a datos para la migración de datos requiere acceso a datos

## <a name="establishing-a-connection-to-db2"></a>Establecer una conexión con DB2

Cuando se conecta a una base de datos, SSMA Lee los metadatos de la base de datos y, a continuación, agrega estos metadatos al archivo de proyecto. SSMA utiliza estos metadatos cuando convierte objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis y cuando migra datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede examinar estos metadatos en el panel Explorador de metadatos DB2 y revisar las propiedades de los objetos de base de datos individuales.  

> [!IMPORTANT]
> Antes de intentar conectarse, asegúrese de que el servidor de base de datos se está ejecutando y de que puede aceptar conexiones.

**Para conectarse a DB2**

1. En el menú **archivo** , seleccione **conectarse a DB2**.

   Si anteriormente se conectó a DB2, el nombre del comando se volverá **a conectar a DB2**.

2. En el cuadro **proveedor** verá el **OLE DB proveedor** , que actualmente es el único proveedor de acceso de cliente de DB2.

3. En el cuadro **Administrador** , puede seleccionar **DB2 para zOs**, **DB2 para LUW** o **DB2 para i**

4. En el cuadro **modo** , seleccione **modo estándar** o modo de **cadena de conexión**.

   Utilice el modo estándar para especificar el nombre del servidor y el puerto. Use el modo de nombre de servicio para especificar manualmente el nombre del servicio DB2. Use el modo de cadena de conexión para proporcionar una cadena de conexión completa.

5. Si selecciona el **modo estándar**, proporcione los valores siguientes:

   - En el cuadro **nombre del servidor** , escriba o seleccione el nombre o la dirección IP del servidor de base de datos.
   - Si el servidor de base de datos no está configurado para aceptar conexiones en el puerto predeterminado (1521), escriba el número de puerto que se utiliza para las conexiones DB2 en el cuadro **Puerto del servidor** .
   - En el cuadro **Puerto del servidor** , escriba el número de puerto TCP/IP.
   - En el cuadro **catálogo inicial** , escriba el nombre de la base de datos.
   - En el cuadro **nombre de usuario** , escriba una cuenta de DB2 que tenga los permisos necesarios.
   - En el cuadro **contraseña** , escriba la contraseña del nombre de usuario especificado.

6. Si selecciona el **modo de cadena de conexión**, proporcione una cadena de conexión en el cuadro **cadena de conexión** .

   En el ejemplo siguiente se muestra una cadena de conexión de OLE DB:

   `Provider=OraOLEDB.DB2;Data Source=MyDB2DB;User Id=myUsername;Password=myPassword;`

   En el ejemplo siguiente se muestra una cadena de conexión de cliente DB2 que usa seguridad integrada:
  
   `Data Source=MyDB2DB;Integrated Security=yes;`

   Para obtener más información, vea [conectarse a Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md).
  
## <a name="reconnecting-to-db2"></a>Volver a conectar a DB2

La conexión al servidor de base de datos permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse si desea una conexión activa a la base de datos. Puede trabajar sin conexión hasta que desee actualizar los metadatos, cargar los objetos de base de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] datos en y migrar los datos.

## <a name="refreshing-db2-metadata"></a>Actualización de metadatos de DB2

Los metadatos de la base de datos DB2 no se actualizan automáticamente. Los metadatos del explorador de metadatos de DB2 son una instantánea de los metadatos cuando se conectó por primera vez o la última vez que se actualizaron los metadatos manualmente. Puede actualizar manualmente los metadatos de todos los esquemas, un esquema único o los objetos de base de datos individuales.

**Para actualizar los metadatos**

1. Asegúrese de que está conectado a la base de datos.
2. En el explorador de metadatos DB2, active la casilla que hay al lado de cada esquema u objeto de base de datos que desee actualizar.
3. Haga clic con el botón secundario en **esquemas** o en el objeto de esquema o base de datos individual y, a continuación, seleccione **actualizar desde base de datos**.

   Si no tiene una conexión activa, SSMA mostrará el cuadro de diálogo **conectar con DB2** para que pueda conectarse.
  
4. En el cuadro de diálogo actualizar desde base de datos, especifique los objetos que se van a actualizar.
   - Para actualizar un objeto, haga clic en el campo **activo** adyacente al objeto hasta que aparezca una flecha.
   - Para evitar que se actualice un objeto, haga clic en el campo **activo** adyacente al objeto hasta que aparezca una **X** .
   - Para actualizar o rechazar una categoría de objetos, haga clic en el campo **activo** adyacente a la carpeta categoría.

     Para ver las definiciones de la codificación de colores, haga clic en el botón **leyenda** .

5. [!INCLUDE[click OK](../../includes/clickok-md.md)]

## <a name="next-step"></a>siguiente paso

- El siguiente paso del proceso de migración consiste en [conectarse a SQL Server](./connecting-to-sql-server-db2tosql.md).

## <a name="see-also"></a>Consulte también

- [Migrar bases de datos DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)
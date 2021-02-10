---
title: Conexión a Oracle Database (OracleToSQL) | Microsoft Docs
description: Obtenga información acerca de cómo conectarse a la base de datos de Oracle para migrar la base de datos de Oracle a SQL Server. SSMA obtiene y muestra los metadatos de todos los esquemas de Oracle.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 06/04/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Refreshing Oracle Metadata
ms.assetid: e276cdbf-3ebc-4ba8-b40d-a7a42befa2b6
ms.author: alexiva
ms.openlocfilehash: 4354a4e2726151177c48c27ce37a74e5b4ac335e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058803"
---
# <a name="connecting-to-oracle-database-oracletosql"></a>Conexión a Oracle Database (OracleToSQL)

Para migrar bases de datos de Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe conectarse a la base de datos de Oracle que desea migrar. Cuando se conecta, SSMA obtiene los metadatos de todos los esquemas de Oracle y, a continuación, los muestra en el panel Explorador de metadatos de Oracle. SSMA almacena información sobre el servidor de base de datos, pero no almacena contraseñas.

La conexión a la base de datos permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse si desea una conexión activa a la base de datos.

Los metadatos de la base de datos de Oracle no se actualizan automáticamente. En su lugar, si desea actualizar los metadatos en el explorador de metadatos de Oracle, debe actualizarlos manualmente. Para obtener más información, vea la sección "actualizar metadatos de Oracle" más adelante en este tema.

## <a name="required-oracle-permissions"></a>Permisos de Oracle requeridos

Como mínimo, la cuenta que se utiliza para conectarse a la base de datos de Oracle debe tener los permisos siguientes:

- `CONNECT`  
  Necesario para conectarse (crear una sesión) a la base de datos.

- `SELECT ANY DICTIONARY`  
  Obligatorio para consultar las tablas de diccionario del sistema (por ejemplo, `SYS.MLOG$` ) con el fin de detectar todos los objetos.

Esto permitirá a SSMA cargar todos los objetos del esquema propiedad del usuario que se conecta. En la mayoría de los escenarios del mundo real existen referencias entre esquemas entre los procedimientos almacenados y SSMA deberán ser capaces de detectar todos los objetos a los que se hace referencia para una conversión correcta. Para obtener los metadatos de los objetos definidos en otros esquemas, la cuenta debe tener los siguientes permisos adicionales:

- `SELECT ANY TABLE`  
  Necesario para detectar tablas, vistas, vistas materializadas y sinónimos en otros esquemas.

- `SELECT ANY SEQUENCE`  
  Necesario para detectar secuencias en otros esquemas.

- `CREATE ANY PROCEDURE`  
  Se requiere para detectar PL/SQL para procedimientos, funciones y paquetes en otros esquemas.

- `CREATE ANY TRIGGER`  
  Necesario para detectar definiciones de desencadenador en otros esquemas.

- `CREATE ANY TYPE`  
  Necesario para detectar tipos definidos en otros esquemas.

Algunas de las características de SSMA requieren permisos adicionales. Por ejemplo, si desea usar el [evaluador](testing-migrated-database-objects-oracletosql.md) y la funcionalidad de [Administración de copias de seguridad](managing-backups-oracletosql.md) , debe conceder a los usuarios que se conectan lo siguiente:

- `EXECUTE ANY PROCEDURE`  
  Necesario para ejecutar procedimientos y funciones que le gustaría probar en todos los esquemas.

- `CREATE ANY TABLE` y `ALTER ANY TABLE`  
  Se requiere para crear y modificar tablas temporales para el seguimiento de cambios y las copias de seguridad.

- `INSERT ANY TABLE` y `UPDATE ANY TABLE`  
  Necesario para insertar los datos de seguimiento de cambios y de copia de seguridad en tablas temporales.

- `DROP ANY TABLE`  
  Necesario para quitar las tablas temporales que se usan para el seguimiento de cambios y las copias de seguridad.

- `CREATE ANY INDEX` y `ALTER ANY INDEX`  
  Se requiere para crear y modificar los índices de las tablas temporales que se usan para el seguimiento de cambios y las copias de seguridad.

- `DROP ANY INDEX`  
  Necesario para quitar índices de las tablas temporales utilizadas para el seguimiento de cambios y las copias de seguridad.

- `CREATE ANY TRIGGER` y `ALTER ANY TRIGGER`  
  Se requiere para crear y modificar los desencadenadores temporales que se usan para el seguimiento de cambios.

- `DROP ANY TRIGGER`  
  Necesario para quitar los desencadenadores temporales utilizados para el seguimiento de cambios.

> [!NOTE]
> Se trata de un conjunto genérico de permisos necesarios para que SSMA funcione correctamente. Si desea restringir el ámbito de la migración a un subconjunto de esquemas, puede hacerlo mediante la concesión de los permisos anteriores a un conjunto limitado de objetos, en lugar de `ALL` . Aunque es posible, es muy difícil identificar correctamente todas las dependencias, lo que impide que SSMA funcione correctamente. Se recomienda encarecidamente ajustarse al conjunto genérico tal y como se ha definido anteriormente para eliminar cualquier posible problema de permisos durante el proceso de migración.

## <a name="establishing-a-connection-to-oracle"></a>Establecer una conexión con Oracle

Cuando se conecta a una base de datos, SSMA Lee los metadatos de la base de datos y, a continuación, agrega estos metadatos al archivo de proyecto. SSMA utiliza estos metadatos cuando convierte objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis y cuando migra datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede examinar estos metadatos en el panel Explorador de metadatos de Oracle y revisar las propiedades de los objetos de base de datos individuales.

> [!IMPORTANT]
> Antes de intentar conectarse, asegúrese de que el servidor de base de datos se está ejecutando y de que puede aceptar conexiones.

**Para conectarse a Oracle**

1. En el menú **archivo** , seleccione **conectarse a Oracle**.  
   Si anteriormente se conectó a Oracle, el nombre del comando se volverá **a conectar a Oracle**.
  
2. En el cuadro **proveedor** , seleccione **proveedor de cliente Oracle** o **proveedor de OLE DB**, dependiendo del proveedor que esté instalado. El valor predeterminado es cliente Oracle.

3. En el cuadro **modo** , seleccione el modo **estándar**, el **modo TNSNAME** o el **modo de cadena de conexión**.  
   Utilice el modo estándar para especificar el nombre del servidor y el puerto. Use el modo de nombre de servicio para especificar manualmente el nombre del servicio de Oracle. Use el modo de cadena de conexión para proporcionar una cadena de conexión completa.

4. Si selecciona el **modo estándar**, proporcione los valores siguientes:
   1. En el cuadro **nombre del servidor** , escriba o seleccione el nombre o la dirección IP del servidor de base de datos.
   2. Si el servidor de base de datos no está configurado para aceptar conexiones en el puerto predeterminado (1521), escriba el número de puerto que se utiliza para las conexiones de Oracle en el cuadro **Puerto del servidor** .
   3. En el cuadro **SID de Oracle** , escriba el identificador del sistema.
   4. En el cuadro **nombre de usuario** , escriba una cuenta de Oracle que tenga los permisos necesarios.
   5. En el cuadro **contraseña** , escriba la contraseña del nombre de usuario especificado.

5. Si selecciona el **modo TNSNAME**, proporcione los valores siguientes:
   1. En el cuadro **identificador de conexión** , escriba el identificador de conexión (alias TNS) de la base de datos.
   2. En el cuadro **nombre de usuario** , escriba una cuenta de Oracle que tenga los permisos necesarios.
   3. En el cuadro **contraseña** , escriba la contraseña del nombre de usuario especificado.
  
6. Si selecciona el **modo de cadena de conexión**, proporcione una cadena de conexión en el cuadro **cadena de conexión** .  
   En el ejemplo siguiente se muestra una cadena de conexión de OLE DB:

   `Provider=OraOLEDB.Oracle;Data Source=MyOracleDB;User Id=myUsername;Password=myPassword;`

   En el ejemplo siguiente se muestra una cadena de conexión de cliente de Oracle que utiliza la seguridad integrada:

   `Data Source=MyOracleDB;Integrated Security=yes;`

   Para obtener más información, vea [conectarse a Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md).

## <a name="reconnecting-to-oracle"></a>Volver a conectar a Oracle

La conexión al servidor de base de datos permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse si desea una conexión activa a la base de datos. Puede trabajar sin conexión hasta que desee actualizar los metadatos, cargar los objetos de base de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] datos en y migrar los datos.

## <a name="refreshing-oracle-metadata"></a>Actualizar los metadatos de Oracle

Los metadatos de la base de datos de Oracle no se actualizan automáticamente. Los metadatos del explorador de metadatos de Oracle son una instantánea de los metadatos al conectarse por primera vez o la última vez que se actualizaron los metadatos manualmente. Puede actualizar manualmente los metadatos de todos los esquemas, un esquema único o los objetos de base de datos individuales.

**Para actualizar los metadatos**

1. Asegúrese de que está conectado a la base de datos.

2. En el explorador de metadatos de Oracle, active la casilla que hay al lado de cada esquema u objeto de base de datos que desee actualizar.

3. Haga clic con el botón secundario en **esquemas** o en el objeto de esquema o base de datos individual y, a continuación, seleccione **actualizar desde base de datos**.  
   Si no tiene una conexión activa, SSMA mostrará el cuadro de diálogo **conectar a Oracle** para que pueda conectarse.

4. En el cuadro de diálogo actualizar desde base de datos, especifique los objetos que se van a actualizar.

   - Para actualizar un objeto, haga clic en el campo **activo** adyacente al objeto hasta que aparezca una flecha.
   - Para evitar que se actualice un objeto, haga clic en el campo **activo** adyacente al objeto hasta que aparezca una **X** .
   - Para actualizar o rechazar una categoría de objetos, haga clic en el campo **activo** adyacente a la carpeta categoría.

   Para ver las definiciones de la codificación de colores, haga clic en el botón **leyenda** .

5. [!INCLUDE[clickOK](../../includes/clickok-md.md)]

## <a name="next-steps"></a>Pasos siguientes

El siguiente paso del proceso de migración consiste en [conectarse a una instancia de SQL Server](connecting-to-sql-server-oracletosql.md).

## <a name="see-also"></a>Consulte también

[Migrar bases de datos de Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)

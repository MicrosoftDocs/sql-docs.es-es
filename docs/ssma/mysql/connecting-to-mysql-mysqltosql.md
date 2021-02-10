---
title: Conexión a MySQL (MySQLToSQL) | Microsoft Docs
description: Obtenga información sobre cómo conectarse a una base de datos de iMySQL de destino para migrar una base de datos MySQL. SSMA obtiene metadatos acerca de las bases de datos en Azure SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to MySQL, MySQL permission
- Connecting to MySQL,reconnecting
ms.assetid: 084c7020-f729-4f91-90e0-143f85fa68d1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d7f3f46a58fccafa68164d9335c66df10c5b7487
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068900"
---
# <a name="connecting-to-mysql-mysqltosql"></a>Conexión a MySQL (MySQLToSQL)
Para migrar las bases de datos de MySQL a SQL Server o SQL Azure, debe conectarse a la base de datos MySQL que desea migrar. Cuando se conecta, SSMA obtiene los metadatos de todos los esquemas de MySQL y, a continuación, los muestra en el panel del explorador de metadatos de MySQL. SSMA almacena información sobre el servidor de base de datos, pero no almacena contraseñas.  
  
La conexión a la base de datos permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse si desea una conexión activa a la base de datos.  
  
Los metadatos de la base de datos MySQL no se actualizan automáticamente. En su lugar, si desea actualizar los metadatos en el explorador de metadatos de MySQL, debe actualizarlos manualmente. Para obtener más información, vea la sección "actualizar metadatos de MySQL" más adelante en este tema.  
  
## <a name="required-mysql-permissions"></a>Permisos de MySQL necesarios  
La cuenta que se utiliza para conectarse a la base de datos MySQL debe tener al menos permisos de **conexión** . Esto permite a SSMA obtener los metadatos de los esquemas que pertenecen al usuario que se conecta. Para obtener los metadatos de los objetos de otros esquemas y, a continuación, convertir los objetos en esos esquemas, la cuenta debe tener los permisos siguientes:  
  
-   Privilegios "Mostrar" en objetos de base de datos  
  
-   Privilegio ' SELECT ' en ' Information_schema '  
  
-   Privilegio ' SELECT ' en MySQL (para UDF)  
  
## <a name="establishing-a-connection-to-mysql"></a>Establecer una conexión con MySQL  
Cuando se conecta a una base de datos, SSMA Lee los metadatos de la base de datos y, a continuación, agrega estos metadatos al archivo de proyecto. SSMA utiliza estos metadatos cuando convierte objetos en SQL Server o SQL Azure sintaxis, y cuando migra datos a SQL Server o SQL Azure. Puede examinar estos metadatos en el panel Explorador de metadatos de MySQL y revisar las propiedades de los objetos de base de datos individuales.  
  
> [!IMPORTANT]  
> Antes de intentar conectarse, asegúrese de que el servidor de base de datos se está ejecutando y de que puede aceptar conexiones.  
  
**Para conectarse a MySQL**  
  
1.  En el menú **archivo** , seleccione **conectarse a MySQL** (esta opción se habilitará después de la creación del proyecto).  
  
    Si ya está conectado a MySQL, el nombre del comando se volverá **a conectar a MySQL**.  
  
2.  En el cuadro **proveedor** , seleccione MySQL ODBC 5,1 driver (de confianza). Es el proveedor predeterminado en el modo estándar.  
  
3.  En el cuadro **modo** , seleccione **modo estándar**. Este es el modo predeterminado.  
  
    Utilice el modo estándar para especificar el nombre del servidor y el puerto.  
  
4.  En el **modo estándar**, proporcione los valores siguientes:  
  
    1.  En el cuadro **nombre del servidor** , escriba el nombre del servidor MySQL. En el cuadro **Puerto del servidor** , escriba el número de puerto para que sea 3306. Es el puerto predeterminado.  
  
    2.  En el cuadro **nombre de usuario** , escriba una cuenta de MySQL que tenga los permisos necesarios.  
  
    3.  En el cuadro **contraseña** , escriba la contraseña del nombre de usuario especificado.  
  
5.  **SSL:** Si desea conectarse de forma segura a MySQL, haga uso de la capa de sockets seguros (SSL). para ello, active la casilla **SSL** .  
  
6.  **Configurar:** Proporciona una opción para configurar la conexión a MySQL a través de la capa de sockets seguros (SSL).  
  
    > [!NOTE]  
    > Para habilitar **configurar**, SSL debe establecerse en **true**.  
  
    Al hacer clic en el botón "configurar", aparece un cuadro de diálogo. Para usar el cifrado al conectarse a la base de datos MySQL, se debe definir la ruta de acceso a los tres archivos de certificado siguientes presentes en el cuadro de diálogo [certificados de correo mejorado de privacidad (PEM)]:  
  
    -   **Entidad de certificación SSL:** Especifica la ruta de acceso a un archivo con una lista de entidades de certificación SSL de confianza.  
  
    -   **Certificado SSL:** Especifica el nombre del archivo de certificado SSL que se va a usar para establecer una conexión segura.  
  
    -   **clave SSL:** Especifica el nombre del archivo de clave SSL que se va a usar para establecer una conexión segura.  
  
    > [!NOTE]  
    > -   El botón **Aceptar** está habilitado cuando se ha proporcionado la información necesaria. Si alguna de las rutas de acceso de archivo no es válida, el botón "Aceptar" permanecerá deshabilitado.  
    > -   El botón **Cancelar** cierra el cuadro de diálogo y **desactiva** la opción SSL del formulario de conexión principal.  
  
7.  Para obtener más información, consulte [conexión a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connect-to-mysql-mysqltosql.md)  
  
## <a name="reconnecting-to-mysql"></a>Volver a conectar a MySQL  
La conexión al servidor de base de datos permanece activa hasta que se cierra el proyecto. Cuando vuelva a abrir el proyecto, debe volver a conectarse si desea una conexión activa a la base de datos. Puede trabajar sin conexión hasta que desee actualizar los metadatos, cargar los objetos de base de datos en SQL Server o SQL Azure y migrar los datos.  
  
## <a name="refreshing-mysql-metadata"></a>Actualización de metadatos de MySQL  
Los metadatos de la base de datos MySQL no se actualizan automáticamente. Los metadatos del explorador de metadatos de MySQL son una instantánea de los metadatos al conectarse por primera vez o la última vez que se actualizaron los metadatos manualmente. Puede actualizar manualmente los metadatos de todos los esquemas, un esquema único o los objetos de base de datos individuales.  
  
**Para actualizar los metadatos**  
  
1.  Asegúrese de que está conectado a la base de datos.  
  
2.  En el explorador de metadatos de MySQL, active la casilla situada junto a cada esquema u objeto de base de datos que desee actualizar.  
  
3.  Haga clic con el botón secundario en **esquemas** o en el objeto de esquema o base de datos individual y, a continuación, seleccione **actualizar desde base de datos**.  
  
    Si no tiene una conexión activa, SSMA mostrará el cuadro de diálogo **conectar con MySQL** para que pueda conectarse.  
  
4.  En el cuadro de diálogo actualizar desde base de datos, especifique los objetos que se van a actualizar.  
  
    -   Para actualizar un objeto, haga clic en el campo **activo** adyacente al objeto hasta que aparezca una flecha.  
  
    -   Para evitar que se actualice un objeto, haga clic en el campo **activo** adyacente al objeto hasta que aparezca una **X** .  
  
    -   Para actualizar o rechazar una categoría de objetos, haga clic en el campo **activo** adyacente a la carpeta categoría.  
  
    -   Para ver las definiciones de la codificación de colores, haga clic en el botón **leyenda** .  
  
5.  Haga clic en **OK**.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [conectarse a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  

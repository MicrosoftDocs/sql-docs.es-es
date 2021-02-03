---
title: Crear un usuario de base de datos | Microsoft Docs
description: Obtenga información sobre cómo crear los tipos más comunes de usuarios de base de datos mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 04/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.user.securables.f1
- SQL13.SWB.DATABASEUSER.GENERAL.F1
helpviewer_keywords:
- database users, creating
- creating users with Management Studio
- mapping users
- users [SQL Server], creating
- database user additions [SQL Server]
- database users, mapping
- CREATE USER [Management Studio]
- users [SQL Server], adding
- mapping database users
ms.assetid: 782798d3-9552-4514-9f58-e87be4b264e4
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce4d006d0df8f17b487a515e3179d43fefd501d5
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236434"
---
# <a name="create-a-database-user"></a>Crear un usuario de base de datos
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  En este tema se describe cómo crear los tipos más comunes de usuarios de base de datos. Hay 11 tipos de usuarios. La lista completa se proporciona en el tema [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md). Todas las variedades de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] admiten usuarios de base de datos, pero no necesariamente todos los tipos de usuarios.  
  
 Puede crear un usuario de base de datos mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
##  <a name="understanding-the-types-of-users"></a><a name="Understanding"></a> Descripción de los tipos de usuarios  
 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] ofrece 6 opciones a la hora de crear un usuario de base de datos. En el gráfico siguiente se muestran las 6 opciones en el recuadro verde y se indica lo que representan.  
  
 ![Diagrama en el que se explican los diferentes tipos de usuarios.](../../../relational-databases/security/authentication-access/media/typesofusers.png "TypesOfUsers")  
  
### <a name="selecting-the-type-of-user"></a>Seleccionar el tipo de usuario  
 **Credenciales de inicio de sesión o usuario no asignado a un inicio de sesión**  
  
 Si no está familiarizado con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], puede resultarle difícil determinar qué tipo de usuario quiere crear. En primer lugar, pregúntese lo siguiente: ¿tiene credenciales de inicio de sesión la persona o el grupo que necesita acceder a la base de datos? Los inicios de sesión en la base de datos maestra son habituales para las personas que administran [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y para las personas que necesitan acceder a la mayoría de las bases de datos en la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)](o a todas). En este caso, creará un **usuario SQL con inicio de sesión**. El usuario de la base de datos es la identidad del inicio de sesión cuando está conectado a una base de datos. El usuario de la base de datos puede utilizar el mismo nombre que el inicio de sesión, pero no es necesario. En este tema se supone que ya existe un inicio de sesión en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para obtener información sobre cómo crear un inicio de sesión, vea [Crear un inicio de sesión](../../../relational-databases/security/authentication-access/create-a-login.md).  
  
 Si la persona o el grupo que necesita tener acceso a la base de datos no tiene credenciales de inicio de sesión y solo necesita tener acceso a una o a pocas bases de datos, cree un **usuario de Windows** o un **usuario SQL con contraseña**. También se denomina "usuario de base de datos independiente" y no está asociado con un inicio de sesión en la base de datos maestra. Se trata de una excelente opción si quiere mover fácilmente su base de datos entre instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para usar esta opción en [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)], es necesario que un administrador habilite las bases de datos independientes para [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]y que la base de datos esté habilitada para la contención. Para obtener más información, vea [Usuarios de base de datos independiente: hacer que la base de datos sea portátil](../../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
> **IMPORTANTE:** Al conectarse como un usuario de base de datos independiente, debe proporcionar el nombre de la base de datos como parte de la cadena de conexión. Para especificar la base de datos en [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], en el cuadro de diálogo **Conectar a** , haga clic en **Opciones** y en la pestaña **Propiedades de la conexión** .  
  
 Seleccione **Usuario SQL con contraseña** o **Usuario SQL con inicio de sesión** basado en un **SQL Server authentication login**(Inicio de sesión de autenticación de SQL Server) cuando la persona que se conecta no pueda autenticarse con Windows. Esto suele ocurrir cuando se conectan a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]personas de fuera de su organización (por ejemplo, clientes).  
  
> **SUGERENCIA** Para las personas de dentro de su organización, la autenticación de Windows es una opción más acertada. De este modo, no tienen que recordar otra contraseña y, además, la autenticación de Windows ofrece características de seguridad adicionales, como Kerberos.  
  
##  <a name="background"></a><a name="Restrictions"></a> Información previa  
 Un usuario es una entidad de seguridad de la base de datos. Los inicios de sesión deben estar asignados a un usuario de base de datos para poder conectarse a una base de datos. Un inicio de sesión se puede asignar a bases de datos diferentes como usuarios diferentes pero solo se puede asignar como un usuario en cada base de datos. En una base de datos parcialmente independiente, puede crearse un usuario que no tenga un inicio de sesión. Para obtener más información sobre los usuarios de la base de datos independiente, vea [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md). Si el usuario invitado de una base de datos está habilitado, un inicio de sesión que no esté asignado a un usuario de la base de datos puede entrar en la base de datos como el usuario invitado.  
  
> **IMPORTANTE:** El usuario invitado suele estar deshabilitado. No habilite al usuario invitado a menos que sea necesario.  
  
 Como entidad de seguridad, se pueden conceder permisos a los usuarios. El ámbito de un usuario es la base de datos. Para establecer conexión con una base de datos concreta de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], un inicio de sesión debe estar asignado a un usuario de la base de datos. Los permisos dentro de la base de datos se conceden y deniegan al usuario de la base de datos, no al inicio de sesión.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Debe tener el permiso **ALTER ANY USER** para la base de datos.  
  
##  <a name="create-a-user-with-ssms"></a><a name="SSMSProcedure"></a> Crear un usuario con SSMS  
  
 
1.  En el Explorador de objetos, expanda la carpeta **Bases de datos** .  
  
2.  Expanda la base de datos en la que se va a crear el usuario de la misma.  
  
3.  Haga clic con el botón derecho en la carpeta **Seguridad**, seleccione **Nuevo** y después **Usuario...** .  
  
4.  En el cuadro de diálogo **Usuario de base de datos - Nuevo**, en la página **General**, seleccione uno de los tipos de usuario siguientes de la lista **Tipo de usuario**:  
  
    -   **usuario SQL con inicio de sesión**  
  
    -   **usuario SQL con contraseña**  
  
    -   **Usuario SQL sin inicio de sesión**  
  
    -   **Usuario asignado a un certificado**  
  
    -   **Usuario asignado a una clave asimétrica**  
  
    -   **usuario de Windows**  
  
5.  Cuando se selecciona una opción, las demás opciones del cuadro de diálogo podrían cambiar. Algunas opciones solo se aplican a tipos específicos de usuarios de base de datos. Algunas opciones pueden dejarse en blanco y usarán un valor predeterminado.  
  
     **Nombre de usuario**  
     Escriba un nombre para el nuevo usuario. Si ha elegido **Usuario de Windows** en la lista **Tipo de usuario**, también puede hacer clic en los puntos suspensivos **(...)** para abrir el cuadro de diálogo **Seleccione Usuario o Grupo**.  
  
     **Nombre de inicio de sesión**  
     Especifique el inicio de sesión del usuario. Como alternativa, haga clic en los puntos suspensivos **(...)** para abrir el cuadro de diálogo **Seleccionar inicio de sesión**. Si selecciona **Usuario SQL con inicio de sesión** o **Usuario de Windows** en la lista **Tipo de usuario** , estará disponible **Nombre de inicio de sesión** .  
  
     **Contraseña** y **Confirmar contraseña**  
     Escriba una contraseña para los usuarios que se autentican en la base de datos.  
  
     **Idioma predeterminado**  
     Escriba el idioma predeterminado del usuario.  
  
     **Esquema predeterminado**  
     Escriba el esquema al que pertenecerán los objetos creados por este usuario. Como alternativa, haga clic en los puntos suspensivos **(...)** para abrir el cuadro de diálogo **Seleccionar esquema**. Si selecciona **Usuario SQL con inicio de sesión** , **Usuario SQL sin inicio de sesión**, **Usuario de Windows** en la lista **Tipo de usuario** , estará disponible **Esquema predeterminado** .  
  
     **Nombre de certificado**  
     Escriba el certificado que se usará para el usuario de base de datos. Como alternativa, haga clic en los puntos suspensivos **(...)** para abrir el cuadro de diálogo **Seleccionar certificado**. Si selecciona **Usuario asignado a un certificado** en la lista **Tipo de usuario** , estará disponible **Nombre de certificado** .  
  
     **Nombre de clave asimétrica**  
     Escriba la clave que se usará para el usuario de base de datos. Como alternativa, haga clic en los puntos suspensivos **(...)** para abrir el cuadro de diálogo **Seleccionar clave asimétrica**. Si selecciona **Usuario asignado a una clave asimétrica** en la lista **Tipo de usuario** , estará disponible **Nombre de clave asimétrica** .  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="additional-options"></a>Opciones adicionales  
 En el cuadro de diálogo **Usuario de la base de datos - Nuevo** también se proporcionan opciones en cuatro páginas adicionales: **Esquemas de propiedad**, **Pertenencia**, **Elementos protegibles** y **Propiedades extendidas**.  
  
-   La página **Esquemas de propiedad** enumera todos los esquemas posibles que pueden ser propiedad del nuevo usuario de base de datos. Para agregar o quitar esquemas en un usuario de base de datos, en **Esquemas propiedad de este usuario**, active o desactive las casillas situadas junto a los esquemas.  
  
-   La página **Pertenencia** enumera todos los roles de pertenencia de base de datos posibles que pueden ser propiedad del nuevo usuario de base de datos. Para agregar o quitar roles en un usuario de base de datos, en **Pertenencia al rol de la base de datos**, active o desactive las casillas situadas junto a los roles.  
  
-   La página **Elementos protegibles** muestra todos los elementos protegibles posibles y los permisos en esos elementos protegibles que se pueden conceder al inicio de sesión.  
  
-   La página **Propiedades extendidas** permite agregar propiedades personalizadas a los usuarios de base de datos. En esta página están disponibles las opciones siguientes.  
  
     **Base de datos**  
     Muestra el nombre de la base de datos seleccionada. Este campo es de solo lectura.  
  
     **Intercalación**  
     Muestra la intercalación utilizada para la base de datos seleccionada. Este campo es de solo lectura.  
  
     **Propiedades**  
     Muestra o especifica las propiedades extendidas del objeto. Cada propiedad extendida está formada por un par nombre/valor de metadatos asociados al objeto.  
  
     **Puntos suspensivos (...)**  
     Haga clic en los puntos suspensivos **(...)** situados detrás de **Valor** para abrir el cuadro de diálogo **Valor para propiedad extendida**. Escriba o muestre el valor de la propiedad extendida en esta ubicación mayor. Para obtener más información, vea [Valor para propiedad extendida (cuadro de diálogo)](../../databases/value-for-extended-property-dialog-box.md).  
  
     **Eliminar**  
     Elimina la propiedad extendida que se ha seleccionado.  
  
##  <a name="create-a-user-using-t-sql"></a><a name="TsqlProcedure"></a> Crear un usuario con T-SQL  
    
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  En la barra **Estándar** , haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- Creates the login AbolrousHazem with password '340$Uuxwp7Mcxo7Khy'.  
    CREATE LOGIN AbolrousHazem   
        WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
    GO  
  
    -- Creates a database user for the login created above.  
    CREATE USER AbolrousHazem FOR LOGIN AbolrousHazem;  
    GO  
    ```  
  
 Para obtener más información, vea [CREATE USER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-user-transact-sql.md) que contiene muchos más ejemplos de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
## <a name="see-also"></a>Consulte también  
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Crear un inicio de sesión](../../../relational-databases/security/authentication-access/create-a-login.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
  

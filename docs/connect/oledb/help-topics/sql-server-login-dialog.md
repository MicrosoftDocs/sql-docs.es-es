---
title: Cuadro de diálogo Inicio de sesión de SQL Server (OLE DB) | Microsoft Docs
description: Cuando intenta conectarse sin especificar suficiente información, OLE DB Driver for SQL Server muestra el cuadro de diálogo Inicio de sesión de SQL Server.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.author: v-beaziz
author: bazizi
ms.openlocfilehash: 647551a314829ef196914e964cbd08237ef085d7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195300"
---
# <a name="sql-server-login-dialog-box"></a>Cuadro de diálogo de inicio de sesión de SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Cuando intenta conectarse sin especificar suficiente información, el controlador OLE DB muestra el cuadro de diálogo **Inicio de sesión de SQL Server**.

> [!NOTE]  
> La propiedad de inicialización de `DBPROP_INIT_PROMPT` controla el comportamiento de petición del cuadro de diálogo Inicio de sesión de SQL Server. Para más información, consulte:
> - [Propiedades de inicialización y autorización](../ole-db-data-source-objects/initialization-and-authorization-properties.md)
> - [Guía del programador de OLE DB](/previous-versions/windows/desktop/ms714342(v=vs.85))

![Captura de pantalla del cuadro de diálogo Inicio de sesión de SQL Server](../media/sql-server-login-dialog.png)

## <a name="options"></a>Opciones
|Opción|Descripción|
|---   |---        |
|Server|Nombre de una instancia de SQL Server en la red. Seleccione un nombre de servidor\instancia de la lista, o escriba el nombre de servidor\instancia en el cuadro **Servidor**. Opcionalmente, se puede crear un alias de servidor en el equipo cliente mediante el **Administrador de configuración de SQL Server** y escribir ese nombre en el cuadro **Servidor**. <br/><br/>Puede escribir "(local)" si está utilizando el mismo equipo que SQL Server. Después puede establecer conexión con una instancia local de SQL Server, incluso si ejecuta una versión que no está en red de SQL Server.<br/><br/>Para más información sobre los nombres de servidor para diferentes tipos de redes, consulte [Instalación de SQL Server](../../../database-engine/install-windows/install-sql-server.md).|
|Modo de autenticación|Puede seleccionar las siguientes opciones de autenticación en la lista desplegable:<br/><ul><li>`Windows Authentication:` autenticación en SQL Server con las credenciales de la cuenta de Windows del usuario que ha iniciado sesión actualmente.</li><li>`SQL Server Authentication:` autenticación con el identificador y la contraseña de inicio de sesión.</li><li>`Active Directory - Integrated:` autenticación integrada con una identidad de Azure Active Directory. Este modo también se puede usar para la autenticación de Windows en SQL Server.</li><li>`Active Directory - Password:` autenticación de identificador de usuario y contraseña con una identidad de Azure Active Directory.</li><li>`Active Directory - Universal with MFA support:` autenticación interactiva con una identidad de Azure Active Directory. Este modo admite Microsoft Azure Multi-Factor Authentication (MFA).</li><li>`Active Directory - Service Principal:` Autenticación con una entidad de servicio de Azure Active Directory. El **identificador de inicio de sesión** debe establecerse en el identificador de la aplicación (cliente). La **contraseña** debe establecerse en el secreto de la aplicación (cliente).</li></ul>|
|Dirección SPN del servidor|Si utiliza una conexión de confianza, puede especificar un nombre principal de servicio (SPN) para el servidor.|
|Id. de inicio de sesión|Especifica el identificador de inicio de sesión que se va a usar en la conexión. El cuadro de texto del identificador de inicio de sesión solo se habilita si `Authentication Mode` se establece en `SQL Server Authentication`, `Active Directory - Password`, `Active Directory - Universal with MFA support` o `Active Directory - Service Principal`.|
|Contraseña|Especifica la contraseña usada para la conexión. El cuadro de texto de contraseña solo se habilita si `Authentication Mode` se establece en `SQL Server Authentication`, `Active Directory - Password` o `Active Directory - Service Principal`.|
|Opciones|Muestra u oculta el grupo **Opciones**. El botón **Opciones** está habilitado si hay un valor en **Servidor**.|
|Cambio de contraseñas|Cuando se activa, habilita los cuadros de texto **Nueva contraseña** y **Confirmar nueva contraseña**.|
|New Password|Especifica la nueva contraseña.|
|Confirmar nueva contraseña|Especifica la nueva contraseña por segunda vez para la confirmación.|
|Base de datos|Seleccione o escriba la base de datos predeterminada que se va a utilizar en la conexión. Este valor invalida la base de datos predeterminada especificada para el inicio de sesión en el servidor. Si no se especifica ninguna base de datos, la conexión utiliza la base de datos predeterminada especificada para el inicio de sesión en el servidor.|
|Servidor reflejado|Especifica el nombre del asociado de conmutación por error de la base de datos que se va a reflejar.|
|SPN del servidor reflejado|Opcionalmente, puede especificar un SPN para el servidor reflejado. El SPN para el servidor reflejado se utiliza para la autenticación mutua entre cliente y servidor.|
|Idioma|Especifica el idioma nacional que se va a utilizar para los mensajes de sistema de SQL Server. El equipo que ejecuta SQL Server debe tener instalado el idioma. Este valor invalida el idioma predeterminado especificado para el inicio de sesión en el servidor. Si no se especifica ningún idioma, la conexión utiliza el idioma predeterminado especificado para el inicio de sesión en el servidor.|
|Nombre de la aplicación|Especifica el nombre de aplicación que se va a almacenar en la columna **program_name** de la fila para esta conexión en **sys.sysprocesses**.|
|Id. de estación de trabajo|Especifica el identificador de la estación de trabajo que se va a almacenar en la columna **hostname** de la fila para esta conexión en **sys.sysprocesses**.|
|Utilizar cifrado de alta seguridad para los datos|Cuando se activa, se cifrarán los datos que se pasen a través de la conexión.|
|Certificado de servidor de confianza|Cuando se activa, se validará el certificado del servidor. El certificado del servidor debe tener el nombre de host correcto del servidor y haber sido emitido por una entidad de certificación de confianza.|

> [!NOTE]  
> Al usar los modos `Windows Authentication` o `SQL Server Authentication`, el **certificado de servidor de confianza** solo se tiene en cuenta cuando la opción **Utilizar cifrado de alta seguridad para los datos** está habilitada.

## <a name="next-steps"></a>Pasos siguientes
- [Autenticación en Azure Active Directory](../features/using-azure-active-directory.md) mediante el controlador OLE DB.
- Establecimiento de la información de conexión mediante el [vínculo de datos universal (UDL)](data-link-pages.md).
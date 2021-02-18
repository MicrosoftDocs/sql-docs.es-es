---
description: Conectar al servidor (motor de base de datos)
title: Conectar al servidor (motor de base de datos)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.connectoserverunknownservertype.f1
- sql13.swb.connection.login.sqlce.f1
- sql13.swb.connecttoce.f1
- SQL13.SWB.CONNECTION.LOGIN.SQLSERVER.F1
- sql13.swb.connection.login.sqlserver.f1
- sql13.swb.manageSS2k.f1
- connect-to-server-database-engine
ms.assetid: ee9017b4-8a19-4360-9003-9e6484082d41
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 04/07/2020
ms.openlocfilehash: 9f0eca39db873b277d0c698407c5c02afa520b35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342659"
---
# <a name="connect-to-server-database-engine"></a>Conectar al servidor (motor de base de datos)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Use este cuadro de diálogo para ver o especificar opciones cuando se conecte a [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]. En la mayoría de los casos, para conectarse, puede escribir el nombre del servidor de base de datos en el cuadro **Nombre del servidor** y hacer clic en **Conectar**. Si se está conectando con una instancia con nombre, use el nombre del equipo seguido de una barra inversa y, después, el nombre de la instancia. Por ejemplo, `mycomputer\myinstance`. Si se está conectando a [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)], use el nombre del equipo seguido de **\sqlexpress**.
  
Hay diversos factores que pueden afectar a la capacidad de conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener ayuda, vea los recursos siguientes:

- [Lección 1 del tutorial: Conexión al motor de base de datos](../../relational-databases/lesson-1-connecting-to-the-database-engine.md)  

- [Solucionar problemas de conexión al motor de base de datos de SQL Server](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)  

- [Solving Connectivity errors to SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) (Solución de problemas de conectividad en SQL Server)   
  
## <a name="options"></a>Opciones

**Tipo de servidor**  
Al registrar un servidor desde el Explorador de objetos, seleccione el tipo de servidor al que se conectará: [!INCLUDE[ssDE](../../includes/ssde_md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]o [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. El resto del cuadro de diálogo muestra simplemente las opciones que se aplican al tipo de servidor seleccionado. Cuando se registra un servidor desde Servidores registrados, el cuadro **Tipo de servidor** es de solo lectura y coincide con el tipo de servidor que se muestra en el componente Servidores registrados. Para registrar un tipo de servidor distinto, seleccione [!INCLUDE[ssDE](../../includes/ssde_md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], [!INCLUDE[ssEW](../../includes/ssew-md.md)]o [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] desde la barra de herramientas Servidores registrados antes de comenzar a registrar un nuevo servidor.  
  
**Nombre del servidor**  
Seleccione la instancia de servidor a la que va a conectarse. De forma predeterminada, aparecerá la instancia de servidor a la que se ha conectado por última vez.  
  
> [!NOTE]  
> Para conectarse a una instancia de usuarios activos de [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)] , use el protocolo Canalizaciones con nombre especificando el nombre de canalización, como `np:\\.\pipe\3C3DF6B1-2262-47\tsql\query`. Para obtener más información, consulte la documentación de [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)] .  

> [!NOTE]  
> Las conexiones normalmente se conservan en el historial "Usados recientemente". Para quitar las entradas de los elementos usados recientemente, simplemente haga clic en el control de cuadro combinado **Nombre del servidor**, seleccione el nombre del servidor para quitar y, a continuación, presione la tecla **SUPR**.  

**Autenticación**  
La versión actual de SSMS ofrece cinco modos de autenticación al conectarse a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)]. Si el cuadro de diálogo de autenticación no coincide con la lista siguiente, descargue la versión más reciente de SSMS desde [Descarga de SQL Server Management Studio (SSMS)](../download-sql-server-management-studio-ssms.md).  

> **Autenticación de Windows**  
> [!INCLUDE[msCoName](../../includes/msconame_md.md)] El modo de autenticación de Windows permite al usuario conectarse mediante una cuenta de usuario de Windows.  
> 
> **Autenticación de SQL Server**  
> Cuando un usuario se conecta con un nombre y una contraseña de inicio de sesión determinados desde una conexión no confiable, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza la autenticación y comprueba si se configuró una cuenta de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y si la contraseña especificada coincide con la almacenada anteriormente. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no tiene configurada una cuenta de inicio de sesión, la autenticación no se realizará correctamente y el usuario recibirá un mensaje de error. Siempre que sea posible, use la autenticación de Windows o la autenticación de contraseña de Active Directory.  
> 
> **Azure Active Directory: universal compatible con MFA**  
> Active Directory - Universal compatible con MFA es un flujo de trabajo interactivo que es compatible con Azure Multi-Factor Authentication (MFA). Azure MFA ayuda a proteger el acceso a los datos y las aplicaciones mientras se cumple la exigencia del usuario en cuanto a un proceso de inicio de sesión simple. Ofrece una autenticación segura con una variedad de opciones de comprobación sencillas, como llamadas de teléfono, mensajes de texto, tarjetas inteligentes con PIN o notificaciones de aplicaciones móviles, que permiten que los usuarios elijan el método que prefieran. Si la cuenta de usuario está configurada para MFA, el flujo de trabajo de autenticación interactivo requiere una interacción adicional por parte del usuario a través de cuadros de diálogo emergentes, las tarjetas inteligentes, etc. Si la cuenta de usuario está configurada para MFA, el usuario debe seleccionar Autenticación universal de Azure para conectarse. Si la cuenta de usuario no requiere MFA, el usuario puede usar de todas maneras las otras dos opciones de Autenticación de Azure Active Directory. Para más información, vea [Compatibilidad de SSMS con Azure AD MFA con SQL Database y Azure Synapse Analytics](/azure/azure-sql/database/authentication-mfa-ssms-overview). Si es necesario, puede cambiar el dominio que autentica el inicio de sesión. Para ello, haga clic en **Opciones**, seleccione la pestaña **Propiedades de conexión** y complete el cuadro **Nombre de dominio o ID de inquilino de AD**.  
> 
> **Azure Active Directory: contraseña**  
> La autenticación de Azure Active Directory es un mecanismo que permite conectarse a [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] mediante identidades de Azure Active Directory (Azure AD).  Use este método para conectarse con [!INCLUDE[ssSDS](../../includes/sssds-md.md)] si ha iniciado sesión en Windows con credenciales de un dominio no federado con Azure o si usa la autenticación de Azure AD con Azure AD basado en el dominio inicial o de cliente. Para más información, consulte [Usar la autenticación de Azure Active Directory para autenticación con SQL](/azure/azure-sql/database/authentication-aad-overview).  
> 
> **Active Directory - Integrado**  
> La autenticación de Azure Active Directory es un mecanismo que permite conectarse a [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] mediante identidades de Azure Active Directory (Azure AD). Use este método para conectarse con [!INCLUDE[ssSDS](../../includes/sssds-md.md)] si ha iniciado sesión en Windows con credenciales de Azure Active Directory de un dominio federado. Para más información, consulte [Usar la autenticación de Azure Active Directory para autenticación con SQL](/azure/azure-sql/database/authentication-aad-overview).  
  
**Nombre de usuario**  
El nombre de usuario de Windows con el que se va a conectar. Esta opción solo está disponible si ha seleccionado la autenticación **Azure Active Directory: contraseña** para conectarse. Es de solo lectura cuando se selecciona **Autenticación de Windows** o la autenticación **Azure Active Directory: integrado**.  
  
**Inicio de sesión**  
Escriba el inicio de sesión con el que va a conectarse. Esta opción solo está disponible si se ha seleccionado Autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Autenticación de contraseña de Active Directory para conectarse.  
  
> [!NOTE]  
> Las conexiones normalmente se conservan en el historial "Usados recientemente". Para quitar las entradas de los elementos usados recientemente, simplemente haga clic en el control de cuadro combinado **Nombre del servidor**, seleccione el nombre del servidor para quitar y, a continuación, presione la tecla **SUPR**. Esto se incluyó por primera vez con SSMS 18.5.

**Contraseña**  
Escriba la contraseña del inicio de sesión. Esta opción solo se puede editar si se ha seleccionado Autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Autenticación de contraseña de Active Directory para conectarse.  

**Conexión**  
Haga clic para conectar con el servidor.

**Opciones**  
Haga clic para mostrar las pestañas **Propiedades de conexión** y **Parámetros de conexión adicionales**.
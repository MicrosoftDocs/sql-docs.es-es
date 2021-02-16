---
title: Elección de un modo de autenticación | Microsoft Docs
description: Elija entre el modo de autenticación de Windows y la autenticación de modo mixto para el Motor de base de datos de SQL Server en el momento de la instalación.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.ins.instwizard.authenticationmode.f1
- sql13.swb.passwordexpired.f1
helpviewer_keywords:
- sa account
- authentication modes
- trusted connection
- SQL Server Installation Wizard, Authentication Mode page
- choose authentication mode
- authentication [SQL Server], choosing a mode
- Windows authentication [SQL Server]
- mixed mode authentication
- mixed authentication mode
- SQL authentication mode
- Password Expired dialog box
ms.assetid: ff7a6a48-3d38-4209-aa0f-7d6c0a8c64ef
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ebcf99ad025477a562f2a050eb1c2c3f3f63e12f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344398"
---
# <a name="choose-an-authentication-mode"></a>Elegir un modo de autenticación
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Durante la instalación, debe seleccionar un modo de autenticación para [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Hay dos posibles modos: modo de autenticación de Windows y modo mixto. El modo de autenticación de Windows habilita la autenticación de Windows y deshabilita la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El modo mixto habilita tanto la autenticación de Windows como la de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La autenticación de Windows está disponible siempre y no se puede deshabilitar.  
  
## <a name="configuring-the-authentication-mode"></a>Configurar el modo de autenticación  
 Si selecciona la autenticación de modo mixto durante la instalación, debe proporcionar una contraseña segura, y confirmarla después, para la cuenta integrada de administrador del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominada sa. La cuenta sa se conecta mediante la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Si selecciona la autenticación de Windows durante la instalación, el programa de instalación crea la cuenta sa para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pero se deshabilita. Si después cambia a la autenticación de modo mixto y desea utilizar la cuenta sa, debe habilitar la cuenta. Cualquier cuenta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o de Windows se puede configurar como del administrador del sistema. Dado que la cuenta sa es muy conocida y a menudo es el objetivo de usuarios malintencionados, no la habilite a menos que la aplicación lo requiera. Nunca establezca una contraseña en blanco o con poca seguridad para la cuenta sa. Para cambiar del modo de autenticación de Windows a la autenticación de modo mixto y usar la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [Cambiar el modo de autenticación del servidor](../../database-engine/configure-windows/change-server-authentication-mode.md).  
  
## <a name="connecting-through-windows-authentication"></a>Conectar a través de la autenticación de Windows  
 Cuando un usuario se conecta a través de una cuenta de usuario de Microsoft Windows, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valida el nombre de cuenta y la contraseña con el token de la entidad de seguridad de Windows del sistema operativo. Esto significa que Windows confirma la identidad del usuario. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no pide la contraseña y no realiza la validación de identidad. La autenticación de Windows es el modo de autenticación predeterminado y es mucho más seguro que la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La autenticación de Windows usa el protocolo de seguridad de Kerberos, proporciona la aplicación de directivas de contraseñas en cuanto a la validación de la complejidad de las contraseñas seguras, ofrece compatibilidad para el bloqueo de cuentas y admite la expiración de las contraseñas. Una conexión realizada utilizando la autenticación de Windows se denomina a veces conexión de confianza, porque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] confía en las credenciales proporcionadas por Windows.  
  
 Cuando se emplea la autenticación de Windows, se pueden crear grupos de Windows en el nivel de dominio y se puede crear un inicio de sesión en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para todo el grupo. La administración del acceso desde el nivel de dominio puede simplificar la administración de cuentas.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
## <a name="connecting-through-sql-server-authentication"></a>Conectar a través de la autenticación de SQL Server  
 Cuando se utiliza la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , los inicios de sesión se crean en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y no se basan en cuentas de usuario de Windows. El nombre de usuario y la contraseña se crean utilizando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y se almacenan en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los usuarios que se conectan usando la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deben indicar sus credenciales (inicio de sesión y contraseña) cada vez que se conectan. Al utilizar la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe establecer contraseñas seguras para todas las cuentas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener las directrices para contraseñas seguras, vea [Strong Passwords](../../relational-databases/security/strong-passwords.md).  
  
 Hay tres directivas de contraseñas opcionales para los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   El usuario debe cambiar la contraseña en el siguiente inicio de sesión  
  
     Exige que el usuario cambie la contraseña la próxima vez que se conecte. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]proporciona la capacidad de cambiar la contraseña. Otros desarrolladores de software deberían proporcionar esta característica si se utiliza esta opción.  
  
-   Exigir expiración de contraseña  
  
     La directiva de duración máxima de la contraseña del equipo se exige para los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Exigir directivas de contraseñas  
  
     Las directivas de contraseñas de Windows del equipo se exigen para los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Esto incluye la longitud y complejidad de las contraseñas. Esta funcionalidad depende de la API `NetValidatePasswordPolicy` , que solo está disponible en [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] y versiones posteriores.  
  
#### <a name="to-determine-the-password-policies-of-the-local-computer"></a>Para determinar las directivas de las contraseñas del equipo local  
  
1.  En el menú **Inicio** , haga clic en **Ejecutar**.  
  
2.  En el cuadro de diálogo **Ejecutar** , escriba **secpol.msc** y haga clic en **Aceptar**.  
  
3.  En la aplicación **Configuración de seguridad local** , expanda **Configuración de seguridad**, expanda **Directivas de cuenta** y, a continuación, haga clic en **Directiva de contraseñas**.  

     Las directivas de contraseñas se describen en el panel de resultados.  
  
### <a name="disadvantages-of-sql-server-authentication"></a>Desventajas de la autenticación de SQL Server  
  
-   Si un usuario del dominio de Windows tiene un inicio de sesión y una contraseña para Windows, aún debe proporcionar otro inicio de sesión y contraseña ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) para conectarse. Hacer el seguimiento de varios nombres y contraseñas es difícil para muchos usuarios. Tener que proporcionar las credenciales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cada vez que se conectan a la base de datos puede resultar molesto.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede utilizar el protocolo de seguridad de Kerberos.  
  
-   Windows proporciona directivas de contraseñas adicionales que no están disponibles para los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   La contraseña de inicio de sesión cifrada de la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se debe pasar a través de la red en el momento de la conexión. Algunas aplicaciones que se conectan automáticamente almacenarán la contraseña en el cliente. Estos puntos de ataque adicionales.  
  
### <a name="advantages-of-sql-server-authentication"></a>Ventajas de la autenticación de SQL Server  
  
-   Permite a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admitir aplicaciones anteriores y aplicaciones proporcionadas por terceros que requieren la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Permite que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admita entornos con sistemas operativos mixtos, en los que un dominio de Windows no autentica a todos los usuarios.  
  
-   Permite a los usuarios conectarse desde dominios desconocidos o que no son de confianza. Por ejemplo, una aplicación en la que los clientes establecidos se conectan con los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asignados para recibir el estado de sus pedidos.  
  
-   Permite que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admita aplicaciones basadas en web en las que los usuarios crean sus propias identidades.  
  
-   Permite a los desarrolladores de software distribuir sus aplicaciones utilizando una jerarquía de permisos compleja basada en los inicios de sesión conocidos y preestablecidos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    > [!NOTE]  
    >  Al utilizar la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , no se limitan los permisos de los administradores locales en el equipo donde se instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Consulte también  
 [Consideraciones de seguridad para una instalación de SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
  
  

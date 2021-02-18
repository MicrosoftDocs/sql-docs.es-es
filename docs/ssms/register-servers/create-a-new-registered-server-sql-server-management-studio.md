---
description: Crear un servidor registrado (SQL Server Management Studio)
title: Crear un servidor registrado
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.registerserver.general.sqlce.f1
- sql13.swb.registerserver.general.sqlserver.f1
helpviewer_keywords:
- Registered Servers [SQL Server], creating new registered servers
ms.assetid: 716ea070-a3b5-4514-9de2-82ce8a96514b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 4c158ff93e5f5e62f8cb3d3d68333c5f48fd6338
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350546"
---
# <a name="create-a-new-registered-server-sql-server-management-studio"></a>Crear un servidor registrado (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este tema se describe cómo guardar la información de conexión para los servidores a los que tiene acceso frecuentemente, registrando el servidor en el componente Servidores registrados de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Un servidor puede registrarse antes de conectarlo o cuando se conecta desde el Explorador de objetos. Hay una opción de menú especial para registrar las instancias de servidor en el equipo local.  
  
 Hay dos tipos de servidores registrados:  
  
-   Grupos de servidores locales  
  
     Use los grupos de servidores locales para conectar fácilmente con los servidores que administra con frecuencia. Tanto los servidores locales como los no locales se registran en grupos de servidores locales. Los grupos de servidores locales son únicos para cada usuario. Para obtener información sobre cómo compartir la información de los servidores registrados, vea [Exportar información de servidores registrados &#40;SQL Server Management Studio&#41;](./export-registered-server-information-sql-server-management-studio.md) e [Importar información de servidores registrados &#40;SQL Server Management Studio&#41;](./import-registered-server-information-sql-server-management-studio.md).  
  
    > [!NOTE]  
    >  Recomendamos que, siempre que sea posible, se utilice la autenticación de Windows.  
  
-   Servidores de administración central  
  
     Los Servidores de administración central almacenan los registros de servidor en el Servidor de administración central en lugar de en el sistema de archivos. Los Servidores de administración central y los servidores secundarios registrados únicamente se pueden registrar utilizando la autenticación de Windows. Una vez registrado un Servidor de administración central, sus servidores registrados asociados se mostrarán automáticamente. Para obtener más información sobre los Servidores de administración central, vea [Administrar varios servidores mediante Servidores de administración central](../../relational-databases/administer-multiple-servers-using-central-management-servers.md). Las versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anteriores a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] no se pueden designar como Servidores de administración central.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-automatically-register-the-local-server-instances"></a>Para registrar automáticamente las instancias de servidor locales  
  
-   En Servidores registrados, haga clic con el botón derecho en cualquier nodo del árbol Servidores registrados y, después, haga clic en **Actualizar registro de servidor local**.  
  
#### <a name="to-create-a-new-registered-server"></a>Para crear un servidor registrado  
  
1.  Si Servidores registrados no está visible en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], en el menú **Ver** , haga clic en **Servidores registrados**.  
  
     **Tipo de servidor**  
     Cuando se registra un servidor desde Servidores registrados, el cuadro **Tipo de servidor** es de solo lectura y coincide con el tipo de servidor que se muestra en el panel Servidores registrados. Para registrar un tipo de servidor diferente, haga clic en **Motor de base de datos**, **Analysis Server**, **Reporting Services** o **Integration Services** en la barra de herramientas de **Servidores registrados** antes de empezar a registrar un nuevo servidor.  
  
     **Nombre del servidor**  
     Seleccione la instancia del servidor que se va a registrar en el formato: *\<servername>* [\\ *\<instancename>* ].  
  
     **Autenticación**  
     Están disponibles dos modos de autenticación cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     **Autenticación de Windows**  
     El modo de autenticación de Windows permite al usuario conectarse mediante una cuenta de usuario de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
     **Autenticación de SQL Server**  
     Cuando un usuario se conecta con un nombre y una contraseña de inicio de sesión específicos desde una conexión que no es de confianza, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza la autenticación y comprueba si se ha configurado una cuenta de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y si la contraseña especificada coincide con la que se ha almacenado anteriormente. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no tiene configurada una cuenta de inicio de sesión, la autenticación no se realizará correctamente y el usuario recibirá un mensaje de error.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)] Para obtener más información, vea [Elegir un modo de autenticación](../../relational-databases/security/choose-an-authentication-mode.md).  
  
     **Nombre de usuario**  
     Muestra el nombre de usuario actual con el que se conecta. Esta opción de solo lectura únicamente está disponible si ha seleccionado la autenticación de Windows para conectarse. Para cambiar **Nombres de usuario**, inicie una sesión en el equipo como un usuario distinto.  
  
     **Inicio de sesión**  
     Escriba el inicio de sesión con el que va a conectarse. Esta opción solo está disponible si ha seleccionado la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conectarse.  
  
     **Contraseña**  
     Escriba la contraseña del inicio de sesión. Esta opción solo puede editarse si ha seleccionado la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conectarse.  
  
     **Recordar contraseña**  
     Seleccione esta opción para que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cifre y guarde la contraseña que ha escrito. Esta opción solo aparece si ha seleccionado la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conectarse.  
  
    > [!NOTE]  
    >  Si ha guardado la contraseña y ya no quiere conservarla, desactive esta casilla y, luego, haga clic en **Guardar**.  
  
     **Nombre del servidor registrado**  
     El nombre que desea que aparezca en Servidores registrados. Este nombre no tiene que coincidir con el cuadro **Nombre del servidor** .  
  
     **Descripción del servidor registrado**  
     Escriba una descripción opcional del servidor.  
  
     **Test**  
     Haga clic para probar la conexión al servidor seleccionado en **Nombre del servidor**.  
  
     **Guardar**  
     Haga clic aquí para guardar la configuración del servidor registrado.  
  
## <a name="multiserver-queries"></a>Consultas multiservidor  
 La ventana del Editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] puede conectarse a varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y consultarlas al mismo tiempo. Los resultados que devuelve la consulta se pueden combinar en un único panel de resultados o se pueden devolver en paneles de resultados independientes. Si se desea, el Editor de consultas puede incluir las columnas que proporcionan el nombre del servidor que generó cada fila y también el inicio de sesión que se usó para conectarse al servidor que proporcionó cada fila. Para obtener más información sobre cómo ejecutar consultas multiservidor, vea [Ejecutar instrucciones con varios servidores simultáneamente &#40;SQL Server Management Studio&#41;](./execute-statements-against-multiple-servers-simultaneously.md).  
  
 Para ejecutar las consultas con todos los servidores de un grupo de servidores locales, haga clic con el botón derecho en el grupo de servidores, haga clic en **Conectar** y, después, haga clic en **Nueva consulta**. Cuando las consultas se ejecuten en la nueva ventana del Editor de consultas, se ejecutarán con todos los servidores del grupo utilizando la información de conexión almacenada incluido el contexto de autenticación del usuario. Los servidores registrados con la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , pero que no guarden la contraseña no podrán conectarse.  
  
 Para ejecutar las consultas con todos los servidores que se registran con un Servidor de administración central, expanda el servidor, haga clic con el botón derecho en el grupo de servidores, haga clic en **Conectar** y, después, haga clic en **Nueva consulta**. Cuando las consultas se ejecuten en la nueva ventana del Editor de consultas, ejecutarán con todos los servidores del grupo de servidores, utilizando la información de conexión almacenada y el contexto de autenticación de Windows del usuario.  
  
## <a name="see-also"></a>Consulte también  
 [Ocultar objetos del sistema en el Explorador de objetos](../object/hide-system-objects-in-object-explorer.md)   
 [Exportar información de servidores registrados &#40;SQL Server Management Studio&#41;](./export-registered-server-information-sql-server-management-studio.md)   
 [Importar información de servidores registrados &#40;SQL Server Management Studio&#41;](./import-registered-server-information-sql-server-management-studio.md)  
  

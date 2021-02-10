---
description: Conectarse a SQL Server (SybaseToSQL)
title: Conexión a SQL Server (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 30179b25-409b-4e23-bc73-2f226657098f
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b2b9c2d3f776ba41263e23d593ee3a4ef0f739df
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064290"
---
# <a name="connect-to-sql-server-sybasetosql"></a>Conectarse a SQL Server (SybaseToSQL)
Utilice el cuadro de diálogo **conectar con SQL Server** para conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que desea migrar. Para tener acceso al cuadro de diálogo **conectar con SQL Server** , en el menú **archivo** , haga clic en **conectar a SQL Server**.  
  
## <a name="options"></a>Opciones  
**Nombre del servidor**  
Escriba o seleccione la instancia de SQL Server a la que se va a conectar. De forma predeterminada, se muestra la instancia de a la que se conectó más recientemente.  
  
-   Si se está conectando a la instancia predeterminada en el equipo local, puede especificar **localhost** o un punto (**.**).  
  
-   Si se va a conectar a la instancia predeterminada en otro equipo, escriba el nombre del equipo.  
  
-   Si se va a conectar a una instancia con nombre en otro equipo, escriba el nombre del equipo, una barra diagonal inversa y el nombre de la instancia, *por ejemplo,* mi \\ *instancia*.  
  
**Puerto del servidor**  
Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no está configurada para aceptar conexiones en el puerto predeterminado (1433), escriba el número de puerto. De lo contrario, deje este valor en blanco.  
  
**Base de datos**  
Especifique la base de datos a la que se van a migrar objetos y datos. Esta opción no está disponible cuando se vuelve a conectar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Autenticación**  
Seleccione el método de autenticación que se utiliza para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para usar la cuenta de Windows actual, seleccione autenticación de Windows. Para especificar un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión y una contraseña, seleccione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.  
  
**Nombre de usuario**  
Si usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la autenticación de, escriba el inicio de sesión de para esa instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si usa la autenticación de Windows, esta opción no está disponible.  
  
**Contraseña**  
Si usa la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación de, escriba la contraseña para el inicio de sesión de en esa instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si usa la autenticación de Windows, esta opción no está disponible.  
  
**Cifrar conexión**  
Si desea conectarse de forma segura a SQL Server, haga uso de cifrar conexión activando la casilla **cifrar conexión** .  
  
**TrustServerCertificate**  
Si desea usar esta opción, active la casilla **confiar en certificado de servidor** .  
  
> [!NOTE]  
> Para habilitar el **certificado de servidor de confianza**, "Encrypt" debe establecerse en **true**.  
  

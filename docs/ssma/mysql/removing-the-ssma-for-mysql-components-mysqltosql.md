---
description: Eliminación de los componentes de SSMA para MySQL (MySQLToSql)
title: Quitar los componentes de SSMA para MySQL (MySQLToSql) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Uninstalling, Extension pack
- Uninstalling, SSMA for MySQL client
ms.assetid: 87cdbd49-a0c9-4b00-8a93-34188b18d11a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 898b55146317a09b43b1b4df63a22a9a74882ee9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074646"
---
# <a name="removing-the-ssma-for-mysql-components-mysqltosql"></a>Eliminación de los componentes de SSMA para MySQL (MySQLToSql)
Cuando haya terminado de migrar las bases de datos de MySQL a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , es posible que desee desinstalar los componentes de SSMA. Puede desinstalar los componentes de cliente en cualquier momento. Sin embargo, Si desinstala el paquete de extensión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA dejará de admitir la migración de datos de MySQL a la base de datos de destino (SQL Server/SQL Azure) mediante el Server-Side motor de migración de datos.  
  
## <a name="uninstalling-the-ssma-for-mysql-client"></a>Desinstalación de SSMA para el cliente MySQL  
Puede desinstalar SSMA mediante **Agregar o quitar programas**.  
  
**Para desinstalar SSMA**  
  
1.  En el Panel de control, abra **Agregar o quitar programas**.  
  
2.  Seleccione **Microsoft SQL Server Migration Assistant para MySQL** y, a continuación, haga clic en **quitar**.  
  
3.  Para confirmar que desea desinstalar SSMA, haga clic en **sí**.  
  
## <a name="uninstalling-the-extension-pack"></a>Desinstalación del paquete de extensiones  
Puede quitar el paquete de extensión mediante **Agregar o quitar programas**.  
  
**Para desinstalar el paquete de extensiones**  
  
1.  En el Panel de control, abra **Agregar o quitar programas**.  
  
2.  Seleccione **Microsoft SQL Server Migration Assistant para el paquete de extensiones de MySQL** y, a continuación, haga clic en **quitar**.  
  
3.  Para confirmar que desea desinstalar el paquete de extensión, haga clic en **sí**.  
  
4.  En la página instancias con scripts de base de datos de utilidades, seleccione una instancia y, a continuación, haga clic en **siguiente**.  
  
5.  En la página parámetros de conexión, seleccione el método de autenticación y, a continuación, haga clic en **siguiente**.  
  
    La autenticación de Windows utilizará las credenciales de Windows para intentar iniciar sesión en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si selecciona [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación, debe escribir un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nombre de inicio de sesión y una contraseña.  
  
6.  En la página operación completada, haga clic en **Aceptar**.  
  
7.  En la página finalizar, haga clic en **salir**.  
  
Una vez completado el proceso de desinstalación, puede confirmar que los objetos del esquema de **sysdb.ssma_MySQL** y, posiblemente, toda la base de datos **sysdb** , se han quitado mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Sin embargo, si utiliza otros productos de SSMA, también usarán la base de datos **sysdb** . Si la base de datos existe y está seguro de que no hay ninguna otra base de datos que haga referencia a los objetos de esta base de datos, puede separar la base de datos.  
  
## <a name="see-also"></a>Consulte también  
[Instalación de SSMA para MySQL Client &#40;MySQLToSQL&#41;](../../ssma/mysql/installing-ssma-for-mysql-client-mysqltosql.md)  
[Instalación de componentes de SSMA en SQL Server](installing-ssma-components-on-sql-server-mysqltosql.md)  
  

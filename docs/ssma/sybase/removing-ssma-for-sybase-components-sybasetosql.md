---
description: Eliminación de componentes de SSMA para Sybase (SybaseToSQL)
title: Quitar SSMA para componentes de Sybase (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: aec09593-17d9-4ec2-ac56-3cd8851406fd
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 02a782e583463e5a986e3c7c1a42152fc27b14cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069950"
---
# <a name="removing-ssma-for-sybase-components-sybasetosql"></a>Eliminación de componentes de SSMA para Sybase (SybaseToSQL)
Cuando haya terminado de migrar las bases de datos de Sybase Adaptive Server Enterprise (ASE) a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , es posible que desee desinstalar los componentes de SSMA. Puede desinstalar los componentes de cliente en cualquier momento, pero no debe desinstalar el paquete de extensiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a menos que esté seguro de que las bases de datos migradas ya no usan funciones en el esquema de **ssma_syb** de la base de datos **sysdb** .  
  
## <a name="uninstalling-the-ssma-for-sybase-client"></a>Desinstalación de SSMA para el cliente de Sybase  
Puede desinstalar SSMA mediante **Agregar o quitar programas**.  
  
**Para desinstalar SSMA**  
  
1.  En el Panel de control, abra **Agregar o quitar programas**.  
  
2.  Seleccione **Microsoft SQL Server Migration Assistant para Sybase** y, a continuación, haga clic en **quitar**.  
  
3.  Para confirmar que desea desinstalar SSMA, haga clic en **sí**.  
  
## <a name="uninstalling-the-extension-pack"></a>Desinstalación del paquete de extensiones  
Si está seguro de que las bases de datos migradas no usan objetos en el esquema de **sysdb.ssma_syb** , puede quitar el paquete de extensión mediante **Agregar o quitar programas**.  
  
Para desinstalar el paquete de extensiones  
  
1.  En el Panel de control, abra **Agregar o quitar programas**.  
  
2.  Seleccione **Microsoft SQL Server Migration Assistant para Sybase Extension Pack** y, a continuación, haga clic en **quitar**.  
  
3.  Para confirmar que desea desinstalar el paquete de extensión, haga clic en **sí**.  
  
4.  En la página instancias con scripts de base de datos de utilidades, haga clic en **siguiente**.  
  
5.  En la página parámetros de conexión, seleccione el método de autenticación y, a continuación, haga clic en **siguiente**.  
  
    La autenticación de Windows usará sus credenciales de Windows para intentar iniciar sesión en la instancia de SQL Server. Si selecciona SQL Server autenticación, debe escribir un nombre de inicio de sesión de SQL Server y una contraseña.  
  
6.  En la página operación completada, haga clic en **Aceptar**.  
  
7.  En la página finalizar, haga clic en **salir**.  
  
Después de desinstalar, puede confirmar que el esquema de **sysdb.ssma_syb** y, posiblemente, toda la base de datos **sysdb** , se ha quitado mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Sin embargo, si utiliza otros productos de SSMA, también usarán la base de datos **sysdb** . Si la base de datos existe y está seguro de que ninguna otra base de datos hace referencia a objetos de esta base de datos, puede separar la base de datos.  
  
## <a name="see-also"></a>Consulte también  
[Instalación de SSMA para el cliente de Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)  
[Instalación de componentes de SSMA en SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
  

---
title: Instalación de SQL Server Migration Assistant para Access (AccessToSQL) | Microsoft Docs
description: Obtenga información sobre los requisitos previos de instalación de SQL Server Migration Assistant (SSMA) para el acceso y cómo instalar, licencia, actualizar y desinstalar.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- installing SSMA
- instructions, installation
- instructions, upgrade
- licensing SSMA
- prerequisites for installing SSMA
- procedure, installation
- procedure, licensing
- procedure, upgrading
- removing SSMA
- Setup
- uninstalling SSMA
- upgrading SSMA
ms.assetid: dd50eebd-75df-4e0d-8c4d-88b511aae4c7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3e34f70a41816dcf9a6009f8b38feb016f409a3a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078976"
---
# <a name="installing-sql-server-migration-assistant-for-access-accesstosql"></a>Instalación de SQL Server Migration Assistant para Access (AccessToSQL)

[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Migration Assistant (SSMA) para el acceso se instala mediante un asistente basado en Windows Installer. En este tema se proporciona información sobre los requisitos previos de instalación, un vínculo a la versión más reciente de SSMA e instrucciones para instalar, conceder licencias, desinstalar y actualizar SSMA.

## <a name="prerequisites"></a>Prerrequisitos

Antes de instalar SSMA, asegúrese de que el sistema cumple los requisitos siguientes:

- Windows 7 o una versión posterior, o Windows Server 2008 o una versión posterior.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o una versión posterior.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] versión .NET Framework 4.7.2 o una versión posterior. La .NET Framework está disponible en [Microsoft .net guía](/dotnet/framework/).
- Acceso y permisos suficientes en el equipo que hospeda la instancia de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] en la que se van a migrar los datos y los objetos de base de datos.
- Proveedor de Microsoft Data Access Object (DAO) versión 12,0 o 14,0. Puede instalar el proveedor de DAO desde Microsoft Office producto 2010/2007 o descargarlo desde el sitio web de Microsoft.
- 4 GB de RAM (recomendado).

## <a name="installing-ssma"></a>Instalación de SSMA

SSMA es una descarga Web. Para descargar la versión más reciente, consulte la [Página de descarga de SQL Server Migration Assistant](https://aka.ms/ssmaforaccess).

> [!IMPORTANT]
> Desinstale todas las versiones anteriores de SSMA para Access antes de instalar la nueva versión.

Para instalar SSMA:
  
1. Haga doble clic en **SSMAforAccess_ *n*. msi**, donde *n* es el número de compilación.
2. En la página de bienvenida, haga clic en **Siguiente**.

   Si no tiene instalados los requisitos previos, aparece un mensaje que indica que primero debe instalar los componentes necesarios. Asegúrese de que ha instalado todos los requisitos previos y, a continuación, vuelva a ejecutar el programa de instalación.

3. Lea el contrato de licencia de End-User. Si está de acuerdo, seleccione Acepto **el contrato** y, a continuación, haga clic en **siguiente**.
4. En la página **elegir tipo de instalación** , haga clic en **típica**.
5. En la página **listo para instalar** puede habilitar o deshabilitar la telemetría y las comprobaciones de actualizaciones automáticas cada vez que se inicia la herramienta. Haga clic en **Instalar** para iniciar la instalación.
  
La ubicación de instalación predeterminada es `C:\Program Files\Microsoft SQL Server Migration Assistant for Access`.

## <a name="uninstalling-ssma-for-access"></a>Desinstalación de SSMA para Access

Desinstale SSMA mediante **Agregar o quitar programas** en el panel de control. Tenga en cuenta que al desinstalar el programa, no se eliminan los archivos de proyecto o de registro de SSMA.

Para desinstalar SSMA:

1. Haga clic en **Inicio**, haga clic en **Panel de Control** y, a continuación, haga clic en **Agregar o quitar programas**.
2. Seleccione **Microsoft SQL Server Migration Assistant para el acceso** y, a continuación, haga clic en **quitar**.

## <a name="upgrading-to-a-later-version"></a>Actualizar a una versión posterior

Si desea actualizar a una versión posterior de SSMA para el acceso, primero debe desinstalar SSMA para el acceso y, a continuación, instalar la versión más reciente. Siga las instrucciones de la sección desinstalación de SSMA for Access para completar este proceso.

Si abre un proyecto creado en una versión anterior de SSMA para Access, SSMA puede preguntar si desea convertir el proyecto a la versión más reciente. Haga clic en **sí** para trabajar con el proyecto en la versión más reciente de SSMA.

## <a name="see-also"></a>Consulte también

- [Preparar las bases de datos de Access para la migración](preparing-access-databases-for-migration-accesstosql.md)
- [Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
- [Vincular aplicaciones de Access a SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)
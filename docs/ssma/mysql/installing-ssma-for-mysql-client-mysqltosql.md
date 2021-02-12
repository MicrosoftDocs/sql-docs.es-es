---
title: Instalación de SSMA para el cliente de MySQL (MySQLToSQL) | Microsoft Docs
description: Obtenga información sobre los requisitos previos de instalación para el SQL Server Migration Assistant (SSMA) para el cliente MySQL y cómo instalar.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Installing client,Licensing
ms.assetid: ede3128c-370d-45a5-a815-3d94eecaea30
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e2eaa9daf9128b43ac061cad66e7e993882e43d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070160"
---
# <a name="installing-ssma-for-mysql-client-mysqltosql"></a>Instalación de SSMA para el cliente de MySQL (MySQLToSQL)

SSMA para el cliente de MySQL se compone de los archivos de programa que realizan las siguientes tareas:

- Conectarse a una base de datos MySQL.  
- Conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .
- Convierta los objetos de base de datos MySQL en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] objetos o.
- Cargue los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .
- Migre datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

En este tema se proporcionan los requisitos previos de instalación y las instrucciones para instalar SSMA para el cliente de MySQL.

## <a name="prerequisites"></a>Prerrequisitos

SSMA para MySQL está diseñado para funcionar con MySQL 4,1 o versiones posteriores y todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 o posterior, y [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

Antes de instalar SSMA, asegúrese de que el equipo cumple los requisitos siguientes:

- Windows 7 o versiones posteriores, o Windows Server 2008 o versiones posteriores.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o una versión posterior.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] versión 4.7.2 o una versión posterior. Puede obtenerlo en el [Centro para desarrolladores de .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- Controlador de MySQL ODBC 5,1 y conectividad con las bases de datos de MySQL que desea migrar. Puede instalar MySQL desde el sitio web de MySQL. Para obtener información sobre la conectividad, consulte [conexión a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md).
- Acceso y permisos suficientes en el equipo que hospeda la instancia de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde se van a migrar los datos y los objetos de base de datos. Para obtener más información, vea [conectarse a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md).
- En el caso de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] los proyectos, acceso a y permisos suficientes para la instancia de donde va a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] migrar los datos y los objetos de base de datos. Para obtener más información, vea [conectarse a Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md).
- se recomiendan 4 GB de RAM.

## <a name="installing-ssma-for-mysql-client"></a>Instalación de SSMA para el cliente de MySQL

SSMA es una descarga Web. Para descargar la versión más reciente, consulte la [Página de descarga de SQL Server Migration Assistant](https://aka.ms/ssmaformysql).

Para instalar el cliente de SSMA:

1. Haga doble clic en **SSMAforMySQL_ *n*. msi**, donde *n* es el número de compilación.
2. En la página **principal**, haga clic en **Siguiente**.

   Si no tiene instalados los requisitos previos, aparecerá un mensaje que indica que primero debe instalar los componentes necesarios. Asegúrese de que ha instalado todos los requisitos previos antes de volver a ejecutar el programa de instalación.

3. Lea el contrato de licencia de End-User. Si está de acuerdo, seleccione Acepto **el contrato** y, a continuación, haga clic en **siguiente**.
4. En la página **elegir tipo de instalación** , haga clic en **típica**.
5. En la página **listo para instalar** puede habilitar o deshabilitar la telemetría y las comprobaciones de actualizaciones automáticas cada vez que se inicia la herramienta. Haga clic en **Instalar** para iniciar la instalación.

> [!IMPORTANT]
> Desinstale todas las versiones anteriores de SSMA para MySQL antes de instalar la nueva versión.

La ubicación de instalación predeterminada es `C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL`.

## <a name="see-also"></a>Consulte también

- [Migración de bases de datos de MySQL a SQL Server-Azure SQL Database](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  

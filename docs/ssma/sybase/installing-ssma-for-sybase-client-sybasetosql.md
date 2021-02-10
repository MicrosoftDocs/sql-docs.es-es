---
title: Instalación de SSMA para el cliente de SAP ASE (SybaseToSQL) | Microsoft Docs
description: Obtenga información sobre los requisitos previos de instalación de SQL Server Migration Assistant (SSMA) for SAP Adaptive Server Enterprise (ASE) y cómo instalar.
ms.custom: ''
ms.date: 07/14/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: e770c2f2-52b9-4471-a207-0d35df41399c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d9b20774b0a32b0d9d007053bedd2674e79d4e12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015376"
---
# <a name="installing-ssma-for-sap-ase-client-sybasetosql"></a>Instalación de SSMA para el cliente de SAP ASE (SybaseToSQL)

El cliente de SSMA está formado por los archivos de programa que se usan para conectarse a un servidor de base de datos de SAP Adaptive Server Enterprise (ASE) y una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , a convertir los objetos de base de datos de ase a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] la sintaxis de, cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] y, a continuación, migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

En este tema se proporcionan los requisitos previos de instalación y las instrucciones para instalar SSMA.

## <a name="prerequisites"></a>Prerrequisitos

SSMA está diseñado para funcionar con SAP ASE 11.9.2 o versiones posteriores y todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Antes de instalar SSMA, asegúrese de que el equipo cumple los requisitos siguientes:

- Windows 7 o versiones posteriores, o Windows Server 2008 o versiones posteriores.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o una versión posterior.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] versión .NET Framework 4.7.2 o una versión posterior. Puede obtenerlo en el [Centro para desarrolladores de .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- El proveedor de Sybase OLE DB/ADO.Net/ODBC y la conectividad con el servidor de base de datos de SAP ASE que contiene las bases de datos que desea migrar. Puede instalar proveedores desde los medios del producto de SAP ASE. Para obtener información sobre la conectividad, consulte [conexión a Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).
- Acceso y permisos suficientes en el equipo que hospeda la instancia de destino de o en el que va a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] migrar los datos y los objetos de base de datos. Para obtener más información, consulte [conexión a SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sql-server-sybasetosql.md) / [conexión a Azure SQL Database &#40;&#41;SybaseToSQL ](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md).
- se recomiendan 4 GB de RAM.

## <a name="installing-the-ssma-for-sybase-client"></a>Instalación de SSMA para el cliente de Sybase

SSMA es una descarga Web. Para descargar la versión más reciente, consulte la [Página de descarga de SQL Server Migration Assistant](https://aka.ms/ssmaforsybase).

Para instalar el cliente de SSMA:

1. Haga doble clic en **SSMAforSybase_ *n*. msi**, donde *n* es el número de compilación.
2. En la página de bienvenida, haga clic en **Siguiente**.

   Si no tiene instalados los requisitos previos, aparecerá un mensaje que indica que primero debe instalar los componentes necesarios. Asegúrese de que ha instalado todos los requisitos previos y, a continuación, vuelva a ejecutar el programa de instalación.

3. Lea el contrato de licencia de End-User. Si está de acuerdo, seleccione Acepto **el contrato** y, a continuación, haga clic en **siguiente**.
4. En la página elegir tipo de instalación, haga clic en **típica**.
5. En la página **listo para instalar** puede habilitar o deshabilitar la telemetría y las comprobaciones de actualizaciones automáticas cada vez que se inicia la herramienta. Haga clic en **Instalar** para iniciar la instalación.

> [!IMPORTANT]
> Desinstale todas las versiones anteriores de SSMA para Sybase antes de instalar la nueva versión.

La ubicación de instalación predeterminada es `C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase`.

Además de los archivos de programa de SSMA, también debe instalar SSMA for Sybase Extension Pack en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, consulte [instalación de componentes de SSMA en SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md).

## <a name="see-also"></a>Consulte también

- [Instalación de componentes de SSMA en SQL Server](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
- [Migración de bases de datos de Sybase ASE a SQL Server-Azure SQL Database](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)

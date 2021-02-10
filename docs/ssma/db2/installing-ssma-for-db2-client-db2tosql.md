---
title: Instalación de SSMA para el cliente DB2 (DB2ToSQL) | Microsoft Docs
description: Obtenga información sobre los requisitos previos de instalación del cliente de SQL Server Migration Assistant (SSMA) para DB2 y cómo instalar.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 3ae2a470-6afd-4512-b6d1-fcbe6afe88ad
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: dd9c4116acd482fa12b74556b9d1ad1fe9b06cdd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014765"
---
# <a name="installing-ssma-for-db2-client-db2tosql"></a>Instalación de SSMA para el cliente DB2 (DB2ToSQL)

El cliente de SSMA está formado por los archivos de programa que realizan las siguientes tareas:

- Conéctese a una base de datos DB2.
- Conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Convierte los objetos de base de datos DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sintaxis.
- Cargue los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
- Migre los datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

En este tema se proporcionan los requisitos previos de instalación y las instrucciones para instalar SSMA.

## <a name="prerequisites"></a>Prerrequisitos

SSMA está diseñado para funcionar con DB2 en la versión 9,0 y 10,0 de z/OS, DB2 en la versión 9,8 de LUW y 10,1 o versiones posteriores, DB2 para i versión 7,1 o superior y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 o versiones posteriores.

Antes de instalar SSMA, asegúrese de que el equipo cumple los requisitos siguientes:

- Windows 7 o versiones posteriores, o Windows Server 2008 o versiones posteriores.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o versiones posteriores.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] versión 4.7.2 o una versión posterior. Puede obtenerlo en el [Centro para desarrolladores de .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- Proveedor OLE DB de Microsoft para DB2 versión 5 o una versión posterior y la conectividad con las bases de datos DB2 que desea migrar.
- Acceso a y permisos suficientes en el equipo que hospeda la instancia de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database donde va a migrar los datos y los objetos de base de datos. Para obtener más información, vea [conectarse a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md).
- se recomiendan 4 GB de RAM.

## <a name="microsoft-ole-db-provider-for-db2"></a>Proveedor Microsoft OLE DB para DB2

Para descargar el proveedor de OLE DB para DB2 versión 6,0, vaya a [Microsoft® SQL Server® 2017 Feature Pack](https://www.microsoft.com/download/details.aspx?id=55992).

SSMA es una descarga Web. Para descargar la versión más reciente, consulte la [Página de descarga de SQL Server Migration Assistant](https://aka.ms/ssmafordb2).

Para instalar el cliente de SSMA:

1. Haga doble clic en **SSMAforDB2_ *n*. msi**, donde *n* es el número de compilación.
2. En la página **principal**, seleccione **Siguiente**.

   Si no tiene instalados los requisitos previos, aparecerá un mensaje que indica que primero debe instalar los componentes necesarios. Asegúrese de que ha instalado todos los requisitos previos y, a continuación, vuelva a ejecutar el programa de instalación.

3. Lea el contrato de licencia de End-User. Si está de acuerdo, seleccione Acepto **el contrato** y, a continuación, seleccione **siguiente**.
4. En la página **elegir tipo de instalación** , seleccione **típica**.
5. En la página **listo para instalar** puede habilitar o deshabilitar la telemetría y las comprobaciones de actualizaciones automáticas cada vez que se inicia la herramienta. Haga clic en **Instalar** para iniciar la instalación.

> [!IMPORTANT]
> Desinstale todas las versiones anteriores de SSMA para DB2 antes de instalar la nueva versión.

La ubicación de instalación predeterminada es `C:\Program Files\Microsoft SQL Server Migration Assistant for DB2`.

## <a name="see-also"></a>Consulte también

- [Instalación de componentes de SSMA en SQL Server](../../ssma/db2/installing-ssma-components-on-sql-server-db2tosql.md)
- [Migración de bases de datos de DB2 a SQL Server](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)

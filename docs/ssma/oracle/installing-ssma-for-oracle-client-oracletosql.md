---
title: Instalación de SSMA para el cliente de Oracle (OracleToSQL) | Microsoft Docs
description: Obtenga información sobre los requisitos previos de instalación para el SQL Server Migration Assistant (SSMA) para el cliente de Oracle y cómo instalar.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d5d4903d-e296-4bbf-8780-63674c4d62d5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 5bed1589c0852acc2d782028f6b9cc253301babe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073499"
---
# <a name="installing-ssma-for-oracle-client-oracletosql"></a>Instalación de SSMA para el cliente de Oracle (OracleToSQL)

El cliente de SSMA está formado por los archivos de programa que realizan las siguientes tareas:  
  
- Conéctese a una base de datos de Oracle.
- Conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Convertir objetos de base de datos de Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sintaxis.
- Cargue los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
- Migre los datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

En este tema se proporcionan los requisitos previos de instalación y las instrucciones para instalar SSMA.

## <a name="prerequisites"></a>Prerrequisitos

SSMA está diseñado para funcionar con Oracle 9 o versiones posteriores y todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Antes de instalar SSMA, asegúrese de que el equipo cumple los requisitos siguientes:

- Windows 7 o versiones posteriores, o Windows Server 2008 o versiones posteriores.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 o una versión posterior.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] versión 4.7.2 o una versión posterior. Puede obtenerlo en el [Centro para desarrolladores de .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- Conectividad con las bases de datos de Oracle que desea migrar.
- Acceso y permisos suficientes en el equipo que hospeda la instancia de destino de o en el que va a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] migrar los datos y los objetos de base de datos. Para obtener más información, vea [conectarse a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md).
- se recomiendan 4 GB de RAM.  
  
## <a name="installing-the-ssma-for-oracle-client"></a>Instalación de SSMA para el cliente de Oracle

SSMA es una descarga Web. Para descargar la versión más reciente, consulte la [Página de descarga de SQL Server Migration Assistant](https://aka.ms/ssmafororacle).

Para instalar el cliente de SSMA:

1. Haga doble clic en **SSMAforOracle_ *n*. msi**, donde *n* es el número de compilación.
2. En la página de bienvenida, haga clic en **Siguiente**.

   Si no tiene instalados los requisitos previos, aparecerá un mensaje que indica que primero debe instalar los componentes necesarios. Asegúrese de que ha instalado todos los requisitos previos y, a continuación, vuelva a ejecutar el programa de instalación.  

3. Lea el contrato de licencia de End-User. Si está de acuerdo, seleccione Acepto **el contrato** y, a continuación, haga clic en **siguiente**.
4. En la página **elegir tipo de instalación** , haga clic en **típica**.
5. En la página **listo para instalar** puede habilitar o deshabilitar la telemetría y las comprobaciones de actualizaciones automáticas cada vez que se inicia la herramienta. Haga clic en **Instalar** para iniciar la instalación.

> [!IMPORTANT]
> Desinstale todas las versiones anteriores de SSMA para Oracle antes de instalar la nueva versión.

La ubicación de instalación predeterminada es `C:\Program Files\Microsoft SQL Server Migration Assistant for Oracle`.

Además de los archivos de programa de SSMA, también debe instalar el paquete de extensión SSMA para Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, consulte [instalación de componentes de SSMA en SQL Server](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md).

## <a name="see-also"></a>Consulte también

- [Instalación de componentes de SSMA en SQL Server](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md)
- [Migrar bases de datos de Oracle a SQL Server](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)

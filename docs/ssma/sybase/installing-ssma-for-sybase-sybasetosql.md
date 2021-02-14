---
title: Instalación de SSMA para SAP ASE (SybaseToSQL) | Microsoft Docs
description: Use estos artículos para instalar, actualizar y desinstalar SQL Server Migration Assistant para SAP ASE, que incluye una aplicación cliente y un paquete de extensión.
ms.custom: ''
ms.date: 11/29/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8d5a4ce6-b747-46e3-9184-645d56e8b35c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 8fca8219dfe89c8229b8d296fe355b97417d9daf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073379"
---
# <a name="installing-ssma-for-sap-ase-sybasetosql"></a>Instalación de SSMA para SAP ASE (SybaseToSQL)
[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) para SAP Adaptive Server Enterprise (ASE) se compone de una aplicación cliente que se utiliza para realizar una migración desde el ASE de SAP a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. También contiene un paquete de extensión que admite la migración de datos y el uso de funciones del sistema ASE en las bases de datos migradas.  
  
Instale la aplicación cliente en el equipo desde el que va a realizar los pasos de migración. Instale los archivos del paquete de extensión en el equipo que ejecuta en el que se van [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a hospedar las bases de datos migradas.  
  
## <a name="upgrading-ssma-for-sap-ase"></a>Actualización de SSMA para SAP ASE  
Si desea actualizar a una versión posterior de SSMA para SAP ASE, primero debe desinstalar el cliente y el paquete de extensiones de servidor. A continuación, instale la versión más reciente.  
  
Si abre un proyecto de una versión anterior de SSMA para SAP ASE, SSMA le pregunta si desea convertir el proyecto a la versión más reciente. Haga clic en **sí** para trabajar con el proyecto en la versión más reciente de SSMA.  
  
## <a name="contents"></a>Contenido  
  
|Artículo|Descripción|  
|---------|---------------|  
|[Instalación de SSMA para el cliente de SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)|Proporciona información e instrucciones para instalar el cliente de SSMA para SAP ASE.|  
|[Instalación de componentes de SSMA en SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)|Proporciona información e instrucciones para instalar el paquete de extensión en las instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Quitar SSMA para componentes de SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/removing-ssma-for-sybase-components-sybasetosql.md)|Proporciona instrucciones para desinstalar el programa cliente y el paquete de extensión.|  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de SAP ASE a SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  

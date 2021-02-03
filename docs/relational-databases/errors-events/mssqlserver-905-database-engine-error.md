---
description: MSSQLSERVER_905
title: MSSQLSERVER_905 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 905 (Database Engine error)
ms.assetid: c828bb2e-e554-4f81-b76c-2b3740d2b944
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8a15042fcfaa82e6fa3380455c9f6a28050a6b20
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161961"
---
# <a name="mssqlserver_905"></a>MSSQLSERVER_905
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|905|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBSTARTUP_EE_PARTITIONING|  
|Texto del mensaje|La base de datos '%. * ls' no se puede iniciar en esta edición de SQL Server porque contiene una función de partición '%.\*ls'. Únicamente la edición Enterprise de SQL Server admite particionamiento.|  
  
## <a name="explanation"></a>Explicación  
La base de datos contiene una o varias tablas o índices con particiones. Esta edición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede utilizar particionamiento. En consecuencia, la base de datos no se puede iniciar correctamente. Las tablas e índices con particiones no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
## <a name="user-action"></a>Acción del usuario  
Separe la base de datos mediante el procedimiento almacenado sp_detach_db. Mueva los archivos si es necesario, y, a continuación, asocie la base de datos a una instancia de una edición [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizando CREATE DATABASE con las opciones FOR ATTACH o FOR ATTACH_REBUILD_LOG. Deshabilite el particionamiento en todas las tablas y quite las funciones de particionamiento. Desasocie la base de datos de nuevo y vuelva a adjuntarla al servidor actual.  
  

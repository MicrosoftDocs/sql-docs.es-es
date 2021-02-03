---
description: MSSQLSERVER_3169
title: MSSQLSERVER_3169 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
f1_keywords:
- "3169"
helpviewer_keywords:
- 3169 (Database Engine error)
ms.assetid: 7d4dbed6-bb94-4908-bc03-2040a9cf63bc
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 176ca9ec55225ea49dbcf6c7fdf5de6c5a2f2192
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194199"
---
# <a name="mssqlserver_3169"></a>MSSQLSERVER_3169
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|3169|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|N/D|  
|Texto del mensaje|Se realizó una copia de seguridad de la base de datos en una versión de servidor %ls. Esta versión no es compatible con este servidor, que utiliza la versión %ls. Restaure la base de datos en un servidor que admita la copia de seguridad, o utilice una copia de seguridad que sea compatible con este servidor.|  
  
## <a name="explanation"></a>Explicación  
Algunas características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afectan a la estructura de los archivos de base de datos. Al restaurar la base de datos en otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], es posible que el formato de archivo no sea compatible con una versión distinta de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
Por ejemplo, este error puede deberse al uso del formato de almacenamiento vardecimal en una versión posterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y a un intento posterior de restaurar los archivos de base de datos a una versión anterior al Service Pack 2 de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
## <a name="user-action"></a>Acción del usuario  
Determine la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que está ejecutándose en el servidor de origen. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], haga clic con el botón derecho en el servidor y, después, haga clic en **Propiedades** o escriba **SELECT \@\@VERSION** en una ventana de consulta. Abra la base de datos utilizando la versión original de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Examine las características que están habilitadas en la base de datos original en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Modifique esta configuración para trabajar con la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde se restaurará la base de datos.  
  

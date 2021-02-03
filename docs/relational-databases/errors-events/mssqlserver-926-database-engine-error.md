---
description: MSSQLSERVER_926
title: MSSQLSERVER_926 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 926 (Database Engine error)
ms.assetid: 57e01668-883b-4be4-84a8-a111caaf0486
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5df8bbda604e3cfc9b55406d2f18732205054ad1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190653"
---
# <a name="mssqlserver_926"></a>MSSQLSERVER_926
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|926|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DB_SUSPECT|  
|Texto del mensaje|No se puede abrir la base de datos '%.*ls'. Tiene la marca SUSPECT para recuperación. Para obtener más información, vea el registro de errores de SQL Server.|  
  
## <a name="explanation"></a>Explicación  
La base de datos se marca como sospechosa porque el proceso de recuperación generó errores que ponen la base de datos en un estado transaccional coherente. Esto puede suceder durante las operaciones siguientes:  
  
-   Al iniciar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Al adjuntar una base de datos.  
  
-   Al usar los procedimientos RESTORE base de datos o RESTORE LOG.  
  
## <a name="user-action"></a>Acción del usuario  
Examine el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y determine la causa del error. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ha reiniciado desde que la recuperación generó errores, busque en los registros de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anteriores para ver la razón de los errores de la recuperación.  
  
Si la recuperación no tiene éxito debido a un error de E/S persistente, una página rasgada u otro posible problema de hardware, solucione el problema de hardware subyacente y restaure la base de datos a partir de una copia de seguridad. Si no hay disponible ninguna copia de seguridad, considere las opciones de reparación de DBCC CHECKDB.  
  
Si no puede resolver este problema, póngase en contacto con su proveedor principal de soporte. Tenga disponible el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para revisarlo.  
  
## <a name="see-also"></a>Consulte también  
[Copia de seguridad y restauración de bases de datos de SQL Server](~/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
[RESTORE &#40;Transact-SQL&#41;](~/t-sql/statements/restore-statements-transact-sql.md)  
[sys.sysdatabases &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/sys-sysdatabases-transact-sql.md)  
[Adjuntar y separar bases de datos &#40;SQL Server&#41;](~/relational-databases/databases/database-detach-and-attach-sql-server.md)  
  

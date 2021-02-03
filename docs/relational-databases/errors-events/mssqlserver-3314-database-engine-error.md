---
description: MSSQLSERVER_3314
title: MSSQLSERVER_3314 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3314 (Database Engine error)
ms.assetid: f3a5ca6a-b502-4cab-b3b1-4bc753763fa9
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7b18cf411c241b3c0dc3549bf4503d93782c6f91
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190898"
---
# <a name="mssqlserver_3314"></a>MSSQLSERVER_3314
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|3314|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|ERR_LOG_RID2|  
|Texto del mensaje|Durante la puesta al día de una operación registrada en la base de datos '%.*ls', se produjo un error en la entrada de registro con id. %S_LSN. Normalmente, el error específico se registra antes como un error en el servicio Registro de eventos de Windows. Restaure la base de datos o el archivo a partir de una copia de seguridad completa o repare la base de datos.|  
  
## <a name="explanation"></a>Explicación  
Este es un error de acumulación de la recuperación de una operación de deshacer. Este error ha colocado la base de datos en el estado SUSPECT. El grupo de archivos principal, y posiblemente otros grupos de archivos, es sospechoso y puede estar dañado. La base de datos no se puede recuperar durante el inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, por consiguiente, no está disponible. Se requiere una acción por parte del usuario para resolver el problema.  
  
Tenga en cuenta que si este error se produce para **tempdb**, la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se cierra.  
  
## <a name="user-action"></a>Acción del usuario  
Una condición transitoria que se dio en el sistema durante un intento determinado de iniciar la instancia del servidor o recuperar una base de datos puede producir este error. También puede deberse a un error permanente que se produce cada vez que se intenta iniciar la base de datos. Para obtener información acerca de la causa, examine el registro de eventos de Windows para ver si contiene un error anterior que indique el error concreto.  
  
Para obtener información sobre la causa de esta repetición del error 3314, busque en el registro de eventos de Windows un error anterior que indique el error específico. La acción del usuario adecuada depende de si la información del registro de eventos de Windows indica que el error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo causó una condición transitoria o un error permanente. Para obtener información sobre las acciones del usuario para solucionar el error 3314, vea los Libros en pantalla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Consulte también  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Restauraciones de base de datos completas &#40;modelo de recuperación simple&#41;](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  

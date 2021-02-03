---
description: MSSQLSERVER_3159
title: MSSQLSERVER_3159 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3159 (Database Engine error)
ms.assetid: c93c1003-0e3a-40aa-9873-44a0f5b8b57e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d83e149a8f17c509f2c34c9284c43b92828091dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194225"
---
# <a name="mssqlserver_3159"></a>MSSQLSERVER_3159
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Id. de evento|3159|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|LDDB_LOGNOTBACKEDUP|  
|Texto del mensaje|No se hizo copia del final del registro de la base de datos "%ls". Use BACKUP LOG WITH NORECOVERY para realizar una copia de seguridad del registro si contiene trabajo que no desea perder. Utilice la cláusula WITH REPLACE o WITH STOPAT de la instrucción RESTORE para sobrescribir el contenido del registro.|  
  
## <a name="explanation"></a>Explicación  
En la mayoría de los casos, en los modelos de recuperación optimizado para cargas masivas de registros [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere que se realice una copia deL final del registro después del error para capturar las entradas del registro de las que todavía no se ha realizado una copia de seguridad. Una copia del final del registro que se realizan después del error, justo antes de una operación de restauración, se denominan copias del final del registro después del error.  
  
Cuando se recupera una base de datos al momento en que se produjo el error, la copia del final del registro después del error es la última copia de seguridad de interés del plan de recuperación. Si no puede realizar una copia del final del registro después del error, puede recuperar una base de datos solo al final de la última copia de seguridad que se creó antes del error.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], por lo general, requiere que se realice una copia del final del registro después del error antes de comenzar a restaurar una base de datos. La copia del final del registro después del error evita la pérdida de trabajo y mantiene intacta la cadena de registros. Sin embargo, no todos los escenarios de restauración requieren una copia del final del registro después del error. No es necesario tener una copia del final del registro si el punto de recuperación está incluido en una copia de seguridad de registros anterior, o si está moviendo o reemplazando (sobrescribiendo) la base de datos y no necesita restaurarla a un momento después de la copia de seguridad más reciente. También, si los archivos de registro están dañados y no se puede crear una copia del final del registro, debe restaurar la base de datos sin utilizar una copia de seguridad de registros después del error. Las transacciones confirmadas después de la última copia de seguridad de registros se perderán. Para obtener más información, vea "Restaurar sin utilizar una copia del final del registro después del error" más adelante en este tema.  
  
> [!CAUTION]  
> REPLACE no debe usarse a menudo y solo después de haberlo pensado detenidamente.  
  
## <a name="user-action"></a>Acción del usuario  
Haga una copia del final del registro después del error y vuelva a intentar la operación de restauración.  
  
Si no puede hacer una copia del final del registro después del error, utilice WITH STOPAT o WITH REPLACE en las instrucciones RESTORE.  
  
## <a name="see-also"></a>Consulte también  
[Restaurar una base de datos de SQL Server a un momento dado &#40;modelo de recuperación completa&#41;](~/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
[Realizar una copia de seguridad del registro de transacciones cuando la base de datos está dañada &#40;SQL Server&#41;](~/relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)  
[Realizar una copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](~/relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
[Copias del final del registro &#40;SQL Server&#41;](~/relational-databases/backup-restore/tail-log-backups-sql-server.md)  
  

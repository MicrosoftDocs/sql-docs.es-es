---
description: 'Instrucciones RESTORE: VERIFYONLY (Transact-SQL)'
title: RESTORE VERIFYONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- VERIFYONLY
- RESTORE VERIFYONLY
- VERIFYONLY_TSQL
- RESTORE_VERIFYONLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE VERIFYONLY statement
- backups [SQL Server], verifying
- verifying backups
- checking backups
ms.assetid: cba3b6a0-b48e-4c94-812b-5b3cbb408bd6
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: b76a49d34d0603d62ee7160bb2f4ef9c32d482a7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237794"
---
# <a name="restore-statements---verifyonly-transact-sql"></a>Instrucciones RESTORE: VERIFYONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Comprueba la copia de seguridad, pero no la restaura, y comprueba si el conjunto de la copia de seguridad se ha completado y se puede leer en su totalidad. Sin embargo, RESTORE VERIFYONLY no intenta comprobar la estructura de los datos que contienen los volúmenes de la copia de seguridad. En [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], RESTORE VERIFYONLY se ha mejorado para realizar comprobaciones adicionales en los datos a fin de aumentar la probabilidad de detectar errores. El objetivo es acercarse lo máximo posible a una operación de restauración real de forma práctica. Para obtener más información, vea la sección Notas.  

 Si la copia de seguridad es válida, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] devuelve un mensaje de operación correcta.  
  
> [!NOTE]  
>  Para obtener las descripciones de los argumentos, vea [Argumentos de RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
RESTORE VERIFYONLY  
FROM <backup_device> [ ,...n ]  
[ WITH    
 {  
   LOADHISTORY   
  
--Restore Operation Option  
 | MOVE 'logical_file_name_in_backup' TO 'operating_system_file_name'   
          [ ,...n ]   
  
--Backup Set Options  
 | FILE = { backup_set_file_number | @backup_set_file_number }   
 | PASSWORD = { password | @password_variable }   
  
--Media Set Options  
 | MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
--Monitoring Options  
 | STATS [ = percentage ]   
  
--Tape Options  
 | { REWIND | NOREWIND }   
 | { UNLOAD | NOUNLOAD }    
 } [ ,...n ]  
]  
[;]  
  
<backup_device> ::=  
{   
   { logical_backup_device_name |  
      @logical_backup_device_name_var }  
   | { DISK | TAPE | URL } = { 'physical_backup_device_name' |  
       @physical_backup_device_name_var }   
}  
  
```  
 > [!NOTE] 
> URL es el formato que se usa para especificar la ubicación y el nombre del archivo para Microsoft Azure Blob Storage y se admite a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 CU2. Aunque Microsoft Azure Storage es un servicio, la implementación es similar al disco y la cinta para permitir una experiencia de restauración coherente y sin problemas para los tres dispositivos.
 
## <a name="arguments"></a>Argumentos  
 Para obtener las descripciones de los argumentos RESTORE VERIFYONLY, vea [RESTORE Arguments &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md) (Argumentos de RESTORE [Transact-SQL]).  
  
## <a name="general-remarks"></a>Notas generales  
 El conjunto de medios o el conjunto de copia de seguridad debe contener la información mínima correcta para interpretarse como formato de cinta de Microsoft. Si no es así, RESTORE VERIFYONLY se detiene e indica que el formato de la copia de seguridad no es válido.  
  
 Entre las comprobaciones realizadas por RESTORE VERIFYONLY, se incluyen las siguientes:  
  
-   Que el conjunto de copia de seguridad ha finalizado y todos los volúmenes pueden leerse.  
  
-   Algunos campos de encabezado de páginas de base de datos, como el Id. de página (como si estuviera a punto de escribir los datos).  
  
-   Suma de comprobación (si está presente en los medios).  
  
-   Comprobar que existe espacio suficiente en los dispositivos de destino.  
  
> [!NOTE]  
>  RESTORE VERIFYONLY no funciona en una instantánea de base de datos. Para comprobar una instantánea de base de datos antes de realizar una operación de reversión, puede ejecutar DBCC CHECKDB.  
  
> [!NOTE]  
>  Con las copias de seguridad de instantánea, RESTORE VERIFYONLY confirma la existencia de las instantáneas en las ubicaciones especificadas en el archivo de copia de seguridad. Las copias de seguridad de instantánea son una característica nueva de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Para obtener más información sobre las copias de seguridad de instantánea, vea [Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
## <a name="security"></a>Seguridad  
 La operación de copia de seguridad puede especificar opcionalmente contraseñas para un conjunto de medios, para un conjunto de copia de seguridad o para ambos. Si se ha definido una contraseña en un conjunto de medios o un conjunto de copia de seguridad, debe especificar la contraseña o contraseñas correctas en la instrucción RESTORE. Estas contraseñas impiden operaciones de restauración y anexiones no autorizadas de los conjuntos de copia de seguridad en medios que utilizan herramientas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No obstante, la contraseña no impide que se sobrescriba el medio con la opción FORMAT de la instrucción BACKUP.  
  
> [!IMPORTANT]  
>  El nivel de protección que proporciona esta contraseña es bajo. El objetivo es impedir una restauración incorrecta con las herramientas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ya sea por parte de usuarios autorizados o no autorizados. No impide la lectura de los datos de las copias de seguridad por otros medios o el reemplazo de la contraseña. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]El procedimiento recomendado para proteger las copias de seguridad consiste en almacenar las cintas de copia de seguridad en una ubicación segura o en hacer una copia de seguridad en archivos de disco protegidos con las listas de control de acceso (ACL) adecuadas. Las ACL se deben establecer en el directorio raíz en el que se crean las copias de seguridad.  
  
### <a name="permissions"></a>Permisos  
 A partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], para obtener información sobre un conjunto de copia de seguridad o un dispositivo de copia de seguridad, es necesario el permiso CREATE DATABASE. Para obtener más información, vea [GRANT &#40;permisos de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
 
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se comprueba la copia de seguridad desde el disco.
  
```sql  
RESTORE VERIFYONLY FROM DISK = 'D:\AdventureWorks.bak';
GO
```  
  
## <a name="see-also"></a>Consulte también  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Historial de copias de seguridad e información de encabezados &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  

---
description: 'Instrucciones RESTORE: LABELONLY (Transact-SQL)'
title: RESTORE LABELONLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LABELONLY
- RESTORE_LABELONLY_TSQL
- LABELONLY_TSQL
- RESTORE LABELONLY
dev_langs:
- TSQL
helpviewer_keywords:
- RESTORE LABELONLY statement
- backup media [SQL Server], content information
ms.assetid: 7cf0641e-0d55-4ffb-9500-ecd6ede85ae5
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: e50ee3411fba5d2d3f5cfd1ddff100141cadf40b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186602"
---
# <a name="restore-statements---labelonly-transact-sql"></a>Instrucciones RESTORE: LABELONLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]
  Devuelve un conjunto de resultados que contiene información acerca del medio de copia de seguridad identificado por el dispositivo de copia de seguridad dado.  
  
> [!NOTE]  
>  Para obtener las descripciones de los argumentos, vea [Argumentos de RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
RESTORE LABELONLY   
FROM <backup_device>   
[ WITH   
 {  
--Media Set Options  
   MEDIANAME = { media_name | @media_name_variable }   
 | MEDIAPASSWORD = { mediapassword | @mediapassword_variable }  
  
--Error Management Options  
 | { CHECKSUM | NO_CHECKSUM }   
 | { STOP_ON_ERROR | CONTINUE_AFTER_ERROR }  
  
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
 Para obtener las descripciones de los argumentos de RESTORE LABELONLY, vea [Argumentos de RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 El conjunto de resultados de RESTORE LABELONLY está compuesto de una sola fila con esta información.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**MediaName**|**nvarchar(128)**|Nombre del medio.|  
|**MediaSetId**|**uniqueidentifier**|Número de identificación único del conjunto de medios.|  
|**FamilyCount**|**int**|Número de familias de medios en el conjunto de medios.|  
|**FamilySequenceNumber**|**int**|Número de secuencia de esta familia.|  
|**MediaFamilyId**|**uniqueidentifier**|Número de identificación único de la familia de medios.|  
|**MediaSequenceNumber**|**int**|Número de secuencia de este medio en la familia de medios.|  
|**MediaLabelPresent**|**tinyint**|Indica si la descripción del medio contiene:<br /><br /> **1** =  Etiqueta de medio en formato de cinta de [!INCLUDE[msCoName](../../includes/msconame-md.md)]<br /><br /> **0** = Descripción del medio|  
|**MediaDescription**|**nvarchar(255)**|Descripción del medio, en texto sin formato o etiqueta de medio en formato de cinta.|  
|**SoftwareName**|**nvarchar(128)**|Nombre del software para realizar copias de seguridad con que se escribió la etiqueta.|  
|**SoftwareVendorId**|**int**|Número de identificación único del fabricante del software con que se escribió la copia de seguridad.|  
|**MediaDate**|**datetime**|Fecha y hora en que se escribió la etiqueta.|  
|**Mirror_Count**|**int**|Número de reflejos en el conjunto (1-4).<br /><br /> Nota: Las etiquetas escritas para los distintos reflejos de un conjunto son idénticas.|  
|**IsCompressed**|**bit**|Si la copia de seguridad está comprimida:<br /><br /> 0 = no comprimida<br /><br /> 1 = comprimida|  
  
> [!NOTE]  
>  Si se definen contraseñas para el conjunto de medios, RESTORE LABELONLY solo mostrará información si se especifica la contraseña correcta en la opción MEDIAPASSWORD del comando.  
  
## <a name="general-remarks"></a>Notas generales  
 Ejecutar RESTORE LABELONLY constituye una forma rápida de averiguar el contenido del medio de copia de seguridad. Puesto que RESTORE LABELONLY solo lee el encabezado de medios, esta instrucción finaliza rápidamente incluso cuando se utilizan dispositivos de cinta de alta capacidad.  
  
## <a name="security"></a>Seguridad  
 Opcionalmente, la operación de copia de seguridad puede especificar contraseñas para un conjunto de medios. Si se ha definido una contraseña en un conjunto de medios, debe especificar la contraseña correcta en la instrucción RESTORE. Esta contraseña impide operaciones de restauración y anexiones no autorizadas de los conjuntos de copia de seguridad en medios que usan herramientas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No obstante, la contraseña no impide que se sobrescriba el medio con la opción FORMAT de la instrucción BACKUP.  
  
> [!IMPORTANT]  
>  El nivel de protección que proporciona esta contraseña es bajo. El objetivo es impedir una restauración incorrecta con las herramientas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ya sea por parte de usuarios autorizados o no autorizados. No impide la lectura de los datos de las copias de seguridad por otros medios o el reemplazo de la contraseña. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]El procedimiento recomendado para proteger las copias de seguridad consiste en almacenar las cintas de copia de seguridad en una ubicación segura o en hacer una copia de seguridad en archivos de disco protegidos con las listas de control de acceso (ACL) adecuadas. Las ACL se deben establecer en el directorio raíz en el que se crean las copias de seguridad.  
  
### <a name="permissions"></a>Permisos  
 En [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, la obtención de información sobre un conjunto de copia de seguridad o un dispositivo de copia de seguridad requiere el permiso CREATE DATABASE. Para obtener más información, vea [GRANT &#40;permisos de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE REWINDONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-rewindonly-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Historial de copias de seguridad e información de encabezados &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-history-and-header-information-sql-server.md)  
  
  

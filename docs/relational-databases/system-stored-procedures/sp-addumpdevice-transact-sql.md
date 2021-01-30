---
description: sp_addumpdevice (Transact-SQL)
title: sp_addumpdevice (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addumpdevice_TSQL
- sp_addumpdevice
dev_langs:
- TSQL
helpviewer_keywords:
- backup devices [SQL Server], defining
- sp_addumpdevice
ms.assetid: c2d2ae49-0808-46d8-8444-db69a69d0ec3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 68bad4307f35aebe34c7c2617881c5dc56c0a4ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198402"
---
# <a name="sp_addumpdevice-transact-sql"></a>sp_addumpdevice (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  

Agrega un dispositivo de copia de seguridad a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addumpdevice [ @devtype = ] 'device_type'   
    , [ @logicalname = ] 'logical_name'   
    , [ @physicalname = ] 'physical_name'  
      [ , { [ @cntrltype = ] controller_type |  
          [ @devstatus = ] 'device_status' }  
      ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @devtype = ] 'device_type'` Es el tipo de dispositivo de copia de seguridad. *device_type* es de tipo **VARCHAR (20)**, no tiene ningún valor predeterminado y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**discos**|Archivo de disco duro que se utiliza como dispositivo de copia de seguridad.|  
|**cinta**|Todos los dispositivos de cinta admitidos por [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.<br /><br /> Nota: La compatibilidad con dispositivos de cinta de copia de seguridad se quitará en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.|  
  
`[ @logicalname = ] 'logical_name'` Es el nombre lógico del dispositivo de copia de seguridad que se usa en las instrucciones BACKUP y RESTOre. *logical_name* es de **tipo sysname**, no tiene ningún valor predeterminado y no puede ser null.  
  
`[ @physicalname = ] 'physical_name'` Es el nombre físico del dispositivo de copia de seguridad. Los nombres físicos tienen que cumplir las reglas de nombres de archivo del sistema operativo o las convenciones de nomenclatura universal para los dispositivos de red, y deben incluir la ruta de acceso completa. *physical_name* es de tipo **nvarchar (260)**, no tiene ningún valor predeterminado y no puede ser null.  
  
 Cuando cree un dispositivo de copia de seguridad en una ubicación de red remota, asegúrese de que el nombre con el que se haya iniciado el [!INCLUDE[ssDE](../../includes/ssde-md.md)] tenga permiso de escritura en el equipo remoto.  
  
 Si agrega un dispositivo de cinta, este parámetro debe ser el nombre físico asignado al dispositivo de cinta local por Windows; por ejemplo, **\\ \\ .\TAPE0** para el primer dispositivo de cinta del equipo. El dispositivo de cinta tiene que estar en el equipo servidor; no se puede utilizar de forma remota. Incluya entre comillas los nombres que contengan caracteres no alfanuméricos.  
  
> [!NOTE]  
>  Este procedimiento escribe en el catálogo el nombre físico especificado. El procedimiento no intenta tener acceso al dispositivo ni crearlo.  
  
`[ @cntrltype = ] 'controller_type'` Obsoleto. Si se especifica, este parámetro se omite. Solo se admite para mantener la compatibilidad con versiones anteriores. Los nuevos usos de **sp_addumpdevice** deben omitir este parámetro.  
  
`[ @devstatus = ] 'device_status'` Obsoleto. Si se especifica, este parámetro se omite. Solo se admite para mantener la compatibilidad con versiones anteriores. Los nuevos usos de **sp_addumpdevice** deben omitir este parámetro.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **sp_addumpdevice** agrega un dispositivo de copia de seguridad a la vista de catálogo de **Sys.backup_devices** . Después, se puede hacer referencia al dispositivo de forma lógica en las instrucciones BACKUP y RESTORE. **sp_addumpdevice** no realiza ningún acceso al dispositivo físico. El acceso al dispositivo especificado solo se produce cuando se ejecuta una instrucción BACKUP o RESTORE. La creación de un dispositivo lógico de copia de seguridad puede simplificar las instrucciones BACKUP y RESTORE, en las que se puede especificar el nombre del dispositivo como alternativa mediante una cláusula "TAPE =" o "DISK =" para indicar la ruta de acceso del dispositivo.  
  
 Los problemas de propiedad y permisos pueden interferir en el uso de los dispositivos de copia de seguridad de disco o de archivo. Asegúrese de que la cuenta de Windows con la que se inicia el [!INCLUDE[ssDE](../../includes/ssde-md.md)] disponga de los permisos de archivo apropiados.  
  
 El [!INCLUDE[ssDE](../../includes/ssde-md.md)] admite copias de seguridad de cinta para dispositivos de cinta compatibles con Windows. Para obtener más información acerca de los dispositivos de cinta admitidos por Windows, vea la lista de compatibilidad de hardware de Windows. Para ver los dispositivos de cinta disponibles en el equipo, utilice [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Utilice solo las cintas que recomienda el fabricante de la unidad para la unidad de cinta específica. Si utiliza unidades de cinta de audio digital (DAT), utilice cintas DAT preparadas para equipos informáticos (Almacenamiento digital de datos, DDS).  
  
 no se puede ejecutar **sp_addumpdevice** dentro de una transacción.  
  
 Para eliminar un dispositivo, use [sp_dropdevice](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md) o[SQL Server Management Studio](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md).  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de servidor **diskadmin** .  
  
 Requiere permiso para escribir en el disco.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-adding-a-disk-dump-device"></a>A. Agregar un dispositivo de volcado en disco  
 En el ejemplo siguiente se muestra cómo agregar un dispositivo de copia de seguridad de disco llamado `mydiskdump`, con el nombre físico `c:\dump\dump1.bak`.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'mydiskdump', 'c:\dump\dump1.bak';  
```  
  
### <a name="b-adding-a-network-disk-backup-device"></a>B. Agregar un dispositivo de copia de seguridad de disco de red  
 En el ejemplo siguiente se muestra cómo agregar un dispositivo de copia de seguridad de disco remoto llamado `networkdevice`. El nombre con el que el [!INCLUDE[ssDE](../../includes/ssde-md.md)] se ha iniciado debe tener permisos de acceso al archivo remoto (`\\<servername>\<sharename>\<path>\<filename>.bak`).  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'networkdevice',  
    '\\<servername>\<sharename>\<path>\<filename>.bak';  
```  
  
### <a name="c-adding-a-tape-backup-device"></a>C. Agregar un dispositivo de copia de seguridad de cinta  
 En el ejemplo siguiente se muestra cómo agregar el dispositivo `tapedump1` con el nombre físico `\\.\tape0`.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'tape', 'tapedump1', '\\.\tape0';  
```  
  
### <a name="d-backing-up-to-a-logical-backup-device"></a>D. Realizar una copia de seguridad en un dispositivo de copia de seguridad lógico  
 En el siguiente ejemplo se crea un dispositivo de copia de seguridad lógico, `AdvWorksData`, para un archivo de copia de seguridad en disco. A continuación, se realiza una copia de seguridad de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en este dispositivo de copia de seguridad lógico.  
  
```  
USE master;  
GO  
EXEC sp_addumpdevice 'disk', 'AdvWorksData',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\AdvWorksData.bak';  
GO  
BACKUP DATABASE AdventureWorks2012   
 TO AdvWorksData  
   WITH FORMAT;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Definir un dispositivo lógico de copia de seguridad para un archivo de disco &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [Definir un dispositivo lógico de copia de seguridad en una unidad de cinta &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [sp_dropdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)   
 [sys.backup_devices &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  

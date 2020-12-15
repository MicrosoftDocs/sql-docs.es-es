---
title: Restauración de bases de datos protegidas mediante TDE
description: Siga estos pasos para restaurar una base de datos cifrada mediante el cifrado de datos transparente en el almacenamiento de datos paralelos de la plataforma de análisis.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 3e794d8311aa79cb12a5be7326b93117518d716f
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489655"
---
# <a name="restore-a-database-protected-by-tde-in-parallel-data-warehouse"></a>Restaurar una base de datos protegida por TDE en almacenamiento de datos paralelos
Siga estos pasos para restaurar una base de datos cifrada mediante el cifrado de datos transparente.  
  
El [uso de cifrado de datos transparente](transparent-data-encryption.md#using-tde) ejemplo tiene código para habilitar TDE en la `AdventureWorksPDW2012` base de datos. El código siguiente continúa este ejemplo mediante la creación de una copia de seguridad de la base de datos en el dispositivo de sistema de plataforma de análisis original (APS) y, a continuación, la restauración del certificado y la base de datos en un dispositivo diferente.  
  
El primer paso es crear una copia de seguridad de la base de datos de origen.  
  
```sql  
BACKUP DATABASE AdventureWorksPDW2012   
TO DISK = '\\SECURE_SERVER\Backups\AdventureWorksPDW2012';  
```  
  
Prepare el nuevo PDW de SQL Server para TDE mediante la creación de una clave maestra, la habilitación del cifrado y la creación de una credencial de red.  
  
```sql  
USE master;  
GO  
  
-- Create a database master key in the master database  
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<UseStrongPasswordHere>';  
GO  
  
-- Enable encryption for PDW  
EXEC sp_pdw_database_encryption 1;  
GO  
  
EXEC sp_pdw_add_network_credentials 'SECURE_SERVER', '<domain>\<Windows_user>', '<password>';  
```  
  
Los dos últimos pasos recrean el certificado mediante el uso de las copias de seguridad del PDW de SQL Server original. Use la contraseña que utilizó cuando creó la copia de seguridad del certificado.  
  
```sql  
-- Create certificate in master  
CREATE CERTIFICATE MyServerCert  
    FROM FILE = '\\SECURE_SERVER\cert\MyServerCert.cer'   
    WITH PRIVATE KEY (FILE = '\\SECURE_SERVER\cert\MyServerCert.key',   
    DECRYPTION BY PASSWORD = '<password>');  
  
RESTORE DATABASE AdventureWorksPDW2012   
    FROM DISK = '\\SECURE_SERVER\Backups\AdventureWorksPDW2012';  
```  
  
## <a name="see-also"></a>Consulte también  
[BACKUP DATABASE](../t-sql/statements/backup-transact-sql.md?view=aps-pdw-2016&preserve-view=true)  
[crear clave maestra](../t-sql/statements/create-master-key-transact-sql.md)  
 [sp_pdw_add_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md)  
[sp_pdw_database_encryption](../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)  
[CREATE CERTIFICATE](../t-sql/statements/create-certificate-transact-sql.md)  
[RESTAURAR BASE DE DATOS](../t-sql/statements/restore-statements-transact-sql.md?view=aps-pdw-2016&preserve-view=true)

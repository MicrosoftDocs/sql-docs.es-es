---
description: RESTORE MASTER KEY (Transact-SQL)
title: RESTORE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE_MASTER_KEY_TSQL
- RESTORE MASTER KEY
- LOAD_MASTER_KEY_TSQL
- LOAD MASTER KEY
dev_langs:
- TSQL
helpviewer_keywords:
- database master key [SQL Server], importing
- encryption [SQL Server], Database Master Key
- copying Database Master Keys
- importing Database Master Keys
- cryptography [SQL Server], Database Master Key
- transferring Database Master Keys
- RESTORE MASTER KEY statement
ms.assetid: 70ceb951-31a2-4fc4-a0c1-e6c18eeb3ae7
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 66ba1a2950f25ad1e0ed5e189c091ed6074f1b4d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180113"
---
# <a name="restore-master-key-transact-sql"></a>RESTORE MASTER KEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Importa una clave maestra de base de datos desde un archivo de copia de seguridad.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
RESTORE MASTER KEY FROM FILE = 'path_to_file'   
    DECRYPTION BY PASSWORD = 'password'  
    ENCRYPTION BY PASSWORD = 'password'  
    [ FORCE ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 FILE ='*path_to_file*'  
 Especifica la ruta completa, incluido el nombre de archivo, de acceso a la clave maestra de la base de datos almacenada. *path_to_file* puede ser una ruta de acceso local o una ruta UNC a una ubicación de red.  
  
 DECRYPTION BY PASSWORD ='*password*'  
 Especifica la contraseña necesaria para descifrar la clave maestra de base de datos que se va a importar desde un archivo.  
  
 ENCRYPTION BY PASSWORD ='*password*'  
 Especifica la contraseña que se utiliza para cifrar la clave maestra de base de datos una vez cargada a la base de datos.  
  
 FORCE  
 Especifica que el proceso RESTORE debe continuar, aunque la clave maestra de base de datos actual no se encuentre abierta o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no pueda descifrar algunas claves privadas cifradas con ella.  
  
## <a name="remarks"></a>Observaciones  
 Al restaurar la clave maestra, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] descifra todas las claves cifradas con la clave maestra actualmente activa y cifra estas claves con la clave maestra restaurada. Esta operación requiere un uso intensivo de recursos, por lo que debe programarse durante un período de baja demanda. Si la clave maestra de base de datos no se encuentra abierta, no puede abrirse o alguna de las claves cifradas con ella no pueden descifrarse, la operación de restauración no se puede realizar.  
  
 Utilice la opción FORCE únicamente si la clave maestra es irrecuperable o si se producen errores en el descifrado. Se perderá la información cifrada solamente con una clave irrecuperable.  
  
 Si se cifró la clave maestra con la clave maestra de servicio, la clave maestra restaurada también se cifrará con la clave maestra de servicio.  
  
 Si no hay una clave maestra en la base de datos actual, RESTORE MASTER KEY creará una clave maestra. La nueva clave maestra no se cifrará automáticamente con la clave maestra de servicio.  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso CONTROL en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se restaura la clave maestra de base de datos en la base de datos `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
RESTORE MASTER KEY   
    FROM FILE = 'c:\backups\keys\AdventureWorks2012_master_key'   
    DECRYPTION BY PASSWORD = '3dH85Hhk003#GHkf02597gheij04'   
    ENCRYPTION BY PASSWORD = '259087M#MyjkFkjhywiyedfgGDFD';  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

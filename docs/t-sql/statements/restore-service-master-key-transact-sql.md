---
description: RESTORE SERVICE MASTER KEY (Transact-SQL)
title: RESTORE SERVICE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RESTORE SERVICE MASTER KEY
- RESTORE_SERVICE_MASTER_KEY_TSQL
- LOAD SERVICE MASTER KEY
- LOAD_SERVICE_MASTER_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- importing Service Master Keys
- copying Service Master Keys
- service master key [SQL Server], importing
- RESTORE SERVICE MASTER KEY statement
- transferring Service Master Keys
ms.assetid: a68fd0ee-70ce-4104-aca0-fcae5f41fc38
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3954958dceda15a2eb68505ffbe12299b335c491
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184432"
---
# <a name="restore-service-master-key-transact-sql"></a>RESTORE SERVICE MASTER KEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Importa una clave maestra de servicio desde un archivo de copia de seguridad.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
RESTORE SERVICE MASTER KEY FROM FILE = 'path_to_file'   
    DECRYPTION BY PASSWORD = 'password' [FORCE]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 FILE **='** _path\_to\_file_ **'**  
 Especifica la ruta completa, incluido el nombre de archivo, de acceso a la clave maestra del servicio almacenado. *path_to_file* puede ser una ruta de acceso local o una ruta UNC a una ubicación de red.  
  
 PASSWORD **='** _password_ **'**  
 Especifica la contraseña necesaria para descifrar la clave maestra de servicio que se va a importar desde un archivo.  
  
 FORCE  
 Fuerza el reemplazo de la clave maestra de servicio, a pesar del riesgo de pérdida de datos.  
  
## <a name="remarks"></a>Observaciones  
 Al restaurar la clave maestra de servicio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] descifra todas las claves y secretos cifrados con la clave maestra de servicio actual y, a continuación, los cifra con la clave maestra de servicio cargada desde el archivo de copia de seguridad.  
  
 Si se producen errores durante cualquier descifrado, se producirán errores en la restauración. Puede utilizar la opción FORCE para omitir los errores, pero esta opción provocará la pérdida de los datos que no sea posible descifrar.  
  
> [!CAUTION]  
>  La clave maestra de servicio es la raíz de la jerarquía de cifrado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La clave maestra de servicio protege directa o indirectamente las demás claves del árbol. Si no es posible descifrar una clave dependiente durante una restauración forzada, se perderán los datos que protege la clave.  
  
 La regeneración de la jerarquía de cifrado es una operación que requiere un uso intensivo de recursos. Debe programarla durante un período de baja demanda.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL SERVER en el servidor.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se restaura la clave maestra de servicio desde un archivo de copia de seguridad.  
  
```sql  
RESTORE SERVICE MASTER KEY   
    FROM FILE = 'c:\temp_backups\keys\service_master_key'   
    DECRYPTION BY PASSWORD = '3dH85Hhk003GHk2597gheij4';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Clave maestra de servicio](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [ALTER SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-service-master-key-transact-sql.md)   
 [BACKUP SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-service-master-key-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)
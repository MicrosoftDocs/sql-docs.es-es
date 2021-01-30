---
description: sys.dm_pdw_nodes_database_encryption_keys (Transact-SQL)
title: sys.dm_pdw_nodes_database_encryption_keys (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: e7fd02b2-5d7e-4816-a0af-b58ae2ac3f7a
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: c6ced0fae2ba55c55550b4a8e674bdbf5223a24c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99141834"
---
# <a name="sysdm_pdw_nodes_database_encryption_keys-transact-sql"></a>sys.dm_pdw_nodes_database_encryption_keys (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Devuelve información sobre el estado de cifrado de una base de datos y sus claves de cifrado de la base de datos asociadas. **Sys.dm_pdw_nodes_database_encryption_keys** proporciona esta información para cada nodo. Para obtener más información acerca del cifrado de base de datos, vea [cifrado de datos transparente (PDW de SQL Server)](../../analytics-platform-system/transparent-data-encryption.md).  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|database_id|**int**|IDENTIFICADOR de la base de datos física en cada nodo.|  
|encryption_state|**int**|Indica si la base de datos de este nodo está cifrada o no está cifrada.<br /><br /> 0 = Ninguna clave de cifrado de la base de datos, sin cifrado<br /><br /> 1 = Sin cifrar<br /><br /> 2 = Cifrado en curso<br /><br /> 3 = Cifrado<br /><br /> 4 = Cambio de clave en curso<br /><br /> 5 = Descifrado en curso<br /><br /> 6 = cambio de protección en curso (el certificado que está cifrando la clave de cifrado de la base de datos se está cambiando).|  
|create_date|**datetime**|Muestra la fecha de creación de la clave de cifrado.|  
|regenerate_date|**datetime**|Muestra la fecha de regeneración de la clave de cifrado.|  
|modify_date|**datetime**|Muestra la fecha de modificación de la clave de cifrado.|  
|set_date|**datetime**|Muestra la fecha de aplicación de la clave de cifrado a la base de datos.|  
|opened_date|**datetime**|Muestra la última vez que se abrió la clave de la base de datos.|  
|key_algorithm|**VARCHAR (?)**|Muestra el algoritmo utilizado por la clave.|  
|key_length|**int**|Muestra la longitud de la clave.|  
|encryptor_thumbprint|**varbin**|Muestra la huella digital del sistema de cifrado.|  
|percent_complete|**real**|Porcentaje completado del cambio de estado del cifrado de la base de datos. Será 0 si no hay ningún cambio de estado.|  
|node_id|**int**|Identificador numérico único asociado al nodo.|  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el ejemplo siguiente se combinan las `sys.dm_pdw_nodes_database_encryption_keys` tablas del sistema para indicar el estado de cifrado de cada nodo de las bases de datos protegidas por TDE.  
  
```  
SELECT D.database_id AS DBIDinMaster, D.name AS UserDatabaseName,   
PD.pdw_node_id AS NodeID, DM.physical_name AS PhysDBName,   
keys.encryption_state  
FROM sys.dm_pdw_nodes_database_encryption_keys AS keys  
JOIN sys.pdw_nodes_pdw_physical_databases AS PD  
    ON keys.database_id = PD.database_id AND keys.pdw_node_id = PD.pdw_node_id  
JOIN sys.pdw_database_mappings AS DM  
    ON DM.physical_name = PD.physical_name  
JOIN sys.databases AS D  
    ON D.database_id = DM.database_id  
ORDER BY D.database_id, PD.pdw_node_ID;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)   
 [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-encryption-key-transact-sql.md)   
 [ALTER DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
 [DROP DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)  
  
  


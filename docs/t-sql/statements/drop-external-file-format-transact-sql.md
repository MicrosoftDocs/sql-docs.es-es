---
description: DROP EXTERNAL FILE FORMAT (Transact-SQL)
title: DROP EXTERNAL FILE FORMAT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 8cf9009b-59f9-4aac-bef1-dcf2cf0708b2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bf9c9eb5c1396e8b391dd0edca78f35f95369e30
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185948"
---
# <a name="drop-external-file-format-transact-sql"></a>DROP EXTERNAL FILE FORMAT (Transact-SQL)
[!INCLUDE [sqlserver2016-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdbmi-asa-pdw.md)]

  Quita un formato de archivo externo de PolyBase.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Drop an external file format  
DROP EXTERNAL FILE FORMAT external_file_format_name  
[;]  
```  
  
## <a name="arguments"></a>Argumentos  
 *external_file_format_name*  
 El nombre del formato de archivo externo que se va a eliminar.  
  
## <a name="metadata"></a>Metadatos  
 Para ver una lista de los formatos de archivo externo, use la vista del sistema [sys.external_file_formats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md).  
  
```sql  
SELECT * FROM sys.external_file_formats;  
```  
  
## <a name="permissions"></a>Permisos  
 Requiere ALTER ANY EXTERNAL FILE FORMAT.  
  
## <a name="general-remarks"></a>Notas generales  
 Al quitar un formato de archivo externo, no se quitan los datos externos.  
  
## <a name="locking"></a>Bloqueo  
 Toma un bloqueo compartido en el objeto de formato de archivo externo.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-basic-syntax"></a>A. Uso de sintaxis básica  
  
```sql  
DROP EXTERNAL FILE FORMAT myfileformat;  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)  
  
  


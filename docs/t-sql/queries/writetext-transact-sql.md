---
description: WRITETEXT (Transact-SQL)
title: WRITETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WRITETEXT_TSQL
- WRITETEXT
dev_langs:
- TSQL
helpviewer_keywords:
- replacing data
- WRITETEXT statement
- updating data [SQL Server]
- nonlogged updating
- minimally logged updating [SQL Server]
- overwriting data
- data updates [SQL Server], WRITETEXT statement
ms.assetid: 80c252fd-a8b8-4a2e-888a-059081ed4109
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f1793b813e763df5cb8078a73fb1cd78c7fe7a6c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193367"
---
# <a name="writetext-transact-sql"></a>WRITETEXT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permite la actualización interactiva de registro mínimo de una columna **text**, **ntext** o **image** existente. WRITETEXT sobrescribe completamente los datos existentes en la columna afectada. No se puede usar WRITETEXT en las columnas **text**, **ntext** e **image** de vistas.  
  
> [!IMPORTANT]
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use los tipos de datos de valores grandes y la cláusula **.** WRITE de la instrucción [UPDATE](../../t-sql/queries/update-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
WRITETEXT [BULK]  
  { table.column text_ptr }  
  [ WITH LOG ] { data }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 BULK  
 Hace posible que las herramientas de carga carguen flujos de datos binarios. La herramienta debe proporcionar flujos en el nivel de protocolo TDS. Cuando el flujo de datos no esté presente el procesador de consultas omite la opción BULK.  
  
> [!IMPORTANT]  
>  Recomendamos que la opción BULK no se utilice en las aplicaciones basadas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Es posible que esta opción se modifique o quite en versiones futuras de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *table* **.column**  
 Es el nombre de la tabla y la columna **text**, **ntext** o **image** que se van a actualizar. Los nombres de tablas y columnas deben cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md). La especificación de los nombres de la base de datos y del propietario es opcional.  
  
 *text_ptr*  
 Es un valor que almacena el puntero a los datos **text**, **ntext** o **image**. *text_ptr* debe ser de tipo **binary(16)**. Para crear un puntero de texto, ejecute una instrucción [INSERT](../../t-sql/statements/insert-transact-sql.md) o [UPDATE](../../t-sql/queries/update-transact-sql.md) con datos que no sean NULL para la columna **text**, **ntext** o **image**.  
  
 WITH LOG  
 Omitido por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El registro está determinado por el modelo de recuperación vigente para la base de datos.  
  
 *data*  
 Son los datos reales de tipo **text**, **ntext** o **image** que se van a almacenar. *data* puede ser un literal o un parámetro. La longitud máxima de texto que se puede insertar interactivamente con WRITETEXT es de 120 KB aproximadamente para datos de tipo **text**, **ntext** e **image**.  
  
## <a name="remarks"></a>Observaciones  
 Use WRITETEXT para reemplazar datos de tipo **text**, **ntext** e **image** y UPDATETEXT para modificar datos de tipo **text**, **ntext** e **image**. UPDATETEXT es más flexible debido a que cambia solo una parte de una columna **text**, **ntext** o **image**, en lugar de la columna completa.  
  
 Para mejorar el rendimiento, se recomienda insertar o actualizar los datos de tipo **text**, **ntext** e **image** en fragmentos que sean múltiplos de 8040 bytes.  
  
 Si el modelo de recuperación de la base de datos es simple u optimizado para cargas masivas de registros, las operaciones **text**, **ntext** e **image** que usen WRITETEXT se registrarán mínimamente cuando se inserten o se anexen datos nuevos.  
  
> [!NOTE]  
>  El registro mínimo no se utiliza cuando se actualizan los datos existentes.  
  
 Para que WRITETEXT funcione correctamente, la columna ya debe contener un puntero de texto válido.  
  
 Si en la tabla no existe texto de fila, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ahorra espacio al no inicializar las columnas **text** cuando se colocan valores NULL explícitos o implícitos en las columnas **text** con INSERT, y no es posible obtener ningún puntero de texto para estos valores NULL. Para inicializar columnas **text** a NULL, use la instrucción UPDATE. Si en la tabla existe texto de fila, no es necesario inicializar a valores NULL la columna de texto y siempre es posible obtener un puntero de texto.  
  
 La función SQLPutData de ODBC es más rápida y usa menos memoria dinámica que WRITETEXT. Esta función puede insertar hasta 2 gigabytes de datos de tipo **text**, **ntext** o **image**.  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], es posible que existan punteros de texto de fila a datos de tipo **text**, **ntext** o **image**, pero que no sean válidos. Para obtener más información sobre la opción "text in row", vea [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Para más información sobre cómo invalidar punteros de texto, vea [sp_invalidate_textptr &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso UPDATE en la base de datos especificada. El permiso se puede transferir cuando se transfiere el permiso UPDATE.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente el puntero de texto se sitúa en la variable local `@ptrval` y, a continuación, `WRITETEXT` sitúa la nueva cadena de texto en la fila a la que señala `@ptrval`.  
  
> [!NOTE]  
>  Para ejecutar este ejemplo, debe instalar la base de datos de ejemplo pubs.  
  
```sql  
USE pubs;  
GO  
ALTER DATABASE pubs SET RECOVERY SIMPLE;  
GO  
DECLARE @ptrval BINARY(16);  
SELECT @ptrval = TEXTPTR(pr_info)   
FROM pub_info pr, publishers p  
WHERE p.pub_id = pr.pub_id   
   AND p.pub_name = 'New Moon Books'  
WRITETEXT pub_info.pr_info @ptrval 'New Moon Books (NMB) has just released another top ten publication. With the latest publication this makes NMB the hottest new publisher of the year!';  
GO  
ALTER DATABASE pubs SET RECOVERY SIMPLE;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)  
  
  

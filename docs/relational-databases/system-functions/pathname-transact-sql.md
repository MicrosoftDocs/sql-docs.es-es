---
description: PathName (Transact-SQL)
title: PathName (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- PathName_TSQL
- PathName
dev_langs:
- TSQL
helpviewer_keywords:
- PathName FILESTREAM [SQL Server]
ms.assetid: 6b95ad90-6c82-4a23-9294-a2adb74934a3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 191d567a310686a881363ff46a9667530dd79112
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207354"
---
# <a name="pathname-transact-sql"></a>PathName (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la ruta de acceso de un objeto binario grande (BLOB) FILESTREAM. La API de OpenSqlFilestream usa esta ruta de acceso para devolver un identificador que una aplicación puede usar para trabajar con los datos del BLOB mediante el uso de las API de Win32. PathName es de solo lectura.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
column_name.PathName ( @option [ , use_replica_computer_name ] )  
```  
  
## <a name="arguments"></a>Argumentos  
 *column_name*  
 Es el nombre de columna de una columna FileStream de tipo **varbinary (Max)** . *column_name* debe ser un nombre de columna. No puede ser una expresión ni el resultado de una instrucción CAST o CONVERT.  
  
 La solicitud del nombreruta para una columna de cualquier otro tipo de datos o de un columnthat **varbinary (Max)** no tiene el atributo de almacenamiento FileStream producirá un error en tiempo de compilación de la consulta.  
  
 *\@desea*  
 [Expresión](../../t-sql/language-elements/expressions-transact-sql.md) de tipo entero que define cómo se debe dar formato al componente de servidor de la ruta de acceso. la *\@ opción* puede ser uno de los valores siguientes. El valor predeterminado es 0.  
  
|Value|Descripción|  
|-----------|-----------------|  
|0|Devuelve el nombre del servidor convertido al formato BIOS, por ejemplo: `\\SERVERNAME\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F19F7-38EA-4AB0-BB89-E6C545DBD3F9`|  
|1|Devuelve el nombre del servidor sin la conversión, por ejemplo: `\\ServerName\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F1`|  
|2|Devuelve la ruta de acceso al servidor completa, por ejemplo: `\\ServerName.MyDomain.com\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F19F7-38EA-4AB0-BB89-E6C545DBD3F9`|  
  
 *use_replica_computer_name*  
 Un valor de bit que define cómo se debe devolver el nombre del servidor en un Always On grupo de disponibilidad.  
  
 Cuando la base de datos no pertenece a un grupo de disponibilidad Always On, se omite el valor de este argumento. El nombre del equipo siempre se usa en la ruta.  
  
 Cuando la base de datos pertenece a un grupo de disponibilidad de Always On, el valor de *use_replica_computer_name* tiene el efecto siguiente en la salida de la función **PathName** :  
  
|Value|Descripción|  
|-----------|-----------------|  
|No especificada.|La función devuelve el nombre de red virtual (VNN) en la ruta de acceso.|  
|0|La función devuelve el nombre de red virtual (VNN) en la ruta de acceso.|  
|1|La función devuelve el nombre del equipo en la ruta de acceso.|  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **nvarchar(max)**  
  
## <a name="return-value"></a>Valor devuelto  
 El valor devuelto es la ruta de acceso de NETBIOS o la ruta de acceso lógica completa del BLOB. PathName no devuelve una dirección IP. Se devuelve NULL cuando no se ha creado el BLOB FILESTREAM.  
  
## <a name="remarks"></a>Observaciones  
 La columna ROWGUID debe estar visible en cualquier consulta que llame a PathName.  
  
 Un BLOB FILESTREAM solo se puede crear utilizando [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-reading-the-path-for-a-filestream-blob"></a>A. Leer la ruta de acceso para un BLOB FILESTREAM  
 En el ejemplo siguiente se asigna la propiedad `PathName` a una variable de tipo `nvarchar(max)`.  
  
```sql  
DECLARE @PathName nvarchar(max);  
SET @PathName = (  
    SELECT TOP 1 photo.PathName()  
    FROM dbo.Customer  
    WHERE LastName = 'CustomerName'  
    );  
```  
  
### <a name="b-displaying-the-paths-for-filestream-blobs-in-a-table"></a>B. Mostrar las rutas de acceso de BLOB FILESTREAM en una tabla  
 En el ejemplo siguiente se crean y muestran las rutas de acceso para tres BLOB FILESTREAM.  
  
```sql  
-- Create a FILESTREAM-enabled database.  
-- The c:\data directory must exist.  
CREATE DATABASE PathNameDB  
ON  
PRIMARY ( NAME = ArchX1,  
    FILENAME = 'c:\data\archdatP1.mdf'),  
FILEGROUP FileStreamGroup1 CONTAINS FILESTREAM( NAME = ArchX3,  
    FILENAME = 'c:\data\filestreamP1')  
LOG ON  ( NAME = ArchlogX1,  
    FILENAME = 'c:\data\archlogP1.ldf');  
GO  
  
USE PathNameDB;  
GO  
  
-- Create a table, add some records, and  
-- create the associated FILESTREAM  
-- BLOB files.  
  
CREATE TABLE TABLE1  
    (  
        ID int,  
        RowGuidColumn UNIQUEIDENTIFIER  
                      NOT NULL UNIQUE ROWGUIDCOL,  
        FILESTREAMColumn varbinary(MAX) FILESTREAM  
    );  
GO  
  
INSERT INTO TABLE1 VALUES  
 (1, NEWID(), 0x00)  
,(2, NEWID(), 0x00)  
,(3, NEWID(), 0x00);  
GO  
  
SELECT FILESTREAMColumn.PathName() AS 'PathName' FROM TABLE1;  
  
--Results  
--PathName  
------------------------------------------------------------------------------------------------------------  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\DD67C792-916E-4A76-8C8A-4A85DC5DB908  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\2907122B-2560-4CB9-86DC-FBE7ABA1843B  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\922BE0E0-CAB9-4403-90BF-945BD258E4BC  
--  
--(3 row(s) affected)  
GO  
  
--Drop the database to clean up.  
USE master;  
GO  
DROP DATABASE PathNameDB;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Binary Large Object &#40;Blob&#41; Data &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)   
 [GET_FILESTREAM_TRANSACTION_CONTEXT &#40;&#41;de Transact-SQL ](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md)   
 [Obtener acceso a los datos FILESTREAM con OpenSqlFilestream](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
  
  

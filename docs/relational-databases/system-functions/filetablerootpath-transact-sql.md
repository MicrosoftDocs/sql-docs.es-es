---
description: FileTableRootPath (Transact-SQL)
title: FileTableRootPath (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- FileTableRootPath_TSQL
- FileTableRootPath
dev_langs:
- TSQL
helpviewer_keywords:
- FileTableRootPath function
ms.assetid: 0cba908a-c85c-4b09-b16a-df1cb333c629
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b21eb896c32b2afdfb56b1318406ee96f93546df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196131"
---
# <a name="filetablerootpath-transact-sql"></a>FileTableRootPath (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la ruta de acceso UNC del nivel raíz de un objeto FileTable específico o de la base de datos actual.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
FileTableRootPath ( [ '[schema_name.]FileTable_name' ], @option )  
```  
  
## <a name="arguments"></a>Argumentos  
 *FileTable_name*  
 Nombre del FileTable. *FileTable_name* es de tipo **nvarchar**. Se trata de un parámetro opcional. El valor predeterminado es la base de datos actual. Especificar *schema_name* también es opcional. Puede pasar NULL para que *FileTable_name* use el valor de parámetro predeterminado.  
  
 *\@desea*  
 Expresión entera que define cómo se debe dar formato al componente de servidor de la ruta de acceso. la *\@ opción* puede tener uno de los siguientes valores:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Devuelve el nombre del servidor convertido a formato NetBIOS, por ejemplo:<br /><br /> `\\SERVERNAME\MSSQLSERVER\MyDocumentDatabase`<br /><br /> Este es el valor predeterminado.|  
|**1**|Devuelve el nombre del servidor sin la conversión, por ejemplo:<br /><br /> `\\ServerName\MSSQLSERVER\MyDocumentDatabase`|  
|**2**|Devuelve la ruta de acceso al servidor completa, por ejemplo:<br /><br /> `\\ServerName.MyDomain.com\MSSQLSERVER\MyDocumentDatabase`|  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **nvarchar(4000)**  
  
 Cuando la base de datos pertenece a un grupo de disponibilidad de Always On, la función **FileTableRootPath** devuelve el nombre de la red virtual (VNN) en lugar del nombre del equipo.  
  
## <a name="general-remarks"></a>Notas generales  
 La función **FileTableRootPath** devuelve NULL cuando se cumple una de las condiciones siguientes:  
  
-   El valor de *FileTable_name* no es válido.  
  
-   El autor de la llamada no tiene suficientes permisos para hacer referencia a la tabla especificada o a la base de datos actual.  
  
-   No se ha establecido la opción FILESTREAM de *database_directory* para la base de datos actual.  
  
 Para más información, consulte [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md).  
  
## <a name="best-practices"></a>Prácticas recomendadas  
 Para mantener independientes del equipo y de la base de datos actuales el código y las aplicaciones, evite escribir código basado en rutas de acceso absolutas de archivos. En su lugar, obtenga la ruta de acceso completa de un archivo en tiempo de ejecución mediante las funciones **FileTableRootPath** y **GetFileNamespacePath** , como se muestra en el ejemplo siguiente. De forma predeterminada, la función **GetFileNamespacePath** devuelve la ruta de acceso relativa del archivo en la ruta de acceso raíz de la base de datos.  
  
```sql  
USE MyDocumentDatabase;  
  
@root varchar(100)  
SELECT @root = FileTableRootPath();  
@fullPath = varchar(1000);  
  
SELECT @fullPath = @root + file_stream.GetFileNamespacePath()  
FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 La función **FileTableRootPath** requiere:  
  
-   Permiso SELECT en el objeto FileTable para obtener la ruta de acceso raíz de un objeto FileTable específico.  
  
-   **db_datareader** o un permiso superior para obtener la ruta de acceso raíz de la base de datos actual.  
  
## <a name="examples"></a>Ejemplos  
 En los siguientes ejemplos se muestra cómo llamar a la función **FileTableRootPath** .  
  
```  
USE MyDocumentDatabase;  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase"  
SELECT FileTableRootPath();  
  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase\MyFileTable"  
SELECT FileTableRootPath(N'dbo.MyFileTable');  
  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase\MyFileTable"  
SELECT FileTableRootPath(N'MyFileTable');  
```  
  
## <a name="see-also"></a>Consulte también  
 [Trabajar con directorios y rutas de acceso de FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  

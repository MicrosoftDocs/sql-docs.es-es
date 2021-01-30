---
description: GetFileNamespacePath (Transact-SQL)
title: GetFileNamespacePath (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- GetFileNamespacePath
- GetFileNamespacePath_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetFileNamespacePath function
ms.assetid: b393ecef-baa8-4d05-a268-b2f309fce89a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 36dc3a717a7ab67ad4db0aa730884597038a0a02
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207448"
---
# <a name="getfilenamespacepath-transact-sql"></a>GetFileNamespacePath (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la ruta de acceso UNC para un archivo o directorio de un objeto FileTable.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<column-name>.GetFileNamespacePath(is_full_path, @option)  
```  
  
## <a name="arguments"></a>Argumentos  
 *nombre de columna*  
 Nombre de columna de la columna de **FILE_STREAM** varbinary (Max) de una filetable.  
  
 El valor de *nombre de columna* debe ser un nombre de columna válido. No puede ser una expresión ni un valor convertido de una columna de otro tipo de datos.  
  
 *is_full_path*  
 Expresión entera que especifica si se devuelve una ruta de acceso absoluta o relativa. *is_full_path* puede tener uno de los valores siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Devuelve la ruta de acceso relativa al directorio de base de datos.<br /><br /> Este es el valor predeterminado|  
|**1**|Devuelve la ruta de acceso UNC completa, empezando por `\\computer_name`.|  
  
 *\@desea*  
 Expresión entera que define cómo se debe dar formato al componente de servidor de la ruta de acceso. la *\@ opción* puede tener uno de los siguientes valores:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Devuelve el nombre del servidor convertido a formato NetBIOS, por ejemplo:<br /><br /> `\\SERVERNAME\MSSQLSERVER\MyDocumentDatabase`<br /><br /> Este es el valor predeterminado.|  
|**1**|Devuelve el nombre del servidor sin la conversión, por ejemplo:<br /><br /> `\\ServerName\MSSQLSERVER\MyDocumentDatabase`|  
|**2**|Devuelve la ruta de acceso al servidor completa, por ejemplo:<br /><br /> `\\ServerName.MyDomain.com\MSSQLSERVER\MyDocumentDatabase`|  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **nvarchar(max)**  
  
 Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está en clúster en un clúster de conmutación por error, el nombre de la máquina que se devuelve como parte de esta ruta de acceso es el nombre del host virtual de la instancia en clúster.  
  
 Cuando la base de datos pertenece a un grupo de disponibilidad de Always On, la función **FileTableRootPath** devuelve el nombre de la red virtual (VNN) en lugar del nombre del equipo.  
  
## <a name="general-remarks"></a>Notas generales  
 La ruta de acceso que devuelve la función **GetFileNamespacePath** es un directorio lógico o una ruta de acceso de archivo con el siguiente formato:  
  
 `\\<machine>\<instance-level FILESTREAM share>\<database-level directory>\<FileTable directory>\...`  
  
 Esta ruta de acceso lógica no se corresponde directamente con una ruta de acceso NTFS física. Se convierte en la ruta de acceso física mediante el controlador de filtro del sistema de archivos de FILESTREAM y el agente de FILESTREAM. Esta separación entre la ruta de acceso lógica y la ruta de acceso física permite a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reorganizar internamente los datos sin afectar a la validez de la ruta de acceso.  
  
## <a name="best-practices"></a>Prácticas recomendadas  
 Para mantener independientes del equipo y de la base de datos actuales el código y las aplicaciones, evite escribir código basado en rutas de acceso absolutas de archivos. En su lugar, obtenga la ruta de acceso completa de un archivo en tiempo de ejecución mediante las funciones **FileTableRootPath** y **GetFileNamespacePath** , como se muestra en el ejemplo siguiente. De forma predeterminada, la función **GetFileNamespacePath** devuelve la ruta de acceso relativa del archivo en la ruta de acceso raíz de la base de datos.  
  
```sql  
USE MyDocumentDatabase;  
@root varchar(100)  
SELECT @root = FileTableRootPath();  
  
@fullPath = varchar(1000);  
SELECT @fullPath = @root + file_stream.GetFileNamespacePath() FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="examples"></a>Ejemplos  
 En los siguientes ejemplos se muestra cómo llamar a la función **GetFileNamespacePath** para obtener la ruta de acceso UNC para un archivo o directorio de una filetable.  
  
```  
-- returns the relative path of the form "\MyFileTable\MyDocDirectory\document.docx"  
SELECT file_stream.GetFileNamespacePath() AS FilePath FROM DocumentStore  
WHERE Name = N'document.docx';  
  
-- returns "\\MyServer\MSSQLSERVER\MyDocumentDatabase\MyFileTable\MyDocDirectory\document.docx"  
SELECT file_stream.GetFileNamespacePath(1, Null) AS FilePath FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Trabajar con directorios y rutas de acceso de FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  

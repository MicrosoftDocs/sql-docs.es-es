---
description: sys.external_library_files (Transact-SQL)
title: sys.external_library_files (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- external_library_files
- external_library_files_TSQL
- sys.external_library_files
- sys.external_library_files_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_library_files catalog view
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b716b8a3264152c3f03a454f22613f56241279d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351738"
---
# <a name="sysexternal_library_files-transact-sql"></a>sys.external_library_files (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Muestra una fila para cada archivo que constituye una biblioteca externa.

|Nombre de la columna |Tipo de datos |Descripción|
|------|------|-----|
|external_library_id | int |IDENTIFICADOR del objeto de biblioteca externa. |
|contenido |varbinary(max) |Contenido del artefacto de archivo de biblioteca externa. |
|platform |TINYINT |IDENTIFICADOR de la plataforma de host en la que está instalado SQL Server. |
|platform_desc | nvarchar(60) |Nombre de la plataforma de host. Los valores válidos son ' WINDOWS ', ' LINUX '. |

### <a name="see-also"></a>Consulte también  

[sys.external_libraries](sys-external-libraries-transact-sql.md)  
[CREAR BIBLIOTECA EXTERNA](../../t-sql/statements/create-external-library-transact-sql.md)  

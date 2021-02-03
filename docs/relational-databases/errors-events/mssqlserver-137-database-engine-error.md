---
description: MSSQLSERVER_137
title: MSSQLSERVER_137 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 137 (Database Engine error)
ms.assetid: 47fb4212-2165-4fec-bc41-6d548465d7be
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c50c997d96029ee91673f8145e20d174c967a8fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197033"
---
# <a name="mssqlserver_137"></a>MSSQLSERVER_137
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|137|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|P_SCALAR_VAR_NOTFOUND|  
|Texto del mensaje|Debe declarar la variable escalar "%.*ls".|  
  
## <a name="explanation"></a>Explicación  
Este error se produce cuando una variable se utiliza en un script SQL sin declararla primero. En el ejemplo siguiente se devuelve el error 137 para las instrucciones SET y SELECT porque no se ha declarado **\@mycol**.  
  
```sql
SET @mycol = 'ContactName';  
  
SELECT @mycol; 
```
  
Una de las causas más complicadas de este error incluye el uso de una variable que se declara fuera de la instrucción EXECUTE. Por ejemplo, la variable **\@mycol** especificada en la instrucción SELECT es local en la instrucción SELECT; por tanto, está fuera de la instrucción EXECUTE.  
  
```sql
USE AdventureWorks2012;  
  
GO  
  
DECLARE @mycol nvarchar(20);  
  
SET @mycol = 'Name';  
  
EXECUTE ('SELECT @mycol FROM Production.Product;'); 
```
  
## <a name="user-action"></a>Acción del usuario  
Compruebe que cualquier variable que se use en un script SQL se declara antes de utilizarse en otra parte del script.  
  
Vuelva a escribir el script para que no haga referencia a las variables de la instrucción EXECUTE que se declaran fuera de ella. Por ejemplo:  
  
```sql
USE AdventureWorks2012;  
  
GO  
  
DECLARE @mycol nvarchar(20) ;  
  
SET @mycol = 'Name';  
  
EXECUTE ('SELECT ' + @mycol + ' FROM Production.Product';) ;
```
  
## <a name="see-also"></a>Consulte también  
[EXECUTE &#40;Transact-SQL&#41;](~/t-sql/language-elements/execute-transact-sql.md)  
[Instrucciones SET &#40;Transact-SQL&#41;](~/t-sql/statements/set-statements-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](~/t-sql/language-elements/declare-local-variable-transact-sql.md)  
  

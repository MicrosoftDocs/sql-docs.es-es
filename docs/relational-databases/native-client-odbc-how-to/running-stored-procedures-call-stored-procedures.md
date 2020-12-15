---
title: Llamar a procedimientos almacenados (ODBC) | Microsoft Docs
description: Obtenga información acerca de cómo agregar un origen de datos mediante el administrador ODBC, mediante programación o mediante un archivo, antes de utilizar las aplicaciones ODBC con SQL Server 2005 o posterior.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- stored procedures [ODBC], calling
ms.assetid: 31176be8-d40e-4f93-8d44-a46e804a3e2d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8f93ad9f3229597fea17d53747dcd97d22172d6c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469516"
---
# <a name="running-stored-procedures---call-stored-procedures"></a>Ejecutar procedimientos almacenados: llamar a procedimientos almacenados
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  El controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permite ejecutar los procedimientos almacenados como procedimientos almacenados remotos. La ejecución de un procedimiento almacenado como un procedimiento almacenado remoto permite al controlador y al servidor optimizar el rendimiento de la ejecución del procedimiento.  
  
  Cuando una instrucción SQL llama a un procedimiento almacenado mediante la cláusula de escape ODBC CALL, el controlador de Microsoft® SQL Server™ envía el procedimiento a SQL Server usando el mecanismo de llamada a un procedimiento almacenado remoto (RPC). Las solicitudes de RPC omiten gran parte del análisis de la instrucción y del procesamiento del parámetro en SQL Server, y es más rápido que usar la instrucción Transact-SQL EXECUTE.  
  
 Para obtener una aplicación de ejemplo que muestra esta característica, vea [procesar códigos de retorno y parámetros de salida &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/running-stored-procedures-process-return-codes-and-output-parameters.md).  
  
### <a name="to-run-a-procedure-as-an-rpc"></a>Para ejecutar un procedimiento como una RPC  
  
1.  Cree una instrucción SQL que use la secuencia de escape ODBC CALL. La instrucción usa marcadores de parámetros para cada parámetro de entrada, de entrada/salida y de salida, así como para el valor devuelto por el procedimiento (si existe):  
  
    ```  
    {? = CALL procname (?,?)}  
    ```  
  
2.  Llame a [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md) para cada parámetro de entrada, de entrada/salida y de salida, así como para el valor devuelto por el procedimiento (si existe).  
  
3.  Ejecute la instrucción con [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md).  
  
> [!NOTE]  
>  Si una aplicación envía un procedimiento con la sintaxis de Transact-SQL EXECUTE (en contraposición al flujo de escape ODBC CALL), el controlador ODBC de SQL Server pasa la llamada al procedimiento a SQL Server como una instrucción SQL en lugar de como una RPC. Además, los parámetros de salida no se devuelven si se usa la instrucción Transact-SQL EXECUTE.  
  
## <a name="see-also"></a>Consulte también  
  [Procesar por lotes las llamadas a procedimientos almacenados](../../relational-databases/native-client-odbc-stored-procedures/batching-stored-procedure-calls.md)   
 [Ejecutar procedimientos almacenados](../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)   
 [Llamar a un procedimiento almacenado](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)   
 [Procedimientos](../../relational-databases/native-client-odbc-queries/executing-statements/procedures.md)  
  

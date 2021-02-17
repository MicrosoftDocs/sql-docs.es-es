---
description: bcp_sendrow
title: bcp_sendrow | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_sendrow
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_sendrow function
ms.assetid: ddbdb4bd-ad4e-4bf1-9a75-656aa26ce10a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2a97c59e9606b0ad5d08d8ed04a41f50f8924e6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350660"
---
# <a name="bcp_sendrow"></a>bcp_sendrow
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Envía una fila de datos de las variables de programa a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
RETCODE bcp_sendrow (  
    HDBC hdbc);  
```  
  
## <a name="arguments"></a>Argumentos  
 *hdbc*  
 Es el identificador de la conexión ODBC habilitada para la copia masiva.  
  
## <a name="returns"></a>Devoluciones  
 SUCCEED o FAIL.  
  
## <a name="remarks"></a>Observaciones  
 La función **bcp_sendrow** genera una fila a partir de las variables de programa y la envía a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Antes de llamar a **a bcp_sendrow**, debe hacer llamadas a [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) para especificar las variables de programa que contienen los datos de la fila.  
  
 Si se llama a **bcp_bind** especificando un tipo de datos largo, de longitud variable, por ejemplo, un parámetro *eDataType* de SQLTEXT y un parámetro *pdata* no NULL, **bcp_sendrow** envía el valor completo de los datos, de la misma forma que para cualquier otro tipo de datos. Si, no obstante, **bcp_bind** tiene un parámetro *pData* NULL, **bcp_sendrow** devuelve inmediatamente el control a la aplicación inmediatamente después de enviar todas las columnas con datos especificados a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. A continuación, la aplicación puede llamar repetidamente a [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md) para enviar los datos largos, de longitud variable a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un fragmento a la vez. Para obtener más información, vea [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md).  
  
 Cuando se utiliza **bcp_sendrow** para la copia masiva de filas de variables de programa en tablas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , las filas solamente se confirman cuando el usuario llama a [a bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md) o a [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md). El usuario puede elegir llamar a **bcp_batch** una vez cada *n* filas o cuando haya un periodo de inactividad entre períodos de entrada de datos. Si nunca se llama a **bcp_batch** , las filas se confirman cuando se llama a **bcp_done** .  
  
 Para obtener información sobre un cambio importante en la copia masiva a partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , vea [realizar operaciones de copia masiva &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md).  
  
## <a name="see-also"></a>Consulte también  
 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  

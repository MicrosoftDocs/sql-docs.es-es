---
description: 'IBCPSession:: BCPReadFmt (proveedor de OLE DB de Native Client)'
title: 'IBCPSession:: BCPReadFmt (proveedor de OLE DB de Native Client) | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IBCPSession::BCPReadFmt (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPReadFmt method
ms.assetid: e2a12050-94e4-48a3-8a48-b780d646f116
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 93820c8729bae62e6df35ba5030ecb2dde4dbeef
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477936"
---
# <a name="ibcpsessionbcpreadfmt-native-client-ole-db-provider"></a>IBCPSession:: BCPReadFmt (proveedor de OLE DB de Native Client)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Lee la información de formato de cada columna en el archivo de formato.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
HRESULT BCPReadFmt(   
      const wchar_t *pwszFormatFile);  
```  
  
## <a name="remarks"></a>Observaciones  
 El método **BCPReadFmt** se utiliza para leer datos de un archivo de formato que especifica el formato de datos en el archivo de datos. Este método es capaz de detectar la versión correcta del archivo de formato. Puede detectar automáticamente si el archivo de formato está en xml o en el formato de texto de estilo anterior y se comporta en consecuencia. Las versiones del archivo de formato admitidas por el proveedor OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client BCP son la 6.0 o más reciente.  
  
 Después de que el método **BCPReadFmt** lee los valores de formato, realiza las llamadas adecuadas a los métodos [IBCPSession::BCPColumns](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md) e [IBCPSession::BCPColFmt](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md). No es necesario que analice un archivo de formato y realice estas llamadas.  
  
 Para guardar un archivo de formato, llame al método [IBCPSession::BCPWriteFmt](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md). Las llamadas al método **BCPReadFmt** pueden hacer referencia a formatos guardados. Como alternativa, la utilidad de copia masiva (**bcp**) puede guardar formatos de datos definidos por el usuario en archivos a los que puede hacer referencia el método **BCPReadFmt** .  
  
 El valor **BCP_OPTION_DELAYREADFMT** del parámetro *eOption* de [IBCPSession::BCPControl](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md) modifica el comportamiento de IBCPSession::BCPReadFmt.  
  
## <a name="arguments"></a>Argumentos  
 *pwszFormatFile*[in]  
 La ruta de acceso y nombre del archivo que contiene los valores de formato para el archivo de datos.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 S_OK  
 El método se ha llevado a cabo de forma correcta.  
  
 E_FAIL  
 Se produjo un error específico del proveedor; para obtener información detallada, use la interfaz [ISQLServerErrorInfo](isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_OUTOFMEMORY  
 Error de memoria insuficiente.  
  
 E_UNEXPECTED  
 No se esperaba la llamada al método. Por ejemplo, no se llamó al método [IBCPSession::BCPInit](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) antes de llamar a este método.  
  
## <a name="see-also"></a>Consulte también  
 [IBCPSession &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-ole-db.md)   
 [Realizar operaciones de copia masiva](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
  

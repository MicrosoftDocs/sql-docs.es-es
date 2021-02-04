---
title: IBCPSession::BCPInit (controlador OLE DB) | Microsoft Docs
description: Obtenga información sobre cómo el método IBCPSession::BCPInit realiza las inicializaciones necesarias para una copia masiva de datos entre la estación de trabajo y SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPInit (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPInit method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e5872028183d2003218fdc3bf65bcdf189927883
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159607"
---
# <a name="ibcpsessionbcpinit-ole-db"></a>IBCPSession::BCPInit (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Inicializa la estructura de copia masiva, realiza algunas comprobaciones de errores, comprueba que los datos y los nombres de archivo de formato son correctos y, a continuación, los abre.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
HRESULT BCPInit(   
      const wchar_t *pwszTable,  
      const wchar_t *pwszDataFile,  
      const wchar_t *pwszErrorFile,  
      int eDirection);  
```  
  
## <a name="remarks"></a>Observaciones  
 Es necesario llamar al método **BCPInit** antes de llamar a cualquier otro método de copia masiva. El método **BCPInit** realiza las inicializaciones necesarias para una copia masiva de datos entre la estación de trabajo y [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 El método **BCPInit** examina la estructura del origen de base de datos o la tabla de destino, no el archivo de datos. Especifica los valores de formato de datos del archivo de datos basándose en cada columna de la tabla de base de datos, la vista o el conjunto de resultados de la instrucción SELECT. Esta especificación incluye el tipo de datos de cada columna, la presencia o ausencia de cadenas de bytes de un indicador de longitud o nulo y de terminador en los datos, y el ancho de los tipos de datos de longitud fija. El método **BCPInit** establece estos valores como sigue:  
  
-   El tipo de datos especificado es el tipo de datos de la columna de la tabla de base de datos, la vista o el conjunto de resultados de una instrucción SELECT. El tipo de datos se enumera mediante tipos de datos nativos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] especificados en el archivo de encabezado de OLE DB Driver for SQL Server (msoledbsql.h). Sus valores están en el modelo de BCP_TYPE_XXX. Los datos están representados en el formato del equipo. Es decir, los datos de una columna de tipo de datos entero están representados por una secuencia de cuatro bytes "big endian" o "little endian" basada en el equipo que creó el archivo de datos.  
  
-   Si un tipo de datos de base de datos es de longitud fija, los datos del archivo de datos también serán de longitud fija. Los métodos de copia masiva que procesan los datos (por ejemplo, [IBCPSession::BCPExec](../../oledb/ole-db-interfaces/ibcpsession-bcpexec-ole-db.md)) analizan las filas de datos esperando que la longitud de los datos del archivo de datos sea idéntica a la longitud de los datos especificados en la tabla de base de datos, la vista o la lista de columnas de la instrucción SELECT. Por ejemplo, los datos de una columna de base de datos definidos como `char(13)` deben estar representados por 13 caracteres por cada fila de datos del archivo. Los datos de longitud fija pueden tener como prefijo un indicador nulo si la columna de base de datos permite valores nulos.  
  
-   Cuando se copian datos en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], el archivo de datos debe tener datos para cada columna de la tabla de base de datos. Cuando se copian datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], los datos de todas las columnas de la tabla de base de datos, la vista o el conjunto de resultados de la instrucción SELECT se copian en el archivo de datos.  
  
-   Cuando se copian datos en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], la posición ordinal de una columna en el archivo de datos debe ser idéntica a la de la columna en la tabla de base de datos. Cuando se copian datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], el método **BCPExec** coloca los datos en función de la posición ordinal de la columna en la tabla de base de datos.  
  
-   Si un tipo de datos de base de datos es de longitud variable (por ejemplo, `varbinary(22)`) o si una columna de base de datos puede contener valores NULL, se antepone a los datos del archivo de datos un indicador de longitud o nulo. El ancho del indicador varía en función del tipo de datos y de la versión de copia masiva. La opción [IBCPSession::BCPControl](../../oledb/ole-db-interfaces/ibcpsession-bcpcontrol-ole-db.md) del método BCP_OPTION_FILEFMT proporciona compatibilidad entre los archivos de datos de copia masiva anteriores y los servidores donde se ejecutan versiones posteriores de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] al indicar cuándo el ancho de los indicadores de los datos es inferior a lo esperado.  
  
> [!NOTE]  
>  Para cambiar los valores de formato de datos especificados para un archivo de datos, use los métodos [IBCPSession::BCPColumns](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md) e [IBCPSession::BCPColFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md).  
  
 Las copias masivas en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se pueden optimizar para las tablas que no contienen índices estableciendo la opción de base de datos **select into/bulkcopy**.  
  
## <a name="arguments"></a>Argumentos  
 *pwszTable*[in]  
 Nombre de la tabla de base de datos en o de la que se va a copiar. El nombre puede incluir el nombre de la base de datos o el nombre del propietario. Por ejemplo, "pubs.username.titles", "pubs..titles", "username.titles".  
  
 Si el argumento eDirection está establecido en BCP_DIRECTION_OUT, el argumento pwszTable puede ser el nombre de una vista de base de datos.  
  
 Si el argumento eDirection está establecido en BCP_DIRECTION_OUT y se especifica una instrucción SELECT con el método **BCPControl** antes de llamar al método **BCPExec**, el argumento *pwszTable* tiene que establecerse en NULL.  
  
 *pwszDataFile*[in]  
 Nombre del archivo de usuario en o del que se va a copiar.  
  
 *pwszErrorFile*[in]  
 Nombre del archivo de error que se va a rellenar con mensajes de progreso, mensajes de error y copias de las filas que, por cualquier razón, no se puedan copiar de un archivo de usuario en una tabla. Si el argumento *pwszErrorFile* se establece en NULL, no se usa ningún archivo de error.  
  
 *eDirection*[in]  
 Dirección de la operación de copia, BCP_DIRECTION_IN o BCP_DIRECTION_OUT. BCP_DIRECTION_IN indica una copia de un archivo de usuario en una tabla de base de datos; BCP_DIRECTION_OUT indica una copia de una tabla de base de datos en un archivo de usuario.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 S_OK  
 El método se ha llevado a cabo de forma correcta.  
  
 E_FAIL  
 Se ha producido un error específico del proveedor; para obtener información detallada, use la interfaz [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_OUTOFMEMORY  
 Error de memoria insuficiente.  
  
 E_INVALIDARG  
 No se han especificado correctamente uno o varios argumentos. Por ejemplo, se proporcionó un nombre de archivo no válido.  
  
## <a name="see-also"></a>Consulte también  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Realizar operaciones de copia masiva](../../oledb/features/performing-bulk-copy-operations.md)  
  

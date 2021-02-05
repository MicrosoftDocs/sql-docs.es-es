---
title: IBCPSession::BCPControl (controlador OLE DB) | Microsoft Docs
description: Obtenga información sobre cómo IBCPSession::BCPControl establece las opciones para una operación de copia masiva en OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 07/03/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPControl (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPControl method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cedb68aa9157f89df053b1ab4c150736835b37b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186273"
---
# <a name="ibcpsessionbcpcontrol-ole-db"></a>IBCPSession::BCPControl (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Establece las opciones de una operación de copia masiva.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
HRESULT BCPControl(   
      int eOption,  
      void *iValue);  
```  
  
## <a name="remarks"></a>Observaciones  
 El método **BCPControl** establece varios parámetros de control para operaciones de copia masiva, incluidos el número de errores permitidos antes de cancelar una copia masiva, los números de la primera y última filas que se copian de un archivo de datos y el tamaño del lote.  
  
 Este método se utiliza también para especificar la instrucción SELECT que se utiliza en la copia masiva de datos desde [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Puede establecer el argumento **eOption** en BCP_OPTION_HINTS y el argumento **iValue** para tener un puntero a una cadena de caracteres anchos que contenga la instrucción SELECT.  
  
 Los valores posibles de *eOption* son:  
  
|Opción|Descripción|  
|------------|-----------------|  
|BCP_OPTION_ABORT|Detiene una operación de copia masiva que ya está en curso. Puede llamar al método **BCPControl** con un argumento *eOption* de BCP_OPTION_ABORT desde otro subproceso para detener una operación de copia masiva en ejecución. El argumento *iValue* se pasa por alto.|  
|BCP_OPTION_BATCH|El número de filas por lote. El valor predeterminado es 0, que indica todas las filas de una tabla cuando se están extrayendo datos, o todas las filas del archivo de datos del usuario cuando los datos se están copiando en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Un valor menor que 1 restablece BCP_OPTION_BATCH al valor predeterminado.|  
|BCP_OPTION_DELAYREADFMT|Un valor booleano, si se establece en true, hace que [IBCPSession::BCPReadFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpreadfmt-ole-db.md) se lea durante la ejecución. Si es false (el valor predeterminado), IBCPSession::BCPReadFmt leerá inmediatamente el archivo de formato. Se producirá un error de secuencia si **BCP_OPTION_DELAYREADFMT** es true y puede llamar a IBCPSession::BCPColumns o IBCPSession::BCPColFmt.<br /><br /> También se producirá un error de secuencia si se llama a `IBCPSession::BCPControl(BCPDELAYREADFMT, (void *)FALSE))` después de llamar a `IBCPSession::BCPControl(BCPDELAYREADFMT, (void *)TRUE)` e IBCPSession::BCPWriteFmt.<br /><br /> Para obtener más información, vea [Detección de metadatos](../../oledb/features/metadata-discovery.md).|  
|BCP_OPTION_FILECP|El argumento *iValue* contiene el número de la página de códigos del archivo de datos. Puede especificar el número de la página de códigos, como 1252 u 850, o uno de los valores siguientes:<br /><br /> BCP_FILECP_ACP: los datos del archivo están en la página de códigos de Microsoft Windows del cliente.<br /><br /> BCP_FILECP_OEMCP: los datos del archivo están en la página de códigos OEM del cliente (valor predeterminado).<br /><br /> BCP_FILECP_RAW: los datos del archivo están en la página de códigos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|BCP_OPTION_FILEFMT|Número de versión del formato de archivo de datos. Puede ser 80 ([!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)]), 90 ([!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]), 100 ([!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] o [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)]) o 110 ([!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]). 110 es el valor predeterminado. Esto resulta útil para exportar e importar datos en formatos admitidos en versiones anteriores del servidor.  Por ejemplo, para importar datos obtenidos de una columna de texto en un servidor [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] a una columna **varchar(max)** de un servidor [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] o posterior, debe especificar 80. Del mismo modo, si especifica 80 al exportar datos de una columna **varchar(max)** , se guardan de la misma forma que las columnas de texto en el formato de [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] y se pueden importar en una columna de texto de un servidor [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)].|  
|BCP_OPTION_FIRST|La primera fila de datos del archivo o la tabla que se copia. El valor predeterminado es 1; un valor menor que 1 reinicializa esta opción a su valor predeterminado.|  
|BCP_OPTION_FIRSTEX|En operaciones de salida de BCP, especifica la primera fila de la tabla de base de datos que se copia en el archivo de datos.<br /><br /> En operaciones de entrada de BCP, especifica la primera fila del archivo de datos que se copia en la tabla de base de datos.<br /><br /> Se espera que el parámetro *iValue* sea la dirección de un entero de 64 bits con signo que contiene el valor. El valor máximo que se puede pasar a BCPFIRSTEX es 2^63-1.|  
|BCP_OPTION_FMTXML|Se utiliza para especificar que el archivo de formato generado debe estar en un formato XML. Está apagado de forma predeterminada y de forma predeterminada los archivos de formato se guardan como archivos de texto. El formato de archivo XML proporciona mayor flexibilidad, pero con algunas restricciones más. Por ejemplo, no puede especificar el prefijo ni el terminador de un campo simultáneamente, lo que es posible en archivos de formatos anteriores.<br /><br /> Nota: Los archivos con formato XML solamente se admiten cuando se instalan herramientas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] junto con OLE DB Driver for SQL Server.|  
|BCP_OPTION_HINTS|El argumento *iValue* contiene un puntero de cadena de caracteres anchos. La cadena a la que apunta especifica sugerencias de procesamiento de copia masiva de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o una instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)] que devuelve un conjunto de resultados. Si se especifica una instrucción [!INCLUDE[tsql](../../../includes/tsql-md.md)] que devuelve más de un conjunto de resultados, se omiten todos los conjuntos de resultados posteriores al primero.|  
|BCP_OPTION_KEEPIDENTITY|Cuando el argumento *iValue* está establecido en TRUE, esta opción especifica que los métodos de copia masiva insertan valores de datos proporcionados para las columnas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] definidas con una restricción de identidad. El archivo de entrada debe proporcionar valores para las columnas de identidad. Si no se establece, se generan nuevos valores de identidad para las filas insertadas. No se tiene en cuenta ningún dato presente en el archivo para las columnas de identidad.|  
|BCP_OPTION_KEEPNULLS|Especifica si los valores de datos vacíos del archivo se convertirán en valores NULL en la tabla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Cuando el argumento *iValue* está establecido en TRUE, los valores vacíos se convierten en NULL en la tabla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Con el valor predeterminado, los valores vacíos se convierten en un valor predeterminado para la columna en la tabla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si existe un valor predeterminado.|  
|BCP_OPTION_LAST|La última fila que se copia. El valor predeterminado es copiar todas las filas. Un valor menor que 1 reinicia esta opción a su valor predeterminado.|  
|BCP_OPTION_LASTEX|En operaciones de salida de BCP, especifica la última fila de la tabla de base de datos que se copia en el archivo de datos.<br /><br /> En operaciones de entrada de BCP, especifica la última fila del archivo de datos que se copia en la tabla de base de datos.<br /><br /> Se espera que el parámetro *iValue* sea la dirección de un entero de 64 bits con signo que contiene el valor. El valor máximo que se puede pasar a BCPLASTEX es 2^63-1.|  
|BCP_OPTION_MAXERRS|El número de errores permitido antes de que la operación de copia masiva genere un error. El valor predeterminado es 10. Un valor menor que 1 reinicia esta opción a su valor predeterminado. La copia masiva impone un máximo de 65.535 errores. Si se intenta establecer esta opción en un valor mayor que 65.535, la opción se establece en 65.535.|  
|BCP_OPTION_ROWCOUNT|Devuelve el número de filas afectado por la operación actual (o última) de BCP.|  
|BCP_OPTION_TEXTFILE|El archivo de datos no es un archivo binario, sino un archivo de texto. BCP detecta si el archivo de texto es Unicode o no comprobando el marcador de byte Unicode en los primeros 2 bytes del archivo de datos.|  
|BCP_OPTION_UNICODEFILE|Cuando se establece en TRUE, esta opción especifica que el archivo de entrada es un formato de archivo de Unicode.|  
  
## <a name="arguments"></a>Argumentos  
 *eOption*[in]  
 Establézcalo en una de las opciones de la lista de la sección anterior de notas.  
  
 *iValue*[in]  
 Valor del argumento *eOption* especificado. El argumento *iValue* es una conversión del valor entero a un puntero nulo para permitir la expansión futura a valores de 64 bits.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 S_OK  
 El método se ha llevado a cabo de forma correcta.  
  
 E_FAIL  
 Se produjo un error específico del proveedor; para obtener información detallada, use la interfaz [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 No se esperaba la llamada al método. Por ejemplo, no se llamó al método [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) antes de llamar a esta función.  
  
 E_OUTOFMEMORY  
 Error de memoria insuficiente.  
  
## <a name="see-also"></a>Consulte también  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Realizar operaciones de copia masiva](../../oledb/features/performing-bulk-copy-operations.md)  
  

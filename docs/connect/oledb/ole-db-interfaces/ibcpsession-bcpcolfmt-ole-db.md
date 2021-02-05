---
title: IBCPSession::BCPColFmt (controlador OLE DB) | Microsoft Docs
description: Obtenga información sobre cómo el método IBCPSession::BCPColFmt crea un enlace entre las variables de programa y las columnas de SQL Server en OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 05/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- IBCPSession::BCPColFmt (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPColFmt method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4806671e534faf347837333b0c667c396751aeef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186297"
---
# <a name="ibcpsessionbcpcolfmt-ole-db"></a>IBCPSession::BCPColFmt (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Crea un enlace entre las variables de programa y las columnas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
HRESULT BCPColFmt(   
      DBORDINAL idxUserDataCol,  
      int eUserDataType,  
      int cbIndicator,  
      int cbUserData,  
      BYTE *pbUserDataTerm,  
      int cbUserDataTerm,  
      DBORDINAL idxServerCol);  
```  
  
## <a name="remarks"></a>Observaciones  
 El método **BCPColFmt** se usa para crear un enlace entre los campos de archivo de datos BCP y las columnas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Toma como parámetros la longitud, el tipo, el terminador y la longitud de prefijo de una columna y establece cada una de estas propiedades para campos individuales.  
  
 Si el usuario elige el modo interactivo, se llama a este método dos veces; una para establecer el formato de columna en función de los valores predeterminados (según el tipo de columna de servidor) y otra para establecer el formato en función del tipo de columna elegido por el cliente durante el modo interactivo para cada columna.  
  
 En modos no interactivos, se llama a este método una sola vez por columna para establecer el tipo de cada columna en el tipo de caracteres o tipo nativo y para establecer los terminadores de columna y fila.  
  
 El método **BCPColFmt** permite especificar el formato de archivo de usuario para las copias masivas. Para la copia masiva, un formato contiene los componentes siguientes:  
  
-   Una asignación de los campos de archivo de usuario a las columnas de base de datos.  
  
-   El tipo de datos de cada campo de archivo de usuario.  
  
-   La longitud del indicador opcional para cada campo.  
  
-   La longitud máxima de los datos por campo de archivo de usuario.  
  
-   La secuencia de bytes de terminación opcional para cada campo <a href="#terminator_note"><sup>**1**</sup></a>.  
  
-   La longitud de la secuencia de bytes de terminación opcional <a href="#terminator_note"><sup>**1**</sup></a>.  
  

> [!IMPORTANT]
> <b id="terminator_note">[1]:</b> No se admite el uso de la secuencia de terminador en escenarios en los que la página de códigos del archivo de datos está establecida en UTF-8. En estos escenarios, **pbUserDataTerm** se debe establecer en `nullptr` y **cbUserDataTerm** en `0`.

 En cada llamada a **BCPColFmt** se especifica el formato para un campo de archivo de usuario. Por ejemplo, para cambiar la configuración predeterminada de tres campos en un archivo de datos de usuario de cinco campos, primero debe llamar a `BCPColumns(5)`y, a continuación, debe llamar a **BCPColFmt** cinco veces, con tres de cuyas llamadas establecerá el formato personalizado. Para las dos llamadas restantes, establezca *eUserDataType* en BCP_TYPE_DEFAULT y establezca *cbIndicator*, *cbUserData* y *cbUserDataTerm* en 0, BCP_VARIABLE_LENGTH y 0, respectivamente. Este procedimiento copia las cinco columnas, tres con el formato personalizado y dos con el formato predeterminado.  
  
> [!NOTE]  
>  Es necesario llamar al método [IBCPSession::BCPColumns](../../oledb/ole-db-interfaces/ibcpsession-bcpcolumns-ole-db.md) antes de realizar cualquier llamada a **BCPColFmt**. Debe llamar a **BCPColFmt** una vez para cada columna del archivo de usuario. Si llama más de una vez a **BCPColFmt** para cualquier columna de archivo de usuario, se generará un error.  
  
 No es necesario copiar todos los datos de un archivo de usuario en una tabla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Para omitir una columna, debe especificar el formato de los datos de la columna estableciendo el parámetro idxServerCol en 0. Si desea omitir un campo, necesitará toda la información para que el método funcione correctamente.  
  
 **Nota** La función [IBCPSession::BCPWriteFmt](../../oledb/ole-db-interfaces/ibcpsession-bcpwritefmt-ole-db.md) puede usarse para conservar la especificación de formato que se proporciona mediante **BCPColFmt**.  
  
## <a name="arguments"></a>Argumentos  
 *idxUserDataCol*[in]  
 Índice de campo del archivo de datos del usuario.  
  
 *eUserDataType*[in]  
 Tipo de datos de campo del archivo de datos del usuario. Los tipos de datos disponibles se enumeran en el archivo de encabezado de OLE DB driver for SQL Server (msoledbsql.h) con formato BCP_TYPE_XXX, por ejemplo, BCP_TYPE_SQLINT4. Si se especifica el valor BCP_TYPE_DEFAULT, el proveedor intenta usar el mismo tipo que el tipo de columna de tabla o vista. Para operaciones de copia masiva fuera de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y en un archivo, cuando el argumento **eUserDataType** sea BCP_TYPE_SQLDECIMAL o BCP_TYPE_SQLNUMERIC:  
  
-   Si la columna de origen no es decimal o numérica, se usarán la precisión y la escala predeterminadas.  
  
-   Si la columna de origen es decimal o numérica, se usarán la precisión y la escala de la columna de origen.  
  
 *cbIndicator*[in]  
 Longitud de prefijo del campo. El valor predeterminado es BCP_PREFIX_DEFAULT. Las longitudes válidas para el prefijo son 0, 1, 2, 4 y 8. Un prefijo de tamaño 8 se usa normalmente para indicar que el campo está fragmentado. Se usa para realizar copias masivas de columnas de tipo de valor grande de forma eficaz.  
  
 *cbUserData*[in]  
 Longitud máxima (en bytes) de los datos de este campo del archivo de usuario, sin incluir la longitud de los indicadores de longitud o terminadores.  
  
 Al establecer **cbUserData** en BCP_LENGTH_NULL se indica que todos los valores de los campos del archivo de datos están, o debería estar, establecidos en NULL. Al establecer **cbUserData** en BCP_LENGTH_VARIABLE se indica que el sistema debería determinar la longitud de datos para cada campo. Para algunos campos, esto podría significar que se genera un indicador de longitud o NULL que precede a los datos en una copia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]o que se espera el indicador en los datos copiados en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Para el carácter y los tipos de datos binarios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], **cbUserData** puede ser BCP_LENGTH_VARIABLE, BCP_LENGTH_NULL, 0 o un valor positivo. Si **cbUserData** es BCP_LENGTH_VARIABLE, el sistema usa el indicador de longitud, si está presente, o una secuencia de terminador que determina la longitud de los datos. Si se proporciona un indicador de longitud y una secuencia de terminador, la copia masiva usa aquél por el que se copia la mínima cantidad de datos. Si **cbUserData** es BCP_LENGTH_VARIABLE, el tipo de datos es un carácter o un tipo binario de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y no se especifica un indicador de longitud ni una secuencia de terminador, el sistema devuelve un mensaje de error.  
  
 Si **cbUserData** es 0 o un valor positivo, el sistema utiliza **cbUserData** como longitud de datos máxima. Sin embargo, si además de un valor de **cbUserData** positivo, se proporciona un indicador de longitud o una secuencia de terminador, el sistema determina la longitud de los datos utilizando el método por el que se copia la cantidad mínima de datos.  
  
 El valor de **cbUserData** representa el recuento de bytes de datos. Si los datos de caracteres se representan mediante caracteres anchos de Unicode, un valor del parámetro **cbUserData** positivo representa el número de caracteres multiplicado por el tamaño, en bytes, de cada carácter.  
  
 *pbUserDataTerm*[size_is][in]  
 Secuencia de terminador que se va a usar para el campo. Este parámetro es principalmente útil para los tipos de datos de caracteres porque todos los demás tipos son de longitud fija o, en el caso de los datos binarios, requieren un indicador de longitud que grabe con precisión el número de bytes presentes.  
  
 Para evitar la terminación de los datos extraídos o indicar que no se terminen los datos de un archivo de usuario, establezca este parámetro en NULL.  
  
 Si se usa más de un medio para especificar una longitud de columna de usuario (como un terminador y un indicador de longitud, o un terminador y una longitud máxima de columna), la copia masiva elige aquél por el que se copia la cantidad mínima de datos.  
  
 La API de copia masiva realiza la conversión de caracteres Unicode a MBCS según sea necesario. Se deben extremar las precauciones para asegurarse de que se establece correctamente la cadena de bytes del terminador y la longitud de la cadena de bytes. Vea la sección [Comentarios](#remarks) anterior para conocer las limitaciones de la codificación UTF-8.

 *cbUserDataTerm*[in]  
 Longitud (en bytes) de la secuencia de terminador que va a usarse para la columna. Si no hay ningún terminador presente o no se desea uno en los datos, establezca este valor en 0. Vea la sección [Comentarios](#remarks) anterior para conocer las limitaciones de la codificación UTF-8.

 *idxServerCol*[in]  
 Posición ordinal de la columna en la tabla de base de datos. El primer número de columna es 1. La posición ordinal de una columna la notifica el método **IColumnsInfo::GetColumnInfo** u otros métodos similares. Si este valor es 0, la copia masiva omite el campo en el archivo de datos.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 S_OK  
 El método se ha llevado a cabo de forma correcta.  
  
 E_FAIL  
 Se ha producido un error específico del proveedor; para obtener más información, use la interfaz [ISQLServerErrorInfo](./isqlservererrorinfo-geterrorinfo-ole-db.md).  
  
 E_UNEXPECTED  
 No se esperaba la llamada al método. Por ejemplo, no se llamó al método [IBCPSession::BCPInit](../../oledb/ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) antes de llamar a este método.  
  
 E_INVALIDARG  
 El argumento no era válido.  
  
 E_OUTOFMEMORY  
 Error de memoria insuficiente.  
  
## <a name="see-also"></a>Consulte también  
 [IBCPSession &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-ole-db.md)   
 [Realizar operaciones de copia masiva](../../oledb/features/performing-bulk-copy-operations.md)  
  

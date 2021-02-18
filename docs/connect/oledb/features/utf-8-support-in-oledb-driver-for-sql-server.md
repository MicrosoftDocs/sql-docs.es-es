---
title: Compatibilidad de UTF-8 con el controlador OLE DB para SQL Server | Microsoft Docs
description: Obtenga información sobre la compatibilidad de OLE DB Driver for SQL Server con la codificación de cliente UTF-8 y la codificación de servidor UTF-8.
ms.custom: ''
ms.date: 05/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-kaywon
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: 063021cfbd1b8668c18c3f0bddfe2c716a790467
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352338"
---
# <a name="utf-8-support-in-ole-db-driver-for-sql-server"></a>Compatibilidad de UTF-8 con el controlador OLE DB para SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

El controlador OLE DB de Microsoft para SQL Server (versión 18.2.1) agrega compatibilidad con la codificación del servidor UTF-8. Para obtener información sobre la compatibilidad con UTF-8 de SQL Server, consulte:
- [Compatibilidad con la intercalación y Unicode](../../../relational-databases/collations/collation-and-unicode-support.md)
- [Compatibilidad con UTF-8](../../../relational-databases/collations/collation-and-unicode-support.md#utf8)

La versión 18.4.0 del controlador agrega compatibilidad para la codificación de cliente UTF-8 (habilitada con la casilla "Uso de Unicode UTF-8 para la compatibilidad con idiomas internacionales" en Configuración regional en Windows 10).

> [!NOTE]  
> Microsoft OLE DB Driver for SQL Server usa la función [GetACP](/windows/win32/api/winnls/nf-winnls-getacp) para determinar la codificación del búfer de entrada DBTYPE_STR.
>
> A partir de la versión 18.4 se admiten escenarios en los que GetACP devuelve una codificación UTF-8 (habilitada con la casilla "Uso de Unicode UTF-8 para la compatibilidad con idiomas internacionales" en Configuración regional en Windows 10). En las versiones anteriores, si el búfer necesita almacenar datos Unicode, el tipo de datos de búfer se debe establecer en *DBTYPE_WSTR* (codificación UTF-16).

## <a name="data-insertion-into-a-utf-8-encoded-char-or-varchar-column"></a>Inserción de datos en una columna CHAR o VARCHAR codificada con UTF-8
Al crear un búfer de parámetro de entrada para la inserción, se describe el búfer mediante el uso de una matriz de [estructuras DBBINDING](/previous-versions/windows/desktop/ms716845(v=vs.85)). Cada estructura DBBINDING asocia un solo parámetro al búfer del consumidor y contiene información como la longitud y el tipo del valor de datos. Para un búfer de parámetro de entrada de tipo CHAR, el valor *wType* de la estructura DBBINDING debe establecerse en DBTYPE_STR. Para un búfer de parámetro de entrada de tipo WCHAR, el valor *wType* de la estructura DBBINDING debe establecerse en DBTYPE_WSTR.

Al ejecutar un comando con parámetros, el controlador crea información de tipo de datos de parámetro. Si el tipo de búfer de entrada y el tipo de datos del parámetro coinciden, no se realiza ninguna conversión en el controlador. En caso contrario, el controlador convierte el búfer del parámetro de entrada en el tipo de datos de parámetro. El tipo de datos de parámetro se puede establecer explícitamente por el usuario mediante una llamada a [ICommandWithParameters::SetParameterInfo](/previous-versions/windows/desktop/ms725393(v=vs.85)). Si no se proporciona la información, el controlador deriva la información del tipo de datos de parámetro mediante a) la recuperación de los metadatos de columna desde el servidor cuando se prepara la instrucción, o b) el intento de realizar una conversión predeterminada desde el tipo de datos de parámetro de entrada.

El búfer de parámetro de entrada puede convertirse a la intercalación de columna del servidor por el controlador o el servidor, según el tipo de datos del búfer de entrada y el tipo de datos de parámetro. Durante la conversión, pueden perderse datos si la página de código del cliente o la página de código de intercalación de la base de datos no puede representar todos los caracteres en el búfer de entrada. La tabla siguiente describe el proceso de conversión al insertar datos en una columna habilitada para UTF-8:

|Tipo de datos del búfer|Tipo de datos de parámetro|Conversión|Precaución del usuario|
|---             |---                |---       |---            |
|DBTYPE_STR|DBTYPE_STR|Conversión del servidor de la página de códigos del cliente a la página de códigos de intercalación de la base de datos; conversión del servidor de la página de códigos de intercalación de base de datos a la página de códigos de intercalación de columna.|Asegúrese de que la página de códigos del cliente y la página de códigos de intercalación de base de datos pueden representar todos los caracteres de los datos de entrada. Por ejemplo, para insertar un carácter polaco, la página de códigos del cliente podría establecerse en 1250 (ANSI Europa Central), y la intercalación de la base de datos podría usar el polaco como el designador de intercalación (por ejemplo, Polish_100_CI_AS_SC) o estar habilitada para UTF-8.|
|DBTYPE_STR|DBTYPE_WSTR|Conversión de controlador de página de códigos del cliente a codificación UTF-16; conversión de servidor de la codificación UTF-16 a la página de códigos de intercalación de columna.|Asegúrese de que la página de códigos del cliente puede representar todos los caracteres de los datos de entrada. Por ejemplo, para insertar un carácter polaco, la página de códigos del cliente podría establecerse en 1250 (ANSI Europa Central).|
|DBTYPE_WSTR|DBTYPE_STR|Conversión del servidor de la codificación UTF-16 a la página de códigos de intercalación de la base de datos; conversión del servidor de la página de códigos de intercalación de base de datos a la página de códigos de intercalación de columna.|Asegúrese de que la página de códigos de intercalación de base de datos pueden representar todos los caracteres de los datos de entrada. Por ejemplo, para insertar un carácter polaco, la página de códigos de intercalación de la base de datos podría usar el polaco como el designador de intercalación (por ejemplo, Polish_100_CI_AS_SC) o estar habilitada para UTF-8.|
|DBTYPE_WSTR|DBTYPE_WSTR|Conversión de servidor de UTF-16 a la página de códigos de intercalación de columna.|Ninguno.|

## <a name="data-retrieval-from-a-utf-8-encoded-char-or-varchar-column"></a>Recuperación de datos de una columna CHAR o VARCHAR codificada con UTF-8
Al crear un búfer para datos recuperados, se describe el búfer mediante el uso de una matriz de [estructuras DBBINDING](/previous-versions/windows/desktop/ms716845(v=vs.85)). Cada estructura DBBINDING asocia una sola columna en la fila recuperada. Para recuperar los datos de columna como CHAR, establezca el valor *wType* de la estructura DBBINDING en DBTYPE_STR. Para recuperar los datos de columna como WCHAR, establezca el valor *wType* de la estructura DBBINDING en DBTYPE_WSTR.

Para el indicador de tipo de búfer de resultados DBTYPE_STR, el controlador convierte los datos con codificación UTF-8 al cliente de codificación. El usuario debe asegurarse de que el cliente de codificación puede representar los datos de la columna de UTF-8, en caso contrario, puede producirse pérdida de datos.

Para el indicador de tipo de búfer de resultados DBTYPE_WSTR, el controlador convierte los datos con codificación UTF-8 a la codificación UTF-16.

## <a name="communication-with-servers-that-dont-support-utf-8"></a>Comunicación con servidores que no son compatibles con UTF-8
Microsoft OLE DB Driver for SQL Server garantiza que los datos se exponen al servidor de una forma que los pueda entender. Cuando se insertan datos de clientes habilitados para UTF-8, el controlador convierte las cadenas con codificación UTF-8 en la página de códigos de intercalación de la base de datos antes de enviarlas al servidor.

> [!NOTE]  
> El uso de la interfaz [ISequentialStream](/previous-versions/windows/desktop/ms718035(v=vs.85)) para insertar datos con codificación UTF-8 en una columna de texto heredado solo se limita a los servidores que admiten UTF-8. Para obtener más información, vea [Blobs y objetos OLE](../ole-db-blobs/blobs-and-ole-objects.md).

## <a name="see-also"></a>Consulte también  
[Controlador OLE DB para las características de SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md) 

[Compatibilidad de UTF-16 con el controlador OLE DB para SQL Server](../../oledb/features/utf-16-support-in-oledb-driver-for-sql-server.md)    
  

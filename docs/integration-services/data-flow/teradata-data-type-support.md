---
description: Compatibilidad con tipos de datos de Teradata
title: Compatibilidad con tipos de datos de Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cb58e1d27e955009cd420a089f8d8f938f26c10f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346522"
---
# <a name="data-type-support"></a>Compatibilidad con tipos de datos

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Los componentes de SSIS usan la API Teradata Parallel Transporter (TPT API) para cargar y transferir datos desde bases de datos Teradata y hacia ellas. Por lo tanto, solo se puede usar el tipo de datos TPT API admitido en SSIS.

> [!NOTE]
>
> Los tipos de datos TIME, TIMESTAMP e INTERVAL en Teradata se controlan mediante TPT API como cadenas de caracteres de tamaño fijo. Los componentes de SSIS para Teradata los controlan como una cadena.

## <a name="data-type-mapping"></a>Asignación de tipos de datos

En la tabla siguiente se muestran los tipos de datos de bases de datos de Teradata y su asignación predeterminada a los tipos de datos de SSIS. También se muestran los tipos de datos de Teradata no compatibles.

|Tipo de datos de SQL|Tipo de datos de SSIS|
|:-|:-|
|DECIMAL/NUMERIC|DT_NUMERIC|
|BYTEINT|DT_I1|
|SMALLINT|DT_I2|
|INTEGER|DT_I4|
|FLOAT/REAL/DOUBLE PRECISION|DT_R8|
|DATE|DT_DBDATE|
|TIME<br>TIME(n)|DT_STR|
|timestamp<br>TIMESTAMP (n)|DT_STR|
|TIME WITH TIMEZONE<br>TIME(n) WITH TIMEZONE|DT_STR|
|TIMESTAMP WITH TIMEZONE<br>TIMESTAMP(n) WITH TIMEZONE|DT_STR|
|INTERVAL YEAR<br>INTERVAL YEAR (n)|DT_STR|
|INTERVAL YEAR TO MONTH<br>INTERVAL YEAR (n) TO MONTH|DT_STR|
|INTERVAL MONTH<br>INTERVAL MONTH (n)|DT_STR|
|INTERVAL DAY<br>INTERVAL DAY (n)|DT_STR|
|INTERVAL DAY TO HOUR<br>INTERVAL DAY (n) TO HOUR|DT_STR|
|INTERVAL DAY TO MINUTE<br>INTERVAL DAY (n) TO MINUTE|DT_STR|
|INTERVAL DAY TO SECOND<br>INTERVAL DAY (n) TO SECOND<br>INTERVAL DAY TO SECOND (m)<br>INTERVAL DAY (n) TO SECOND (m)|DT_STR|
|INTERVAL HOUR<br>INTERVAL HOUR (n)|DT_STR|
|INTERVAL HOUR TO MINUTE<br>INTERVAL HOUR (n) TO MINUTE|DT_STR
|INTERVAL HOUR TO SECOND<br>INTERVAL HOUR (n) TO SECOND<br>INTERVAL HOUR TO SECOND (m)<br>INTERVAL HOUR (n) TO SECOND (m)|DT_STR|
|INTERVAL MINUTE<br>INTERVAL MINUTE (n)|DT_STR|
|INTERVAL MINUTE TO SECOND<br>INTERVAL MINUTE (n) TO SECOND<br>INTERVAL MINUTE TO SECOND (m)<br>INTERVAL MINUTE (n) TO SECOND (m)|DT_STR|
|INTERVAL SECOND<br>INTERVAL SECOND (n)<br>INTERVAL SECOND (n,m)|DT_STR|
|PERIOD(DATE)|DT_STR|
|PERIOD(TIME)|DT_STR|
|NUMBER|DT_STR|
|CHARACTER|DT_STR|
|VARCHAR|DT_STR (DT_WSTR para el juego de caracteres Unicode)<br>**Notas**:<br> La longitud máxima de VARCHAR admitida es de 32 000 caracteres. <br> La longitud máxima permitida de DT_STR es de 8000 caracteres, DT_WSTR es de 4000 caracteres. Los datos se truncan si se supera este límite.|
|LONG VARCHAR|No compatible|
|CLOB|No compatible|
|BYTE|DT_BYTES<br>**Nota**: La longitud máxima permitida es de 8000 bytes. Los datos se truncan si se supera este límite.|
|VARBYTE|DT_BYTES<br>**Nota**: La longitud máxima permitida es de 8000 bytes. Los datos se truncan si se supera este límite.|
|BLOB|No compatible|

## <a name="next-steps"></a>Pasos siguientes

- Configurar el [administrador de conexiones de Teradata](teradata-connection-manager.md)
- Configurar el [origen de Teradata](teradata-source.md)
- Configurar el [destino de Teradata](teradata-destination.md)
- Si tiene alguna pregunta, visite [TechCommunity](https://aka.ms/AA6iwdw).

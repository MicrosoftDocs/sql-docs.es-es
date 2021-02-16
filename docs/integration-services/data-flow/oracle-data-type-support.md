---
description: Compatibilidad de tipos de datos con Microsoft Connector for Oracle
title: Compatibilidad de tipos de datos con Microsoft Connector for Oracle | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8ef719fc0fe41120fe42d324beb563b06bb81060
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343208"
---
# <a name="microsoft-connector-for-oracle-data-type-support"></a>Compatibilidad de tipos de datos con Microsoft Connector for Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Los componentes de SSIS para Oracle no admiten todos los tipos de datos de Oracle. Las columnas con tipos de datos no compatibles tendrán una advertencia al diseñar paquetes en SSDT y se eliminarán de las columnas de asignación. Los datos no se pueden cargar en una columna con un tipo de datos no compatible.

## <a name="data-type-mapping"></a>Asignación de tipos de datos

En la tabla siguiente se muestran los tipos de datos de bases de datos de Oracle y su asignación predeterminada a los tipos de datos de SSIS. También se muestran los tipos de datos de Oracle no compatibles.

|Tipo de datos de base de datos de Oracle|Tipo de datos de SSIS|Comentarios|
|:-|:-|:-|
|VARCHAR2|DT_STR||
|NVARCHAR2|DT_WSTR||
|CHAR|DT_STR||
|NUMBER|DT_R8|Se puede cambiar a DT_NUMERIC con precisión y escala específicas. La precisión y escala las define el usuario según las necesidades. La salida serán los datos de la columna como un número con precisión y escala fijas.|
|NUMBER(P, S)| Si la escala es 0, de acuerdo con la precisión (P) <li> DT_I1 <Li> DT_I2 <Li> DT_I4 <Li> DT_NUMBERIC(P,0)||
||DT_NUMERIC(P,S)||
|DATE|DT_DBTIMESTAMP||
|<li>timestamp <li>TIMESTAMP WITH TIME ZONE <li>INTERVAL YEAR TO MONTH <li>INTERVAL DAY TO SECOND <li>TIMESTAMP WITH LOCAL TIME ZONE|DT_STR||
|RAW|DT_BYTES||
|CLOB|DT_TEXT|Los tipos de datos CLOB, NCLOB y BLOB solo se admiten en el modo de matriz y no en el modo de carga rápida.|
|NCLOB|DT_NTEXT||
|BLOB|DT_IMAGE||
|UROWID|No compatible||
|REF|No compatible||
|BFILE|No compatible||
|LONG|No compatible||
|LONG RAW|No compatible||
|ROWID|No compatible||
|Tipo definido por el usuario (tipo de objeto, VARRAY, tabla anidada)|No compatible||

## <a name="next-steps"></a>Pasos siguientes

- Configure el [Administrador de conexiones de Oracle](oracle-connection-manager.md).
- Configure el [origen de Oracle](oracle-source.md).
- Configure el [destino de Oracle](oracle-destination.md).
- Si tiene alguna pregunta, visite [TechCommunity](https://aka.ms/AA5u35j).

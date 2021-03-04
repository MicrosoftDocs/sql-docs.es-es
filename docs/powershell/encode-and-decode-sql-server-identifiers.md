---
title: Codificar y descodificar identificadores de SQL Server
description: Los identificadores delimitados de SQL Server a veces contienen caracteres que no se admiten en las rutas de acceso de Windows PowerShell. Obtenga información sobre cómo incluirlos mediante su representación con sus valores hexadecimales.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 13b32a62791aaa932311223f4f648d8006eb33e7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838050"
---
# <a name="encode-and-decode-sql-server-identifiers"></a>Codificar y descodificar identificadores de SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Los identificadores delimitados de SQL Server a veces contienen caracteres no admitidos en las rutas de acceso de Windows PowerShell. Estos caracteres se pueden especificar codificando sus valores hexadecimales.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Los caracteres que no se permiten en los nombres de ruta de Windows PowerShell se pueden representar, o codificar, como el carácter "%" seguido del valor hexadecimal del modelo de bits que representa el carácter, como en " **%** xx". La codificación siempre se puede usar para controlar los caracteres que no se admiten en las rutas de Windows PowerShell.

El cmdlet **Encode-SqlName** toma como entrada un identificador de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Genera una cadena con todos los caracteres que no son admitidos por el lenguaje de Windows PowerShell codificados con "%xx". El cmdlet **Decode-SqlName** toma como entrada un identificador de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] codificado y devuelve el identificador original.  

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Los cmdlets **Encode-Sqlname** y **Decode-Sqlname** solo codifican o descodifican caracteres que se permiten en los identificadores delimitados de SQL Server, pero no solo se admiten en las rutas de PowerShell. Estos son los caracteres codificados por **Encode-SqlName** y descodificados por **Decode-SqlName**:

|||||||||||||
|-|-|-|-|-|-|-|-|-|-|-|-|
|**Carácter**|\ |/|:|%|\<|>|*|?|[|]|&#124;|  
|**Codificación hexadecimal**|%5C|%2F|%3A|%25|%3C|%3E|%2A|%3F|%5B|%5D|%7C|

## <a name="encoding-an-identifier"></a>Codificar un identificador  

### <a name="to-encode-a-sql-server-identifier-in-a-powershell-path"></a>Para codificar un identificador de SQL Server en una ruta de acceso de PowerShell

- Use uno de los dos métodos para codificar un identificador de SQL Server:
    - Especifique el código hexadecimal para el carácter no compatible mediante la sintaxis %XX, donde el código hexadecimal es XX.
    - Transfiera el identificador como una cadena entrecomillada al cmdlet **Encode-Sqlname** .

### <a name="examples-encoding"></a>Ejemplos (codificación)

Este ejemplo especifica la versión codificada del carácter (%3A) ":":

```powershell
Set-Location Table%3ATest
```

También puede usar **Encode-SqlName** para compilar un nombre admitido por Windows PowerShell:

```powershell
Set-Location (Encode-SqlName "Table:Test")
```

## <a name="decoding-an-identifier"></a>Descodificar un identificador

### <a name="to-decode-a-sql-server-identifier-from-a-powershell-path"></a>Descodificar un identificador de SQL Server de una ruta de PowerShell

Use el cmdlet **Decode-Sqlname** para reemplazar las codificaciones hexadecimales por caracteres representados por la codificación.

### <a name="examples-decoding"></a>Ejemplos (descodificación)

Este ejemplo devuelve “Table:Test”:

```powershell
Decode-SqlName "Table%3ATest"
```

## <a name="see-also"></a>Consulte también

- [Identificadores de SQL Server en PowerShell](sql-server-identifiers-in-powershell.md)
- [Proveedor de SQL Server PowerShell Provider](sql-server-powershell-provider.md)
- [SQL Server PowerShell](sql-server-powershell.md)  

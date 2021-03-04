---
title: Identificadores de SQL Server de escape
description: Los identificadores delimitados de SQL Server a veces contienen caracteres que no se admiten en las rutas de acceso de Windows PowerShell. Aprenda cómo se pueden escapar algunos de ellos con el carácter de tilde.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 105acf1e3e67d558ccbb0ae450afba9468de3a5f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838039"
---
# <a name="escape-sql-server-identifiers"></a>Identificadores de SQL Server de escape

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

A menudo, se puede usar el carácter de escape de acento invertido (`) para hacer que se eludan los caracteres que se permiten en los identificadores delimitados de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pero no en los nombres de ruta de Windows PowerShell. Sin embargo, algunos caracteres no se pueden evitar. Por ejemplo, no puede hacer que se eluda el carácter de dos puntos (:) en Windows PowerShell. Los identificadores con ese carácter deben codificarse. La codificación es más confiable que hacer que el carácter se eluda porque funciona para todos los caracteres.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

El carácter de acento invertido (`) suele estar en la tecla de la parte superior izquierda del teclado, debajo de la tecla ESC.  

## <a name="examples"></a>Ejemplos

Este es un ejemplo en el que se hace que se eluda un carácter #:  

```powershell
cd SQLSERVER:\SQL\MyComputer\MyInstance\MyDatabase\MySchema\`#MyTempTable  
```

Este es un ejemplo en el que se hace que se eluda un paréntesis al especificar (local) como nombre de equipo:  

```powershell
Set-Location SQLSERVER:\SQL\`(local`)\DEFAULT  
```

## <a name="see-also"></a>Consulte también

- [Identificadores de SQL Server en PowerShell](sql-server-identifiers-in-powershell.md)
- [Proveedor de SQL Server PowerShell Provider](sql-server-powershell-provider.md)
- [SQL Server PowerShell](sql-server-powershell.md)
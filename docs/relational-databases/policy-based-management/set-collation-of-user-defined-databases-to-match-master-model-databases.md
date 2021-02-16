---
title: Establecimiento de la intercalación de bases de datos definidas por el usuario para que coincidan con las bases de datos modelo y maestra
description: Obtenga información sobre cómo habilitar una directiva para comprobar si las intercalaciones de las bases de datos definidas por el usuario y las del sistema son las mismas.
ms.custom: seo-lt-2019
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 7c196d81609a9bb223caa0dd2ffa055a4e9b2bce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272346"
---
# <a name="set-the-collation-of-user-defined-databases-to-match-master-and-model-databases"></a>Establecimiento de la intercalación de bases de datos definidas por el usuario para que coincidan con las bases de datos modelo y maestra
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Esta regla comprueba si las bases de datos definidas por el usuario se definen utilizando una intercalación de base de datos que es igual a la intercalación de las bases de datos maestra o modelo.
  
## <a name="best-practices-recommendations"></a>Procedimientos recomendados  
 Recomendamos que las intercalaciones de bases de datos definidas por el usuario coincidan con la intercalación de las bases de datos maestra o modelo. De lo contrario, pueden producirse conflictos de intercalación que podrían impedir que el código se ejecute. Por ejemplo, cuando un procedimiento almacenado combina una tabla a una tabla temporal, es posible que SQL Server finalice el lote y devuelva un error de conflicto de intercalación si las intercalaciones de la base de datos definida por el usuario y la base de datos modelo son diferentes. Esto se debe a que las tablas temporales se crean en tempdb, que basa su intercalación en la de la base de datos modelo.

  Si experimenta errores de conflictos de intercalación, considere una de las soluciones siguientes:

  - Exporte los datos de la base de datos de usuario e impórtelos en tablas nuevas que tengan la misma intercalación que las bases de datos modelo y maestra.

  - Vuelva a generar las bases de datos del sistema para utilizar una intercalación que coincida con la intercalación de las bases de datos de usuario. Para obtener más información sobre cómo recompilar las bases de datos del sistema, vea [Recompilación de las bases de datos del sistema](../databases/rebuild-system-databases.md).

  - Modifique cualquier procedimiento almacenado que una las tablas de usuario a las tablas en tempdb para crear las tablas en tempdb utilizando la intercalación de la base de datos de usuario. Para ello, agregue la cláusula COLLATE database_default a las definiciones de columna de la tabla temporal, como se muestra en el ejemplo siguiente:
  
    ```
    CREATE TABLE #temp1 ( c1 int, c2 varchar(30) COLLATE database_default )
    ```

## <a name="see-also"></a>Consulte también
  
 [Configurar o cambiar la intercalación del servidor](../collations/set-or-change-the-server-collation.md)  

 [Establecer o cambiar la intercalación de base de datos](../collations/set-or-change-the-database-collation.md)

 [Establecer o cambiar la intercalación de columnas](../collations/set-or-change-the-column-collation.md)
 
 [Ver información de intercalación](../collations/view-collation-information.md)    
  
  

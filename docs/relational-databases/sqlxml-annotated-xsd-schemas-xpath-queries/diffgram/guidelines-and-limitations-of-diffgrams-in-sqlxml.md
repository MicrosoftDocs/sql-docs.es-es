---
description: Instrucciones y limitaciones de DiffGrams en SQLXML
title: Instrucciones y limitaciones de DiffGrams en SQLXML
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- DiffGrams [SQLXML], about DiffGrams
ms.assetid: cf8689c4-2a63-4d05-b202-21b5ff187d7f
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f5d9cbcd08d41f4ce134a663fdcc1cf1bd1c3298
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97415045"
---
# <a name="guidelines-and-limitations-of-diffgrams-in-sqlxml"></a>Instrucciones y limitaciones de DiffGrams en SQLXML
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Recuerde lo siguiente al usar DiffGrams con SQLXML 4.0:  
  
-   Los tipos de objetos binarios grandes (BLOB) como **text/ntext** e imágenes no se deben usar en el **\<diffgr:before>** bloque de al trabajar con DiffGrams, porque esto los incluiría para su uso en el control de simultaneidad. Esto puede producir problemas con [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debido a las limitaciones en la comparación para los tipos BLOB. Por ejemplo, la palabra clave LIKE se usa en la cláusula WHERE para comparar entre las columnas del tipo de datos **Text** ; sin embargo, se producirá un error en las comparaciones en el caso de los tipos BLOB donde el tamaño de los datos es mayor que 8 k.  
  
-   Los caracteres especiales en los datos **ntext** pueden producir problemas con SQLXML 4,0 debido a las limitaciones de la comparación de los tipos de BLOB. Por ejemplo, el uso de "[Serializable]" en el **\<diffgr:before>** bloque de un DiffGram cuando se usa en la comprobación de simultaneidad de una columna de tipo **ntext** producirá un error con la siguiente descripción del error SQLOLEDB:  
  
    ```  
    Empty update, no updatable rows found   Transaction aborted  
    ```  
  
  

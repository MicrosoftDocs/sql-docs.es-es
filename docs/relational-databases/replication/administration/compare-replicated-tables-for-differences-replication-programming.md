---
title: Comparación de las diferencias entre las tablas replicadas (procedimientos almacenados de replicación)
description: Use los procedimientos almacenados de replicación para comparar las diferencias entre las tablas replicadas en el publicador y el suscriptor.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- tablediff utility
- comparing replicated tables
ms.assetid: cd253a17-0c85-42b4-912c-690169ebe799
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 0b9717a6c71d5aef31dae428a67de37e9d9188ca
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97417081"
---
# <a name="compare-differences-between-replicated-tables-replication-programming"></a>Comparación de las diferencias entre las tablas replicadas (programación de la replicación)
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Para determinar si los datos publicados en los artículos de tabla del publicador y el suscriptor no son idénticos, lo que puede indicar una falta de convergencia, se usa la validación de artículos. Para obtener más información, vea [Validar datos replicados](../../../relational-databases/replication/validate-data-at-the-subscriber.md). La validación, sin embargo, solo devuelve información sobre la existencia o no de diferencias, pero no indica las diferencias concretas entre las tablas de origen y destino. La utilidad de símbolo del sistema **tablediff** devuelve información detallada sobre las diferencias entre dos tablas y puede generar un script [!INCLUDE[tsql](../../../includes/tsql-md.md)] para establecer la convergencia de la suscripción con los datos del publicador.  
  
> [!NOTE]  
>  La utilidad **tablediff** solo se admite en servidores [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
### <a name="to-compare-replicated-tables-for-differences-using-tablediff"></a>Para buscar diferencias en tablas replicarse con tablediff  
  
1.  En el símbolo del sistema de cualquier servidor incluido en una topología de replicación, ejecute la [tablediff Utility](../../../tools/tablediff-utility.md). Especifique los parámetros siguientes:  
  
    -   **-sourceserver** : el nombre del servidor donde se sabe que los datos son correctos, normalmente el publicador.  
  
    -   **-sourcedatabase** : el nombre de la base de datos que contiene los datos correctos.  
  
    -   **-sourcetable** : el nombre de la tabla de origen del artículo que se compara.  
  
    -   (Opcional) **-sourceschema** : el propietario del esquema de la tabla de origen, si no se trata del esquema predeterminado.  
  
    -   (Opcional) **-sourceuser** y **-sourcepassword** si se usa la autenticación de SQL Server para conectar al publicador.  
  
        > [!IMPORTANT]  
        >  Siempre que sea posible, utilice la autenticación de Windows. Si debe usar la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , pida a los usuarios que escriban las credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
    -   **-destinationserver** : el nombre del servidor en el que se comparan los datos, normalmente un suscriptor.  
  
    -   **-destinationdatabase** : el nombre de la base de datos que se compara.  
  
    -   **-destinationtable** : el nombre de la tabla que se compara.  
  
    -   (Opcional) **-destinationschema** : el propietario del esquema de la tabla de destino, si no se trata del esquema predeterminado.  
  
    -   (Opcional) **-destinationuser** y **-destinationpassword** si usa la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para conectar al suscriptor.  
  
        > [!IMPORTANT]  
        >  Siempre que sea posible, utilice la autenticación de Windows. Si debe usar la autenticación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , pida a los usuarios que escriban las credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
    -   (Opcional) Use **-c** para realizar una comparación de columna.  
  
    -   (Opcional) Use **-q** para realizar una comparación rápida solo del esquema y de recuento de filas.  
  
    -   (Opcional) Especifique un nombre de archivo y una ruta de acceso en **-o** para generar los resultados en un archivo.  
  
    -   (Opcional) Especifique una tabla de la base de datos de suscripciones en la que insertar los resultados de **-et**. Si la tabla ya existe, especifique **-dt** para quitarla previamente.  
  
    -   (Opcional) Use **-f** para generar un archivo [!INCLUDE[tsql](../../../includes/tsql-md.md)] y corregir los datos en el suscriptor de forma que coincidan con los datos en el publicador. Use **-df** para especificar el número de instrucciones [!INCLUDE[tsql](../../../includes/tsql-md.md)] de cada archivo.  
  
    -   (Opcional) Use **-rc** y **-ri** para especificar el número de veces que se intentará una operación y el intervalo de reintento.  
  
    -   (Opcional) Use **-strict** para exigir la comparación estricta del esquema entre las tablas de origen y destino.  
  
## <a name="see-also"></a>Consulte también  
 [Validar datos en el suscriptor](../../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  

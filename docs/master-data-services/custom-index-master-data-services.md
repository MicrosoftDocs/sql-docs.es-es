---
description: Índice personalizado (Master Data Services)
title: Índice personalizado
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: c57bf8b8-55a6-4b6c-9adb-91b5f4f1ee3c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4431155c44db9a76f8ebf94f9a6be228e9fd358a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339086"
---
# <a name="custom-index-master-data-services"></a>Índice personalizado (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Los índices personalizados crean un índice no clúster en un atributo (índice único) o en una lista de atributos (índice compuesto) en una entidad. Normalmente los índices mejoran el rendimiento del proceso de consulta. Para obtener más información sobre los índices de SQL Server, consulte [Índices](../relational-databases/indexes/indexes.md).  
  
## <a name="type-of-indexes"></a>Tipo de índices  
 Puede crear los siguientes tipos de índices personalizados para cada entidad.  
  
-   Índice único  
  
-   Índice no único  
  
 Un índice único garantiza que la columna indizada no contenga valores duplicados. En los índices únicos compuestos, el índice garantiza que cada combinación de valores de la lista de atributos seleccionados sea única. No se puede crear un índice único si existen valores duplicados para los atributos seleccionados.  
  
## <a name="rules"></a>Reglas  
 Las reglas siguientes se aplican a los índices personalizados, tanto únicos como no únicos.  
  
-   Para crear un índice personalizado, asegúrese de seleccionar al menos un atributo.  
  
-   Si intenta guardar un índice que tenga la misma lista de atributos y marca de unicidad que otro índice, no se podrá guardar. Aparece un error.  
  
    > [!NOTE]  
    >  MDS crea automáticamente índices para determinados atributos (como los DBA y el código). Esto significa que no se puede crear otro índice que contenga uno de estos atributos y ningún otro más.  
  
-   Los atributos se pueden incluir en más de un índice personalizado siempre que haya al menos un atributo diferente en los otros índices. De lo contrario, los índices son iguales.  
  
-   Si crea un índice que contenga muchos atributos o atributos de gran tamaño y el tamaño total de los atributos seleccionados supera el tamaño de clave de índice máximo (900 bytes), no se puede guardar el índice.  
  
-   Se puede crear un índice personalizado en los atributos de miembro hoja, excluidos los atributos de archivo.  
  
-   Si quiere eliminar un atributo incluido en un índice personalizado, se aplica lo siguiente.  
  
    -   Si el índice se crea en un solo atributo (índice único), tanto el atributo como el índice se eliminarán.  
  
    -   Si el índice se crea en más de un atributo (índice compuesto), el atributo no se puede eliminar hasta que edite el índice.  
  
-   No se puede cambiar el tipo de un atributo incluido en un índice personalizado.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Creación de un índice|[Creación de un índice personalizado &#40;Master Data Services&#41;](../master-data-services/create-an-index-master-data-services.md)|  
|Editar y eliminar un índice|[Editar y eliminar un índice &#40;Master Data Services&#41;](../master-data-services/edit-and-delete-an-index-master-data-services.md)|  
  
  

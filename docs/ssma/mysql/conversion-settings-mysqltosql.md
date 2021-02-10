---
description: Configuración de conversión (MySQLToSQL)
title: Configuración de la conversión (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f551cf6e-1575-4206-9cca-975b5b43a6b8
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 42f7d0f36f0e0adfcf067ad0bbe55c5e2c169e9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068869"
---
# <a name="conversion-settings-mysqltosql"></a>Configuración de conversión (MySQLToSQL)
La pestaña **"configuración"** permite al usuario establecer la configuración de nivel de nodo. La pestaña estará disponible en los siguientes nodos de la metabase:  
  
-   Nodo de base de datos  
  
-   Categoría de funciones  
  
-   Nodo Function  
  
-   Categoría de tablas  
  
-   Nodo de tabla  
  
## <a name="specifications"></a>Especificaciones:  
La pestaña **configuración** tiene dos configuraciones de usuario, es decir,:  
  
1.  Conversión de funciones  
  
2.  Conversión de tabla  
  
Esta configuración estará disponible en función del tipo de nodo de la metabase. Por ejemplo, la configuración relacionada con la conversión de funciones no estará disponible en el nodo de tabla  
  
> [!NOTE]  
> -   Los cambios realizados por el usuario se guardarán en el área de trabajo del proyecto como un archivo de preferencias independiente.  
> -   La extensión de este archivo será **ccprefs**.  
  
1.  **Configuración de la conversión de funciones:**  
  
    1.  Esta pestaña contiene la opción **forzar conversión de función** . La opción puede tener uno de los cuatro valores siguientes:  
  
        -   Convertir según la configuración del proyecto [heredado]  
  
        -   Convertir siempre en una función  
  
        -   Convertir siempre en un procedimiento  
  
        -   Convertir según la configuración del proyecto  
  
    2.  En función de la configuración, la función se convertirá en una función o en un procedimiento almacenado.  
  
    3.  La configuración realizada por el usuario se guarda en el archivo de preferencias en cascada al hacer clic en el botón **aplicar** .  
  
2.  **Configuración de conversión de tabla:**  
  
    1.  Esta pestaña contiene la opción **' suprimir la generación de columnas auxiliares ROWID '** . La opción puede tener uno de los cuatro valores siguientes:  
  
        -   Convertir según la configuración del proyecto [heredado]  
  
        -   Sí  
  
        -   No  
  
        -   Convertir según la configuración del proyecto  
  
    2.  Si es **"Yes"**, esta configuración prohíbe la creación de la creación de columnas auxiliares ROWID en las tablas de destino.  
  
    3.  La configuración realizada por el usuario se guarda en el archivo de preferencias en cascada al hacer clic en el botón **aplicar** .  
  
## <a name="see-also"></a>Consulte también  
[Configuración del proyecto (conversión) (MySQL en SQL)](./project-settings-conversion-mysqltosql.md)  

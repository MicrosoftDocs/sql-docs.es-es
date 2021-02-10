---
description: Visualización de informes de casos de prueba (OracleToSQL)
title: Ver informes de casos de prueba (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8da14323-9dd6-4019-bf79-3e8b972a9bc0
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2666b73a4ce335504352c2a838ab31ce13838350
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068760"
---
# <a name="viewing-test-case-reports-oracletosql"></a>Visualización de informes de casos de prueba (OracleToSQL)
El informe caso de prueba muestra los resultados de la comprobación de la prueba y la información general de las pruebas. En caso de que se produzca un error en la prueba, también se muestra información sobre los datos que no coinciden en los objetos comprobados.  
  
## <a name="report-structure"></a>Estructura del informe  
En la parte superior del informe se muestran estas estadísticas:  
  
-   El número total de objetos probados y el número de objetos para los que la prueba se realizó correctamente.  
  
-   El número total de tablas comprobadas y claves externas, y el número de tablas y claves externas que coinciden correctamente.  
  
-   La hora de inicio, la hora de finalización del caso de prueba y el tiempo total necesario para la ejecución.  
  
El resto del informe muestra información en cuatro categorías:  
  
**Errores de requisitos previos**  
Muestra los errores que se produjeron en el **paso de requisitos previos.** Normalmente, se omite.  
  
**Inicialización**  
Muestra el estado de ejecución como **correcto** o **error**.  
  
**Resultado de objetos de prueba**  
Comparación de los resultados (éxito o error) y las discrepancias que ha detectado SSMA Tester en caso de error.  
  
**Finalización**  
Muestra el estado de ejecución como **correcto** o **error**.  
  
## <a name="see-also"></a>Consulte también  
[Ejecutar casos de prueba &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Probar objetos de base de datos migrados &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  

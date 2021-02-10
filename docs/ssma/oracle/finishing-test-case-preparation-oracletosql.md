---
description: Finalización de la preparación del caso de prueba (OracleToSQL)
title: Finalizando preparación del caso de prueba (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 32f38713-7ae4-48d3-980d-74cadc8545a0
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: f1d0f99987a58a8d942c9d7601124c3cfc39b09d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014626"
---
# <a name="finishing-test-case-preparation-oracletosql"></a>Finalización de la preparación del caso de prueba (OracleToSQL)
La página final del asistente muestra la descripción del caso de prueba e información sobre los objetos implicados en la prueba. Además, en esta página puede establecer las opciones de ejecución de pruebas.  
  
La sección **información del caso de prueba** muestra el nombre y la descripción del caso de prueba.  
  
La sección **objetos seleccionados para probar** contiene la lista con nombre de objetos probados agrupados por tipo de objeto.  
  
La sección **objetos afectados por la prueba que se va a analizar** muestra la lista con nombre de los objetos que se deben comparar después de la ejecución de los objetos probados.  
  
## <a name="test-case-settings"></a>Configuración del caso de prueba  
En la sección **configuración de casos de prueba** puede establecer las siguientes opciones de prueba de ejecución:  
  
### <a name="stop-test-execution-after-first-failure"></a>Detener la ejecución de pruebas después del primer error  
Especifica que se interrumpa la prueba si se produce un error durante la ejecución de la prueba.  
  
-   Si elige **sí**, la ejecución de prueba se interrumpe si se produce un error.  
  
-   Si elige **no**, la ejecución de la prueba continúa después de un error.  
  
### <a name="perform-data-rollback"></a>Realizar la reversión de los datos  
Habilita la reversión automática de datos después de la ejecución de pruebas.  
  
-   Si elige **sí**, los cambios de datos se perderán después de la ejecución de la prueba.  
  
-   Si elige **no**, se guardarán todos los datos de ejecución de pruebas.  
  
### <a name="auxiliary-tables-saving-mode"></a>Modo de guardado de tablas auxiliares  
Define el modo de guardado de las tablas auxiliares creadas durante la ejecución de la prueba. Vea la descripción de las tablas auxiliares en los [casos de prueba en ejecución &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md) tema.  
  
-   Si selecciona **Guardar siempre**, los datos de la tabla auxiliar siempre se almacenarán para su uso posterior.  
  
-   Si selecciona **Guardar si se** produce un error en la comparación de tablas, los datos de la tabla auxiliar se almacenarán solo si se produce un error.  
  
-   Si selecciona **eliminar siempre**, las tablas auxiliares siempre se eliminarán después de la ejecución de la prueba.  
  
-   Si selecciona **preguntar al usuario si se** produce un error en la comparación de tablas, el usuario puede seleccionar la acción necesaria si se produce un error.  
  
Haga clic en el botón **Finalizar** para guardar el caso de prueba preparado en [mediante repositorios de prueba (OracleToSQL)](./using-test-repositories-oracletosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Uso de repositorios de prueba &#40;OracleToSQL&#41;](../../ssma/oracle/using-test-repositories-oracletosql.md)  
[Ejecutar casos de prueba &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Probar objetos de base de datos migrados &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  

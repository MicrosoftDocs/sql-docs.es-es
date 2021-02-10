---
description: Uso de repositorios de prueba (SybaseToSQL)
title: Uso de repositorios de prueba (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Test Repositories
ms.assetid: c359c25c-db2a-4a20-afa9-62d87a62df72
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 05e91905930b011976f61b149abeeaccc1d00398
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023178"
---
# <a name="using-test-repositories-sybasetosql"></a>Uso de repositorios de prueba (SybaseToSQL)
El repositorio de prueba de SSMA almacena los casos de prueba y los resultados de pruebas del evaluador de SSMA para su uso posterior. Los datos del repositorio se guardan en las tablas SQL Server **TestCaseRepository** y **RunTestCaseResultRepository** del esquema **ssma_sybase_utilities** de **ssmatesterdb_syb** base de datos.  
  
Los siguientes botones están disponibles en el cuadro de diálogo repositorio de casos de prueba:  
  
-   Haga clic en el botón **Actualizar** para actualizar los casos de prueba o la lista de resultados de pruebas.  
  
-   Haga clic en el botón **cerrar** para cerrar el cuadro de diálogo repositorio de casos de prueba.  
  
## <a name="test-cases-repository"></a>Repositorio de casos de prueba  
Para ver el repositorio de casos de prueba, haga clic en **casos de prueba...** en el menú de **evaluador** . SSMA muestra el **repositorio de la ventana de diálogo de casos de prueba** con una lista de casos de prueba guardados en la página **casos de prueba** .  
  
La cuadrícula muestra la siguiente información sobre cada caso de prueba:  
  
-   Nombre: el nombre del caso de prueba.  
  
-   Creado: la fecha de creación del caso de prueba.  
  
-   Modificado: fecha de última modificación del caso de prueba.  
  
-   Descripción: las descripciones de los casos de prueba.  
  
Los siguientes botones están disponibles en la página casos de prueba:  
  
-   Haga clic en el botón **Agregar** para ejecutar el Asistente para casos de prueba y crear una nueva prueba.  
  
-   Haga clic en el botón **quitar** para eliminar la prueba seleccionada del repositorio. Cuando se elimina un caso de prueba, también se eliminan todos los Resultados de pruebas relacionados.  
  
-   Haga clic en el botón **Editar** para ejecutar el Asistente para casos de prueba y cambiar la prueba seleccionada.  
  
-   Haga clic en el botón **Ejecutar** para abrir los [casos de prueba en ejecución &#40;](../../ssma/sybase/running-test-cases-sybasetosql.md) cuadro de diálogo&#41;SybaseToSQL y ejecutar la prueba seleccionada.  
  
## <a name="test-results-repository"></a>Resultados de pruebas repositorio  
Puede ver el repositorio de Resultados de pruebas en la página **resultados de pruebas** de la ventana **repositorio de casos de prueba** . Ábralo haciendo clic en **resultados de pruebas...** en el menú de **evaluador** .  
  
Puede usar dos filtros en **resultados de pruebas** página:  
  
-   El filtro de nombre de caso de prueba: permite elegir resultados de pruebas por nombre de caso de prueba. El valor de **todos los** casos de prueba de este filtro permite mostrar los resultados de pruebas para todos los casos de prueba.  
  
-   El filtro fecha de ejecución del caso de prueba: filtra los resultados de la prueba por la fecha de guardado. El valor de **todos los períodos** de este filtro permite mostrar los resultados de las pruebas para cualquier fecha de guardado.  
  
En la cuadrícula se muestra la siguiente información sobre los resultados de las pruebas.  
  
-   Nombre: nombre del caso de prueba.  
  
-   Iniciado: fecha del caso de prueba de en ejecución.  
  
-   Resultado: un breve resumen de la ejecución de la prueba (la información sobre herramientas de esta celda muestra un resumen completo de la ejecución de la prueba).  
  
Los siguientes botones están disponibles en la página resultado de la prueba:  
  
-   Haga clic en el botón **Ver** para abrir [ver informes de casos de prueba &#40;SybaseToSQL&#41;](../../ssma/sybase/viewing-test-case-reports-sybasetosql.md) del resultado del caso de prueba actual.  
  
-   Haga clic en el botón **eliminar** para eliminar el resultado de la prueba seleccionada.  
  
## <a name="see-also"></a>Consulte también  
[Ejecutar casos de prueba &#40;SybaseToSQL&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md)  
[Probar objetos de base de datos migrados &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  

---
description: Ejecución de casos de prueba (OracleToSQL)
title: Ejecutar casos de prueba (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: fc208cdb-7373-4f6b-8f6c-cdff9d3dcd02
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: bc811575dd845be43aef7a9398d8b4fb3458db1b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067720"
---
# <a name="running-test-cases-oracletosql"></a>Ejecución de casos de prueba (OracleToSQL)
Cuando SSMA Tester ejecuta un caso de prueba, ejecuta los objetos seleccionados para las pruebas y crea un informe sobre los resultados de la comprobación. Si los resultados son idénticos en ambas plataformas, la prueba se realizó correctamente. La correspondencia de objetos entre Oracle y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se determina según la configuración de asignación de esquemas del proyecto SSMA actual.  
  
Un requisito necesario para una prueba correcta es que todos los objetos de Oracle se convierten y se cargan en la base de datos de destino. Además, los datos de la tabla se deben migrar para que el contenido de las tablas en ambas plataformas esté sincronizado.  
  
## <a name="run-test-case"></a>Ejecutar caso de prueba  
Para ejecutar el caso de prueba preparado:  
  
1.  Haga clic en el botón **Ejecutar**.  
  
2.  En el cuadro de diálogo **conectar con Oracle** , escriba la información de conexión y, a continuación, haga clic en **conectar**.  
  
Una vez finalizada la prueba, se crea el informe de casos de prueba. Haga clic en el botón **Informe** para ver el [Informe de casos de prueba](viewing-test-case-reports-oracletosql.md). El resultado de la prueba (informe de casos de prueba) se almacena automáticamente en el [repositorio resultados de pruebas](using-test-repositories-oracletosql.md) para su uso posterior.  
  
## <a name="test-case-execution-steps"></a>Pasos de ejecución del caso de prueba  
  
### <a name="prerequisites"></a>Prerrequisitos  
SSMA Tester comprueba si se cumplen todos los requisitos previos para la ejecución de pruebas antes de iniciar la prueba. Si no se cumplen algunas condiciones, aparece un mensaje de error.  
  
### <a name="initialization"></a>Inicialización  
En este paso, el evaluador de SSMA crea objetos auxiliares (tablas, desencadenadores y vistas) en el esquema de SSMATESTER_ORACLE del servidor de Oracle. Permiten realizar cambios en el seguimiento de los objetos afectados elegidos para la comprobación.  
  
Supongamos que la tabla comprobada se denomina USER_TABLE. Para este tipo de tabla, se crean los siguientes objetos auxiliares en Oracle.  
  
|Nombre|Tipo|Descripción|  
|-|-|-|  
|USER_TABLE $ Trg|desencadenador|Desencadenador de auditoría de los cambios en la tabla comprobada.|  
|USER_TABLE $ AUD|table|Tabla donde se guardan las filas eliminadas y sobrescritas.|  
|USER_TABLE $ AUDID|table|Tabla donde se guardan las filas nuevas y modificadas.|  
|USER_TABLE|ver|Representación simplificada de las modificaciones de la tabla.|  
|USER_TABLE $ NEW|ver|Representación simplificada de filas insertadas y sobrescritas.|  
|USER_TABLE $ NEW_ID|ver|Identificación de las filas insertadas y modificadas.|  
|USER_TABLE $ OLD|ver|Representación simplificada de filas eliminadas y reemplazadas.|  
  
El siguiente objeto se crea en el esquema de la tabla comprobado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Nombre|Tipo|Descripción|  
|-|-|-|  
|USER_TABLE $ Trg|desencadenador|Desencadenador de auditoría de los cambios en la tabla comprobada.|  
  
Y los siguientes objetos se crean en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de datos ssmatesterdb.  
  
|Nombre|Tipo|Descripción|  
|-|-|-|  
|USER_TABLE $ AUD|table|Tabla donde se guardan las filas eliminadas y sobrescritas.|  
|USER_TABLE $ AudID|table|Tabla donde se guardan las filas nuevas y modificadas.|  
|USER_TABLE|ver|Representación simplificada de las modificaciones de la tabla.|  
|USER_TABLE $ New|ver|Representación simplificada de filas insertadas y sobrescritas.|  
|USER_TABLE $ new_id|ver|Identificación de las filas insertadas y modificadas.|  
|USER_TABLE $ Old|ver|Representación simplificada de filas eliminadas y reemplazadas.|  
  
### <a name="test-object-calls"></a>Llamadas a objetos de prueba  
En este paso, SSMA Tester invoca cada objeto seleccionado para las pruebas, compara los resultados y muestra el informe.  
  
### <a name="finalization"></a>Finalización  
Durante la finalización, el evaluador de SSMA limpia los objetos auxiliares creados en el paso de **inicialización** .  
  
## <a name="next-step"></a>siguiente paso  
[Ver informes de casos de prueba &#40;OracleToSQL&#41;](../../ssma/oracle/viewing-test-case-reports-oracletosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Seleccionar y configurar objetos para probar &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-objects-to-test-oracletosql.md)  
[Seleccionar y configurar objetos afectados &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-affected-objects-oracletosql.md)  
[Probar objetos de base de datos migrados &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  

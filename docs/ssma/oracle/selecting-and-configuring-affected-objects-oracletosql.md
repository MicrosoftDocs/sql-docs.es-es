---
description: Selección y configuración de objetos afectados (OracleToSQL)
title: Selección y configuración de objetos afectados (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Columns Comparison Settings
- Selection of Affected Objects
ms.assetid: 545eeda2-9829-4187-a858-619a96b4b71d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: b8256909a2ebc460da2dd2db826e809dd6dfb50c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067630"
---
# <a name="selecting-and-configuring-affected-objects-oracletosql"></a>Selección y configuración de objetos afectados (OracleToSQL)
En esta página puede seleccionar las tablas y las claves externas, los cambios en los que debe compararse cuando SSMA comprueba los resultados de la ejecución de los objetos elegidos en el paso anterior. Además, puede personalizar los parámetros de comprobación.  
  
## <a name="selection-of-affected-objects"></a>Selección de los objetos afectados  
En el árbol de objetos de Oracle que se encuentra en el lado izquierdo de la ventana, compruebe las tablas y las claves externas, los cambios en los que se debe comparar para ser idénticos.  
  
Si SSMA Tester no puede comprobar ninguno de estos objetos, verá el vínculo con la etiqueta **algunos objetos seleccionados que contienen errores** en el árbol de objetos. Haga clic en este vínculo para ver los motivos por los que estos objetos no se pueden comparar y borrar la selección de objetos incorrectos.  
  
## <a name="table"></a>Tabla  
La pestaña tabla contiene la vista de cuadrícula de la tabla seleccionada. La cuadrícula contiene la siguiente información acerca de la tabla seleccionada:  
  
-   Nombre de columna  
  
-   Tipo de datos  
  
-   Precisión  
  
-   Escala  
  
-   Regla  
  
-   Valor predeterminado  
  
-   Identidad  
  
-   Nullable  
  
## <a name="sql"></a>Sql  
La pestaña SQL contiene el SQL "crear tabla" de la tabla seleccionada.  
  
## <a name="data"></a>data  
Pestaña datos muestra los datos presentes en la tabla seleccionada.  
  
## <a name="properties"></a>Propiedades  
Pestaña propiedades muestra las propiedades de la tabla seleccionada. Los campos siguientes están presentes en la pestaña propiedades:  
  
-   Creado o modificado por última vez  
  
-   Nombre de objeto  
  
## <a name="columns-comparison-settings"></a>Configuración de comparación de columnas  
Establezca las reglas de comparación para las columnas de la tabla en la página de **comparación de columnas** . Puede establecer la siguiente configuración.  
  
### <a name="use-during-test-comparisons"></a>Usar durante las comparaciones de pruebas  
Determine si esta columna participará en la comprobación de los resultados de pruebas.  
  
-   Si elige **true**, SSMA comparará el contenido de esta columna después de ejecutar la prueba en Oracle con el contenido de la columna en SQL Server. 
  
-   Si elige **false**, la columna se excluirá de la comprobación de los resultados.  
  
### <a name="use-custom-scale"></a>Usar escalado personalizado  
En el caso de las columnas de tipo de datos numérico, puede establecer una escala personalizada para la comparación.  
  
-   Si elige **true**, los valores numéricos se redondearán según el valor de **escala de comparación** antes de compararlos.  
  
-   Si elige **false**, la comparación numérica será exacta.  
  
### <a name="comparing-scale"></a>Comparar escala  
  
-   Solo está disponible si la opción **usar escalado personalizado** está establecida en **true**. Esta es la precisión de la comparación numérica.  
  
### <a name="date-time-comparing"></a>Fecha y hora de comparación  
Define cómo se comparan los valores de fecha y hora.  
  
-   Si selecciona **comparar fecha completa**, se realizará una comparación completa de los valores de ambas plataformas.  
  
-   Si selecciona **solo comparar fecha**, se omitirá la parte de hora.  
  
-   Si selecciona **solo comparar hora**, se omitirá la parte de la fecha.  
  
-   Si selecciona **omitir milisegundos**, los resultados se compararán hasta segundos.  
  
-   Si selecciona **omitir fecha y milisegundos**, el resultado se comparará solo por parte de la hora y omitirá las fracciones de segundo.  
  
### <a name="ignore-strings-case"></a>Omitir mayúsculas de cadenas  
Controla la distinción de mayúsculas y minúsculas de la comparación.  
  
-   Si elige **true**, la comparación no distinguirá mayúsculas de minúsculas.  
  
-   Si elige **false**, la comparación tendrá en cuenta las mayúsculas y minúsculas de la letra.  
  
## <a name="comparing-sql"></a>Comparar SQL  
Puede ver las instrucciones SELECT generadas por SSMA Tester en la página **comparar SQL** . El evaluador comparará los conjuntos de resultados de estas instrucciones fila a fila. Cada fila siguiente de un conjunto de resultados de Oracle debe ser igual a la siguiente fila del conjunto de resultados generado en SQL Server.
  
Puede editar esas instrucciones SELECT para proporcionar una comprobación personalizada. Para guardar los cambios en Oracle y en las instrucciones de SQL Server, use los botones **aplicar** en el SQL de origen y en el de destino, según corresponda.  
  
## <a name="next-step"></a>siguiente paso  
[Personalizar el orden de llamadas &#40;OracleToSQL&#41;](../../ssma/oracle/customizing-calls-order-oracletosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Finalizando la preparación del caso de prueba &#40;OracleToSQL&#41;](../../ssma/oracle/finishing-test-case-preparation-oracletosql.md)  
[Ejecutar casos de prueba &#40;OracleToSQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Probar objetos de base de datos migrados &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  

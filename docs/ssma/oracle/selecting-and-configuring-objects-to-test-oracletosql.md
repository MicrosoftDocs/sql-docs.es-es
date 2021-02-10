---
description: Selección y configuración de objetos de prueba (OracleToSQL)
title: Seleccionar y configurar los objetos que se van a probar (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Selection of Objects to Test,Parameter Comparison Settings
ms.assetid: 29fb6542-5c1f-4b14-a822-87700beb7623
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 2cf0aab82f56740dfe73476af89899fd9fd135f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067617"
---
# <a name="selecting-and-configuring-objects-to-test-oracletosql"></a>Selección y configuración de objetos de prueba (OracleToSQL)
En este paso se seleccionan los objetos que se van a probar y se configuran los valores para comparar los procedimientos y los parámetros de salida de las funciones, así como los valores devueltos de las funciones.  
  
## <a name="selection-of-objects-to-test"></a>Selección de objetos que se van a probar  
En el árbol de objetos de Oracle que se encuentra en el lado izquierdo de la ventana, compruebe los objetos que desea invocar durante el proceso de prueba. Consulte la lista completa de objetos que se han comprobado en el tema Testing remissioned [Database objects &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md) .  
  
Si SSMA Tester no es compatible con ninguno de los objetos seleccionados para las pruebas, verá que el vínculo con la etiqueta **algunos objetos seleccionados contiene errores** en el árbol de objetos. Haga clic en este vínculo para ver los motivos por los que estos objetos no se pueden probar y borrar la selección de objetos incorrectos.  
  
En el lado derecho, puede ver varias páginas. en la página **SQL** se muestra la definición del objeto actual. En la página **parámetros** se muestran los parámetros si el objeto es un procedimiento almacenado o una función. En la página **propiedades** se muestran características adicionales del objeto. Vea las páginas Descripción de las **comparaciones de parámetros** y **valores de llamada** siguientes.  
  
## <a name="parameter-comparison-settings"></a>Configuración de comparación de parámetros  
Establezca las reglas de comparación para los parámetros de salida y los valores devueltos en la página **comparaciones de parámetros** . Puede establecer la siguiente configuración.  
  
### <a name="use-during-test-comparisons"></a>Usar durante las comparaciones de pruebas  
Habilita el uso del parámetro seleccionado en la comparación de resultados de pruebas.  
  
-   Si elige **true**, SSMA comparará el valor de salida de este parámetro después de ejecutar el procedimiento en Oracle con el valor correspondiente en SQL Server.
  
-   Si elige **false**, el parámetro se excluirá de la comprobación de los resultados.  
  
### <a name="use-custom-scale"></a>Usar escalado personalizado  
En el caso de los parámetros de tipo de datos numéricos, puede establecer una escala personalizada para la comparación.  
  
-   Si elige **true**, los valores numéricos se redondearán según el valor de **escala de comparación** antes de compararlos.  
  
-   Si elige **false**, la comparación numérica será exacta.  
  
### <a name="comparing-scale"></a>Comparar escala  
Solo está disponible si la opción **usar escalado personalizado** está establecida en **true**. Esta es la precisión de la comparación numérica.  
  
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
  
-   Si elige **false**, la comparación distinguirá mayúsculas de minúsculas.  
  
### <a name="ignore-trailing-spaces"></a>Omitir espacios finales  
Controla cómo se tratan los espacios finales durante la comparación.  
  
-   Si elige **true**, las cadenas comparadas se recortarán a la derecha antes de la comparación.  
  
-   Si elige **false**, las cadenas comparadas conservarán el espacio en blanco final.  
  
## <a name="specify-input-values-for-procedures-and-functions-call-values"></a>Especificar valores de entrada para procedimientos y funciones (valores de llamada)  
Puede especificar valores de parámetro de entrada en la página **valores de llamada** . El botón **Agregar llamada** agrega una nueva llamada con valores de parámetro vacíos. El botón **quitar llamadas** quita la llamada actual.  
  
## <a name="next-step"></a>siguiente paso  
[Seleccionar y configurar objetos afectados &#40;OracleToSQL&#41;](../../ssma/oracle/selecting-and-configuring-affected-objects-oracletosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Probar objetos de base de datos migrados &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  

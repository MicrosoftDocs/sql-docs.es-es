---
description: Selección y configuración de objetos de prueba (SybaseToSQL)
title: Seleccionar y configurar los objetos que se van a probar (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Parameter Comparision Setting
- Tester Component,Selecting Objects
ms.assetid: 89c23aad-bfee-4917-bc16-175288390ac0
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a43c4d8b1448949725820d69c6489aff26770d98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060585"
---
# <a name="selecting-and-configuring-objects-to-test-sybasetosql"></a>Selección y configuración de objetos de prueba (SybaseToSQL)
En este paso se seleccionan los objetos que se van a probar y se configuran los valores para comparar los procedimientos y los parámetros de salida de las funciones, así como los valores devueltos de las funciones.  
  
## <a name="selection-of-objects-to-test"></a>Selección de objetos que se van a probar  
En el árbol de objetos de Sybase situado en el lado izquierdo de la ventana, compruebe los objetos que desea invocar durante el proceso de prueba. Consulte la lista completa de objetos que se han comprobado en el tema Testing remissioned [Database objects &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md) .  
  
Si SSMA Tester no es compatible con ninguno de los objetos seleccionados para las pruebas, verá que el vínculo con la etiqueta **algunos objetos seleccionados contiene errores** en el árbol de objetos. Haga clic en este vínculo para ver los motivos por los que estos objetos no se pueden probar y borrar la selección de objetos incorrectos.  
  
En el lado derecho, puede ver varias páginas. en la página **SQL** se muestra la definición del objeto actual. En las páginas **anteriores** a SQL y **post SQL** pueden especificar los scripts que se ejecutan antes y después de que se inicie la invocación del objeto de prueba. Esto puede ser útil cuando el objeto requiere un objeto adicional, tales como las tablas temporales o los cursores. En la página **parámetros** se muestran los parámetros si el objeto es un procedimiento almacenado o una función. En la página **propiedades** se muestran características adicionales del objeto. Vea la descripción de los **parámetros** comparaciones y **valores de llamada** en las páginas siguientes.  
  
## <a name="parameter-comparison-settings"></a>Configuración de comparación de parámetros  
Establezca las reglas de comparación para los parámetros de salida y los valores devueltos en la página de **comparación de parámetros** . Puede establecer la siguiente configuración.  
  
### <a name="use-during-comparisons"></a>Usar durante las comparaciones  
Habilita el uso del parámetro seleccionado en la comparación de resultados de pruebas.  
  
-   Si elige **true**, SSMA comparará el valor de salida de este parámetro después de ejecutar el procedimiento en Sybase con el valor correspondiente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Si elige **false**, el parámetro se excluirá de la comprobación de los resultados.  
  
### <a name="use-custom-scale"></a>Usar escalado personalizado  
Para los parámetros de tipo de datos numéricos aproximados y de longitud fija, puede establecer una escala personalizada para la comparación.  
  
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
Puede especificar valores de parámetro de entrada en la página **valores de llamada** . El botón **Agregar llamada** agrega una nueva llamada con valores de parámetro vacíos. El botón **quitar llamada** quita la llamada actual.  
  
## <a name="next-step"></a>siguiente paso  
[Seleccionar y configurar objetos afectados &#40;SybaseToSQL&#41;](../../ssma/sybase/selecting-and-configuring-affected-objects-sybasetosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Probar objetos de base de datos migrados &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  

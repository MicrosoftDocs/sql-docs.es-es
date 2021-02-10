---
description: Evaluación de esquemas de Oracle para la conversión (OracleToSQL)
title: Evaluar los esquemas de Oracle para la conversión (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Analyzing Conversion Problems
ms.assetid: 4de9bcf6-1346-4740-87f9-7f24a8226357
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 1ac1ea4b1193fbf2a65d948983c071aad817ffbd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058970"
---
# <a name="assessing-oracle-schemas-for-conversion-oracletosql"></a>Evaluación de esquemas de Oracle para la conversión (OracleToSQL)
Antes de cargar objetos y migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe determinar la complejidad de la migración y el tiempo que tardará la migración. SSMA puede crear un informe de evaluación que muestre el porcentaje de objetos que se convertirán correctamente. SSMA también le permite ver los problemas específicos que causan errores de conversión.  
  
## <a name="creating-assessment-reports"></a>Creación de informes de evaluación  
Cuando crea este informe de evaluación, SSMA convierte los objetos de base de datos de Oracle seleccionados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis y, a continuación, muestra los resultados.  
  
**Para crear un informe de evaluación**  
  
1.  En el explorador de metadatos de Oracle, seleccione los esquemas que se van a evaluar.  
  
2.  Para omitir los objetos individuales, desactive las casillas situadas junto a ellos.  
  
3.  Haga clic con el botón derecho en **esquemas** y seleccione **crear informe**.  
  
    También puede analizar objetos individuales haciendo clic con el botón secundario en un objeto y, a continuación, seleccionando **crear informe**.  
  
    SSMA mostrará el progreso en la barra de estado en la parte inferior de la ventana. Si el panel de salida está visible, también verá mensajes en el panel de salida.  
  
    Una vez completada la evaluación, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aparecerá la ventana de informe Migration Assistant de Oracle: evaluación.  
  
## <a name="using-assessment-reports"></a>Uso de informes de evaluación  
La ventana Informe de evaluación contiene tres paneles:  
  
-   El panel izquierdo contiene la jerarquía de objetos que se incluyen en el informe de evaluación. Puede examinar la jerarquía y seleccionar objetos y categorías de objetos para ver las estadísticas y el código de la conversión.  
  
-   El contenido del panel derecho depende del elemento seleccionado en el panel izquierdo.  
  
    Si se selecciona un grupo de objetos, tal esquema o si se selecciona una tabla, el panel derecho contiene un panel de estadísticas de conversión y un panel de objetos por categorías. En el panel de estadísticas de conversión se muestran las estadísticas de conversión de los objetos seleccionados. El panel objetos por categorías muestra las estadísticas de conversión del objeto o categorías de objetos.  
  
    Si se selecciona una función, paquete, procedimiento, secuencia o vista, el panel derecho contiene las estadísticas, el código fuente y el código de destino.  
  
    -   En el área superior se muestran las estadísticas generales del objeto. Es posible que tenga que expandir las **estadísticas** para ver esta información.  
  
    -   En el área de origen se muestra el código fuente del objeto seleccionado en el panel izquierdo. Las áreas resaltadas muestran código fuente problemático.  
  
    -   El área de destino muestra el código convertido. El texto rojo muestra código problemático y mensajes de error.  
  
-   En el panel inferior se muestran los mensajes de conversión, agrupados por número de mensaje. Puede hacer clic en **errores**, **advertencias** o **información** para ver las categorías de mensajes y, a continuación, expandir un grupo de mensajes. Haga clic en un mensaje individual para seleccionar el objeto en el panel izquierdo y mostrar los detalles en el panel derecho.  
  
## <a name="analyzing-conversion-problems-by-using-the-assessment-report"></a>Analizar problemas de conversión mediante el informe de evaluación  
En el panel de estadísticas de conversión se muestran las estadísticas de conversión. Si el porcentaje de cualquier categoría es inferior al 100 por ciento, debe determinar por qué la conversión no fue correcta.  
  
**Para ver los problemas de conversión**  
  
1.  Cree el informe de evaluación siguiendo las instrucciones del procedimiento anterior.  
  
2.  En el panel izquierdo, expanda esquemas o carpetas que tengan un icono de error rojo. Continúe expandiendo los elementos hasta que seleccione un elemento individual que no se pudo convertir.  
  
3.  En la parte superior del panel origen, haga clic en **siguiente problema**.  
  
    El código problemático está resaltado, al igual que el código relacionado en el panel de navegación de destino.  
  
4.  Revise los mensajes de error y, a continuación, determine qué desea hacer con el objeto que causó el problema de conversión:  
  
    -   Actualice la sintaxis de Oracle en SSMA. Puede actualizar la sintaxis de procedimientos, funciones, desencadenadores, funciones empaquetadas y procedimientos empaquetados. Para actualizar la sintaxis, seleccione el objeto en el panel Explorador de metadatos de Oracle, haga clic en la pestaña **SQL** y, a continuación, modifique el código SQL. Cuando salga del elemento, se le pedirá que guarde la sintaxis actualizada. Puede ver los errores notificados del objeto en la pestaña **Informe** .  
  
    -   En Oracle, puede modificar el objeto de Oracle para quitar o revisar el código problemático. Para cargar el código actualizado en SSMA, tendrá que actualizar los metadatos. Para obtener más información, vea [conectarse a Oracle Database &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-oracle-database-oracletosql.md).  
  
    -   Puede excluir el objeto de la migración. En el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos y en el explorador de metadatos de Oracle, desactive la casilla situada junto al elemento antes de cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y migrar datos de Oracle.  
  
## <a name="next-step"></a>siguiente paso  
[Convertir esquemas de Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/converting-oracle-schemas-oracletosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Oracle a SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  

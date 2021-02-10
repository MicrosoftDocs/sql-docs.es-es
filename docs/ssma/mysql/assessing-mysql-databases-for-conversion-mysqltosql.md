---
description: Evaluación de las bases de datos de MySQL para la conversión (MySQLToSQL)
title: Evaluación de las bases de datos de MySQL para la conversión (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Assessment reports
ms.assetid: 2a56a003-3b0f-453a-963c-00c9e40933ec
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 61e8cd99640b45cfdfd74de60beb2fcbd9393308
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069200"
---
# <a name="assessing-mysql-databases-for-conversion-mysqltosql"></a>Evaluación de las bases de datos de MySQL para la conversión (MySQLToSQL)
Antes de cargar objetos y migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, debe determinar la complejidad de la migración y el tiempo que tardará la migración. SSMA puede crear un informe de evaluación que muestre el porcentaje de objetos que se convertirán correctamente. SSMA también le permite ver los problemas específicos que causan errores de conversión.  
  
## <a name="creating-assessment-reports"></a>Creación de informes de evaluación  
Cuando crea este informe de evaluación, SSMA convierte los objetos de base de datos de MySQL seleccionados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Sintaxis o SQL Azure y, a continuación, muestra los resultados.  
  
**Para crear un informe de evaluación**  
  
1.  En el explorador de metadatos de MySQL, seleccione los esquemas que se van a evaluar.  
  
2.  Para omitir los objetos individuales, desactive las casillas situadas junto a ellos.  
  
3.  Haga clic con el botón derecho en **esquemas** y seleccione **crear informe**.  
  
    Haga clic con el botón secundario en un objeto para analizar objetos individuales. A continuación, seleccione **crear informe**.  
  
    SSMA mostrará el progreso en la barra de estado en la parte inferior de la ventana. Si el panel de salida está visible, también verá mensajes en el panel de salida.  
  
    Una vez completada la evaluación, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aparecerá la ventana Migration Assistant para MySQL, informe de evaluación.  
  
## <a name="using-assessment-reports"></a>Uso de informes de evaluación  
La ventana Informe de evaluación contiene tres paneles:  
  
-   El panel izquierdo contiene la jerarquía de objetos que se incluyen en el informe de evaluación. Puede examinar la jerarquía y seleccionar objetos y categorías de objetos para ver las estadísticas y el código de la conversión.  
  
-   El contenido del panel derecho depende del elemento seleccionado en el panel izquierdo.  
  
    Si se selecciona un grupo de objetos, como esquema, el panel derecho contiene un panel de estadísticas de conversión y el panel objetos por categorías. En el panel de estadísticas de conversión se muestran las estadísticas de conversión de los objetos seleccionados. El panel objetos por categorías muestra las estadísticas de conversión del objeto o categorías de objetos.  
  
    Si se selecciona una función, un procedimiento, una tabla o una vista, el panel derecho contiene las estadísticas, el código fuente y el código de destino.  
  
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
  
4.  Revise los mensajes de error y, a continuación, determine qué desea hacer con el objeto que causó el problema de conversión.  
  
-   Actualice la sintaxis de MySQL en SSMA. Solo puede actualizar la sintaxis de procedimientos y funciones. Para actualizar la sintaxis, seleccione el objeto en el panel Explorador de metadatos de MySQL, haga clic en la pestaña **SQL** y, a continuación, modifique el código SQL. Cuando salga del elemento, se le pedirá que guarde la sintaxis actualizada. Puede ver los errores notificados del objeto en la pestaña **Informe** .  
  
-   En MySQL, puede modificar el objeto MySQL para quitar o revisar el código problemático. Para cargar el código actualizado en SSMA, tendrá que actualizar los metadatos. Para obtener más información, consulte [conexión a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md).  
  
-   Puede excluir el objeto de la migración. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure el explorador de metadatos y el explorador de metadatos de MySQL, desactive la casilla situada junto al elemento antes de cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure y migrar datos de MySQL.  
  
## <a name="next-step"></a>siguiente paso  
[Conversión de bases de datos MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  

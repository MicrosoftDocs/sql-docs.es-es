---
description: Evaluación de objetos de base de datos de SAP ASE para la conversión (SybaseToSQL)
title: Evaluación de objetos de base de datos de SAP ASE para la conversión (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: eb996b7c-1eef-4f73-b5e6-2fa6faf7336c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e0926c350d02bf2cdbb5ae7fe4fdefbda3ef6863
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077226"
---
# <a name="assessing-sap-ase-database-objects-for-conversion-sybasetosql"></a>Evaluación de objetos de base de datos de SAP ASE para la conversión (SybaseToSQL)
Antes de cargar objetos y migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a Azure SQL, debe determinar la complejidad de la migración y el tiempo que debe tardar. SSMA puede crear un informe de evaluación que muestre el porcentaje de objetos y procedimientos que se convertirán correctamente en [!INCLUDE[tsql](../../includes/tsql-md.md)] . SSMA también le permite ver los problemas específicos que pueden producir errores de conversión.  
  
## <a name="create-assessment-reports"></a>Crear informes de evaluación  
Al crear este informe de evaluación, SSMA convierte los objetos de base de datos de SAP Adaptive Server Enterprise (ASE) seleccionados en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sintaxis SQL de Azure y, después, muestra los resultados.  
  
**Para crear un informe de evaluación**  
  
1.  En el explorador de metadatos de Sybase, seleccione las bases de datos que desea evaluar.  
  
2.  Para omitir objetos individuales, desactive las casillas situadas junto a los objetos que no desea evaluar.  
  
3.  Haga clic con el botón derecho en **bases de datos** y, a continuación, seleccione **crear informe**.  
  
    También puede analizar objetos individuales haciendo clic con el botón secundario en un objeto y, a continuación, seleccionando **crear informe**.  
  
    SSMA muestra el progreso en la barra de estado en la parte inferior de la ventana. Si el panel de salida está visible, también verá los mensajes relacionados.  
  
    Una vez completada la evaluación, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aparecerá la ventana de informe Migration Assistant de Sybase: Assessment.  
  
## <a name="use-assessment-reports"></a>Usar informes de evaluación  
La ventana Informe de evaluación contiene tres paneles:  
  
-   El panel izquierdo contiene la jerarquía de objetos que se incluyen en el informe de evaluación. Puede examinar la jerarquía y seleccionar objetos y categorías de objetos para ver las estadísticas y el código de la conversión.  
  
-   El contenido del panel derecho varía en función del elemento seleccionado en el panel izquierdo.  
  
    Si se selecciona un grupo de objetos (por ejemplo, un esquema) o una tabla, en el panel derecho se muestran dos paneles. En el panel de **estadísticas de conversión** se muestran las estadísticas de conversión de los objetos seleccionados. El panel **objetos por categorías** muestra las estadísticas de conversión del objeto o categorías de objetos.  
  
    Si se selecciona un procedimiento almacenado, una vista o un desencadenador, el panel derecho contiene las estadísticas, el código fuente y el código de destino.  
  
    -   En el área superior se muestran las estadísticas generales del objeto. Es posible que tenga que expandir las **estadísticas** para ver esta información. 
    -   En el área de origen se muestra el código fuente del objeto seleccionado en el panel izquierdo. Las áreas resaltadas muestran código fuente problemático.  
    -   El área de destino muestra el código convertido. El texto rojo muestra código problemático y mensajes de error.  
  
-   En el panel inferior se muestran los mensajes de conversión, agrupados por número de mensaje. Seleccione **errores**, **advertencias** o **información** para ver las categorías de mensajes y, a continuación, expanda un grupo de mensajes. Haga clic en un mensaje individual para seleccionar el objeto en el panel izquierdo y, a continuación, muestre los detalles en el panel derecho.  
  
## <a name="analyze-conversion-problems-by-using-the-assessment-report"></a>Analizar problemas de conversión mediante el informe de evaluación  
Los **paneles de estadísticas de conversión** muestran las estadísticas de conversión. Si el porcentaje de cualquier categoría es inferior al 100 por ciento, debe determinar por qué la conversión no fue correcta.  
  
**Para ver los problemas de conversión**  
  
1.  Cree el informe de evaluación siguiendo las instrucciones del procedimiento anterior.  
  
2.  En el panel izquierdo, expanda esquemas o carpetas que tengan un icono de error rojo. Continúe expandiendo los elementos hasta que seleccione un elemento individual que no se pudo convertir.  
  
3.  En la parte superior del panel origen, seleccione **problema siguiente**.  
    El código problemático está resaltado, al igual que el código relacionado en el panel de **navegación de destino** .  
  
4.  Revise los mensajes de error y, a continuación, determine qué desea hacer con el objeto que causó el problema de conversión:  
  
    -   Actualice la sintaxis de ASE en SSMA. Solo puede actualizar la sintaxis de los procedimientos almacenados y los desencadenadores. Para actualizar la sintaxis, seleccione el objeto en el panel Explorador de metadatos de Sybase, haga clic en la pestaña **SQL** y, a continuación, edite el código SQL. Cuando salga del elemento, se le pedirá que guarde la sintaxis actualizada. Vea los errores notificados para el objeto en la pestaña **Informe** .  
  
    -   En ASE, puede modificar el objeto ASE para quitar o revisar el código problemático. Para cargar el código actualizado en SSMA, tendrá que actualizar los metadatos. Para obtener más información, consulte [conexión a Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).  
  
    -   Puede excluir el objeto de la migración. En el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos de Azure SQL y el explorador de metadatos de Sybase, desactive la casilla situada junto al elemento antes de cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL y migrar datos desde ase.
  
## <a name="next-steps"></a>Pasos siguientes  
[Conversión de objetos de base de datos de SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de SAP ASE a SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  

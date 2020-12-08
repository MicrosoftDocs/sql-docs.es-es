---
title: Creación y prueba de guías de plan
titleSuffix: SQL Server Profiler
description: Use SQL Server Profiler para capturar el texto exacto de la consulta y usarlo en el argumento statement_text del procedimiento almacenado sp_create_plan_guide en SQL Server.
ms.custom: seo-dt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- checking plan guides
- plan guides [SQL Server], testing
- plan guides [SQL Server], SQL Server Profiler
- matching queries to plan guides [SQL Server]
- testing plan guides
- SQL Server Profiler, plan guides
- plan guides [SQL Server], creating
- capturing query text [SQL Server]
- verifying plan guides
- Profiler [SQL Server Profiler], plan guides
- query-to-plan guide matching [SQL Server]
ms.assetid: 7018dbf0-1a1a-411a-88af-327bedf9cfbd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ff2a27878acfcbcc9c5dda12533ee90cb6a8a171
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504929"
---
# <a name="use-sql-server-profiler-to-create-and-test-plan-guides"></a>Uso de SQL Server Profiler para crear y probar guías de plan
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Cuando cree una guía de plan, puede usar [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para capturar el texto exacto de la consulta con el fin de usarlo en el argumento *statement_text* del procedimiento almacenado **sp_create_plan_guide** . Esto contribuye a garantizar que la guía de plan coincidirá con la consulta en tiempo de compilación. Una vez creada la guía de plan, se puede utilizar también el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para probar que, realmente, esa guía de plan coincidirá con la consulta. Por lo general, debe probar las guías de plan mediante el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para comprobar que se buscan las coincidencias de la consulta con la guía de plan.  
  
## <a name="capturing-query-text-by-using-sql-server-profiler"></a>Captura de texto de consulta utilizando SQL Server Profiler  
 Si ejecuta una consulta y captura el texto exactamente como se envió a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizando el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], puede crear una guía de plan de tipo SQL o TEMPLATE que coincidirá exactamente con el texto de la consulta. Esto garantiza que el optimizador de consultas utiliza la guía de plan.  
  
 Considere la siguiente consulta que envía una aplicación como lote independiente:  
  
```  
SELECT COUNT(*) AS c  
FROM Sales.SalesOrderHeader AS h  
INNER JOIN Sales.SalesOrderDetail AS d  
  ON h.SalesOrderID = d.SalesOrderID  
WHERE h.OrderDate BETWEEN '20000101' and '20050101';  
```  
  
 Imagine que desea ejecutar esta consulta utilizando una operación de combinación de mezcla, pero SHOWPLAN indica que la consulta no utiliza una combinación de mezcla. No puede cambiar la consulta directamente en la aplicación; por tanto, en su lugar crea una guía de plan para especificar que se adjunte la sugerencia de consulta MERGE JOIN a la consulta en tiempo de compilación.  
  
 Para capturar el texto de la consulta exactamente como lo recibe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , siga estos pasos:  
  
1.  Inicie un seguimiento del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , asegurándose de que selecciona el tipo de evento **SQL:BatchStarting** .  
  
2.  Haga que la aplicación ejecute la consulta.  
  
3.  Pause el seguimiento del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
4.  Haga clic en el evento **SQL:BatchStarting** que corresponde a la consulta.  
  
5.  Haga clic con el botón derecho y seleccione **Extraer datos de evento**.  
  
    > [!IMPORTANT]  
    >  No intente copiar el texto del lote seleccionándolo del panel inferior de la ventana de Seguimiento de SQL Server Profiler. Esto puede dar lugar a que la guía de plan que cree no coincida con el lote original.  
  
6.  Guarde los datos del evento en un archivo. Éste es el texto del lote.  
  
7.  Abra el archivo de texto del lote en el Bloc de notas y copie el texto en el búfer de copiar y pegar.  
  
8.  Cree la guía de plan y pegue el texto copiado entre las comillas ( **''** ) especificadas para el argumento **\@stmt**. Debe utilizar un carácter de escape con las comillas simples en el argumento **\@stmt** utilizando delante de ellas otra comilla simple. Al insertar estas comillas simples, tenga cuidado de no agregar ni quitar ningún otro carácter. Por ejemplo, el literal de fecha **'** 20000101 **'** debe delimitarse como **''** 20000101 **''** .  
  
 Ésta es la guía de plan:  
  
```  
EXEC sp_create_plan_guide   
    @name = N'MyGuide1',  
    @stmt = N'<paste the text copied from the batch text file here>',  
    @type = N'SQL',  
    @module_or_batch = NULL,  
    @params = NULL,  
    @hints = N'OPTION (MERGE JOIN)';  
```  
  
## <a name="testing-plan-guides-by-using-sql-server-profiler"></a>Prueba de guías de plan utilizando SQL Server Profiler  
 Para comprobar que una guía de plan coincide con una consulta, siga estos pasos:  
  
1.  Inicie un seguimiento de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , asegurándose de seleccionar el tipo de evento **Showplan XML** (ubicado en el nodo **Performance** ).  
  
2.  Haga que la aplicación ejecute la consulta.  
  
3.  Pause el seguimiento del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  
  
4.  Busque el evento **Showplan XML** de la consulta afectada.  
  
    > [!NOTE]  
    >  No se puede utilizar el evento **Showplan XML for Query Compile** . **PlanGuideDB** no existe en ese evento.  
  
5.  Si la guía de plan es de tipo OBJECT o SQL, compruebe que el evento **Showplan XML** contiene los atributos **PlanGuideDB** y **PlanGuideName** para la guía de plan que espera que coincida con la consulta. O bien, en el caso de una guía de plan TEMPLATE, compruebe que el evento **Showplan XML** contiene los atributos **TemplatePlanGuideDB** y **TemplatePlanGuideName** de la guía de plan que se espera. Esto comprueba que la guía de plan funciona. Estos atributos se incluyen en el elemento del plan de **\<StmtSimple>** .  
  
## <a name="see-also"></a>Consulte también  
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
  
  

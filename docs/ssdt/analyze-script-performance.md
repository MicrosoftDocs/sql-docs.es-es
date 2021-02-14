---
title: Analizar el rendimiento de los scripts
description: Obtenga información sobre cómo las estadísticas de cliente, los planes de ejecución y Code Analysis de SQL Server Data Tools pueden ayudar a mejorar el rendimiento de las consultas, los procedimientos almacenados y los scripts.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f4bbdd31-12a5-4c57-b0fe-1c6683820f11
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5f8d17fa156cd144a87b2dc387564cd93556d8da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077196"
---
# <a name="analyze-script-performance"></a>Analizar el rendimiento de los scripts

Puede usar las herramientas proporcionadas por SQL Server Data Tools para determinar si puede mejorar el rendimiento de la consulta, los procedimientos almacenados o los scripts. Por ejemplo, supervisando estadísticas de cliente como los tiempos de respuesta a las consultas usadas con frecuencia, puede determinar si es necesario cambiar la consulta o los índices de las tablas. Esas estadísticas pueden incluir el tiempo de ejecución del cliente, el perfil de consulta y los paquetes/bytes enviados y recibidos.  
  
Además, algunos problemas de rendimiento se solucionan mejor mediante el análisis de las consultas y actualizaciones que la aplicación envía a la base de datos, y la forma en que estas consultas y actualizaciones interactúan con los datos y el esquema de la base de datos. Los planes de ejecución muestran gráficamente los métodos de recuperación de datos elegidos por el optimizador de consultas de SQL Server, y muestran el costo de ejecución de determinadas instrucciones y consultas. Por tanto, pueden ayudarle a entender cómo SQL Server procesa la consulta de SQL y a determinar la causa de la degradación del rendimiento.  
  
## <a name="using-client-statistics"></a>Usar estadísticas de cliente  
Cuando ejecuta un script o una consulta en el Editor de Transact\-SQL, puede elegir recopilar estadísticas de cliente como el perfil de aplicación, la red y estadísticas de tiempo para la ejecución. Esas métricas le permiten medir la eficiencia del script o realizar pruebas comparativas de distintos scripts.  
  
Para alternar la recopilación de estadísticas de cliente, cuando el Editor de Transact\-SQL esté abierto, en el menú **Datos**, seleccione **Editor de Transact\-SQL**, haga clic en **Configuración de ejecución** y, a continuación, haga clic en **Incluir estadísticas de cliente**. O bien, haga clic en el botón **Incluir estadísticas de cliente** (el quinto desde la derecha) de la barra de herramientas del Editor de Transact\-SQL o haga clic con el botón derecho en el Editor de Transact\-SQL y seleccione **Configuración de ejecución** e **Incluir estadísticas de cliente**. Tenga en cuenta que para recopilar estadísticas para una consulta, tiene que activar esta característica antes de ejecutarla.  
  
Si activó las estadísticas de cliente, aparecerá la pestaña **Estadísticas** junto a la pestaña **Mensaje** cuando se ejecute la consulta. Si ha desactivado las estadísticas de cliente, la pestaña **Estadísticas** no aparecerá. Las estadísticas de ejecuciones de consultas sucesivas se muestran junto con los valores promedio.  
  
Para obtener más información sobre las estadísticas recopiladas, vea [Consultar el panel de estadísticas de ventana](https://msdn.microsoft.com/library/aa216969(SQL.80).aspx) y la sección ["Pestaña Estadísticas de cliente" de este tema](/previous-versions/visualstudio/visual-studio-2010/aa833205(v=vs.100)).  
  
## <a name="using-execution-plans"></a>Usar planes de ejecución  
Los planes de ejecución muestran cómo el motor de base de datos navega por tablas y usa índices para obtener acceso a datos o procesarlos para una consulta u otra instrucción DML, como una actualización. Este enfoque gráfico resulta muy útil para comprender las características de rendimiento de una consulta.  
  
Abra o escriba un script de Transact\-SQL que contenga las consultas que desee analizar en el editor de consultas de Transact\-SQL. Puede resaltar el código que desea revisar y elegir que se muestre un plan de ejecución estimado haciendo clic en el botón **Mostrar plan de ejecución estimado** de la barra de herramientas del editor. Si hace clic en **Mostrar plan de ejecución estimado**, las consultas de Transact\-SQL o lotes no se ejecutarán. En su lugar, se analiza el script y se muestra el plan de ejecución de la consulta que el motor de base de datos usaría con más probabilidad si se ejecutaran realmente las consultas.  
  
Cuando el script se haya analizado o ejecutado, haga clic en la pestaña **Plan de ejecución** para ver una representación gráfica del mismo.  
  
La salida del plan de ejecución gráfico se lee de derecha a izquierda y de arriba abajo. Todas las consultas del lote se analizan y se presentan, incluido el costo de cada consulta como un porcentaje del costo total del lote. Para ver información adicional como el costo y la operación para cada paso, mantenga el puntero sobre los [iconos de operador lógico y físico](/previous-versions/sql/sql-server-2008-r2/ms175913(v=sql.105)) en el plan gráfico.  
  
Para cambiar la visualización del plan de ejecución, haga clic con el botón derecho en el **plan de ejecución** y seleccione **Acercar**, **Alejar**, **Zoom personalizado** o **Zoom para ajustar**. **Acercar** y **Alejar** permiten aumentar o reducir el plan de ejecución en incrementos fijos. **Zoom personalizado** le permite definir su propia ampliación de la visualización, como alejar hasta un 80 por ciento.  **Zoom para ajustar** ajusta el plan de ejecución para adaptarse al panel de resultados.  
  
Los planes de ejecución se pueden guardar y volver a abrir más adelante para su examen. Para ello, haga clic con el botón derecho en el **Plan de ejecución** y seleccione **Guardar plan de ejecución como**. Después, puede abrir el plan en Visual Studio como si abriera cualquier otro tipo de archivo.  
  
## <a name="using-code-analysis"></a>Usar Análisis de código  
Puede usar Análisis de código para detectar posibles problemas en los scripts, como problemas de diseño, nomenclatura y rendimiento.  Las reglas para los proyectos de base de datos están organizadas en conjuntos de reglas predefinidos dirigidos a determinadas áreas y puede habilitar o deshabilitar cualquier regla en la pestaña **Análisis de código** de la página **Propiedades del proyecto** . En la misma pestaña, puede especificar que el análisis de código se ejecute automáticamente cada vez que se compile un proyecto o si las advertencias deben tratarse como errores.  
  
Para usar Análisis de código manualmente, haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y seleccione **Ejecutar análisis de código**. Las advertencias de análisis de código se mostrarán en la ventana **Lista de errores** . Puede hacer doble clic en una advertencia para navegar hasta el código fuente que contiene el problema, y puede ver información adicional y posibles correcciones de una advertencia usando el menú contextual **Ayuda para Mostrar mensaje**.  
  
Para obtener más información sobre Análisis de código, vea [Analizar el código de base de datos para mejorar la calidad del código](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  

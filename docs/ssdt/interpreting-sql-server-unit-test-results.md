---
title: Interpretar los resultados de pruebas unitarias de SQL Server
description: Descubra cómo usar la ventana Resultados de pruebas o los archivos .trx para ver los resultados de las pruebas unitarias de SQL Server. Vea cómo obtener información detallada sobre los resultados.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: fde3c95b-2f68-483d-a197-0f7161b72fa3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 88fc08dcc00e64c4b107a99608b891a43f236d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017915"
---
# <a name="interpreting-sql-server-unit-test-results"></a>Interpretar los resultados de pruebas unitarias de SQL Server

Al ejecutar una prueba unitaria de SQL Server, los resultados de prueba se generan automáticamente, se guardan en el disco y se resumen en la ventana **Resultados de pruebas**. En cuanto se inicia una serie de pruebas, aparece la ventana **Resultados de pruebas**, que muestra el progreso de la serie de pruebas. Esta pantalla incluye las pruebas que se están ejecutando y las pruebas que se han completado.  
  
Para ver información detallada sobre un resultado de prueba, haga doble clic en la ventana **Resultados de pruebas** para mostrar la página **Detalles de resultados de la prueba**. Para obtener más información acerca de un resultado de pruebas, haga doble clic en él.  
  
Para más información sobre cómo cambiar la presentación de la ventana **Resultados de pruebas**, consulte [Cómo: Agregar o quitar columnas de las ventanas de pruebas (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182508(v=vs.100)) o [Cómo: Agregar o quitar columnas de las ventanas de pruebas (Visual Studio 2012)](/previous-versions/visualstudio/visual-studio-2012/ms182508(v=vs.110)).  
  
## <a name="storing-test-results"></a>Almacenar los resultados de pruebas  
Los resultados de las pruebas unitarias se almacenan en el disco duro en archivos con la extensión .trx. Un archivo .trx es un archivo XML que contiene los detalles de una serie de pruebas. Puede cargar los archivos .trx de series de pruebas anteriores para revisar los resultados de esas series de pruebas o volver a ejecutar pruebas anteriores. Para más información, vea: [Cómo: Vuelva a ejecutar una prueba (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182472(v=vs.100)).  
  
> [!NOTE]  
> No puede ejecutar pruebas unitarias de forma remota.  
  
Si el equipo está usando un proyecto de equipo de Visual Studio Team Foundation Server para ayudar a administrar su trabajo, también puede publicar los datos de prueba en una base de datos de SQL Server conocida como almacén operativo.  
  
Para más información sobre cómo guardar los resultados de pruebas, reutilizarlos y compartirlos con su equipo, consulte [Cómo: Guardar y abrir resultados de pruebas en Visual Studio 2010](/previous-versions/visualstudio/visual-studio-2010/ms404662(v=vs.100)) o [Cómo: Guardar y abrir resultados de pruebas en Visual Studio 2012](/previous-versions/ms404662(v=vs.140)).  
  
## <a name="see-also"></a>Consulte también  
[Ejecutar pruebas unitarias de SQL Server](../ssdt/running-sql-server-unit-tests.md)  

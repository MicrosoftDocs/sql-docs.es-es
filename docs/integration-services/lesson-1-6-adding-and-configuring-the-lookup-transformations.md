---
description: 'Lección 1-6: Adición y configuración de las transformaciones de búsqueda'
title: 'Paso 6: Adición y configuración de las transformaciones de búsqueda | Microsoft Docs'
ms.custom: ''
ms.date: 03/19/2019
ms.prod: sql
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5c59f723-9707-4407-80ae-f05f483cf65f
author: chugugrace
ms.author: chugu
ms.reviewer: ''
ms.openlocfilehash: 7c88bd5e8abafdb15eba50fa7cdea22f29910bc4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354318"
---
# <a name="lesson-1-6-add-and-configure-the-lookup-transformations"></a>Lección 1-6: Adición y configuración de las transformaciones de búsqueda

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Tras configurar el origen de archivo plano para extraer datos del archivo de origen, se definen las transformaciones de búsqueda necesarias para obtener los valores para **CurrencyKey** y **DateKey**. Una transformación Búsqueda realiza una búsqueda combinando datos de la columna de entrada especificada en una columna de un conjunto de datos de referencia. El conjunto de datos de referencia puede ser una tabla o una vista existente, una tabla nueva o el resultado de una instrucción SQL. En este tutorial, la transformación Búsqueda usa un administrador de conexiones OLE DB para conectarse a la base de datos que contiene los datos de origen del conjunto de datos de referencia.  
  
> [!NOTE]  
> También puede configurar la transformación de Búsqueda para conectar con una caché que contiene el conjunto de datos de referencia. Para más información, vea [Transformación de búsqueda](../integration-services/data-flow/transformations/lookup-transformation.md).  
  
En esta tarea, agregará y configurará los dos componentes de la transformación Búsqueda siguientes al paquete:  
  
-   Una transformación para realizar una búsqueda de valores de la columna **CurrencyKey** de la tabla de dimensiones **DimCurrency**, en función de la coincidencia de los valores de la columna **CurrencyID** del archivo plano.  
  
-   Una transformación para realizar una búsqueda de valores de la columna **DateKey** de la tabla de dimensiones **DimDate**, en función de la coincidencia de los valores de la columna **CurrencyDate** del archivo plano.  
  
En ambos casos, en la transformación de búsqueda se usa el administrador de conexiones OLE DB creado anteriormente.  
  
## <a name="add-and-configure-the-lookup-currency-key-transformation"></a>Adición y configuración de la transformación Lookup Currency Key  
  
1.  En el **cuadro de herramientas de SSIS**, expanda **Comunes** y arrastre **Búsqueda** a la superficie de diseño de la pestaña **Flujo de datos** . Coloque **Búsqueda** directamente bajo el origen **Extract Sample Currency Data**.  
  
2.  Haga clic en el origen de archivo plano **Extract Sample Currency Data** y arrastre la flecha de color azul a la transformación **Búsqueda** recién agregada para conectar los dos componentes.  
  
3.  En la superficie de diseño **Flujo de datos**, haga clic en **Búsqueda** en la transformación **Búsqueda** y cambie el nombre por **Lookup Currency Key**.  
  
4.  Haga doble clic en la transformación **Lookup Currency Key** para mostrar el **Editor de transformación Búsqueda**.  
  
5.  En la página **General** , realice las selecciones siguientes:  
  
    1.  Seleccione **Caché completa**.  
  
    2.  En el área **Tipo de conexión** , seleccione **Administrador de conexiones OLE DB**.  
  
6.  En la página **Conexión** , realice las selecciones siguientes:  
  
    1.  En el cuadro de diálogo **Administrador de conexiones OLE DB** , asegúrese de que se muestra **localhost.AdventureWorksDW2012** .  
  
    2.  Seleccione **Usar los resultados de una consulta SQL** y, después, escriba o pegue la instrucción SQL siguiente:  
  
        ```sql
        SELECT * FROM [Sales].[Currency]
        WHERE [CurrencyCode]
        IN ('ARS', 'AUD', 'BRL', 'CAD', 'CNY',
            'DEM', 'EUR', 'FRF', 'GBP', 'JPY',
            'MXN', 'SAR', 'USD', 'VEB')
        ```  
    3.  Haga clic en **Vista previa** para comprobar los resultados de la consulta.
  
7.  En la página **Columnas** , realice las selecciones siguientes:  
  
    1.  En el panel **Columnas de entrada disponibles** , arrastre **CurrencyID** al panel **Columnas de búsqueda disponibles** y suéltelo en **CurrencyAlternateKey**.  
  
    2.  En la lista **Columnas de búsqueda disponibles** , active la casilla situada a la izquierda de **CurrencyKey**.  
  
8.  Haga clic en **Aceptar** para volver a la superficie de diseño **Flujo de datos**.  
  
9. Haga clic con el botón derecho en la transformación Lookup Currency Key y seleccione **Propiedades**.  
  
10. En la ventana **Propiedades**, compruebe que la propiedad **LocaleID** esté establecida en **Inglés (Estados Unidos)** y la propiedad **DefaultCodePage** en **1252**.  
  
## <a name="add-and-configure-the-lookup-date-key-transformation"></a>Adición y configuración de la transformación Lookup Date Key  
  
1.  En el **cuadro de herramientas de SSIS**, arrastre **Búsqueda** a la superficie de diseño **Flujo de datos** . Coloque esta instancia de **Búsqueda** justo debajo de la transformación **Lookup Currency Key**.  
  
2.  Haga clic en la transformación **Lookup Currency Key** y arrastre la flecha de color azul hasta la nueva transformación **Búsqueda** para conectar los dos componentes.  
  
3.  En el cuadro de diálogo **Selección de entrada y salida**, haga clic en **Salida de entradas coincidentes de búsqueda** en el cuadro de lista **Salida** y, después, haga clic en **Aceptar**.  
  
4.  En la superficie de diseño **Flujo de datos**, seleccione el nombre **Búsqueda** en la transformación **Búsqueda** recién agregada y cambie el nombre por **Lookup Date Key**.  
  
5.  Haga doble clic en la transformación **Lookup Date Key** .  
  
6.  En la página **General** , seleccione **Caché parcial**.  
  
7.  En la página **Conexión** , realice las selecciones siguientes:  
  
    1.  En el cuadro de diálogo **Administrador de conexiones OLEDB**, asegúrese de que se muestra **localhost.AdventureWorksDW2012**.  
  
    2.  En el cuadro **Usar una tabla o vista**, escriba o seleccione **[dbo].[DimDate]**.  
  
8.  En la página **Columnas** , realice las selecciones siguientes:  
  
    1.  En el panel **Columnas de entrada disponibles** , arrastre **CurrencyDate** al panel **Columnas de búsqueda disponibles** y suéltelo en **FullDateAlternateKey**.  Si ve un mensaje que indica un error de coincidencia de tipo de datos, cambie el tipo de datos de CurrencyDate a [DT_DBDATE].
  
    2.  En la lista **Columnas de búsqueda disponibles** , active la casilla situada a la izquierda de **DateKey**.  
  
9. En la página **Avanzadas** , revise las opciones de almacenamiento en memoria caché.  
  
10. Haga clic en **Aceptar** para volver a la superficie de diseño **Flujo de datos**.  
  
11. Haga clic con el botón derecho en la transformación **Lookup Date Key** y seleccione **Propiedades**.
  
12. En la ventana **Propiedades**, compruebe que la propiedad **LocaleID** esté establecida en **Inglés (Estados Unidos)** y la propiedad **DefaultCodePage** en **1252**.  
  
## <a name="go-to-next-task"></a>Ir a la tarea siguiente
[Paso 7: Adición y configuración del destino de OLE DB](../integration-services/lesson-1-7-adding-and-configuring-the-ole-db-destination.md)  
  
## <a name="see-also"></a>Consulte también  
[Transformación Búsqueda](../integration-services/data-flow/transformations/lookup-transformation.md)  

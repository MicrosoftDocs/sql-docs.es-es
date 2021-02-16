---
description: Transformación Búsqueda en el modo Caché completa - Administrador de conexiones de caché
title: Transformación Búsqueda en el modo Caché completa - Administrador de conexiones de caché | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Lookup transformation [Integration Services]
ms.assetid: 58bc7611-5fb5-4113-9742-10959e06b94c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c6560951e789dc16c78788b278579abb906fa3e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350328"
---
# <a name="lookup-transformation-full-cache-mode---cache-connection-manager"></a>Transformación Búsqueda en el modo Caché completa - Administrador de conexiones de caché

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Puede configurar la transformación de búsqueda para utilizar el modo de caché completa y un Administrador de conexiones de caché. En el modo de caché completa, el conjunto de datos de referencia se carga en la memoria caché antes de que se ejecute la transformación Búsqueda.  
  
> [!NOTE]  
>  El administrador de conexiones de caché no admite los tipos de datos de objetos binarios grandes (BLOB) DT_TEXT, DT_NTEXT y DT_IMAGE. Si el conjunto de datos de referencia contiene un tipo de datos BLOB, se producirá un error en el componente al ejecutar el paquete. Puede utilizar el **Editor del administrador de conexiones de caché** para modificar los tipos de datos de columna. Para obtener más información, vea [Cache Connection Manager Editor](./cache-connection-manager.md).  
  
 La transformación Búsqueda realiza búsquedas mediante la combinación de datos de las columnas de entrada procedentes de un origen de datos conectado con columnas de un conjunto de datos de referencia. Para más información, consulte [Lookup Transformation](../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
 Puede elegir entre las siguientes opciones para generar un conjunto de datos de referencia:  
  
-   Archivo caché (.caw)  
  
     Se configura el administrador de conexiones de caché para leer los datos de un archivo caché existente.  
  
-   Origen de datos conectado en el flujo de datos  
  
     Se utiliza una transformación de caché para escribir los datos procedentes de un origen de datos conectado del flujo de datos en un administrador de conexiones de caché. Los datos siempre se almacenan en memoria.  
  
     Debe agregar la transformación Búsqueda a un flujo de datos independiente. Esto permite a la transformación de caché rellenar el administrador de conexiones de caché antes de que se ejecute la transformación Búsqueda. Los flujos de datos pueden estar en el mismo paquete o en dos paquetes independientes.  
  
     Si los flujos de datos están en el mismo paquete, utilice una restricción de precedencia para conectar los flujos de datos. Esto permite a la transformación de caché ejecutarse antes de que se ejecute la transformación Búsqueda.  
  
     Si los flujos de datos están en paquetes independientes, puede utilizar la tarea Ejecutar paquete para llamar al paquete secundario desde el paquete primario. Puede llamar a varios paquetes secundarios si agrega varias tareas Ejecutar Paquete a una tarea del contenedor de secuencias del paquete primario.  
  
 Puede compartir el conjunto de datos de referencia almacenado en caché entre varias transformaciones Búsqueda si utiliza uno de los métodos siguientes:  
  
-   Configurar las transformaciones Búsqueda de un único paquete para utilizar el mismo administrador de conexiones de caché.  
  
-   Configurar los administradores de conexiones de caché en paquetes diferentes para utilizar el mismo archivo caché.  
  
 Para obtener más información, vea los temas siguientes:  
  
-   [Transformación de caché](../../integration-services/data-flow/transformations/cache-transform.md)  
  
-   [Administrador de conexiones de caché](../../integration-services/connection-manager/cache-connection-manager.md)  
  
-   [Restricciones de precedencia](../../integration-services/control-flow/precedence-constraints.md)  
  
-   [Tarea Ejecutar paquete](../../integration-services/control-flow/execute-package-task.md)  
  
-   [contenedor de secuencias](../../integration-services/control-flow/sequence-container.md)  
  
 Para ver un vídeo donde se muestra cómo implementar una transformación de búsquedas en el modo de caché completa con el Administrador de conexiones de caché, visite [Cómo implementar una transformación Búsqueda en modo de memoria caché completa (vídeo de SQL Server)](/previous-versions/sql/sql-server-2008/cc952929(v=sql.100)).  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-in-one-package-by-using-cache-connection-manager-and-a-data-source-in-the-data-flow"></a>Implementar una transformación Búsqueda en el modo de caché completa en un paquete utilizando el Administrador de conexiones de caché y un origen de datos del flujo de datos  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra un proyecto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y, a continuación, abra un paquete.  
  
2.  En la pestaña **Flujo de control** , agregue dos tareas Flujo de datos y conéctelas utilizando un conector verde:  
  
3.  En el primer flujo de datos, agregue una transformación de caché y, a continuación, conéctela a un origen de datos.  
  
     Configure el origen de datos según sea necesario.  
  
4.  Haga doble clic en la transformación de caché y, después, en el **Editor de transformación Caché**, en la página **Administrador de conexiones** , haga clic en **Nuevo** para crear un administrador de conexiones de caché.  
  
5.  Haga clic en la pestaña **Columnas** del cuadro de diálogo **Editor del administrador de conexiones de caché** y, a continuación, utilice la opción **Posición de índice** para especificar cuáles son las columnas de índice.  
  
     Para las columnas de no índice, la posición de índice es 0. Para las columnas de índice, la posición de índice es un número secuencial positivo.  
  
    > [!NOTE]  
    >  Cuando la transformación Búsqueda se configura para utilizar un administrador de conexiones de caché, a las columnas de entrada solo se les puede asignar las columnas de índice del conjunto de datos de referencia. Asimismo, todas las columnas de índice deben estar asignadas. Para obtener más información, vea [Cache Connection Manager Editor](./cache-connection-manager.md).  
  
6.  Para guardar la memoria caché en un archivo, en el **Editor del administrador de conexiones de caché**, en la pestaña **General** , configure el administrador a través de las opciones siguientes:  
  
    -   Seleccione **Utilizar la caché del archivo**.  
  
    -   En **Nombre de archivo**, escriba la ruta de acceso al archivo o haga clic en **Examinar** para seleccionar el archivo.  
  
         Si escribe una ruta de acceso para un archivo que no existe, el sistema crea el archivo al ejecutar el paquete.  
  
    > [!NOTE]  
    >  El nivel de protección del paquete no se aplica al archivo caché. Si el archivo caché contiene información confidencial, utilice una lista de control de acceso (ACL) para restringir el acceso a la ubicación o carpeta en la que almacena el archivo. Solo debería permitir el acceso a ciertas cuentas. Para más información, vea [Acceso a los archivos usados por los paquetes](../../integration-services/security/security-overview-integration-services.md#files).  
  
7.  Configure la transformación de caché según sea necesario. Para obtener más información, vea [Editor de transformación Caché &#40;página Administrador de conexiones&#41;](../data-flow/transformations/cache-transform.md) y [Editor de transformación Caché &#40;página Asignaciones&#41;](../data-flow/transformations/cache-transform.md).  
  
8.  En el segundo flujo de datos, agregue una transformación Búsqueda y, a continuación, realice las tareas siguientes para configurar la transformación:  
  
    1.  Conecte la transformación Búsqueda al flujo de datos arrastrando un conector desde un origen o una transformación anterior hasta la transformación Búsqueda.  
  
        > [!NOTE]  
        >  Una transformación Búsqueda podría no validarse si se conecta a un archivo plano que contiene un campo de fecha vacío. El que la transformación se valide depende de si el administrador de conexiones para el archivo plano se ha configurado para conservar los valores NULL. Para asegurarse de que la transformación Búsqueda se valida, en el **Editor de origen de archivos planos**, de la página **Administrador de conexiones**, seleccione la opción **Conservar los valores null del origen como valores null en el flujo de datos** .  
  
    2.  Haga doble clic en el origen o la transformación anterior para configurar el componente.  
  
    3.  Haga doble clic en la transformación Búsqueda y, después, en el **Editor de transformación Búsqueda**, en la página **General** , seleccione **Caché completa**.  
  
    4.  En el área **Tipo de conexión** , seleccione **Administrador de conexiones de caché**.  
  
    5.  En la lista **Especificar cómo administrar las filas sin entradas coincidentes** , seleccione una opción de control de errores.  
  
    6.  En la página **Conexión** , en la lista **Administrador de conexiones de caché** , seleccione un administrador de conexiones de caché.  
  
    7.  Haga clic en la página **Columnas** y, a continuación, arrastre por lo menos una columna desde la lista **Columnas de entrada disponibles** hasta una columna de la lista **Columnas de búsqueda disponibles** .  
  
        > [!NOTE]  
        >  La transformación Búsqueda asigna automáticamente las columnas que tienen el mismo nombre y el mismo tipo de datos.  
  
        > [!NOTE]  
        >  Las columnas deben tener tipos coincidentes de datos para asignarse. Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  En la lista **Columnas de búsqueda disponibles** , seleccione las columnas. A continuación, en la lista **Operación de búsqueda** , especifique si los valores de las columnas de búsqueda reemplazan los valores de la columna de entrada o se escriben en una nueva columna.  
  
    9. Para configurar la salida de errores, haga clic en la página **Salida de error** y establezca las opciones de control de errores. Para obtener más información, vea [Editor de transformación Búsqueda &#40;página Salida de error&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Haga clic en **Aceptar** para guardar los cambios realizados en la transformación Búsqueda.  
  
9. Ejecute el paquete.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-in-two-packages-by-using-cache-connection-manager-and-a-data-source-in-the-data-flow"></a>Para implementar una transformación Búsqueda en el modo de caché completa en dos paquetes mediante el administrador de conexiones de caché y un origen de datos del flujo de datos  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra un proyecto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y, a continuación, abra dos paquetes.  
  
2.  En la pestaña Flujo de control de cada paquete, agregue una tarea Flujo de datos.  
  
3.  En el paquete principal, agregue una transformación de caché al flujo de datos y, a continuación, conéctela a un origen de datos.  
  
     Configure el origen de datos según sea necesario.  
  
4.  Haga doble clic en la transformación de caché y, después, en el **Editor de transformación Caché**, en la página **Administrador de conexiones** , haga clic en **Nuevo** para crear un administrador de conexiones de caché.  
  
5.  En el **Editor del administrador de conexiones de caché**, en la pestaña **General** , configure el administrador estableciendo las opciones siguientes:  
  
    -   Seleccione **Utilizar la caché del archivo**.  
  
    -   En **Nombre de archivo**, escriba la ruta de acceso al archivo o haga clic en **Examinar** para seleccionar el archivo.  
  
         Si escribe una ruta de acceso para un archivo que no existe, el sistema crea el archivo al ejecutar el paquete.  
  
    > [!NOTE]  
    >  El nivel de protección del paquete no se aplica al archivo caché. Si el archivo caché contiene información confidencial, utilice una lista de control de acceso (ACL) para restringir el acceso a la ubicación o carpeta en la que almacena el archivo. Solo debería permitir el acceso a ciertas cuentas. Para más información, vea [Acceso a los archivos usados por los paquetes](../../integration-services/security/security-overview-integration-services.md#files).  
  
6.  Haga clic en la pestaña **Columnas** e indique qué columnas son las columnas de índice mediante la opción **Posición de índice** .  
  
     Para las columnas de no índice, la posición de índice es 0. Para las columnas de índice, la posición de índice es un número secuencial positivo.  
  
    > [!NOTE]  
    >  Cuando la transformación Búsqueda se configura para utilizar un administrador de conexiones de caché, a las columnas de entrada solo se les puede asignar las columnas de índice del conjunto de datos de referencia. Asimismo, todas las columnas de índice deben estar asignadas. Para obtener más información, vea [Cache Connection Manager Editor](./cache-connection-manager.md).  
  
7.  Configure la transformación de caché según sea necesario. Para obtener más información, vea [Editor de transformación Caché &#40;página Administrador de conexiones&#41;](../data-flow/transformations/cache-transform.md) y [Editor de transformación Caché &#40;página Asignaciones&#41;](../data-flow/transformations/cache-transform.md).  
  
8.  Realice una de las siguientes acciones para rellenar el administrador de conexiones de caché que se utiliza en el segundo paquete:  
  
    -   Ejecute el paquete primario.  
  
    -   Haga doble clic en el administrador de conexiones de caché que ha creado en el paso 4, haga clic en **Columnas**, seleccione las filas y, después, presione CTRL+C para copiar los metadatos de columna.  
  
9. En el paquete secundario, cree un administrador de conexiones de caché. Para hacerlo, haga clic con el botón derecho en el área **Administradores de conexiones** , haga clic en **Nueva conexión**, seleccione **CACHÉ** en el cuadro de diálogo **Agregar administrador de conexiones SSIS** y, después, haga clic en **Agregar**.  
  
     El área **Administradores de conexión** aparece en la parte inferior de las pestañas **Flujo de control**, **Flujo de datos** y **Controladores de eventos** del Diseñador de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
10. En el **Editor del administrador de conexiones de caché**, en la pestaña **General** , configure el administrador para leer los datos del archivo caché seleccionado a través de las opciones siguientes:  
  
    -   Seleccione **Utilizar la caché del archivo**.  
  
    -   En **Nombre de archivo**, escriba la ruta de acceso al archivo o haga clic en **Examinar** para seleccionar el archivo.  
  
    > [!NOTE]  
    >  El nivel de protección del paquete no se aplica al archivo caché. Si el archivo caché contiene información confidencial, utilice una lista de control de acceso (ACL) para restringir el acceso a la ubicación o carpeta en la que almacena el archivo. Solo debería permitir el acceso a ciertas cuentas. Para más información, vea [Acceso a los archivos usados por los paquetes](../../integration-services/security/security-overview-integration-services.md#files).  
  
11. Si ha copiado los metadatos de columna en el paso 8, haga clic en **Columnas**, seleccione la fila vacía y, después, presione CTRL+V para pegar los metadatos de columna.  
  
12. Agregue una transformación Búsqueda y, a continuación, configúrela mediante las tareas siguientes:  
  
    1.  Conecte la transformación Búsqueda al flujo de datos arrastrando un conector desde un origen o una transformación anterior hasta la transformación Búsqueda.  
  
        > [!NOTE]  
        >  Una transformación Búsqueda podría no validarse si se conecta a un archivo plano que contiene un campo de fecha vacío. El que la transformación se valide depende de si el administrador de conexiones para el archivo plano se ha configurado para conservar los valores NULL. Para asegurarse de que la transformación Búsqueda se valida, en el **Editor de origen de archivos planos**, de la página **Administrador de conexiones**, seleccione la opción **Conservar los valores null del origen como valores null en el flujo de datos** .  
  
    2.  Haga doble clic en el origen o la transformación anterior para configurar el componente.  
  
    3.  Haga doble clic en la transformación Búsqueda y, después, seleccione **Caché completa** en la página **General** del **Editor de transformación Búsqueda**.  
  
    4.  En el área **Tipo de conexión** , seleccione **Administrador de conexiones de caché** .  
  
    5.  En la lista **Especificar cómo administrar las filas sin entradas coincidentes** , seleccione una opción de control de errores para las filas sin entradas coincidentes.  
  
    6.  En la página **Conexión** , en la lista **Administrador de conexiones de caché** , seleccione el administrador de conexiones de caché que agregó anteriormente.  
  
    7.  Haga clic en la página **Columnas** y, a continuación, arrastre por lo menos una columna desde la lista **Columnas de entrada disponibles** hasta una columna de la lista **Columnas de búsqueda disponibles** .  
  
        > [!NOTE]  
        >  La transformación Búsqueda asigna automáticamente las columnas que tienen el mismo nombre y el mismo tipo de datos.  
  
        > [!NOTE]  
        >  Las columnas deben tener tipos coincidentes de datos para asignarse. Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  En la lista **Columnas de búsqueda disponibles** , seleccione las columnas. A continuación, en la lista **Operación de búsqueda** , especifique si los valores de las columnas de búsqueda reemplazan los valores de la columna de entrada o se escriben en una nueva columna.  
  
    9. Para configurar la salida de errores, haga clic en la página **Salida de error** y establezca las opciones de control de errores. Para obtener más información, vea [Editor de transformación Búsqueda &#40;página Salida de error&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Haga clic en **Aceptar** para guardar los cambios realizados en la transformación Búsqueda.  
  
13. Abra el paquete primario, agregue una tarea Ejecutar paquete al flujo de control y, a continuación, configure la tarea para llamar al paquete secundario. Para más información, consulte [Execute Package Task](../../integration-services/control-flow/execute-package-task.md).  
  
14. Ejecute los paquetes.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-by-using-cache-connection-manager-and-an-existing-cache-file"></a>Para implementar una transformación Búsqueda en el modo de caché completa mediante el administrador de conexiones de caché y un archivo caché existente  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra un proyecto [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y, a continuación, abra un paquete.  
  
2.  Haga clic con el botón derecho en el área **Administradores de conexiones** y, después, en **Nueva conexión**.  
  
     El área **Administradores de conexión** aparece en la parte inferior de las pestañas **Flujo de control**, **Flujo de datos** y **Controladores de eventos** del Diseñador de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
3.  En el cuadro de diálogo **Agregar administrador de conexiones SSIS** , seleccione **CACHÉ** y, a continuación, haga clic en **Agregar** para agregar un administrador de conexiones de caché.  
  
4.  Haga doble clic en el Administrador de conexiones de caché para abrir el **Editor del administrador de conexiones de caché**.  
  
5.  En el **Editor del administrador de conexiones de caché**, en la pestaña **General** , configure el administrador estableciendo las opciones siguientes:  
  
    -   Seleccione **Utilizar la caché del archivo**.  
  
    -   En **Nombre de archivo**, escriba la ruta de acceso al archivo o haga clic en **Examinar** para seleccionar el archivo.  
  
    > [!NOTE]  
    >  El nivel de protección del paquete no se aplica al archivo caché. Si el archivo caché contiene información confidencial, utilice una lista de control de acceso (ACL) para restringir el acceso a la ubicación o carpeta en la que almacena el archivo. Solo debería permitir el acceso a ciertas cuentas. Para más información, vea [Acceso a los archivos usados por los paquetes](../../integration-services/security/security-overview-integration-services.md#files).  
  
6.  Haga clic en la pestaña **Columnas** e indique qué columnas son las columnas de índice mediante la opción **Posición de índice** .  
  
     Para las columnas de no índice, la posición de índice es 0. Para las columnas de índice, la posición de índice es un número secuencial positivo.  
  
    > [!NOTE]  
    >  Cuando la transformación Búsqueda se configura para utilizar un administrador de conexiones de caché, a las columnas de entrada solo se les puede asignar las columnas de índice del conjunto de datos de referencia. Asimismo, todas las columnas de índice deben estar asignadas. Para obtener más información, vea [Cache Connection Manager Editor](./cache-connection-manager.md).  
  
7.  En la pestaña **Flujo de control** , agregue una tarea Flujo de datos al paquete y, a continuación, agregue una transformación Búsqueda al flujo de datos.  
  
8.  Para configurar la transformación Búsqueda, realice una de las siguientes acciones:  
  
    1.  Conecte la transformación Búsqueda al flujo de datos arrastrando un conector desde un origen o una transformación anterior hasta la transformación Búsqueda.  
  
        > [!NOTE]  
        >  Una transformación Búsqueda podría no validarse si se conecta a un archivo plano que contiene un campo de fecha vacío. El que la transformación se valide depende de si el administrador de conexiones para el archivo plano se ha configurado para conservar los valores NULL. Para asegurarse de que la transformación Búsqueda se valida, en el **Editor de origen de archivos planos**, de la página **Administrador de conexiones**, seleccione la opción **Conservar los valores null del origen como valores null en el flujo de datos** .  
  
    2.  Haga doble clic en el origen o la transformación anterior para configurar el componente.  
  
    3.  Haga doble clic en la transformación Búsqueda y, después, en el **Editor de transformación Búsqueda**, en la página **General** , seleccione **Caché completa**.  
  
    4.  En el área **Tipo de conexión** , seleccione **Administrador de conexiones de caché** .  
  
    5.  En la lista **Especificar cómo administrar las filas sin entradas coincidentes** , seleccione una opción de control de errores para las filas sin entradas coincidentes.  
  
    6.  En la página **Conexión** , en la lista **Administrador de conexiones de caché** , seleccione el administrador de conexiones de caché que agregó anteriormente.  
  
    7.  Haga clic en la página **Columnas** y, a continuación, arrastre por lo menos una columna desde la lista **Columnas de entrada disponibles** hasta una columna de la lista **Columnas de búsqueda disponibles** .  
  
        > [!NOTE]  
        >  La transformación Búsqueda asigna automáticamente las columnas que tienen el mismo nombre y el mismo tipo de datos.  
  
        > [!NOTE]  
        >  Las columnas deben tener tipos coincidentes de datos para asignarse. Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  En la lista **Columnas de búsqueda disponibles** , seleccione las columnas. A continuación, en la lista **Operación de búsqueda** , especifique si los valores de las columnas de búsqueda reemplazan los valores de la columna de entrada o se escriben en una nueva columna.  
  
    9. Para configurar la salida de errores, haga clic en la página **Salida de error** y establezca las opciones de control de errores. Para obtener más información, vea [Editor de transformación Búsqueda &#40;página Salida de error&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Haga clic en **Aceptar** para guardar los cambios realizados en la transformación Búsqueda.  
  
9. Ejecute el paquete.  
  
## <a name="see-also"></a>Vea también  
 [Implementación de una transformación Búsqueda en el modo Caché completa con el administrador de conexiones OLE DB](../../integration-services/connection-manager/lookup-transformation-full-cache-mode-ole-db-connection-manager.md)   
 [Implementación de una búsqueda en los modos No hay caché o Caché parcial](../../integration-services/data-flow/transformations/implement-a-lookup-in-no-cache-or-partial-cache-mode.md)   
 [Transformaciones de Integration Services](../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  

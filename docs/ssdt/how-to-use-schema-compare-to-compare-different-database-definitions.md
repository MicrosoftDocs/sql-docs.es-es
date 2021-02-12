---
title: Uso de Comparación de esquemas para comparar distintas definiciones de base de datos
description: Obtenga información sobre cómo comparar definiciones de base de datos con Comparación de esquemas. Vea cómo excluir diferencias específicas y actualizar el destino o crear un script de actualización.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.schemacompare.SchemaCompareOptionsDialog
- sql.data.tools.schemacompare.watermark.f1
- sql.data.tools.schemacompare.f1
- sql.data.tools.schemacompare.connectiondialog.f1
- sql.data.tools.schemacompare.connectiondialog.error.f1
ms.assetid: 7f0905a4-081c-46e2-bd7d-325b63e5c675
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 414a6eb4f6e63d2bb0c02e831f60ebcff97dc590
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018035"
---
# <a name="how-to-use-schema-compare-to-compare-different-database-definitions"></a>Procedimientos: Uso de Comparación de esquemas para comparar distintas definiciones de base de datos

SQL Server Data Tools (SSDT) incluye una utilidad Comparación de esquemas que puede usar para comparar dos definiciones de base de datos.  El origen y el destino de la comparación pueden ser cualquier combinación de una base de datos conectada, un proyecto de base de datos de SQL Server, o un archivo de instantánea o .dacpac.  Los resultados de la comparación se muestran como una serie de acciones que hay que realizar con el destino para que sea igual que el origen.  Una vez completada la comparación puede actualizar el destino directamente (si es un proyecto o una base de datos) o generar un script de actualización que produzca el mismo efecto.  
  
Las diferencias entre el origen y el destino aparecen en una cuadrícula para facilitar su examen.  Puede profundizar y examinar cada diferencia en la cuadrícula de resultados o mediante un script.  También puede excluir selectivamente diferencias específicas.  
  
Puede guardar las comparaciones, ya sea como parte de un proyecto de base de datos de SQL Server o como un archivo independiente.  También puede establecer opciones que controlen el ámbito de la comparación y los aspectos de la actualización.  Después puede guardar la comparación para repetirla más adelante o para usarla como punto de partida para una nueva comparación.  
  
En el procedimiento siguiente se compara el esquema de un proyecto de base de datos con una base de datos conectada.  
  
> [!WARNING]  
> Si se especifica un proyecto como destino de la comparación, la longitud máxima de la ruta de acceso admitida (excluyendo la letra de unidad, el signo de dos puntos y la barra diagonal inversa inicial) para el proyecto es de 256 caracteres. Si la ruta de acceso del proyecto tiene más de 256 caracteres, podrá comparar su esquema con una base de datos u otro proyecto. Sin embargo, no podrá actualizar su esquema.  
  
> [!WARNING]  
> Los procedimientos siguientes usan entidades creadas en procedimientos anteriores de las secciones [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md) y [Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-compare-database-definitions"></a>Para comparar definiciones de base de datos  
  
1.  En el menú **Herramientas**, seleccione **SQL Server** y, después, haga clic en **Nueva comparación de esquemas**.  
  
    O bien, haga clic con el botón derecho en el proyecto **TradeDev** en el **Explorador de soluciones** y seleccione **Comparación de esquemas**.  
  
    Se abrirá la ventana **Comparación de esquemas** y Visual Studio le asignará automáticamente un nombre como `SqlSchemaCompare1`.  
  
    Justo debajo de la barra de herramientas de la ventana **Comparación de esquemas** aparecerán dos menús desplegables con una flecha verde entre ellos. Estos menús le permiten seleccionar definiciones de base de datos para el origen y el destino de la comparación.  
  
2.  En el cuadro desplegable **Seleccionar origen**, elija **Seleccionar origen**; aparecerá el cuadro de diálogo **Seleccionar esquema de origen**.  
  
    Tenga en cuenta que si abrió la ventana **Comparación de esquemas** haciendo clic con el botón derecho en el nombre de proyecto, el esquema de origen ya aparecerá relleno y podrá continuar en el paso 4.  
  
3.  Seleccione el botón de opción **Proyecto** y, a continuación, seleccione el proyecto de base de datos **TradeDev** que ha creado en el procedimiento anterior.  
  
4.  En el cuadro desplegable **Seleccionar destino** de la ventana **Comparación de esquemas**, elija **Seleccionar destino**; aparecerá el cuadro de diálogo **Seleccionar esquema de destino**. En la sección **Esquema**, haga clic en el botón de opción **Base de datos** y, a continuación, haga clic en el botón **Nueva conexión**.  
  
5.  En el cuadro de diálogo **Propiedades de conexión**, escriba el nombre del servidor donde reside la base de datos `TradeDev` y asegúrese de que se proporcionen las credenciales de autenticación correctas. Después, seleccione **TradeDev** en **Conectar con una base de datos** y haga clic en **Aceptar**.  
  
    También puede hacer clic en el botón **Opciones** de la barra de herramientas de la ventana **Comparación de esquemas** para especificar los objetos que se van a comparar, qué tipos de diferencias se van a omitir y otras opciones.  
  
6.  Haga clic en el botón **Comparar** de la barra de herramientas de la ventana **Comparación de esquemas** para iniciar el proceso de comparación.  
  
    Cuando la comparación se complete, las diferencias estructurales entre el proyecto y la base de datos aparecerán en el panel **Resultados**, en la parte superior de la ventana. De forma predeterminada, los resultados de la comparación agrupan todas las diferencias por acción (como Eliminar, Cambiar o Agregar). El panel **Resultados** muestra una fila por cada objeto de base de datos que es distinto entre las definiciones de base de datos. Cada fila identifica el objeto del esquema de origen o de destino (o de ambos) y la acción que se realizará en el esquema de destino para hacer que el objeto de destino sea igual que el objeto de origen.  Si un objeto se ha refactorizado y cambiado de nombre o movido a un nuevo esquema, los nombres de origen y de destino son diferentes y el nombre de origen aparece en negrita para resaltar la diferencia.  
  
    De forma predeterminada, la lista de resultados oculta los objetos que son iguales en ambos esquemas o que no se admiten para la actualización (por ejemplo, los objetos integrados).  Puede hacer clic en los botones de filtro apropiados de la barra de herramientas para mostrar estos objetos.  
  
    Para cambiar la preferencia de agrupación, haga clic en la lista desplegable **Agrupar los resultados** de la barra de herramientas.  Seleccione **Tipo** para agrupar los resultados por tipo de objeto (por ejemplo, por tablas, vistas o procedimientos almacenados).  
  
7.  Busque la tabla `Products` en el grupo `Tables`. Haga clic en la fila y observe que las definiciones de origen y de destino de la tabla aparecen en el panel **Definiciones de objeto** con las diferencias resaltadas. También puede expandir la fila de la tabla `Products` en el panel **Resultados** para inspeccionar los elementos específicos de la tabla que son diferentes.  
  
8.  De forma predeterminada, todas las diferencias se incluyen en el ámbito de la acción Actualizar destino. Puede excluir las diferencias que no desee sincronizar. Para ello, desactive la casilla de la columna **Acción** en el centro de cada fila. También puede hacer clic con el botón derecho en una fila en el panel Esquema y seleccionar **Excluir**. Observe que la fila se deshabilita inmediatamente. Cuando llegue el momento de actualizar la base de datos de destino, no se tendrá en cuenta esta fila para ningún cambio pendiente.  
  
    También puede hacer clic con el botón derecho en una fila de grupo y seleccionar **Excluir todo** o **Incluir todo**, que equivale a desactivar o activar todas las diferencias de ese grupo. Cuando se agrupan resultados por esquema, esta es una forma útil de incluir o excluir todos los cambios a un esquema determinado.  
  
    > [!WARNING]  
    > Si la fila que se va a excluir tiene objetos dependientes (por ejemplo, una fila **Tabla** a la que se hace referencia mediante una fila **Vista**), esta fila se deshabilitará, pero su casilla no se mostrará desactivada. Una vez que se desactiven todas las filas que dependen de ella, la fila deshabilitada se mostrará desactivada. Por otra parte, si una fila se refactoriza (se le cambia el nombre o se traslada a otro esquema), se desactivará su casilla y todas las filas secundarias dependientes de la misma.  
    >   
    > Tenga en cuenta que si actualiza la comparación, se pasarán por alto las diferencias que haya elegido omitir.  
  
Para actualizar el esquema del destino, tiene dos opciones. Si el destino es una base de datos o un proyecto, puede actualizarlo directamente desde la ventana **Comparación de esquemas** o bien, si el destino es una base de datos o un archivo de base de datos, puede generar un script de actualización.  Los scripts generados aparecen en el Editor de Transact\-SQL, desde donde puede inspeccionar el script y ejecutarlo en una base de datos. En los procedimientos siguientes se describen estas opciones con más detalle.  
  
> [!WARNING]  
> Se producirá un error en la actualización porque nuestro cambio implica cambiar una columna de NOT NULL a NULL y, por tanto, produce pérdidas de datos. Si desea continuar con la actualización, haga clic en el botón **Opciones** (el quinto empezando por la izquierda) de la barra de herramientas de Comparación de esquemas y desactive la opción **Bloquear la implementación incremental si puede dar lugar a pérdida de datos**.  
  
### <a name="to-update-directly-in-the-schema-compare-window"></a>Para actualizar directamente en la ventana Comparación de esquemas  
  
1.  Haga clic en el botón **Actualizar** de la barra de herramientas de la ventana Comparación de esquemas.  
  
2.  Examine el script de cambios generado. Puede guardar el script usando la opción Nuevo del menú Archivo. Esto puede resultar adecuado para aquellas situaciones en las que no tiene autorización para actualizar una base de datos de producción, en cuyo caso puede dar el script a un DBA para su posterior implementación.  
  
3.  Si tiene el permiso necesario para actualizar la base de datos, haga clic en el botón **Ejecutar consulta** de la barra de herramientas del panel de edición para ejecutar el script.  
  
### <a name="to-update-by-script"></a>Para actualizar mediante un script  
  
1.  Haga clic en el botón **Generar script** (el cuarto empezando por la izquierda) de la barra de herramientas de la ventana Comparación de esquemas.  
  
    El script generado aparecerá en una nueva ventana del Editor de Transact\-SQL.  
  
    > [!WARNING]  
    > Solo los archivos .dacpac producidos por el proceso de instantáneas de SSDT admiten este comportamiento.  No se puede tener como destino un archivo .dacpac producido por las herramientas o el marco de Aplicación de capa de datos (DAC) de SQL en este momento.  
  
2.  Examine el script de cambios generado. Puede guardar el script mediante el comando de menú **Archivo/Guardar** o Archivo/Guardar como.  
  
    Un script guardado puede ser útil cuando no tenga autorización para actualizar una base de datos de producción. En estos casos, puede dar el script a un DBA para su implementación posterior.  
  
    O bien, puede conectar el Editor de Transact\-SQL a un servidor adecuado y ejecutar el script directamente. Para poder realizar este procedimiento debe tener los permisos necesarios para crear o actualizar la base de datos. Si tiene el permiso necesario para actualizar la base de datos, haga clic en el botón **Ejecutar consulta** de la barra de herramientas del panel de edición para ejecutar el script.  
  
3.  Haga clic en el botón **Conectar**. Esta acción le conecta al servidor actual, o le pide que especifique o seleccione uno en el cuadro de diálogo Conectar con el servidor.  Tenga en cuenta que el nombre de la base de datos se define en el script como una variable de comando.  
  
4.  Inspeccione el script y, si es necesario, realice cambios en las variables de comando que definen el nombre de la base de datos de destino, y el prefijo y las rutas de acceso de archivo asociados.  
  
5.  Haga clic en el botón **Ejecutar** de la barra de herramientas del panel de edición para ejecutar el script.  
  

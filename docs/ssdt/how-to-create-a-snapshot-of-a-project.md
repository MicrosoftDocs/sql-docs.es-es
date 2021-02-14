---
title: Creación de una instantánea de un proyecto
description: Familiarícese con los archivos de aplicación de capa de datos o las instantáneas, y vea cómo usarlos. Obtenga información sobre cómo crear o importar instantáneas, y cómo compararlas.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.SqlProjectImportSnapshotSummaryDialog.dialog
- sql.data.tools.SqlProjectImportSnapshotDialog.dialog
ms.assetid: bed670a3-13bd-4d88-91a1-58d5b9524a97
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0a3c615e98f16c5894924817fc2d4d5acf37b962
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018346"
---
# <a name="how-to-create-a-snapshot-of-a-project"></a>Procedimientos: Creación de una instantánea de un proyecto

Un archivo de **Aplicación de capa de datos** ofrece una representación de solo lectura del esquema de la base de datos en el momento de su creación. Básicamente se considera un esquema de la base de datos desde el que puede volver a importar los objetos de esquema en un proyecto. También puede compararla con el esquema de una base de datos o de un proyecto y actualizar la base de datos o el proyecto para reflejar el esquema definido en la instantánea.  
  
En caso de que se produzca un error de usuario en un proyecto de base de datos de origen, puede revertir el proyecto de origen al estado en que se encontraba cuando se creó la instantánea. También puede establecer instantáneas en diversas etapas del desarrollo para definir líneas base.  
  
> [!WARNING]  
> Los procedimientos siguientes usan entidades creadas en procedimientos anteriores de las secciones [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md) y [Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-create-a-snapshot"></a>Para crear una instantánea  
  
1.  Haga clic con el botón derecho en el proyecto **TradeDev** en el **Explorador de soluciones** y seleccione **Aplicación de capa de datos (\*.dacpac)…** .  
  
2.  SSDT intentará compilar el proyecto primero. Si no hay ningún error de compilación, se creará una carpeta **Instantánea** en el **Explorador de soluciones**. Dentro de esta carpeta, SSDT crea un archivo .dacpac usando el formato "<Project Name>_AAAAMMDD_HH-MM-SS.dacpac".  
  
3.  Haga clic con el botón derecho en el archivo .dacpac y seleccione **Cambiar nombre**. Cambie el nombre de archivo predeterminado a "TradeDev1.dacpac".  
  
4.  Haga clic con el botón derecho en la función **GetProductsBySupplier** en el **Explorador de soluciones** y seleccione **Eliminar** para quitarla del proyecto.  
  
5.  Siga los pasos anteriores para crear una nueva instantánea denominada **TradeDev2.dacpac**.  
  
### <a name="to-import-a-snapshot"></a>Para importar una instantánea  
  
1.  Haga clic con el botón secundario en el proyecto **TradeDev** en el **Explorador de soluciones**, seleccione **Importar** y después haga clic en **Aplicación de capa de datos (\*.dacpac)…** en los menús contextuales.  
  
2.  En el cuadro de diálogo **Importar aplicación de capa de datos**, haga clic en **Examinar** para seleccionar que el archivo **TradeDev1.dacpac** se usará como origen de la importación.  
  
    Observe que la sección **Proyecto de destino** se ha deshabilitado, ya que el proyecto actual es el destino predeterminado. Haga clic en **Iniciar** para iniciar la importación.  
  
3.  Haga clic en **Finalizar** en la página **Resumen**. En el **Explorador de soluciones**, observe que la tabla eliminada se ha restaurado en el proyecto.  
  
    > [!WARNING]  
    > La instantánea importará todas las entidades de base de datos del esquema de instantánea en el proyecto. Esto podría crear entidades duplicadas. Por ejemplo, cada una de las tablas y vistas contiene ahora una copia adicional de sí misma denominada <NombreDeObjeto_1>. Haga clic con el botón derecho en cada uno de estos objetos duplicados en el **Explorador de soluciones** y seleccione **Eliminar** para quitarlo del proyecto.  
  
### <a name="to-compare-snapshots"></a>Para comparar instantáneas  
  
1.  Haga clic con el botón derecho en **TradeDev1.dacpac** en el Explorador de soluciones y seleccione **Comparación de esquemas**. Se abrirá la ventana **Comparación de esquemas**.  
  
2.  Use las opciones de **Archivo de aplicación de capa de datos** para establecer los esquemas de origen y de destino. Asegúrese de que el **Esquema de origen** esté establecido en **TradeDev1.dacpac** en **Archivo de aplicación de capa de datos** y que el **Esquema de destino** esté establecido en **TradeDev2.dacpac**.  
  
3.  Haga clic en **Aceptar** para iniciar la comparación. Observe que la función eliminada se está resaltando como una diferencia entre la instantánea anterior y la nueva.  
  
    Puede encontrar fácilmente las diferencias entre distintas instantáneas mediante Comparación de esquemas. En este caso, puede saber cómo evoluciona el proyecto durante el proceso de desarrollo.  
  
## <a name="see-also"></a>Consulte también  
[Cómo: Usar Comparación de esquemas para comparar distintas definiciones de base de datos](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md)  
  

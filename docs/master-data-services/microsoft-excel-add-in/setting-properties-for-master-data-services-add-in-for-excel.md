---
description: Establecer propiedades para el complemento Master Data Services para Excel
title: Establecer propiedades
ms.custom: microsoft-excel-add-in, seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cab1c662-5d40-4c16-9f5c-36ff9608810b
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: af9b942b8fb2cf6b1d0838327d5e32d278068146
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352972"
---
# <a name="setting-properties-for-master-data-services-add-in-for-excel"></a>Establecer propiedades para el complemento Master Data Services para Excel

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  La configuración del complemento Master Data Services para Excel determina el modo en que se cargan los datos desde MDS en el complemento de Excel y el modo en que se publican desde el complemento de Excel en MDS.  
  
 Para configurar el complemento de Excel, abra **Excel**, haga clic en el menú **Datos maestros** y, luego, haga clic en **Configuración**. Cualquier usuario con acceso a Excel puede cambiar esta configuración. La configuración se aplica al equipo en el que está abierto Excel.  
  
## <a name="excel-add-in-settings"></a>Configuración del complemento de Excel  
  
|Pestaña y sección|Configuración|Descripción|  
|-|-|-|  
|Configuración: Publicación|Mostrar el cuadro de diálogo **Publicar y anotar** al publicar|Seleccione esta opción para mostrar el cuadro de diálogo **Publicar y anotar** después de hacer clic en **Publicar**; esto le permitirá especificar una sola anotación para todos los cambios o una para cada cambio.<br /><br /> Anule la selección de esta opción si desea que el proceso de publicación se inicie sin mostrar el cuadro de diálogo **Publicar y anotar** . No tendrá la oportunidad de especificar ninguna anotación.|  
|Configuración: Versión|Selección de versión|Seleccione la versión de los datos maestros que se cargarán en el complemento de Excel. Puede ser:<br /><br /> **Ninguno** para que no haya ninguna versión predeterminada<br /><br /> **Más antiguo** para usar como versión predeterminada la más antigua o **Más reciente** para usar como versión predeterminada la más reciente.|  
|Configuración: Registro|Activar el registro detallado|Habilita el registro del proceso de carga de los datos maestros desde MDS en el complemento de Excel, de forma que quedan registrados los resultados de todos los comandos del servicio.|  
|Configuración: Telemetría|Activar la recopilación de datos de telemetría|Habilite la telemetría para ayudar a mejorar la calidad, la confiabilidad y el rendimiento del complemento de Excel de [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .|  
|Configuración: Tamaño de lotes|Número de celdas para cargar|Seleccione cuántos miles de celdas se incluirán en cada lote que se cargue desde el servidor de MDS en Excel. El valor predeterminado es 50.000 celdas.|  
|Configuración: Tamaño de lotes|Número de celdas para publicar|Seleccione cuántos miles de celdas se publicarán en cada lote que Excel devuelva al servidor. El valor predeterminado es 50.000 celdas.|  
|Configuración: Servidores agregados a la lista segura|Borrar todo|Haga clic en esta opción para borrar la lista de conexiones que se designaron como seguras cuando se abrió el archivo de consulta de acceso directo asociado.|  
|Datos: Filtros|Mostrar advertencia de filtro para grandes conjuntos de datos|Haga clic en esta opción para mostrar una advertencia si el conjunto de datos que se va a cargar desde MDS en Excel supera el número máximo de filas o columnas.|  
|Datos: Filtros|Número máximo de filas|Seleccione el umbral para el número de filas que se cargarán, superado el cual se mostrará una advertencia de filtro.|  
|Datos: Filtros|Máximo de columnas|Seleccione el umbral para el número de columnas que se cargarán, superado el cual se mostrará una advertencia de filtro.|  
|Datos: Formato de celda|Cambiar el color de la celda cuando: Cambio de valores de atributo|Haga clic en esta opción si desea que el color de una celda cambie si cambia el valor de atributo de dicha celda al actualizar la tabla del complemento de Excel con nuevos datos procedentes del repositorio de MDS.|  
|Datos: Formato de celda|Cambiar el color de la celda cuando: Los miembros se agregan|Haga clic para especificar que el color de las celdas de una fila cambie si se agrega un nuevo miembro a la fila al actualizar la tabla del complemento de Excel con nuevos datos procedentes del repositorio de MDS.|  
|Datos: Formato de celda|Formato de presentación|Seleccione el formato preferido para mostrar los valores de atributos basados en dominio. Las opciones son Código {Name}, Código y Nombre {Code}.|  
  
  

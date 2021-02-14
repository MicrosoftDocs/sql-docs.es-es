---
title: Notas de la versión para Reporting Services 2017 y posterior | Microsoft Docs
description: Obtenga más información sobre los cambios en SQL Server Reporting Services (SSRS), para las versiones 2017 y posteriores.
ms.date: 10/11/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.reviewer: maggies
author: casualoak
ms.author: rhys
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: ccf46bca321f3b23b1cd9216aa122e2202ff8c24
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031167"
---
# <a name="release-notes-for-sql-server-reporting-services-ssrs-2017-and-later"></a>Notas de la versión de SQL Server Reporting Services (SSRS) 2017 y versiones posteriores

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2017-and-later](../includes/ssrs-appliesto-2017-and-later.md)]

En este artículo se describen los cambios en [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] (SSRS) para las versiones 2017 y posteriores.

Para ver las notas de la versión de los controles del visor de informes, vea [Notas de la versión de los controles del visor de informes para WebForms y WinForms de SSRS](application-integration/release-notes-ssrs-application-integration.md).

<!--
We are "standardizing" all our 'Release Notes' style articles:

  - File names:   'release-notes-[TechArea-Name].md'


  - Content format:   2-column tables.  No longer using bullet lists.

    - Ideally, the 'Details' column should contain a link to another SSRS documentation article wherein the particular issue fix is discussed in any way.  Or if there is more to say beyond one sentence, the other sentences of elaboration would go into the 'Details' column.


  - H2 header names are kept short, for better display.


  - Try to keep all Release Notes in one .md file.  Avoid having multiple R.N. files whose names differ only by version or date.

    - Seriously consider erasing info about ancient releases that are so old that nobody cares about them anymore.  If you really want to retain ancient info, consider doing so in an HTML comment at the end of the .md file, just in case a Microsoft employee needs to re-examine ancient info.  In any case, this file cannot get ever longer, and some deletion or hiding of oldest info must eventually occur.


  - Do use '::: moniker range=' zones when version 2017 is no longer the only version represented inside this .md file.

    - Use the '=' operator on the moniker, not the '>=' operator.
    - In contrast, in our 'Whats New' articles we do use the '>=', rather than '='.

GeneMi, DevOps = 1467988 (MsEng > TechnicalContent) , 2019/03/19
-->
## <a name="sql-server-2019-reporting-services"></a>SQL Server 2019 Reporting Services

## <a name="15075454810-20200831"></a>15.0.7545.4810, 2020/08/31 
*(Versión del producto: 15.0.1102.861)*

| Problema corregido | Detalles |
| :---------- | :------ |
| Actualizaciones de seguridad  | &nbsp; |
| Compatibilidad con datos adjuntos de comentarios restringidos para dejar de permitir documentos PDF  | &nbsp; |
| El truncamiento del nombre de archivo fijo al exportar informes con un punto en el nombre  | &nbsp; |
| Se ha corregido un problema relacionado con las suscripciones y la referencia cultural zh-TW que daba lugar a errores de formato de fecha no válido  | &nbsp; |
| Se ha corregido un problema con algunos informes que hacía que el acceso a la opción de parámetros diera lugar a un desbordamiento indefinido  | &nbsp; |
| Se han corregido problemas relacionados con comillas simples en nombres de informes  | &nbsp; |
| Se ha corregido un problema en el acceso URL que hacía que FindString no encontrara coincidencias  | &nbsp; |
| Se ha corregido un problema que hacía que el texto alternativo para la exportación a PDF no se codificara correctamente para caracteres multibyte  | &nbsp; |
| Se ha corregido la apariencia no deseada de una imagen vacía en un elemento lineal  | &nbsp; |
| Se ha corregido un error no admitido erróneo para la autenticación personalizada en Web Edition  | &nbsp; |
| Se ha corregido un problema que hacía que un lector de pantalla leyese una fila y columna adicionales en un Tablix  | &nbsp; |
| Se ha corregido un problema de truncamiento con el ajuste de tamaño al ampliarse hasta la página entera  | &nbsp; |
| La actualización de la línea de comandos ya no requiere la marca EULA  | &nbsp; |

## <a name="150724337714-20191101"></a>15.0.7243.37714, 01/11/2019
*(Versión del producto: 15.0.1102.675)*

Versión inicial.


## <a name="sql-server-2017-reporting-services"></a>SQL Server 2017 Reporting Services

## <a name="1406001669-20200831"></a>14.0.600.1669, 2020/08/31 

| Problema corregido | Detalles |
| :---------- | :------ |
| Actualizaciones de seguridad  | &nbsp; |
| Compatibilidad con datos adjuntos de comentarios restringidos para dejar de permitir documentos PDF  | &nbsp; |
| El truncamiento del nombre de archivo fijo al exportar informes con un punto en el nombre  | &nbsp; |
| Se ha corregido un problema relacionado con las suscripciones y la referencia cultural zh-TW que daba lugar a errores de formato de fecha no válido  | &nbsp; |

## <a name="1406001572-20200406"></a>14.0.600.1572, 2020/04/06 

| Problema corregido | Detalles |
| :---------- | :------ |
| Interfaz de usuario de JQuery actualizada a 1.12  | &nbsp; |
| Se ha corregido la distinción de mayúsculas y minúsculas en direcciones URL.  | &nbsp; |
| Se ha corregido la ubicación de parámetro cuando hay muchos parámetros.  | &nbsp; |
| Se ha corregido FindString cuando no funciona en la representación de HTML5.  | &nbsp; |
| Se han actualizado las bibliotecas cliente de Analysis Services para abordar el desuso de TLS 1.0/1.1. | &nbsp; |

## <a name="1406001451-20191113"></a>14.0.600.1451, 2019/11/13 

| Problema corregido | Detalles |
| :---------- | :------ |
| Actualizaciones de seguridad | &nbsp; |
| Los informes paginados no funcionaban correctamente con los parámetros de filtro cuando la instantánea está habilitada.  | &nbsp; |
| Los usuarios con la función de explorador y la configuración predeterminada no tenían permisos para descargar archivos de Excel.  | &nbsp; |
| No se pudo actualizar a Power BI Report Server desde SQL Server 2016 Reporting Services durante la actualización. | &nbsp; |
| Después de actualizar desde SQL Server 2012 Reporting Services, se produjo un error en las suscripciones que indicaba que se encontró un carácter no válido en el mensaje de encabezado de correo electrónico. | &nbsp; |
| Herramienta de configuración: si se cancelan las ventanas modales en la sección de la base de datos, se reiniciará el servicio Reporting Services. | &nbsp; |
| La expresión de propiedad BorderStyle de controles TextBox no se representó en formato Excel.  | &nbsp; |
| Problema de paginación que pudo provocar el bloqueo de determinados informes que representaban la misma página sin llegar nunca a la última página del informe. | &nbsp; |

## <a name="1406001274-20190701"></a>14.0.600.1274, 2019/07/01

| Problema corregido | Detalles |
| :---------- | :------ |
| Actualizaciones de seguridad | &nbsp; |
| No se pueden seleccionar los días de la semana al crear una programación compartida semanal. | &nbsp; |
| El informe no muestra los retornos de carro correctamente en formato de Word. | &nbsp; |
| System Center Operations Manager (SCOM) 2019 ya no funciona con las actualizaciones recientes de SSRS 2017. | &nbsp; |
| Error al invocar la extensión de autorización para el conjunto de datos compartido. | &nbsp; |
| Se ha cambiado la lógica del procedimiento almacenado GetAllProperties en SSRS 2017 y PBIRS, lo que hace que el método ReportingService2010.GetProperties del punto de conexión de servicio Web no pueda obtener datos para el informe vinculado. | &nbsp; |
| El encabezado de fila de cuadrícula simple en el informe para dispositivos móviles desaparece cuando se hace clic en un elemento de la cuadrícula. | &nbsp; |
| No se puede usar el campo de fecha en el parámetro de suscripción controlada por datos. | &nbsp; |
| El elemento Tablix anidado muestra una fuente pequeña o parcial en SSRS 2016 y versiones posteriores. | &nbsp; |
| Suscripciones con error de parámetro de fecha y hora después de que un usuario con una configuración regional diferente edite la suscripción. | &nbsp; |
| La creación de una suscripción controlada por datos con una extensión de entrega nula indica que se ha producido un error de entrega. | &nbsp; |
| La codificación de la dirección URL es incorrecta al establecer el valor en formato Excel\Word. | &nbsp; |

## <a name="1406001109-20190212"></a>14.0.600.1109, 2019/02/12

| Problema corregido | Detalles |
| :---------- | :------ |
| El almacenamiento en caché de las programaciones de instantáneas de informes cambia a "Programación específica del informe" tras modificar la suscripción. | &nbsp; |
| rc:Toolbar=false no funciona en la edición Express. | &nbsp; |
| Algunos caracteres tailandeses se representan de forma incorrecta al exportar informes paginados a PDF. | &nbsp; |
| Interbloqueo durante la notificación de suscripciones controladas por datos. | &nbsp; |
| Las imágenes insertadas no se muestran en determinadas circunstancias cuando se usa el parámetro rc:Toolbar=False. | &nbsp; |
| No se pueden crear suscripciones controladas por datos para informes que usan parámetros en cascada. | &nbsp; |
| No se pueden editar suscripciones configuradas con un intervalo no válido. | &nbsp; |
| Actualizaciones de seguridad. | &nbsp; |
| No se muestra la interfaz de usuario de los informes vinculados. | &nbsp; |
| Algunos informes paginados con controles Tablix anidados tienen fuentes incorrectas. | &nbsp; |
| Se agrega un espacio en blanco de forma errónea a determinados informes paginados que contienen regiones de datos Tablix. | &nbsp; |
| Las filas de encabezado desaparecen al expandir las cuadrículas de datos simples de un informe móvil. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600906-20180912"></a>14.0.600.906, 2018/09/12

Se corrigió el problema siguiente:

| Problema corregido | Detalles |
| :---------- | :------ |
| La autenticación personalizada no devuelve información de cookies correcta. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600892-20180831"></a>14.0.600.892, 2018/08/31

| Problema corregido | Detalles |
| :---------- | :------ |
| El cuadro de texto dentro del rectángulo hace que el rectángulo no crezca verticalmente si rc:Toolbar=False y tiene texto largo. | &nbsp; |
| El tamaño del texto no se escala si pageHeight es inferior a 0,5 pulgadas. | &nbsp; |
| Se produce un interbloqueo en la base de datos del catálogo de SSRS cuando se usa con CRM. | &nbsp; |
| Encabezados de columna alineados de manera vertical mostrados de forma incorrecta al desplazarse hasta el informe. | &nbsp; |
| Los usuarios agregados al rol de informes de System Center Operations Manager tienen bloqueado el acceso al portal web de SSRS. | &nbsp; |
| El carácter tailandés no se exporta correctamente a PDF. | &nbsp; |
| Cambio de comportamiento del rol Explorador. | &nbsp; |
| rc:Toolbar=false no funciona en la edición Express. | &nbsp; |
| Falta la barra de desplazamiento vertical en el área de mensajes del parámetro. | &nbsp; |
| Tiempo de ejecución de informes móviles actualizado. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600744-20180425"></a>14.0.600.744, 2018/04/25

| Problema corregido | Detalles |
| :---------- | :------ |
| La página Suscripción controlada por datos no muestra la opción de entrega una vez creada. | &nbsp; |
| La actualización de SSRS 2012 a SSRS 2017 tiene como resultado que RSManagement lanza una excepción cada pocos segundos. | &nbsp; |
| No se pueden modificar los valores predeterminados para parámetros de varios valores en IE11. | &nbsp; |
| Los horarios están vacíos cada vez que se ejecuta una programación compartida. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600689-20180228"></a>14.0.600.689, 2018/02/28

| Problema corregido | Detalles |
| :---------- | :------ |
| La visibilidad del parámetro de informe de un informe vinculado se revierte después de editar sus propiedades. | &nbsp; |
| El parámetro de la dirección URL rc:Toolbar=false no funciona en la edición Express. | &nbsp; |
| Las expresiones en TextBox con la propiedad CanGrow establecida en false hace que no se muestren los valores. | &nbsp; |
| Se agregó el vínculo _Más información_ para la clave de producto en la configuración. | &nbsp; |
| El portal web con autenticación de formularios personalizados omite el deslizamiento de la cookie de expiración. | &nbsp; |
| Exportar a Word crea un alto de fila desigual si el contenido de la fila está vacío. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600594-20180109"></a>14.0.600.594, 2018/01/09

Se implementaron algunas actualizaciones de seguridad.

### <a name="140600490-20171101"></a>14.0.600.490, 2017/11/01

Problemas resueltos con la actualización de SKU.

## <a name="140600451-20170930"></a>14.0.600.451, 2017/09/30

Versión inicial.

## <a name="next-steps"></a>Pasos siguientes

[Novedades de Reporting Services (SSRS)](what-s-new-in-sql-server-reporting-services-ssrs.md)

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).

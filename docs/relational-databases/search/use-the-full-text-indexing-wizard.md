---
description: Usar el Asistente para indización de texto completo
title: Usar el Asistente para indización de texto completo | Microsoft Docs
ms.date: 08/19/2016
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
f1_keywords:
- sql13.swb.fulltextindexingwizard.welcome.f1
- sql13.swb.fulltextindexingwizard.selectorcreatepopschedules.f1
- sql13.swb.fulltextindexingwizard.progress.f1
- sql13.swb.fulltextindexingwizard.selectchangetracking.f1
- sql13.swb.fulltextindexingwizard.selectacatalog.f1
- sql13.swb.fulltextindexingwizard.selectatableorview.f1
- sql13.swb.fulltextindexingwizard.selectanindex.f1
- sql13.swb.fulltextindexingwizard.summary.f1
- sql13.swb.fulltextindexingwizard.selecttablecolumns.f1
helpviewer_keywords:
- Full-Text Indexing Wizard
- full-text search [SQL Server], Full-Text Indexing Wizard
ms.assetid: 3e9d9605-6525-4781-9168-fdaa06db3459
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 17fd1c0c884f606d1ede8e6dfa392fd0645185f7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479396"
---
# <a name="use-the-full-text-indexing-wizard"></a>Usar el Asistente para indización de texto completo
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  El Asistente para indización de texto completo de SSMS le guía por una serie de pasos diseñados para ayudarle a crear un índice de texto completo.  
  
## <a name="create-a--full-text-index"></a>Crear un índice de texto completo 

1. En el Explorador de objetos, haga clic con el botón derecho en la tabla en la que quiere crear un índice de texto completo, seleccione **Índice de texto completo** y, luego, haga clic en **Definir índice de texto completo**. Esta acción inicia el Asistente en una ventana independiente.
   Haga clic en Next (Siguiente). 
  
2. **Índice único.**  Seleccione un índice de la lista desplegable. El índice deberá ser un índice de columna de una sola clave, único y que no admita valores NULL. Seleccione el índice de clave única más pequeño para la clave única de texto completo. Para obtener mejores resultados, se recomienda utilizar un índice clúster.  
  
3.  **Columnas disponibles.** Active el cuadro situado junto a todos los nombres de columna correspondientes a las columnas que desea incluir.  casilla situada junto al nombre de la columna. Las columnas ilegibles están atenuadas y, sus casillas, deshabilitadas.  
  
4. **Idioma del separador de palabras.** Seleccione un idioma en la lista desplegable. Esta opción se usará para identificar los separadores de palabras correctos para el índice. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa separadores de palabras para identificar límites de palabras en los datos indexados de texto completo.  
  
5.  **Columna Tipo.** Seleccione el nombre de la columna que incluye el tipo de documento de la columna que se va a incluir en el índice de texto completo.  

> **NOTA:** La  **Columna Tipo** solo se habilita cuando la columna mencionada en la columna **Columnas disponibles** es del tipo **varbinary(max)** o **image**.  
  
6. **Semántica estadística.** Seleccione si desea habilitar la indización semántica para la columna seleccionada. Para obtener más información, vea [Búsqueda semántica &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
>**NOTAS** 
>
>Si su idioma seleccionado no tiene un modelo de idioma semántico asociado, la casilla **Semántica estadística** no está habilitada. Si selecciona **Semántica estadística** antes de seleccionar **Idioma**, los idiomas disponibles en el cuadro combinado desplegable estarán limitados a aquellos para los que exista un modelo de idioma semántico.  
>
> Búsqueda semántica no **está disponible para Azure SQL Database.** La opción Semántica estadística no aparece cuando se ejecuta este Asistente en Azure SQL Database.
  
7. Seleccione las opciones de seguimiento de cambios.  
  
     **Automáticamente**  
     Seleccione este botón de opción para que el índice de texto completo se actualice automáticamente cuando se produzcan cambios en los datos subyacentes.  
  
     **Manualmente**  
     Seleccione este botón de opción si no desea que el índice de texto completo se actualice automáticamente cuando se produzcan cambios en los datos subyacentes. Los cambios en los datos se mantienen; sin embargo, para aplicarlos al índice de texto completo es necesario iniciar o programar este proceso manualmente.  
  
     **No realizar seguimiento de cambios**  
     Seleccione este botón de opción si no desea que el índice de texto completo se actualice con los cambios en los datos subyacentes.  
  
8.  Iniciar llenado completo al crear el índice (solo disponible cuando no realiza un seguimiento de cambios).
  
     Seleccione este botón de opción para iniciar un llenado completo tras la finalización correcta de este asistente. De esta forma, se creará la estructura del índice de texto completo en el catálogo y se llenará con los datos indizados de texto completo.  
     
     Haga clic en Next (Siguiente).
  
## <a name="catalog-index-filegroup-and-stoplist"></a>Catálogo, Grupo de archivos de índice y Lista de palabras irrelevantes   
  
9.  **Seleccionar catálogo de texto completo**  

     **Seleccionar un catálogo:** seleccione un catálogo de texto completo de la lista. El catálogo predeterminado de la base de datos será el elemento seleccionado de manera predeterminada en la lista. Si no hay catálogos disponibles, la lista estará deshabilitada y la casilla **Crear un nuevo catálogo** estará activada y deshabilitada.  
  
  O BIEN
  
 10. **Creación de un catálogo**
 - Seleccione un catálogo de texto completo.  
  
    a. **Nombre**  
     Escriba un nombre para el nuevo catálogo de texto completo.  
  
     b. **Establecer como catálogo predeterminado**  
     Seleccione esta opción para hacer que el catálogo sea el valor predeterminado para esta base de datos.  
  
     c. **Distinción de acentos**  
     Especifique si el nuevo catálogo distinguirá acentos. Si la base de datos distingue acentos, la opción **Con distinción** se selecciona de manera predeterminada.  
  
     d. **Seleccionar grupo de archivos de índice**  
     Especifique el grupo de archivos en el que crear el índice de texto completo.  
  
     e. Seleccionar un valor:  
      |Value|Descripción|  
      |-----------|-----------------|
      |**<default>**| Si la tabla o la vista no tienen particiones, seleccione este valor para usar el mismo grupo de archivos que la tabla o vista subyacente. Si la tabla o la vista tienen particiones, se usa el grupo de archivos principal.|
      |**PRIMARY**|Seleccione este valor para usar el grupo de archivos principal para el nuevo índice de texto completo.|
      *grupo de archivos predeterminado especificado por el usuario*|Si existe una lista de palabras irrelevantes predeterminada definida por el usuario, seleccione su nombre en la lista para usar ese grupo de archivos para el nuevo índice de texto completo.|   
  
     
 11. **Seleccionar lista de palabras irrelevantes de texto completo**  
     Especifique una lista de palabras irrelevantes para usarla en el índice de texto completo, o deshabilite el uso de la lista de palabras irrelevantes.  
  
     Las palabras irrelevantes se administran en bases de datos mediante objetos denominados listas de palabras irrelevantes. Una *lista de palabras irrelevantes* es una lista de palabras que, cuando se asocia a un índice de texto completo, se aplica a las consultas de texto completo en ese índice. Para obtener más información, vea [Configurar y administrar palabras irrelevantes y listas de palabras irrelevantes para la búsqueda de texto completo](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md).  
  
     Seleccione uno de los siguientes valores:  
  
   |Value|Descripción|  
    |-----------|-----------------|  
    |**<system>**|Seleccione este valor para utilizar la lista de palabras irrelevantes del sistema en el nuevo índice de texto completo. Este es el valor predeterminado.|  
    |**<off>**|Seleccione este valor para deshabilitar las listas de palabras irrelevantes para el nuevo índice de texto completo.|  
    |*user-defined-stoplist-name*|La lista muestra el nombre de cada lista de palabras irrelevantes definida por el usuario, si hay alguna, que se haya creado en la base de datos. Seleccione la lista de palabras irrelevantes definida por el usuario que desee para utilizarla para el nuevo índice de texto completo.|  
  
  Haga clic en Next (Siguiente).
  
11. Opcionalmente, solo en SQL Server, defina la programación de rellenado. Las operaciones de indización comenzarán inmediatamente, a menos que se hayan programado para que se ejecuten posteriormente. Las programaciones se crearán inmediatamente, aunque no se ejecuten hasta la hora programada.  
  
     **Nueva programación de tabla**  
     Permite definir una programación de llenado para una tabla.  
  
     **Nueva programación de catálogo**  
     Permite definir una programación de llenado para un catálogo de texto completo.  
  
     **Edición**  
     Permite editar una programación.  
  
     **Eliminar**  
     Permite eliminar una programación.  
  
5.  Vea o controle el progreso del Asistente para indización de texto completo.  
  
     **Detención**  
     Interrumpe la operación actual e impide que el asistente lleve a cabo operaciones de texto completo posteriores durante esta sesión.  
  
     **Report**  
     Cuando finalice la ejecución de todas las operaciones, haga clic en este botón para obtener acceso a un informe sobre las operaciones efectuadas. Puede ver el informe, imprimirlo en un archivo, copiarlo al portapapeles o enviarlo por correo electrónico.  
  
  

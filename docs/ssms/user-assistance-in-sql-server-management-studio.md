---
description: Asistencia al usuario en SQL Server Management Studio
title: Asistencia al usuario en SQL Server Management Studio
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Help [SQL Server Management Studio]
- SQL Server Management Studio [SQL Server], user assistance
ms.assetid: 3c33a474-e507-4712-86fe-ae40e8370319
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c1d8ad8f855a2888dc1a37fd985f2b1a3bfc0357
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338792"
---
# <a name="user-assistance-in-sql-server-management-studio"></a>Asistencia al usuario en SQL Server Management Studio
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
En [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], la asistencia al usuario está disponible mediante el menú Ayuda y los Libros en pantalla de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. El menú Ayuda de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] ofrece diferentes rutas a la información sobre [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. También proporciona acceso a la comunidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y a los recursos de MSDN Online no disponibles anteriormente desde el entorno de la Ayuda. Además, el entorno de la Ayuda ahora se puede configurar de modo que se inicie dentro del entorno de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] o en una ventana externa asociada independiente.  
  
## <a name="the-help-interface"></a>La interfaz de la Ayuda  
El **Contenido** y el **Índice** ofrecen una funcionalidad y una interfaz con las que los usuarios de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ya están familiarizados. Las demás opciones son:  
  
-   **¿Cómo...?**  
  
    Proporciona un conjunto jerárquico de páginas vinculadas que contienen temas de utilidad relacionados con las tareas comunes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . El contenido está organizado por componente y tarea, por ejemplo, temas sobre replicación.  
  
-   **Búsqueda**  
  
    Busca por temas, con o sin filtros predefinidos. Buscar en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es una página con pestañas independiente. Los usuarios pueden restringir las búsquedas con uno o más filtros predefinidos de tipo de tema, lenguaje o tecnología. De forma predeterminada, Buscar no utiliza ninguno de los filtros predefinidos. Solo se busca en los temas de las colecciones instaladas.  
  
    Los usuarios pueden incluir en su búsqueda los recursos en línea si habilitan la Ayuda en pantalla. Para obtener más información, vea "MSDN Online y comunidades de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ", más adelante en este tema.  
  
-   **Ayuda dinámica**  
  
    Muestra automáticamente los vínculos a la información relevante mientras los usuarios trabajan en el entorno de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] .  
  
-   **Favoritos de la Ayuda**  
  
    Almacena marcadores de temas del usuario para facilitar el acceso en otras ocasiones.  
  
Ayuda sobre la Ayuda (Ayuda de [!INCLUDE[msCoName](../includes/msconame_md.md)] Document Explorer) vincula a los usuarios con la documentación sobre el Visor de ayuda, pero los temas se encuentran en una colección independiente de los Libros en pantalla de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Para más información acerca del Visor de ayuda, seleccione **Ayuda sobre la Ayuda** en el menú Ayuda de los Libros en pantalla de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="msdn-online-and-sql-server-communities"></a>MSDN Online y comunidades de SQL Server  
La Ayuda de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] también ofrece a los usuarios formas de ponerse en contacto con MSDN Online y con comunidades centradas en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]en la Web para obtener información. Puede hacer lo siguiente:  
  
-   Obtener acceso a las comunidades de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] desde la página Cómo.  
  
-   Buscar en los sitios de MSDN Online y la comunidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
#### <a name="to-access-sql-server-focused-communities-from-the-how-do-i-page"></a>Para obtener acceso a las comunidades centradas en SQL Server desde la página Cómo  
  
1.  En [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], en el menú **Ayuda** , haga clic en **Cómo**.  
  
2.  Se abrirá la página [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] **Cómo**. En la barra lateral de vínculos de comunidad, haga clic en el nombre del sitio de la comunidad al que desea obtener acceso.  
  
    > [!NOTE]  
    > El equipo que ejecute [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] debe disponer de una conexión directa a Internet.  
  
    Antes de poder buscar en MSDN Online o en las comunidades de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , es necesario habilitar la búsqueda en línea.  
  
#### <a name="to-enable-online-search"></a>Para habilitar la búsqueda en línea  
  
1.  En el menú **Herramientas** , haga clic en **Opciones**. En el cuadro de diálogo **Opciones** , si fuera necesario, expanda los nodos **Entorno** y **Ayuda** y, luego, haga clic en **En línea**.  
  
2.  En el área **Cuando cargue contenido de la Ayuda** , seleccione una opción en línea.  
  
3.  En la lista **Buscar estos proveedores**, active los buscadores en los que desea buscar y desactive los que no vaya a necesitar.  
  
4.  Si **Comunidad de Codezone** es uno de los buscadores que activa, en la lista **Comunidad de Codezone** , active y desactive los elementos adecuados.  
  
5.  Haga clic en **Aceptar**.  
  
#### <a name="to-search-msdn-online-and-sql-server-focused-communities-from-the-search-page"></a>Para buscar en MSDN Online y en comunidades centradas en SQL Server desde la página Buscar  
  
1.  En el menú **Ayuda**, haga clic en **Buscar**.  
  
2.  Escriba los términos de búsqueda en el cuadro de texto **Buscar** y, a continuación, haga clic en **Buscar**.  
  
Tanto si realiza la búsqueda mediante los filtros disponibles (tecnología, lenguaje y tipo de tema) como si no, ésta se ejecutará en todos los buscadores que haya seleccionado.  
  
## <a name="launching-help"></a>Iniciar la Ayuda  
Existen dos formas de mostrar la Ayuda desde [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. De forma predeterminada, cuando se abren los Libros en pantalla de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] desde [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)], lo hacen en una ventana externa independiente del entorno de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] . Esta ventana de los Libros en pantalla sigue estando asociada a [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)], puede responder a algunos eventos de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] y se cierra al cerrar [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]. La apertura de los Libros en pantalla de esta forma resulta muy útil cuando se están utilizando dos monitores: puede arrastrar la ventana de los Libros en pantalla al segundo monitor para que no interfiera con el trabajo que se está llevando a cabo en el primero y, al mismo tiempo, puede consultarla con facilidad.  
  
Los Libros en pantalla también se pueden abrir como una ventana dentro de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]. Esta forma es preferible cuando se dispone de un espacio limitado en la pantalla y se desea aprovechar [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] y su capacidad de ocultar ventanas.  
  
> [!NOTE]  
> Si desea que sean completamente independientes de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)], abra los Libros en pantalla de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] desde el menú **Inicio** . De esta forma, no reaccionarán a las acciones que lleve a cabo en el entorno de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] ni se cerrarán cuando salga de [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)].  
  
#### <a name="to-configure-help-and-sql-server-books-online-to-launch-inside-the-management-studio-window"></a>Para configurar la Ayuda y los Libros en pantalla de SQL Server de modo que se inicien dentro de la ventana Management Studio  
  
1.  En el menú **Herramientas** , haga clic en **Opciones**, expanda **Entorno**, expanda **Ayuda** y, a continuación, haga clic en **General**.  
  
2.  En el cuadro **Mostrar la Ayuda usando** , haga clic en **Visor de ayuda integrada**.  
  

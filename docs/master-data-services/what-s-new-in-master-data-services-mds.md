---
title: Novedades
description: Obtenga información sobre los cambios, las mejoras y las actualizaciones en la versión SQL Server 2019 de Master Data Services.
ms.custom: ''
ms.date: 07/08/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ad530f60-d480-4457-ba7a-93a10c8a1695
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 7c5a436e6958d89f26ec0836b82850bd914d6c41
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237141"
---
# <a name="what39s-new-in-master-data-services-mds"></a>Novedades de Master Data Services (MDS)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Este tema resume los cambios y actualizaciones en la versión [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. 
  
 Para información general sobre cómo organizar los datos en [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)], consulte [Introducción a Master Data Services](../master-data-services/master-data-services-overview-mds.md). 
  
 **Para instalar Master Data Services, configurar la base de datos y el sitio web e implementar los modelos de ejemplo, consulte** [Información general de Master Data Services (MDS)](../master-data-services/master-data-services-overview-mds.md).  
  
 **Descargar**  
  
-   Para descargar [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], vaya al  **[Centro de evaluación](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)** .  
  
-   ¿Tiene una cuenta de Azure?  Si es así, haga clic **[aquí](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2019-ws2019?tab=Overview)** para poner en marcha una máquina virtual con [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] ya instalado.  
  
##  <a name="improved-performance"></a>Mejorar el rendimiento  
  
 Las mejoras de rendimiento permiten crear modelos más grandes, cargar datos de manera más eficaz y obtener un mejor rendimiento general. Esto incluye la mejora del rendimiento del complemento para Microsoft Excel, que se ha optimizado para reducir los tiempos de carga de datos y habilitar el complemento para administrar entidades mayores.  
  
 Para obtener más información sobre el complemento para Microsoft Excel, vea [Complemento Master Data Services para Microsoft Excel](../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md).  
  
 Se incluyen las siguientes mejoras de características.  
  
-   Se ofrece la compresión de datos en el nivel de entidad, que está habilitada de forma predeterminada. Cuando la compresión de datos está habilitada, todas las tablas e índices relacionados con la entidad se comprimen con la compresión del nivel de fila de SQL. Esto reduce considerablemente la E/S del disco al leer o actualizar los datos maestros, sobre todo cuando los datos maestros tienen millones de filas o numerosas columnas de valores NULL.  
  
     Dado que el uso de CPU aumenta ligeramente por parte del motor de SQL Server, si tiene la CPU enlazada al servidor puede editar la entidad para desactivar la compresión de datos.  
  
     Para más información, consulte [Crear una entidad &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md)y [Compresión de datos](../relational-databases/data-compression/data-compression.md).  
  
-   La característica IIS de compresión de contenido dinámico está habilitada de forma predeterminada. Esto disminuye considerablemente el tamaño de la respuesta xml y reduce la E/S de red, aunque el uso de CPU aumenta. Si dispone de una CPU enlazada al servidor, puede agregar la configuración siguiente al archivo Web.config de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] para desactivar la compresión de datos.  
  
    ```  
    <configuration>  
       \<system.webServer>  
          <urlCompression doStaticCompression="true" doDynamicCompression="false " />  
       \</system.webServer>  
    </configuration>  
  
    ```  
  
     Para obtener más información, consulte [URL Compression](https://www.iis.net/configreference/system.webserver/urlcompression)(Compresión de URL).  
  
-   Los siguientes trabajos nuevos del Agente SQL Server indexan y registran el mantenimiento.  
  
    -   MDS_MDM_Sample_Index_Maintenace  
  
    -   MDS_MDM_Sample_Log_Maintenace  
  
 El trabajo MDS_MDM_Sample_Index_Maintenance se ejecuta semanalmente de forma predeterminada. Puede modificar la programación. También puede ejecutar manualmente el trabajo en cualquier momento mediante el procedimiento almacenado udpDefragmentation. Se recomienda ejecutar el procedimiento almacenado cada vez que se inserte o se actualice un gran volumen de datos maestros, o bien después de crear una versión nueva a partir de la versión existente.  
  
 Se vuelve a generar en línea un índice con más de un 30 % de fragmentación. Durante la regeneración, el rendimiento se ve afectado en la operación CRUD en la misma tabla. Si la degradación del rendimiento es un problema, se recomienda que ejecute el procedimiento almacenado fuera del horario laboral. Para obtener más información acerca de la fragmentación de índices, vea [Reorganize and Rebuild Indexes](../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
 Para obtener más información, consulte esta publicación en el blog de Master Data Services, [Performance and Scale Improvement in SQL Server 2016](https://techcommunity.microsoft.com/t5/sql-server-integration-services/performance-and-scale-improvement-in-sql-server-2016/ba-p/388225)(Mejora del rendimiento y la escala en SQL Server 2016).  
  
##  <a name="improved-security"></a>Seguridad mejorada  
  
 El nuevo permiso de función de superusuario proporciona a un usuario o a un grupo los mismos permisos de los que dispone el administrador del servidor en la versión anterior de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. El permiso de superusuario puede asignarse a varios usuarios y grupos. En la versión anterior, el usuario que instaló originalmente [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] era el administrador del servidor y era difícil transferir este permiso a otro usuario o grupo. Para obtener más información, consulte [Permisos del área funcional &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
 Ahora es posible asignar explícitamente a un usuario el permiso de administrador en el nivel de modelo. Esto significa que si más adelante se le asignan permisos al usuario en el subárbol del modelo, como el nivel de entidad, no perderá el permiso de administrador.  
  
 En esta versión de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], se proporcionan más niveles de permisos mediante la introducción de los siguientes permisos nuevos: lectura, creación, actualización y eliminación. Por ejemplo, un usuario que solo tenga el permiso de actualización, ahora puede actualizar los datos maestros sin crear ni eliminar los datos. Cuando se le concede a un usuario el permiso de creación, actualización y eliminación, se le asigna automáticamente el permiso de lectura. También puede combinar los permisos de lectura, creación, actualización y eliminación.  
  
 Cuando se actualiza a [!INCLUDE[sssql15-md](../includes/sssql16-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], los permisos anteriores se convierten en permisos nuevos, tal como se muestra en la tabla siguiente.  
  
|Permiso en la versión anterior|Permiso nuevo|  
|------------------------------------|--------------------|  
|El usuario que instala originalmente [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] tiene el permiso de administrador del servidor.|El usuario tiene el permiso de función de superusuario.|  
|El usuario tiene permisos de actualización en el nivel de modelo y ningún permiso en el subárbol de modelo, por lo que implícitamente es administrador del modelo.|El usuario tiene permisos explícitos de administrador en el nivel de modelo.|  
|El usuario tiene permisos de solo lectura.|El usuario tiene permisos de acceso de lectura.|  
|El usuario tiene permisos de actualización.|El usuario tiene los cuatro permisos de acceso: creación, actualización, eliminación y lectura.|  
|El usuario tiene permisos de denegación.|El usuario tiene permisos de denegación.|  
  
 Para más información sobre los permisos, consulte [Seguridad &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md).  
  
##  <a name="improved-transaction-log-maintenance"></a>Mantenimiento del registro de transacciones mejorado  
  
 Ahora puede limpiar los registros de transacciones a intervalos predeterminados o según una programación mediante la configuración del sistema y en el nivel de modelo. En el caso de un sistema MDS con numerosos cambios de datos y procesos ETL, estas tablas pueden aumentar exponencialmente y causar problemas relacionados con la degradación del rendimiento y el espacio de almacenamiento.  
  
 Se pueden quitar de los registros los siguientes tipos de datos.  
  
-   El historial de transacciones anterior a un número de días especificado.  
  
-   El historial de problemas de validación anterior a un número de días especificado.  
  
-   Los lotes de almacenamiento provisional que se ejecutaron antes de un número de días especificado.  
  
 Puede configurar la frecuencia con la que se quitan los datos de los registros de transacciones mediante la configuración del sistema y en el nivel de modelo. Para más información, consulte [Configuración del sistema &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md)y [Crear un modelo &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md). Para obtener más información sobre las transacciones, consulte [Transacciones &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md).  
  
 El trabajo MDS_MDM_Sample_Log_Maintenace del Agente SQL Server desencadena la limpieza de los registros de transacciones y se ejecuta todas las noches. Puede usar el Agente SQL Server para modificar la programación de este trabajo.  
  
 También puede llamar a procedimientos almacenados para limpiar los registros de transacciones. Para obtener más información, consulte [Transacciones &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md).  
  
## <a name="improved-troubleshooting"></a>Mejora en la solución de problemas  
  
 En [!INCLUDE[sssql15-md](../includes/sssql16-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], se agregaron características para mejorar la depuración y facilitar la solución de problemas. Para más información, consulte [Seguimiento &#40;Master Data Services&#41;](../master-data-services/tracing-master-data-services.md).  
  
## <a name="improved-manageability"></a>Mejora en la capacidad de administración  
  
 Las mejoras en la facilidad de uso ayudan a reducir los costos de mantenimiento y a optimizar la rentabilidad de la inversión (ROI). Entre estas mejoras se incluyen el mantenimiento del registro de transacciones y las mejoras en la seguridad, así como las siguientes características nuevas.  
  
-   Uso de nombres de atributo de más de 50 caracteres.  
  
-   Cambio del nombre y ocultación de los atributos Name y Code.  
  
 Para obtener más información, vea los siguientes temas.  
  
-   [Modelos &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)  
  
-   [Entidades &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)  
  
-   [Transacciones &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md)  
  
-   [Seguridad &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md)  

## <a name="business-rule-improvements"></a>Mejoras de reglas de negocios
 **Administración de reglas de negocios (complemento MDS para Excel)**  
  
 En el complemento Master Data Services para Excel puede administrar reglas de negocios, como la creación y la edición de reglas de negocios. Las reglas de negocios se usan para validar los datos.  
 
 **Business Rules Extension**  
  
 Puede aplicar scripts SQL definidos por el usuario como una extensión de las acciones y las condiciones de las reglas de negocios. Las funciones SQL pueden usarse como una condición. Los procedimientos almacenados SQL pueden usarse como una acción. Para más información, consulte [Ejecución de reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-extension-master-data-services.md). 
 
 **Experiencia rediseñada de administración de reglas de negocios**  
  
 Se ha rediseñado completamente la experiencia de administración de reglas de negocios en MDS para mejorarla. Para más información sobre esta característica, consulte [Reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md).  
  
 **Eliminación de la funcionalidad de administración de reglas de negocio del complemento MDS para Excel**  
  
 La funcionalidad de administración de reglas de negocios se ha quitado del complemento MDS para Excel porque la experiencia se ha rediseñado.    

 **Nuevas condiciones de reglas de negocios**  
  
 Se han agregado siete nuevas condiciones de reglas de negocios para proporcionar un conjunto completo de condiciones. Para más información, consulte [Condiciones de reglas de negocios &#40;Master Data Services&#41;](../master-data-services/business-rule-conditions-master-data-services.md).  

## <a name="derived-hierarchy-improvements"></a>Mejoras de la jerarquía derivada

 **Relaciones varios a varios en jerarquías derivadas**  
  
 Ahora puede crear una jerarquía derivada que muestre las relaciones varios a varios. Se puede modelar una relación varios a varios entre dos entidades mediante el uso de una entidad de terceros que proporcione una asignación entre ellas. La entidad de asignación es una entidad que tiene dos o más atributos basados en dominio que hacen referencia a otras entidades.  
  
 Por ejemplo, la entidad M tiene un atributo basado en dominio que hace referencia a A y un atributo basado en dominio que hace referencia a B. Puede crear una jerarquía de A a B mediante la entidad de asignación.  
  
 Para más información, consulte [Visualización de relaciones varios a varios en jerarquías derivadas &#40;Master Data Services&#41;](../master-data-services/show-many-to-many-relationships-in-derived-hierarchies-master-data-services.md)  
 
 **Edición de relaciones varios a varios en las jerarquías derivadas**  
  
 Puede editar la relación varios a varios mediante la modificación de los miembros de la entidad de asignación. Para obtener más información, vea [Mostrar relaciones varios a varios en jerarquías derivadas &#40;Master Data Services&#41;](../master-data-services/show-many-to-many-relationships-in-derived-hierarchies-master-data-services.md).  
 
 **Mejora en la experiencia de administración de la jerarquía derivada**  
  
 Se ha mejorado la experiencia de administración de la jerarquía derivada en MDS. Para más información sobre esta característica, consulte [Crear una jerarquía derivada &#40;Master Data Services&#41;](../master-data-services/create-a-derived-hierarchy-master-data-services.md).  
  
 La funcionalidad de administración de reglas de negocios se ha quitado del complemento MDS para Excel porque la experiencia se ha rediseñado.  
 
## <a name="attribute-improvements"></a>Mejoras de atributos   
    
 **Índices personalizados**  
  
 Puede crear un índice no clúster en un atributo (índice único) o en una lista de atributos (índice compuesto) en una entidad para ayudar a mejorar el rendimiento de las consultas. Para obtener más información, consulte [Índice personalizado &#40;Master Data Services&#41;](../master-data-services/custom-index-master-data-services.md).  
 
  **Filtros de atributo**  
  
 En el caso de un atributo basado en dominio, para un miembro hoja, puede usar un atributo primario de filtro para restringir los valores permitidos para el atributo basado en dominio. Para obtener más información, consulte [Crear un atributo basado en dominio &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md).  
 
## <a name="entity-and-member-improvements"></a>Mejoras de entidades y miembros 
  
 **Relación de sincronización de entidades**  
  
 Puede compartir datos de entidad entre diferentes modelos mediante la creación de una relación de sincronización de entidades. Para más información, consulte [Relación de sincronización de entidades &#40;Master Data Services&#41;](../master-data-services/entity-sync-relationship-master-data-services.md).  
  
 **Purga de los miembros eliminados temporalmente**  
  
 Ahora puede purgar (eliminar de forma permanente) todos los miembros eliminados temporalmente en una versión de modelo. La eliminación de un miembro solo lo desactiva, es decir, lo elimina temporalmente. Para más información, consulte [Purgar miembros de la versión &#40;Master Data Services&#41;](../master-data-services/purge-version-members-master-data-services.md).  
 
## <a name="improvements-for-managing-changes"></a>Mejoras para administrar los cambios 
  
 **Historial de revisiones de miembro**  
  
 Cuando se cambia un miembro, se registra un historial de revisiones de miembro. Puede revertir un historial de revisiones, así como ver y anotar las revisiones. Mediante la propiedad **Log Retention Days** , puede especificar durante cuánto tiempo se conservan los datos históricos. Para más información, consulte [Historial de revisiones de miembro &#40;Master Data Services&#41;](../master-data-services/member-revision-history-master-data-services.md).  
  
 **Conflictos de combinación**  
  
 Si intenta publicar los datos que ha cambiado otro usuario, se producirá un error de conflicto en la publicación. Para resolver este error, puede ejecutar los conflictos de fusión mediante combinación y volver a publicar los cambios. Para más información, consulte [Conflictos de combinación (Master Data Services)](../master-data-services/merge-conflicts-master-data-services.md) y [Conflictos de combinación (complemento MDS para Excel)](../master-data-services/microsoft-excel-add-in/merge-conflicts-mds-add-in-for-excel.md).  
  
 **Conjuntos de cambios**  
  
 Puede usar conjuntos de cambios para guardar los cambios pendientes en una entidad. Además, puede ver y modificar los cambios pendientes. Si la entidad requiere la aprobación de los cambios, debe guardar los cambios pendientes en un conjunto de cambios y enviarlo para recibir la aprobación del administrador. Para obtener más información, vea [conjuntos de cambios &#40;Master Data Services&#41;](../master-data-services/changesets-master-data-services.md).  
  
 **Correo electrónico y administración del conjunto de cambios**  
  
 En esta versión, ya puede ver y administrar todos los cambios por modelo y versión. También puede recibir notificaciones por correo electrónico cada vez que cambie el estado de un conjunto de cambios de una entidad que requiere aprobación. Para más información, consulte [Administración de conjuntos de cambios &#40;Master Data Services&#41;](../master-data-services/manage-changesets-master-data-services.md) y [Notificaciones &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md).  
  
 **Visualización y administración del historial de revisiones**  
  
 Puede ver y administrar el historial de revisiones por entidad y miembro. Si tiene permisos de actualización, puede revertir un miembro a una versión anterior. Para más información, consulte [Historial de revisiones de miembro &#40;Master Data Services&#41;](../master-data-services/member-revision-history-master-data-services.md).  
 
## <a name="tool-and-sample-improvements"></a>Mejoras de ejemplos y herramientas 
  
 **Guardar o abrir archivos de consulta en el complemento MDS para Excel**  
  
 En la página del Explorador de entidades, puede hacer clic en **Excel** para guardar los archivos de consulta de acceso directo. También puede abrir el archivo de consulta almacenado en el equipo en el complemento MDS para Excel. El archivo guardado se puede abrir con la aplicación QueryOpener. Para más información, consulte [Archivos de consulta de acceso directo &#40;Complemento MDS para Excel&#41;](../master-data-services/microsoft-excel-add-in/shortcut-query-files-mds-add-in-for-excel.md).  
  
 El archivo de consulta contiene los filtros y la información de la jerarquía de la página del explorador.  
   
 **Actualización de los paquetes de implementación de modelos de ejemplo**  
  
 Los paquetes de ejemplo se han actualizado para admitir nuevos escenarios. Para más información, consulte [Ejemplos de SQL Server: paquetes de implementación de modelos (MDS)](../master-data-services/sql-server-samples-model-deployment-packages-mds.md).  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
  
## <a name="see-also"></a>Consulte también  
 [Características de Master Data Services y Data Quality Services compatibles con las ediciones de SQL Server 2016](../master-data-services/master-data-services-and-data-quality-services-features-support.md)  
 [Características en desuso de Master Data Services](../master-data-services/deprecated-master-data-services-features.md)  
 [Características descontinuadas de Master Data Services](../master-data-services/discontinued-master-data-services-features.md)

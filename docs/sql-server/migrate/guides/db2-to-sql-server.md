---
title: 'Guía de migración: DB2 para SQL Server'
description: Siga esta guía para migrar el servidor DB2 a SQL Server.
ms.custom: ''
ms.date: 08/17/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.topic: conceptual
helpviewer_keywords:
- processors [SQL Server], supported
- number of processors supported
- maximum number of processors supported
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8f07ae68309e78316ad7dc310ea5a0118e734f0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341641"
---
# <a name="migration-guide-db2-to-sql-server"></a>Guía de migración: DB2 para SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

Con esta guía de migración aprenderá a migrar bases de datos de usuario de DB2 a SQL Server mediante SQL Server Migration Assistant para DB2. 

Para ver otras guías de migración, consulte [Migración de bases de datos](https://datamigration.microsoft.com/). 


## <a name="prerequisites"></a>Requisitos previos

Para migrar la base de datos de DB2 a SQL Server, necesita:

- Comprobar que el entorno de origen es compatible.
- [SQL Server Migration Assistant (SSMA) para DB2](https://www.microsoft.com/download/details.aspx?id=54254).



## <a name="pre-migration"></a>Antes de la migración

Una vez cumplidos los requisitos previos, estará a punto para detectar la topología de su entorno y evaluar la viabilidad de la migración. 

### <a name="assess"></a>Evaluar 

Cree una evaluación mediante SQL Server Migration Assistant (SSMA). 

Para crear una evaluación, siga estos pasos:

1. Abra SQL Server Migration Assistant (SSMA) para DB2. 
1. Seleccione **Archivo** y, luego, elija **Nuevo proyecto**. 
1. Facilite el nombre del proyecto y una ubicación para guardarlo y, luego, seleccione un destino de la migración de SQL Server en la lista desplegable. Seleccione **Aceptar**. 

   :::image type="content" source="media/db2-to-sql-server/new-project.png" alt-text="Especifique los detalles del proyecto y seleccione Aceptar para guardarlos.":::


1. Escriba los valores de los detalles de la conexión de DB2 en el cuadro de diálogo **Conectarse a DB2**. 

   :::image type="content" source="media/db2-to-sql-server/connect-to-db2.png" alt-text="Conexión a la instancia de DB2":::


1. Haga clic con el botón derecho en el esquema de DB2 que quiera migrar y, luego, elija **Crear informe**. Se generará un informe HTML. También puede elegir **Crear informe** en la barra de navegación después de seleccionar el esquema. 

   :::image type="content" source="media/db2-to-sql-server/create-report.png" alt-text="Haga clic con el botón derecho en el esquema y elija Crear informe.":::

1. Revise el informe en HTML para comprender las estadísticas de conversión, y los errores o advertencias. También puede abrir el informe en Excel para obtener un inventario de objetos DB2 y conocer el esfuerzo necesario para realizar las conversiones de esquema. La ubicación predeterminada del informe es la carpeta de informes dentro de SSMAProjects.

   Por ejemplo: `drive:\<username>\Documents\SSMAProjects\MyDB2Migration\report\report_<date>`. 

   :::image type="content" source="media/db2-to-sql-server/report.png" alt-text="Revise el informe para identificar los errores o advertencias.":::


### <a name="validate-data-types"></a>Validación de los tipos de datos

Valide las asignaciones predeterminadas de los tipos de datos y cámbielas según los requisitos, si es necesario. Para hacerlo, siga estos pasos: 

1. Seleccione **Herramientas** en el menú principal. 
1. Seleccione **Configuración del proyecto**. 
1. Seleccione la pestaña **Type mappings** (Asignaciones de tipos). 

   :::image type="content" source="media/db2-to-sql-server/type-mapping.png" alt-text="Seleccione el esquema y la asignación de tipos.":::

1. Puede cambiar la asignación de tipos de cada tabla seleccionando la tabla en el **Explorador de metadatos de DB2**. 

### <a name="convert-schema"></a>Convertir esquema 

Para convertir el esquema, siga estos pasos:

1. (Opcional) Agregue consultas dinámicas o ad hoc a las instrucciones. Haga clic con el botón derecho en el nodo y elija **Add statements** (Agregar instrucciones). 
1. Seleccione **Conectarse a SQL Server**. 
    1. Escriba los detalles de la conexión para conectarse a la instancia de SQL Server. 
    1. Elija esta opción para conectarse a una base de datos existente en el servidor de destino o especifique un nuevo nombre para crear una base de datos en el servidor de destino. 
    1. Seleccione **Conectar**. 

   :::image type="content" source="media/db2-to-sql-server/connect-to-sql-server.png" alt-text="Rellene los detalles para conectarse a SQL Server.":::


1. Haga clic con el botón derecho en el esquema y elija **Convertir esquema**. También puede elegir **Convertir esquema** en la barra de navegación superior después de seleccionar el esquema. 

   :::image type="content" source="media/db2-to-sql-server/convert-schema.png" alt-text="Haga clic con el botón derecho en el esquema y elija Convertir esquema.":::

1. Una vez finalizada la conversión, compare y revise la estructura del esquema para identificar posibles problemas y solucionarlos en función de las recomendaciones. 

   :::image type="content" source="media/db2-to-sql-server/compare-review-schema-structure.png" alt-text="Compare y revise la estructura del esquema para identificar posibles problemas y solucionarlos en función de las recomendaciones.":::

1. Guarde el proyecto localmente para un ejercicio de corrección de esquema sin conexión. Seleccione **Guardar proyecto** en el menú **Archivo**. 


## <a name="migrate"></a>Migrar

Cuando haya terminado de evaluar las bases de datos y de resolver las discrepancias, el siguiente paso consiste en ejecutar el proceso de migración.

Para publicar el esquema y migrar los datos, siga estos pasos:

1. Publique el esquema: haga clic con el botón derecho en la base de datos en el nodo **Bases de datos** del **Explorador de metadatos de SQL Server** y elija **Sincronizar con base de datos**.

   :::image type="content" source="media/db2-to-sql-server/synchronize-with-database.png" alt-text="Haga clic con el botón derecho en la base de datos y elija Sincronizar con base de datos.":::

1. Migre los datos: haga clic con el botón derecho en el **Explorador de metadatos de DB2** y elija **Migrar datos**. 

   :::image type="content" source="media/db2-to-sql-server/migrate-data.png" alt-text="Haga clic con el botón derecho en el esquema y elija Migrar datos":::

1. Especifique los detalles de la conexión para las instancias de DB2 y SQL Server. 
1. Vea el **Informe de migración de datos**. 

   :::image type="content" source="media/db2-to-sql-server/data-migration-report.png" alt-text="Revise el informe de migración de datos.":::

1. Conéctese a la instancia de SQL Server mediante SQL Server Management Studio, y revise los datos y el esquema para validar la migración. 

   :::image type="content" source="media/db2-to-sql-server/compare-schema-in-ssms.png" alt-text="Compare el esquema en SSMS.":::

## <a name="post-migration"></a>Después de la migración 

Cuando haya completado correctamente la fase de migración, deberá realizar una serie de tareas posteriores para asegurarse de que todo funcione de la forma más fluida y eficaz posible.

### <a name="remediate-applications"></a>Corrección de las aplicaciones 

Cuando se hayan migrado los datos al entorno de destino, todas las aplicaciones que antes utilizaban el origen deben empezar a utilizar el destino. Lograr esto requerirá en algunos casos realizar cambios en las aplicaciones.

### <a name="perform-tests"></a>Realización de pruebas

El método de prueba para la migración de bases de datos consta de las siguientes actividades:

1. **Desarrollar pruebas de validación**: para probar la migración de bases de datos, debe utilizar consultas SQL. Debe crear las consultas de validación para que se ejecuten en las bases de datos de origen y destino. Las consultas de validación deben abarcar el ámbito definido.
1. **Configurar un entorno de prueba**: el entorno de prueba debe contener una copia de la base de datos de origen y la base de datos de destino. Asegúrese de aislar el entorno de prueba.
1. **Ejecutar pruebas de validación**: ejecute las pruebas de validación en el origen y el destino y, luego, analice los resultados.
1. **Ejecutar pruebas de rendimiento**: ejecute la prueba de rendimiento en el origen y el destino y, luego, analice y compare los resultados.

   > [!NOTE]
   > Para obtener ayuda sobre el desarrollo y la ejecución de pruebas de validación tras la migración, tenga en cuenta la solución de calidad de datos [QuerySurge](https://www.querysurge.com/company/partners/microsoft). 

## <a name="migration-assets"></a>Recursos de migración 

Para obtener más ayuda, consulte los siguientes recursos, que se desarrollaron como apoyo de la participación en un proyecto de migración en el mundo real:

|Recurso  |Descripción  |
|---------|---------|
|[Herramienta y modelo de evaluación de la carga de trabajo de datos](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Esta herramienta proporciona las plataformas de destino de ajuste perfecto sugeridas, la preparación para la nube, y el nivel de corrección de la aplicación o base de datos para una carga de trabajo determinada. Ofrece un cálculo sencillo con un solo clic y una función de generación de informes que ayuda a acelerar las evaluaciones de grandes volúmenes, ya que proporciona un proceso de toma de decisiones de plataforma de destino uniforme y automatizado.|
|[Paquete de detección y evaluación de recursos de datos de DB2 zOS](https://github.com/Microsoft/DataMigrationTeam/tree/master/DB2%20zOS%20Data%20Assets%20Discovery%20and%20Assessment%20Package)|Después de ejecutar el script de SQL en una base de datos, puede exportar los resultados a un archivo en el sistema de archivos. Se admiten varios formatos de archivo, incluido *.csv, para que se puedan capturar los resultados en herramientas externas, como hojas de cálculo. Este método puede resultar útil si quiere compartir fácilmente los resultados con equipos que no tengan el área de trabajo instalada.|
|[Artefactos y scripts de inventario de IBM DB2 LUW](https://github.com/Microsoft/DataMigrationTeam/tree/master/IBM%20DB2%20LUW%20Inventory%20Scripts%20and%20Artifacts)|Este recurso incluye una consulta SQL que accede a las tablas del sistema IBM DB2 LUW, versión 11.1, y proporciona un recuento de objetos por esquema y tipo de objeto, una estimación aproximada de datos sin procesar en cada esquema y el tamaño de las tablas de cada esquema, cuyos resultados se almacenan en formato CSV.|
|[Escalado puro de DB2 LUW en Azure: guía de instalación](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/DB2%20PureScale%20on%20Azure.pdf)|Esta guía sirve como punto de partida para un plan de implementación de DB2. Aunque sus requisitos empresariales varíen, se aplica el mismo patrón básico. Este patrón de arquitectura también se puede utilizar para aplicaciones OLAP en Azure.|

Estos recursos se desarrollaron como parte del programa Ninja SQL de datos, que está patrocinado por el equipo de ingeniería de grupos de datos de Azure. El objetivo principal del programa Ninja SQL de datos es lograr y acelerar la modernización compleja, y competir por las oportunidades de migración de las plataformas de datos a la de Azure, de Microsoft. Si cree que su organización estaría interesada en participar en el programa Ninja SQL de datos, póngase en contacto con su equipo de cuentas y pídale que le envíe una nominación.

## <a name="next-steps"></a>Pasos siguientes

Después de la migración, revise la [Guía de optimización y validación posterior a la migración](../../../relational-databases/post-migration-validation-and-optimization-guide.md). 

Para obtener una matriz de los servicios y herramientas de Microsoft y de otros fabricantes que están disponibles para ayudarle en diversos escenarios de migración de datos y bases de datos, además de las tareas especializadas, consulte [Servicios y herramientas de migración de datos](/azure/dms/dms-tools-matrix).

Para ver otras guías de migración, consulte [Migración de bases de datos](https://datamigration.microsoft.com/). 

En cuanto al contenido de vídeo, consulte:
- [Procedimiento para usar la guía de migración de bases de datos](https://azure.microsoft.com/resources/videos/how-to-use-the-azure-database-migration-guide/)
- [Información general sobre el proceso de migración](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)

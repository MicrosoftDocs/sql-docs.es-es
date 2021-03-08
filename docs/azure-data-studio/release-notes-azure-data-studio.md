---
title: Notas de la versión de Azure Data Studio
description: Este artículo contiene notas de las versiones de Azure Data Studio desde noviembre de 2017 hasta el momento actual. Muchos de los problemas resumidos incluyen vínculos para obtener detalles adicionales.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: seodec18
ms.date: 2/18/2021
ms.openlocfilehash: c666ae644200c1b2beefb7f73c8a819294831985
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836032"
---
# <a name="release-notes-for-azure-data-studio"></a>Notas de la versión de Azure Data Studio

**[Descargue e instale la versión más reciente](./download-azure-data-studio.md)** .

## <a name="february-2021"></a>Febrero de 2021

18 de febrero de 2021 &nbsp; / &nbsp; versión 1.26.0

&nbsp;

| Change | Detalles |
| ------ | ------- |
| Correcciones de errores | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22February+2021+Release%22+is%3Aclosed). |
| Actualización de extensiones | [Dacpac](extensions/sql-server-dacpac-extension.md) <br/> [Kusto (KQL)](extensions/kusto-extension.md) </br> [MachineLearning](extensions/machine-learning-extension.md) </br> [Generador de perfiles](extensions/sql-server-profiler-extension.md) </br> [SchemaCompare](extensions/schema-compare-extension.md) </br> [SQLDatabaseProjects](extensions/sql-database-project-extension.md) |
| Nuevas características de Azure Arc | Ahora se admiten varios controladores de datos. <br/> Opciones del cuadro de diálogo de nueva conexión como el archivo kube config <br/> Mejoras del panel de Postgres |
| Nuevas características de los cuadernos | Mejora del tiempo de inicio del servidor de Jupyter en un 50 % en Windows <br/> Se agregó compatibilidad para editar los libros de Jupyter mediante el clic con el botón derecho. <br/> Se agregó compatibilidad con la parametrización del cuaderno de URI, así como la [documentación de parametrización del cuaderno](https://docs.microsoft.com/sql/azure-data-studio/notebooks/notebooks-parameterization). |

## <a name="december-2020-hotfix"></a>Diciembre de 2020 (revisión)

Versión del &nbsp; / &nbsp; 10 de febrero de 2021: 1.25.3

| Change | Detalles |
| ------ | ------- |
| Corrección del error [nº 13899](https://github.com/microsoft/azuredatastudio/issues/13899) | Desplazamiento a los vínculos de referencia cruzada adecuados en Notebooks |
| Actualización de Electron para incorporar correcciones de errores importantes| N/D |

## <a name="december-2020"></a>Diciembre de 2020

9 de diciembre de 2020 &nbsp; / &nbsp; versión: 1.25.0

&nbsp;

| Change | Detalles |
| ------ | ------- |
| Correcciones de errores | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22December+2020+Release%22). |
| Actualización de la extensión Database Projects | Se han agregado áreas de trabajo y se ha mejorado la barra lateral. |

## <a name="november-2020"></a>Noviembre de 2020

12 de noviembre de 2020 &nbsp; / &nbsp; Versión: 1.24.0

&nbsp;

| Change | Detalles |
| ------ | ------- |
| Correcciones de errores | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2020+Release%22+is%3Aclosed). |
| Cuadro de diálogo de conexión | Se ha agregado una nueva pestaña para examinar en el cuadro de diálogo de conexión. |
| Actualización de extensiones | Se ha publicado la actualización de la extensión Postgres. |
| Nuevas características de los cuadernos | Se han agregado nuevas características a la compatibilidad con SQL y Notebook. <br/> Se han agregado nuevas características a la compatibilidad con la parametrización de Notebook. <br/>  Se han agregado nuevas características al streaming de resultados para cuadernos SQL. |
| Instalación de Python | El paquete PROSE se ha quitado de la instalación predeterminada de Python. |

### <a name="known-issues-1240"></a>Incidencias conocidas (1.24.0)

| Nuevo elemento | Detalles | Solución alternativa |
|----------|---------|------------|
| Extensión Azure Arc | [Problema conocido:](https://github.com/microsoft/azuredatastudio/issues/13319) El botón "Script a Notebook" para implementaciones de Arc MIAA & PG no realiza la validación de campos antes de generar scripts para el cuaderno. Esto significa que si los usuarios escriben una contraseña incorrecta en las entradas de confirmación de contraseña, es posible que terminen con un cuaderno que tenga el valor incorrecto para la contraseña.| El botón "Implementar" funciona según lo previsto, por lo que los usuarios deben utilizarlo en su lugar. |
| Explorador de objetos | Las versiones de ADS anteriores a 1.24.0 tienen un cambio importante en el explorador de objetos debido a los cambios del motor relacionados con el [grupo de SQL sin servidor de Azure Synapse Analytics](/azure/synapse-analytics/sql/on-demand-workspace-overview). | Para seguir usando el explorador de objetos en Azure Data Studio con el grupo de SQL sin servidor de Azure Synapse Analytics, debe usar Azure Data Studio 1.24.0 o posterior. |

Puede consultar los [comentarios de Azure Data Studio](https://github.com/microsoft/azuredatastudio) para obtener otras incidencias conocidas y proporcionar comentarios al equipo del producto.

## <a name="october-2020"></a>Octubre de 2020

14 de octubre de 2020 &nbsp; / &nbsp; versión: 1.23.0

&nbsp;

| Change | Detalles |
| ------ | ------- |
| Azure SQL Edge | Compatibilidad con objetos de Azure SQL Edge. |
| Correcciones de errores | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is:issue+milestone:%22October+2020+Release%22+is:closed). |
| Bases de datos| Compatibilidad con la misma referencia de base de datos. |
| Actualizaciones de extensiones | [Azure Arc](extensions/azure-arc-extension.md)</br>[azdata](../azdata/install/deploy-install-azdata.md)</br>[Machine Learning](extensions/machine-learning-extension.md)</br>[Kusto (KQL)](extensions/kusto-extension.md)</br>[Comparación de esquemas](extensions/schema-compare-extension.md)</br>Evaluación de SQL</br>[Proyectos de SQL Database](extensions/sql-database-project-extension.md)</br>[Importación de SQL Server](extensions/sql-server-import-extension.md) |
| Nuevas características de implementación | Se han agregado las implementaciones de Azure SQL Database y VM. |
| PowerShell | Se ha agregado compatibilidad con streaming de resultados del kernel de PowerShell. |

## <a name="september-2020-hotfix"></a>Septiembre de 2020 (revisión)

30 de septiembre de 2020; versión: 1.22.1

&nbsp;

| Change | Detalles |
| ------ | ------- |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/releases/tag/1.22.1). |

## <a name="september-2020"></a>Septiembre de 2020

22 de septiembre de 2020 &nbsp; / &nbsp; versión: 1.22.0

&nbsp;

| Change | Detalles |
| ------ | ------- |
| Nuevas características de los cuadernos | <br/> &bull; &nbsp; Admite la nueva experiencia de edición de celdas de texto basada en el formato de texto enriquecido y la conversión simplificada a Markdown, lo que también se conoce como barra de herramientas WYSIWYG (lo que se ve es lo que se imprime) <br/> &bull; &nbsp; Admite el kernel de Kusto <br/> &bull; &nbsp; Admite el anclaje de cuadernos <br/> &bull; &nbsp; Se ha agregado compatibilidad con la nueva versión de Jupyter Books <br/> &bull; &nbsp; Accesos directos de Jupyter mejorados <br/> &bull; &nbsp; Se han incorporado mejoras en la carga de rendimiento |
| Extensión SQL Database Projects | La extensión SQL Database Projects incorpora el desarrollo de bases de datos basadas en proyectos a Azure Data Studio. En esta versión preliminar, se pueden crear y publicar proyectos de SQL desde Azure Data Studio. |
| Extensión Kusto (KQL) | Incorpora a Azure Data Studio experiencias nativas de Kusto para la exploración y el análisis de datos en una enorme cantidad de datos de streaming en tiempo real almacenados en Azure Data Explorer. Esta versión preliminar admite la conexión a clústeres de Azure Data Explorer y su examen, la escritura de consultas de KQL y la creación de cuadernos con el kernel de Kusto. |
| Extensión Azure Arc | Los usuarios pueden probar la versión preliminar pública de Azure Arc mediante Azure Data Studio. Esto incluye: <br/> &bull; &nbsp; Implementación de controladores de datos <br/> &bull; &nbsp; Implementación de Postgres <br/> &bull; &nbsp; Implementación de Managed Instance para Azure Arc <br/> &bull; &nbsp; Conexión a un controlador de datos <br/> &bull; &nbsp; Acceso a los paneles del servicio de datos <br/> &bull; &nbsp; Libro de Jupyter de Azure Arc |
| Opciones de implementación | <br/> &bull; &nbsp; Azure SQL Database Edge <br/> (Edge requiere la extensión de implementación de Azure SQL Edge) |
| Disponibilidad general de la extensión de importación de SQL Server | Anuncio de la disponibilidad general de la extensión de importación de SQL Server, cuyas características ya no están en versión preliminar. Esta extensión facilita la importación de archivos csv/txt. Obtenga más información sobre la extensión en [este artículo](./extensions/sql-server-import-extension.md). |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22September+2020+Release%22+is%3Aclosed). |

## <a name="august-2020"></a>Agosto de 2020

12 de agosto de 2020 &nbsp; / &nbsp; versión: 1.21.0

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Nuevas características de los cuadernos | &bull; &nbsp; Mover ubicaciones de celda <br/> &bull; &nbsp; Convertir celdas en celda de texto o celda de código
| Selector de Jupyter Books | Los usuarios ya pueden seleccionar Jupyter Books en las versiones de GitHub y abrir sin problemas en Azure Data Studio |
| Búsqueda agregada al viewlet de cuadernos | Los usuarios pueden buscar fácilmente en el contenido de sus cuadernos y Jupyter Books |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22August+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="july-2020-hotfix"></a>Julio de 2020 (revisión, hotfix)

17 de julio de 2020 &nbsp; / &nbsp; versión: 1.20.1

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Corrección del error #11372. Al arrastrar y colocar una tabla en el Explorador de objetos, los nombres de tabla se ajustan de manera incorrecta. | [#11372](https://github.com/microsoft/azuredatastudio/issues/11372) |
| Corrección del error #11356. El tema oscuro es ahora el tema predeterminado. | [#11356](https://github.com/microsoft/azuredatastudio/issues/11356) |
| &nbsp; | &nbsp; |

### <a name="known-issue"></a>Problema conocido

- Algunos usuarios han detectado errores de conexión del nuevo Microsoft.Data.SqlClient v2.0.0 incluido en esta versión. Los usuarios han comprobado que las [siguientes instrucciones](https://github.com/microsoft/azuredatastudio/issues/11367#issuecomment-659614111) permiten conectarse correctamente.

## <a name="july-2020"></a>Julio de 2020

15 de julio de 2020 &nbsp; / &nbsp; versión: 1.20.0

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Nueva guía de características agregada | En la página de bienvenida y en la paleta de comandos, los usuarios ahora pueden iniciar una guía de características para obtener un tutorial de las características más usadas, como el viewlet de conexiones, el viewlet de cuadernos y el marketplace de extensiones. |
| Nuevas características de los cuadernos | &bull; &nbsp; Compatibilidad con encabezados en la barra de herramientas de Markdown<br/> &bull; &nbsp; Vista previa de Markdown en paralelo en las celdas de texto
| Posibilidad de arrastrar y colocar columnas y tablas en el editor de consultas | Los usuarios ahora pueden arrastrar y colocar directamente columnas y tablas del viewlet de conexiones al editor de consultas. |
| Icono de la cuenta de Azure agregado a la barra de actividades | Ahora los usuarios pueden ver fácilmente dónde iniciar sesión en Azure |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22July+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="june-2020"></a>Junio de 2020

15 de junio de 2020 &nbsp; / &nbsp; versión: 1.19.0

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Adición de Azure Data Studio a la integración de Azure Portal | Ahora los usuarios pueden iniciar directamente Azure Portal desde una conexión de Azure SQL Database, Azure Postgres, etc. |
| Nuevas características de los cuadernos | &bull; &nbsp; Nueva barra de herramientas Cuaderno <br/> &bull; &nbsp; Nueva barra de herramientas Editar celda <br/> &bull; &nbsp; Actualizaciones de la experiencia del usuario del Asistente para dependencias de Python <br/> &bull; &nbsp; Espaciado mejorado entre cuadernos |
| Anuncio de la extensión de API SQL Assessment | Esta extensión agrega en ADS la valoración de procedimientos recomendados de SQL Server. Expone SQL Assessment API, que anteriormente estaba disponible para su uso en el módulo SqlServer de PowerShell y solo en SMO, para permitirle evaluar las instancias de SQL Server y recibir recomendaciones para ellas por parte del equipo de SQL Server. Obtenga más información sobre SQL Assessment API y sus funciones [en este artículo.](../tools/sql-assessment-api/sql-assessment-api-overview.md) |
| [Mejoras de la extensión Machine Learning](./extensions/machine-learning-extension.md) | Ahora se admite Azure SQL Managed Instance. |
| Mejoras de la extensión de virtualización de datos | Ahora es compatible con MongoDB y Teradata |
| Correcciones de errores de la extensión Postgres | Se ha corregido Azure MFA |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="may-2020-hotfix"></a>Mayo de 2020 (revisión)

27 de mayo de 2020 &nbsp; / &nbsp; versión: 1.18.1

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Corrección del error n.º 10538 El enlace de teclado "Ejecutar consulta actual" ya no se comporta de la forma esperada | [n.º 10538](https://github.com/microsoft/azuredatastudio/issues/10538)  |
| Corrección del error n.º 10537 No se pueden abrir archivos de SQL nuevos o existentes en v1.18 | [n.º 10537](https://github.com/microsoft/azuredatastudio/issues/10537)  |
| &nbsp; | &nbsp; |

## <a name="may-2020"></a>Mayo de 2020

20 de mayo de 2020 &nbsp; / &nbsp; versión: 1.18.0

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Anuncio de la extensión Redgate SQL Prompt | Esta extensión le permite administrar los estilos de formato directamente en Azure Data Studio, para que pueda crearlos y editarlos sin tener que cerrar el IDE. |
| Anuncio de la extensión Machine Learning | Esta extensión le permite: <br/> &bull; &nbsp; Administrar paquetes de Python y R con Machine Learning Services de SQL Server en Azure Data Studio.<br/> &bull; &nbsp; Usar el modelo ONNX para realizar predicciones en Azure SQL Edge.<br/> &bull; &nbsp; Ver modelos de ONNX en una base de datos de Azure SQL Edge. <br/> &bull; &nbsp; Importar modelos de ONNX desde un archivo o Azure Machine Learning en la base de datos de Azure SQL Edge. <br/> &bull; &nbsp; Crear un cuaderno para ejecutar experimentos. |
| Nuevas características de los cuadernos | &bull; &nbsp; Se ha agregado el nuevo Asistente para dependencias de Python para facilitar la instalación de dependencias de Python <br/> &bull; &nbsp; Se ha agregado compatibilidad con subrayado a la barra de herramientas de Markdown |
| Parametrización de Always Encrypted | Permite ejecutar consultas que insertan, actualizan o filtran columnas de base de datos cifradas.|
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22May+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="april-2020-hotfix"></a>Abril de 2020 (revisión)

30 de abril de 2020&nbsp; / &nbsp;versión: 1.17.1

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Corrección del error n.º 10197: no se puede conectar mediante MFA | [N.º 10197](https://github.com/microsoft/azuredatastudio/issues/10197)  |
| &nbsp; | &nbsp; |

## <a name="april-2020"></a>Abril de 2020

27 de abril de 2020&nbsp; / &nbsp;versión: 1.17.0

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Mejoras en la página principal | Se ha actualizado la interfaz de usuario en la página principal para facilitar la vista de las acciones comunes y el resaltado de las extensiones. |
| Nuevas características de los cuadernos | &bull; &nbsp; Se ha agregado la barra de herramientas de Markdown en la edición de celdas de texto para ayudar a escribir con Markdown. <br/> &bull; &nbsp; Se ha renovado el viewlet de Jupyter Books para convertirlo en un viewlet de Notebooks donde Jupyter Books y Notebooks pueden administrarse juntos. <br/>&bull; &nbsp; Se ha agregado compatibilidad con gráficos persistentes al guardar un cuaderno. <br/> &bull; &nbsp; Se ha agregado compatibilidad con KQL magic en los cuadernos de Python.|
| Mejoras en los paneles | Se han actualizado los paneles de Azure Data Studio con los patrones de diseño más recientes, incluida una barra de herramientas de acciones. Esto también se aplica a muchas extensiones. |
| Se ha agregado integración de Cloud Shell en la vista de Azure. | |
| Compatibilidad con Always Encrypted y Always Encrypted con enclaves seguros. | |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22April+2020+Release%22). |
| &nbsp; | &nbsp; |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22April+2020+Release%22). |
| &nbsp; | &nbsp; |

## <a name="march-2020"></a>Marzo de 2020

18 de marzo de 2020 &nbsp; / &nbsp; versión: 1.16.0 

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Se ha agregado compatibilidad con gráficos en cuadernos de SQL | Cuando se ejecuta una consulta SQL en una celda de código, ahora los usuarios pueden crear y guardar gráficos. |
| Se ha agregado experiencia de creación de libros de Jupyter | Ahora los usuarios pueden crear sus propios libros de Jupyter con un cuaderno. |
| Se ha agregado compatibilidad con Azure AD para la extensión Postgres. | |
| Se han corregido muchos errores de accesibilidad | [Lista de errores de accesibilidad](https://github.com/microsoft/azuredatastudio/issues?page=1&q=is%3Aissue+is%3Aclosed+milestone%3A%22S360+-+Accessibility%22+label%3AA11y_AzureDataStudio) |
| Fusión de VS Code en 1.42 | En esta versión se incluyen actualizaciones para VS Code de las tres versiones anteriores de VS Code. [Lea las notas de la versión](https://code.visualstudio.com/updates/v1_42) para obtener más información. |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22March+2020%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="february-hotfix"></a>Febrero (revisión)

Versión del 19 de febrero de 2020 &nbsp; / &nbsp;: 1.15.1

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Corrección del error #9149 Mostrar conexiones activas | [#9149](https://github.com/microsoft/azuredatastudio/issues/9149)  |
| Corrección del error #9061 La cuadrícula de edición de datos no cambia de tamaño correctamente al mostrar u ocultar el panel SQL | [#9061](https://github.com/microsoft/azuredatastudio/issues/9061)  |
| &nbsp; | &nbsp; |

## <a name="february-2020"></a>Febrero de 2020

Versión del 13 de febrero de 2020 &nbsp; / &nbsp;: 1.15.0

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Nueva mejora del inicio de sesión de Azure | Se ha agregado una experiencia mejorada de inicio de sesión de Azure, incluida la eliminación de la copia y pegado de código de dispositivo para lograr una experiencia de conexión directa. |
| Compatibilidad con Buscar en cuaderno | Ahora los usuarios pueden usar Ctrl+F dentro de un cuaderno. La Compatibilidad con Buscar en cuaderno busca línea por línea a través de las celdas de texto y de código. |
| Fusión de la versión 1.38 de VS Code en 1.42 | En esta versión se incluyen actualizaciones para VS Code de las tres versiones anteriores de VS Code. [Lea las notas de la versión](https://code.visualstudio.com/updates/v1_42) para obtener más información. |
| Corrección para el problema de ["pantalla en blanco"](https://github.com/microsoft/azuredatastudio/issues/8775) comunicado por muchos usuarios. | |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22February+2020%22). |
| &nbsp; | &nbsp; |

### <a name="known-issue"></a>Problema conocido

- Los usuarios de macOS Catalina tendrán que hacer clic con el botón derecho en Azure Data Studio y, después, hacer clic en Abrir.

## <a name="december-2019-hotfix"></a>Diciembre de 2019 (revisión)

26 de diciembre de 2019 &nbsp; / &nbsp; versión: 1.14.1

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Corrección del error #8747 Error de expansión de OE | [#8747](https://github.com/microsoft/azuredatastudio/issues/8747)  |
| &nbsp; | &nbsp; |

## <a name="december-2019"></a>Diciembre de 2019

19 de diciembre de 2019 &nbsp; / &nbsp; versión: 1.14.0 

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Se ha cambiado la lista desplegable de vincular a la conexión de Notebooks para que solo se muestre la conexión activa actualmente. | [#8129](https://github.com/microsoft/azuredatastudio/issues/8129) |
| Se ha agregado la configuración bigdatacluster.ignoreSslVerification para permitir omitir los errores de comprobación de TLS/SSL al conectarse a un BDC. | [#8582](https://github.com/microsoft/azuredatastudio/pull/8582) |
| Se permite cambiar el tipo de idioma predeterminado para los editores de consultas sin conexión. | [#8419](https://github.com/microsoft/azuredatastudio/pull/8419) |
| Estado de disponibilidad general para Clúster de macrodatos y Características de SQL 2019. | [#8269](https://github.com/microsoft/azuredatastudio/issues/8269) |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/milestone/44?closed=1). |
| &nbsp; | &nbsp; |


## <a name="november-2019-hotfix"></a>Noviembre de 2019 (revisión)

15 de noviembre de 2019 &nbsp; / &nbsp; Versión: 1.13.1

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Se corregido el error n.º 8210 que indica que los resultados de copiar y pegar están desordenados. |  |
| &nbsp; | &nbsp; |

## <a name="november-2019"></a>Noviembre de 2019

4 de noviembre de 2019 &nbsp; / &nbsp; Versión: 1.13.0 

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Nueva compatibilidad con SQL Server 2019 | &bull; &nbsp; Implementación de un clúster de macrodatos de SQL Server 2019 con el asistente de implementación de BDC <br/>&bull; &nbsp; Administración del mantenimiento del clúster con el panel del controlador <br/>&bull; &nbsp; Administración de las listas de control de acceso de HDFS mediante el cuadro de diálogo ACL de seguridad <br/> &bull; &nbsp; Adición de montajes mediante el cuadro de diálogo Niveles de HDFS <br/> &bull; &nbsp; Solución de problemas con el libro de Jupyter integrado y la guía de SQL Server 2019 <br/> &bull; &nbsp; Se ha cambiado el nombre a extensión de SQL vNext y extensión de virtualización de datos <br/> &bull; &nbsp; Se ha agregado compatibilidad con Teradata y Mongo en el Asistente para tablas externas|
| Nuevas características de los cuadernos | &bull; &nbsp; Anuncio de los cuadernos de PowerShell <br/> &bull; &nbsp; Anuncio de las celdas de código contraíble <br/>&bull; &nbsp; Mejoras de rendimiento en Notebooks <br/> &bull; &nbsp; [Aquí](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2019+Release%22+is%3Aclosed+label%3A%22Area+-+Notebooks%22) puede ver la lista completa de las mejoras. |
| Anuncio de libros de Jupyter  | Los libros de Jupyter son una colección de cuadernos y archivos Markdown organizados en una tabla de contenidos. |
| Nuevo Asistente para la implementación de SQL Server  | Ahora incluye compatibilidad con la implementación de: <br/> &bull; &nbsp; SQL Server 2019 en Windows <br/> &bull; &nbsp; SQL Server 2017 en Windows <br/> &bull; &nbsp; SQL Server 2019 en Docker <br/> &bull; &nbsp; SQL Server 2017 en Docker |
| Anuncio de la disponibilidad general de la extensión Comparación de esquemas| &bull; &nbsp; Modo SQLCMD <br/> &bull; &nbsp; Compatibilidad de localización <br/> &bull; &nbsp; Correcciones de accesibilidad <br/> &bull; &nbsp; Errores de seguridad  |
| Anuncio de la disponibilidad general de la extensión Dacpac de SQL Server| <br/> &bull; &nbsp; Compatibilidad de localización <br/> &bull; &nbsp; Correcciones de accesibilidad <br/> &bull; &nbsp; Errores de seguridad |
| Anuncio de la extensión IntelliCode de Visual Studio | IntelliCode de Visual Studio ahora admite SQL, lo que permite sugerencias más inteligentes de palabras clave reservadas. |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2019+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="october-2019-hotfix-2"></a>Octubre de 2019 (revisión 2)

11 de octubre de 2019 &nbsp; / &nbsp; versión: 1.12.2

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Deshabilitación del inicio automático de EH en el modo de inspección |  |
| &nbsp; | &nbsp; |

## <a name="october-2019-hotfix"></a>Octubre de 2019 (revisión)

8 de octubre de 2019 &nbsp; / &nbsp; versión: 1.12.1

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Se corrigió el problema de las comillas y las barras diagonales inversas en Notebooks como caracteres de escape correctos. |  |
| &nbsp; | &nbsp; |

## <a name="october-2019"></a>Octubre de 2019

2 de octubre de 2019 &nbsp; / &nbsp; versión: 1.12.0

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Publicación de la extensión del historial de consultas | La extensión Historial de SQL guarda todas las consultas pasadas ejecutadas en una sesión de Azure Data Studio y las muestra en orden de ejecución. Los usuarios pueden ver, abrir, ejecutar y eliminar la consulta, pausar el historial de consultas o eliminar todas las entradas del historial de consultas. |
| Nuevos resultados de Copiar y pegar | Se han agregado otras maneras de copiar y pegar los resultados desde la cuadrícula de resultados. |
| Actualización de la extensión de PowerShell |  |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/milestone/42?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemas conocidos

- Cuaderno
  - [7080](https://github.com/microsoft/azuredatastudio/issues/7080) Caso excepcional de serialización incorrecta de Notebook

## <a name="september-2019"></a>Septiembre de 2019

10 de septiembre de 2019 &nbsp; / &nbsp; versión: 1.11.0 

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Habilitación del modo SQLCMD | Ahora el Editor de consultas admite la alternancia del modo SQLCMD para escribir y modificar consultas como scripts SQLCMD |
| Extensión de la comunidad: Query Editor Boost | Query Editor Boost es una extensión de código abierto centrada en mejorar el editor de consultas de Azure Data Studio para los usuarios que escriben consultas con frecuencia. &bull; &nbsp; Guardado de la consulta actual como un fragmento de código <br/>&bull; &nbsp; Intercambio de bases de datos mediante Ctrl+U <br/> &bull; &nbsp; Nueva consulta a partir de una plantilla <br/> &bull; &nbsp; [Aquí](https://github.com/dzsquared/query-editor-boost) puede ver la lista completa de las mejoras. |
| Mejoras de Notebook | &bull; &nbsp; Mejoras de rendimiento para admitir archivos de cuaderno más grandes <br/> &bull; &nbsp; [Aquí](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22September+2019+Release%22+label%3A%22Area%3A+Notebooks%22+is%3Aclosed) puede ver la lista completa de las mejoras. |
| Combinación de la versión de agosto de Visual Studio Code 1.38 | [Aquí](https://code.visualstudio.com/updates/v1_38) encontrará las mejoras más recientes. |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/milestone/39?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemas conocidos

- Cuaderno
  - [7080](https://github.com/microsoft/azuredatastudio/issues/7080) Caso excepcional de serialización incorrecta de Notebook

## <a name="august-2019"></a>Agosto de 2019

15 de agosto de 2019 &nbsp; / &nbsp; versión: 1.10.0 

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Publicación de la extensión SandDance 1.3.1 | &bull; &nbsp; Detección inteligente de gráficos <br/>&bull; &nbsp; Visualizaciones 3D <br/> &bull; &nbsp; Filtrado de datos |
| Mejoras de Notebook | &bull; &nbsp; Adición de celda de texto o código en línea <br/>&bull; &nbsp; Adición de la posibilidad de hacer clic con el botón derecho en la cuadrícula de resultados de SQL para guardar el resultado como CSV, JSON, etc. <br/> &bull; &nbsp; Mejora en el rendimiento de carga de cuadernos para acelerar la carga de JSON <br/> &bull; &nbsp; [Aquí](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+label%3A%22Area%3A+Notebooks%22+milestone%3A%22August+2019+Release%22+is%3Aclosed) puede ver la lista completa de las mejoras. |
| Compatibilidad con SQL Server 2019 |  Esta versión incluye compatibilidad con otras características del clúster de macrodatos de SQL Server 2019: <br/> &bull; &nbsp; Se ha reducido el tiempo necesario para cargar la información de tablas y columnas en la página de asignación de objetos. <br/> &bull; &nbsp; Se ha corregido un error al cargar las credenciales con ámbito de base de datos existentes en la página de detalles de la conexión. <br/> &bull; &nbsp; Se ha aumentado el tamaño de ejemplo predeterminado que se usa para el análisis de PROSE. | 
| La extensión dacpac ahora es compatible con Azure AD. | 
| Combinación de la versión de julio de Visual Studio Code 1.37 | [Aquí](https://code.visualstudio.com/updates/v1_37) encontrará las mejoras más recientes. |
| Problemas y errores resueltos | Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/milestone/39?closed=1). |
| &nbsp; | &nbsp; |

## <a name="july-2019"></a>Julio de 2019

11 de julio de 2019 &nbsp; / &nbsp; versión: 1.9.0 

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Publicación de la extensión SentryOne Plan Explorer | Nuestro apreciado partner de Microsoft, SentryOne, publicará su [extensión SentryOne Plan Explorer para Azure Data Studio](https://www.sentryone.com/products/sentryone-plan-explorer-extension-azure-data-studio). <br> Se trata de una extensión gratuita que proporciona diagramas de plan mejorados para las consultas que se ejecutan en Azure Data Studio, con algoritmos de diseño optimizados y codificación de colores intuitiva que ayudan a identificar rápidamente los operadores más caros que afectan al rendimiento de las consultas. Para obtener más información sobre la extensión, consulte [aquí](https://sqlperformance.com/2019/07/sentryone/plan-explorer-extension-azure-data-studio) la publicación del blog de SentryOne. |
| Nuevas características que se incorporarán a Comparación de esquemas | &bull; &nbsp; Compatibilidad con archivos de Comparación de esquemas (.SCMP) <br/>&bull; &nbsp; Compatibilidad con cancelación de Comparación de esquemas <br/>&bull; &nbsp; [Aquí](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A%22July+2019+Release%22+label%3A%22Area%3A+Schema+Compare%22+is%3Aclosed+) puede ver los cambios completos.|
| Mejoras de Notebook | &bull; &nbsp; Compatibilidad con Plotly Python <br/>&bull; &nbsp; Apertura de Notebook desde el explorador <br/> &bull; &nbsp; Cuadro de diálogo Administración de paquetes de Python <br/> &bull; &nbsp; Mejoras de rendimiento y de Markdown <br/> &bull; &nbsp; Actualización de los métodos abreviados de teclado <br/>  &bull; &nbsp; [Aquí](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A%22July+2019+Release%22+is%3Aclosed+label%3A%22Area%3A+Notebooks%22+) puede encontrar las correcciones de errores y las características secundarias. |
| Compatibilidad con SQL Server 2019 |  Esta versión incluye compatibilidad con otras características del clúster de macrodatos de SQL Server 2019: <br/> &bull; &nbsp; Tabla de puntos de conexión de servicio en el panel de administración en la que se enumeran todos los servicios clave del clúster. <br/> &bull; &nbsp; El cuaderno Estado del clúster muestra cómo puede consultar el estado del clúster en todos los servicios y pods, y ofrece soluciones a los posibles problemas.| 
| Paquetes de idioma actualizados disponibles| Ahora hay 10 paquetes de idioma disponibles en el Marketplace de Administrador de extensiones. Tan solo hay que buscar el idioma específico con el Marketplace de extensiones e instalarlo. Una vez instalado el idioma seleccionado, Azure Data Studio le pedirá que reinicie con el nuevo idioma. |
| Actualización de SQL Server Profiler | La extensión Perfil de SQL Server se ha actualizado para incluir nuevas características, entre las que se incluyen: <br/> &bull; &nbsp; Filtrado por nombre de base de datos <br/> &bull; &nbsp; Compatibilidad con copiar y pegar <br/> &bull; &nbsp; Filtro Guardar/Cargar <br/>[Aquí](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aclosed+milestone%3A%22July+2019+Release%22+label%3A%22Area%3A+SQL+Profiler%22+) encontrará una lista completa de las mejoras de la extensión SQL Server Profiler.  |
| Combinación de versión de mayo de Visual Studio Code 1.35 | [Aquí](https://code.visualstudio.com/updates/v1_35) encontrará las mejoras más recientes. |
| Problemas y errores resueltos | En las versiones anteriores de Azure Data Studio, si se seleccionaba una base de datos de usuario al conectarse desde el cuadro de diálogo de conexión, la entrada resultante del Explorador de objetos estaba enfocada por completo a esa única base de datos. A partir de esta versión, ese comportamiento se está cambiando para que las propiedades de nivel de servidor también se muestren en el Explorador de objetos. <br/> Para obtener una la lista completa de las correcciones, vea [Correcciones y problemas en GitHub](https://github.com/microsoft/azuredatastudio/milestone/35?closed=1). |
| &nbsp; | &nbsp; |

## <a name="june-2019"></a>Junio de 2019

6 de junio de 2019 &nbsp; / &nbsp; versión: 1.8.0

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Publicación de la extensión Servidores de administración central (CMS) de SQL Server | La extensión Servidores de administración central almacena una lista de instancias de SQL Server que se organizan en uno o varios grupos de este tipo de servidores. Los usuarios pueden conectarse a sus propios servidores CMS existentes y administrarlos, por ejemplo, para agregar y quitar servidores. [Aquí](../relational-databases/administer-multiple-servers-using-central-management-servers.md) puede obtener más información. |
| Publicación de las extensiones de la herramienta de administración de bases de datos para Windows | Esta extensión inicia dos de las experiencias más usadas en SQL Server Management Studio desde Azure Data Studio. Los usuarios pueden hacer clic con el botón derecho en gran cantidad de objetos diferentes (como bases de datos, tablas, columnas, vistas, etc.) y seleccionar Propiedades para ver el cuadro de diálogo Propiedades de SSMS correspondiente a ese objeto. Además, los usuarios pueden hacer clic con el botón derecho en una base de datos y seleccionar Generar scripts para iniciar el ya conocido Asistente para generar scripts de SSMS. 
| Mejoras en la Comparación de esquemas | &bull; &nbsp; Adición de opciones de exclusión e inclusión <br/>&bull; &nbsp; Generar script abre el script tras generarlo <br/>&bull; &nbsp; Eliminación de las barras de desplazamiento dobles  <br/>&bull; &nbsp; Mejoras de formato y diseño <br/>&bull; &nbsp; [Aquí](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2019+Release%22+label%3A%22Area%3A+Schema+Compare%22+is%3Aclosed) puede ver los cambios completos.|
| Sección Mensajes movida a su propia pestaña | Cuando los usuarios ejecutaban consultas SQL, los resultados y los mensajes se encontraban en paneles apilados. Ahora se encuentran en pestañas independientes en un panel como en SSMS. |
| Mejoras en SQL Notebook | &bull; &nbsp; Ahora los usuarios pueden optar por usar sus propias instalaciones de Python 3 o Anaconda en los cuadernos. <br/>&bull; &nbsp; Varias correcciones de estabilidad y ajuste <br/> &bull; &nbsp; [Aquí](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2019+Release%22+is%3Aclosed+label%3A%22Area%3A+Notebooks%22) puede ver la lista completa de las mejoras.|
| Combinación de la versión de abril de Visual Studio Code 1.34 | [Aquí](https://code.visualstudio.com/updates/v1_34) encontrará las mejoras más recientes. |
| Problemas y errores resueltos | Consulte [Errores y problemas en GitHub](https://github.com/microsoft/azuredatastudio/milestone/32?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemas conocidos

- Extensiones de la herramienta de administración de bases de datos para Windows
    - No se pueden iniciar propiedades desde un nodo de servidor desconectado.
    - No se pueden iniciar las propiedades de los servidores de Azure.
    - No todos los objetos tienen cuadros de diálogo de propiedades.
    - Los diálogos tardan mucho tiempo en iniciarse.
    - Errores al iniciar servidores con algunos tipos de conexiones (como Azure AD).
- Cuaderno
    - [5838](https://github.com/microsoft/azuredatastudio/issues/5838) Permitir a los usuarios utilizar el sistema Python para los cuadernos.
- Comparación de esquemas
    - [5804](https://github.com/microsoft/azuredatastudio/issues/5804) Las tareas de Comparación de esquemas muestran el menú contextual Cancelar predeterminado, que no hace nada.

## <a name="may-2019"></a>Mayo de 2019

8 de mayo de 2019 &nbsp; / &nbsp; versión: 1.7.0 

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Publicación de la extensión Comparación de esquemas | Comparación de esquemas es una característica ya conocida en SQL Server Data Tools (SSDT), y su caso de uso principal es comparar y visualizar las diferencias entre las bases de datos y archivos .dacpac, y ejecutar acciones para que sean iguales. |
| Vista de tareas movida a la ventana de salida | Los usuarios ahora pueden ver el estado de las tareas de larga duración (como copia de seguridad, restauración y comparación de esquemas) de la vista de tareas en la ventana de salida.
| Página de bienvenida agregada | &bull; &nbsp; Vínculos a acciones comunes como Nueva consulta, Nuevo archivo, Nuevo cuaderno <br/>&bull; &nbsp; Vínculos a documentación y GitHub |
| Mejoras en SQL Notebook | &bull; &nbsp; Mejoras en la representación de Markdown, incluida una mayor compatibilidad con notas y tablas <br/>&bull; &nbsp; Mejoras de facilidad de uso en la barra de herramientas <br/>&bull; &nbsp; Los vínculos de Markdown para cuadernos de confianza ya no requieren Cmd/Ctrl + clic y se puede hacer clic en ellos directamente. <br/>&bull; &nbsp; Mejoras en la limpieza de procesos de Jupyter después de cerrar cuadernos y reducción de errores al iniciar varios cuadernos simultáneamente <br/>&bull; &nbsp; Mejoras en las conexiones de cuadernos de SQL para evitar errores al ejecutar dos cuadernos en la misma base de datos <br/>&bull; &nbsp; Mejoras en el desplazamiento automático de cuadernos a la celda actualmente en ejecución al hacer clic en el botón Ejecutar celdas de la barra de herramientas <br/>&bull; &nbsp; Mejoras generales en estabilidad y rendimiento |
| Problemas y errores resueltos | Consulte [Errores y problemas en GitHub](https://github.com/microsoft/azuredatastudio/milestone/31?closed=1). |
| &nbsp; | &nbsp; |

## <a name="april-2019"></a>Abril de 2019

18 de abril de 2019 &nbsp; / &nbsp; versión: 1.6.0 

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Nombre de la pestaña **Servidores** cambiado a **Conexiones** | |
| Migración de Azure Resource Explorer como viewlet de Azure a Conexiones | Los usuarios ahora pueden ver sus instancias de Azure SQL a través de un viewlet de Azure en la vista Conexiones, y expandir para ver los objetos de cada servidor o base de datos.|
| Mejoras en SQL Notebook | &bull; &nbsp; Botón agregado a la barra de herramientas para borrar la salida de todas las celdas <br/>&bull; &nbsp; Botón agregado a la barra de herramientas para ejecutar todas las celdas <br/>&bull; &nbsp; Nombre fijo de la conexión en lugar del nombre del servidor (si se establece) en la lista desplegable Anexar a <br/>&bull; &nbsp; Corrección de imágenes en Markdown que no se representaban al usar rutas de acceso de imagen relativas <br/>&bull; &nbsp; Funcionalidad mejorada en las cuadrículas de cuadernos con la adición de un cambio de tamaño de columna automático mediante doble clic, y mejora de la compatibilidad con la rueda del mouse <br/>&bull; &nbsp; Mejoras en el control de errores y la resistencia de instalación de Python al instalar Python a través de cuadernos <br/>&bull; &nbsp; Mejoras en la funcionalidad "Seleccionar todo" al seleccionar celdas de cuaderno <br/>&bull; &nbsp; Mejoras en las conexiones de cuadernos para evitar cerrar un cuaderno y afectar a la conexión del explorador de objetos <br/>&bull; &nbsp; Experiencia de cuadernos mejorada para mostrar un mensaje al usuario cuando el cuaderno está desconectado y necesita una conexión para ejecutar las celdas<br/>&bull; &nbsp; Compatibilidad mejorada con cuadernos no guardados para rehidratar en ADS cuando se inicia de nuevo ADS |
| Problemas y errores resueltos | Consulte [Errores y problemas en GitHub](https://github.com/Microsoft/azuredatastudio/milestone/26?closed=1). |
| &nbsp; | &nbsp; |

## <a name="march-2019-hotfix"></a>Marzo de 2019 (revisión)

22 de marzo de 2019 &nbsp; / &nbsp; versión: 1.5.2 &nbsp; / &nbsp; Versión de revisión

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Corrección de algunos problemas detectados en 1.5.1 | Consulte [Publicación de revisión de marzo en GitHub](https://github.com/Microsoft/azuredatastudio/milestone/28).<br/> <br/>&bull; &nbsp; Se ha corregido un problema por el que el usuario no podía cerrar un cuaderno abierto desde la tarea "Abrir cuaderno" en el panel. <br/>&bull; &nbsp; Se ha corregido un problema por el que el código JSON del Notebook tenía un signo } adicional después de guardar. <br/>&bull; &nbsp; Se ha corregido un problema por el que las cuadrículas de cuaderno no respondían a los cambios de tema. <br/>&bull; &nbsp; Se ha corregido un problema por el que se mostraba la ruta de acceso completa del cuaderno en el encabezado de pestaña. Ahora solo se muestra el nombre de archivo. |
| &nbsp; | &nbsp; |

## <a name="march-2019"></a>Marzo de 2019

18 de marzo de 2019 &nbsp; / &nbsp; versión: 1.5.1

&nbsp;

| Change | Detalles |
| :----- | :------ |
| [Extensión PostgreSQL para Azure Data Studio](./extensions/postgres-extension.md) agregada | Características admitidas: <br/>&bull; &nbsp; Cuadro de diálogo de conexión <br/>&bull; &nbsp; Explorador de objetos <br/>&bull; &nbsp; Editor de consultas <br/>&bull; &nbsp; Gráficos <br/>&bull; &nbsp; Paneles <br/>&bull; &nbsp; Fragmentos de código <br/>&bull; &nbsp; Editar datos <br/>&bull; &nbsp; Notebooks |
| Cuadernos de SQL agregados | Se ha agregado compatibilidad con el kernel de SQL al visor de Notebook integrado: <br/>&bull; &nbsp; Admite T-SQL <br/>&bull; &nbsp; Admite PGSQL |
| Extensión de PowerShell agregada | Ofrece la experiencia de la [extensión de PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) de VS Code.  |
| Extensión dacpac de SQL Server agregada  | Quita el asistente para aplicación de capa de datos de la extensión de importación de SQL Server y la agrega a una nueva extensión.  |
| Extensión de Community QueryPlan.show agregada | Integra compatibilidad para visualizar planes de consulta.  |
| Extensión de SQL Server 2019, versión preliminar, agregada | &bull; &nbsp; La compatibilidad con Jupyter Notebook (en concreto, con los kernels de Python 3 y Spark) se ha pasado a la herramienta básica de Azure Data Studio. <br/>&bull; &nbsp; Corrección de errores en el asistente para datos externos |
| Problemas y errores resueltos | Consulte [Errores y problemas en GitHub](https://github.com/Microsoft/azuredatastudio/milestone/25?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemas conocidos
- [#4427](https://github.com/Microsoft/azuredatastudio/issues/4427): si se hace clic en ejecutar en la celda antes de que el kernel esté listo para Spark, se produce un error fatal. **Solución alternativa:** espere a que se carguen los kernels antes de ejecutar las celdas.
- [#4493](https://github.com/Microsoft/azuredatastudio/issues/4493): ADS iniciado desde SSMS mediante autenticación de SQL: se pide al usuario la contraseña. **Solución alternativa:** use la autenticación de Windows por ahora. 
- [#4494](https://github.com/Microsoft/azuredatastudio/issues/4494): no se puede instalar la característica de cuadernos de SQL. <br/>
**Solución alternativa:** Siga [aquí](https://github.com/Microsoft/azuredatastudio/issues/4494#issuecomment-473043832) los pasos de solución de errores. 
- [#4503](https://github.com/Microsoft/azuredatastudio/issues/4503): Azure Data Studio no se pueden abrir directamente desde la carpeta de descargas (Mac). <br />
**Solución alternativa:** reinicie el equipo después de descomprimir la aplicación. Pendiente de investigación. 
- [#4539](https://github.com/Microsoft/azuredatastudio/issues/4539):  la característica Guardar como de Notebook pierde el contexto de conexión. <br />
**Solución alternativa:** Se corregirá en la próxima versión. 
- [#4458](https://github.com/Microsoft/azuredatastudio/issues/4458): la extracción de dacpac bloquea SqlToolsService si se usa una versión no válida. <br/>
**Solución alternativa:** reinicie Azure Data Studio y asegúrese de que se usa la versión correcta.
- Se pierden los iconos de Nuevo cuaderno y Abrir cuaderno. <br/>
**Solución alternativa:** el tipo de conexión heredado está en desuso. Se recomienda conectarse al punto de conexión de SQL Server para obtener todas las acciones previstas (Nuevo cuaderno, Trabajo de Spark). 

## <a name="february-2019"></a>Febrero de 2019

13 de febrero de 2019 &nbsp; / &nbsp; versión: 1.4.5

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Paquete de extensión **Admin Pack PARA SQL Server** agregado. | Esto facilita la instalación de extensiones relacionadas con la administración de SQL Server. Esto incluye:<br/>&bull; &nbsp; [Agente SQL Server](./extensions/sql-server-agent-extension.md)<br/>&bull; &nbsp; [SQL Server Profiler](./extensions/sql-server-profiler-extension.md)<br/>&bull; &nbsp; [Importación de SQL Server](./extensions/sql-server-import-extension.md) |
| Se ha agregado compatibilidad con eventos extendidos de filtrado en la extensión de Profiler. | &nbsp; |
| Se ha agregado la característica Guardar como XML que puede guardar los resultados de T-SQL como XML. | &nbsp; |
| Se han agregado mejoras en el Asistente para importar aplicaciones de capa de datos. | &bull; &nbsp; Se ha agregado el botón Generar script.<br/>&bull; &nbsp; Se ha agregado la vista para proporcionar advertencias de posibles pérdidas de datos durante la implementación. |
| Actualizaciones en la versión preliminar de SQL Server 2019. | Consulte [Extensión de virtualización de datos](./extensions/data-virtualization-extension.md). |
| Streaming de resultados habilitado de forma predeterminada para las consultas de larga duración. | &nbsp; |
| Problemas y errores resueltos | Consulte [Errores y problemas en GitHub](https://github.com/Microsoft/azuredatastudio/milestone/23?closed=1). |
| &nbsp; | &nbsp; |

## <a name="january-2019-hotfix"></a>Enero de 2019 (revisión)

16 de enero de 2019 &nbsp; / &nbsp; versión: 1.3.9 &nbsp; / &nbsp; Versión de revisión

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Corrección de algunos problemas detectados en 1.3.8. | Consulte [Publicación de revisión de enero en GitHub](https://github.com/Microsoft/azuredatastudio/milestone/24?closed=1).<br/><br/>Para obtener información detallada, vea:<br/>&bull; &nbsp; [Registro de cambios en GitHub](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md).<br/>&bull; &nbsp; [Versiones en GitHub](https://github.com/Microsoft/azuredatastudio/releases). |
| &nbsp; | &nbsp; |

## <a name="january-2019"></a>Enero de 2019

9 de enero de 2019 &nbsp; / &nbsp; versión: 1.3.8

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Se ha agregado un nuevo instalador de usuario para Windows. | A diferencia del instalador de sistema existente, el nuevo instalador de usuario no requiere privilegios de administrador. Esto también permite una experiencia de actualización más sencilla para los usuarios que no son administradores. |
| Se ha agregado compatibilidad con la autenticación de Azure Active Directory. | &nbsp; |
| Anuncio de Idera SQL DM Performance Insights (versión preliminar). | &nbsp; |
| Compatibilidad con el Asistente para aplicaciones de capa de datos en la extensión Importación de SQL Server. | &nbsp; |
| Actualización de la extensión de SQL Server 2019, versión preliminar. | Consulte [Extensión de virtualización de datos](./extensions/data-virtualization-extension.md). |
| Mejoras en SQL Server Profiler. | &nbsp; |
| Streaming de resultados para consultas de gran tamaño (vista previa). | &nbsp; |
| Extensiones de la comunidad: sp_executesql para SQL y Nueva base de datos. | &nbsp; |
| Problemas y errores resueltos | Consulte [Errores y problemas en GitHub](https://github.com/Microsoft/azuredatastudio/milestone/19?closed=1). |
| &nbsp; | &nbsp; |

## <a name="november-2018"></a>Noviembre de 2018

6 de noviembre de 2018 &nbsp; / &nbsp; versión: 1.2.4

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Actualización de la extensión de SQL Server 2019, versión preliminar. | Consulte [Extensión de virtualización de datos](./extensions/data-virtualization-extension.md). |
| Introducción de la extensión Paste the Plan (Pegado del plan). | &nbsp; |
| Introducción de la extensión de consultas High color (Color alto), incluido el tema del editor de SSMS. | &nbsp; |
| Correcciones en las extensiones Agente SQL Server, Profiler e Importación. | &nbsp; |
| Corrección del problema de .NET Core Socket KeepAlive que produce conexiones inactivas en macOS. | &nbsp; |
| Actualización de SQL Tools Service a .NET Core 2.2, versión preliminar 3 (para compatibilidad eventual con Azure AD). | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-november-2018"></a>Correcciones de errores, noviembre de 2018

- Corrección del [problema #2933](https://github.com/Microsoft/azuredatastudio/issues/2933): Conexión perdida a Azure SQL Database
- Corrección del [problema #2914](https://github.com/Microsoft/azuredatastudio/issues/2914): Excepción "Argumento no válido" al expandir el nodo de base de datos OE.
- Corrección del [problema #2935](https://github.com/Microsoft/azuredatastudio/pull/2935): Visualización correcta de mensajes de varias líneas en los resultados de la consulta.
- Corrección del [problema #2906](https://github.com/Microsoft/azuredatastudio/pull/2906): Corrección del nombre del documento de Editar datos cuando el nombre de la tabla contiene caracteres especiales.
- Corrección del [problema #2929](https://github.com/Microsoft/azuredatastudio/issues/2929): El registro de cambios de extensión integrado indica que se comprueben los cambios en las notas de la versión de VSCode.
- Corrección del [problema #2719](https://github.com/Microsoft/azuredatastudio/issues/2719): Iconos dobles o triples de tema de contraste alto.
- Corrección del [problema #3047](https://github.com/Microsoft/azuredatastudio/pull/3047): Se ha agregado una interfaz de línea de comandos para conectarse a un servidor SQL Server.
- Corrección del [problema #3031](https://github.com/Microsoft/azuredatastudio/pull/3031): Adición de compatibilidad con el tema del plan de consulta.

## <a name="october-2018"></a>Octubre de 2018

29 de octubre de 2018&nbsp; / &nbsp; versión: 1.1.4

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Introducción de Azure Resource Explorer para examinar Azure SQL Database. | &nbsp; |
| Mejora de la solidez de la conexión del Explorador de objetos y del Editor de consultas. | &nbsp; |
| Mejoras en las extensiones del Agente SQL. | &nbsp; |
| Actualización de la extensión de SQL Server 2019, versión preliminar. | Consulte [Extensión de virtualización de datos](./extensions/data-virtualization-extension.md). |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-october-2018"></a>Correcciones de errores, octubre de 2018

- Corrección del [problema #2717](https://github.com/Microsoft/azuredatastudio/issues/2717): Formato con clic de los resultados de la columna XML.
- Corrección del [problema #2993](https://github.com/Microsoft/azuredatastudio/issues/2993): Ancho de ventanas de resultados incompleto.
- Corrección del [problema #2999](https://github.com/Microsoft/azuredatastudio/issues/2999): No se pudo cargar el archivo System.Diagnostics.Tracing en Mac al conectarse a la base de datos.
- Corrección del [problema #2851](https://github.com/Microsoft/azuredatastudio/issues/2851): El gráfico TimeSeries no se representa correctamente.
- Corrección del [problema #2996](https://github.com/Microsoft/azuredatastudio/issues/2996): Pérdida de tabla Temp debido a un cambio de sesión repentino.

Para obtener información detallada, vea [Registro de cambios](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md) y [Versiones](https://github.com/Microsoft/azuredatastudio/releases).

## <a name="september-2018-ga-release"></a>Septiembre de 2018 (versión GA)

24 de septiembre de 2018 &nbsp; / &nbsp; versión: 1.0 &nbsp; / &nbsp; versión GA

Versión de disponibilidad general de Azure Data Studio (anteriormente SQL Operations Studio).

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Mejoras en el rendimiento de la cuadrícula de resultados de la consulta y la experiencia del usuario para un gran número de conjuntos de resultados. | &nbsp; |
| El código fuente de Visual Studio Code se actualiza de la versión 1.23 a la versión 1.26.1 con diseño de cuadrícula y mejora del editor de configuración (versión preliminar). | &nbsp; |
| Mejoras de accesibilidad para el lector de pantalla, la navegación con el teclado y el contraste alto. | &nbsp; |
| Se ha agregado la opción `Connection name` para proporcionar un nombre para mostrar alternativo en el viewlet de servidores. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="announcing-the-sql-server-2019-preview-extension"></a>Anuncio de la extensión de SQL Server 2019, versión preliminar

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Compatibilidad con características en vista previa de SQL Server 2019, incluido el [clúster de macrodatos](../big-data-cluster/big-data-cluster-overview.md) | Conéctese a la puerta de enlace HDFS/Spark incluida con la versión preliminar de SQL Server 2019.<br/><br/>Examine HDFS, cargue y guarde archivos, e inicie acciones prácticas como analizar en Notebook para archivos CSV.<br/><br/>Envíe trabajos de Spark desde el panel o haga clic con el botón derecho en una conexión de HDFS/Spark en el Explorador de objetos. |
| Azure Data Studio Notebooks | Cree o abra cuadernos con un visor de Notebook integrado. En esta versión, el visor de Notebook solo admite la conexión a kernels locales y al clúster de macrodatos de SQL Server 2019.<br/><br/>Use las bibliotecas del acelerador de código PROSE en Notebook para obtener información sobre el formato de archivo y los tipos de datos para la preparación rápida de los datos. |
| Azure Resource Explorer | La vista de Azure Resource Explorer permite examinar los puntos de conexión relacionados con los datos de las cuentas de Azure y crear conexiones con ellos en el Explorador de objetos. En esta versión se admite Azure SQL Database. |
| Asistente para crear tablas externas de SQL Server PolyBase | Cree una tabla externa y sus estructuras de metadatos auxiliares con un asistente fácil de usar. En esta versión, se admiten los servidores remotos SQL Server y Oracle. |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-september-2018"></a>Correcciones de errores, septiembre de 2018

- Corrección del [problema #2647](https://github.com/Microsoft/azuredatastudio/issues/143): Los gráficos han retrocedido considerablemente.
- Corrección del [problema #2648](https://github.com/Microsoft/azuredatastudio/issues/143): SELECT que devuelve JSON con hipervínculos en toda la columna.

Para obtener información detallada, vea [Registro de cambios](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md) y [Versiones](https://github.com/Microsoft/azuredatastudio/releases).

## <a name="august-2018"></a>Agosto de 2018

30 de agosto de 2018 &nbsp; / &nbsp; versión: 0.32.8 &nbsp; / &nbsp; Versión preliminar pública

La *versión preliminar pública de agosto* se centra en las correcciones de errores, la estabilización del producto y el relleno de vacíos en escenarios existentes.

_0.32.8 contiene correcciones para un par de regresiones encontradas en 0.32.7 ([#1971](https://github.com/Microsoft/azuredatastudio/issues/1971), [#2372](https://github.com/Microsoft/azuredatastudio/issues/2372))_

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Anuncio de la extensión de importación de SQL Server | &nbsp; |
| Administración de sesiones de SQL Server Profiler | &nbsp; |
| Compatibilidad con la plantilla de sesiones de SQL Server Profiler | &nbsp; |
| Mejoras en el Agente SQL Server | &nbsp; |
| Nueva extensión de la comunidad: Kit de primer respondedor | &nbsp; |
| Mejoras en la calidad de la vida: Cadenas de conexión | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-august-2018"></a>Correcciones de errores, agosto de 2018

- Analizar SQL en una ventana del Editor de consultas mediante el comando `Parse Syntax`.
- Corrección del [problema #143](https://github.com/Microsoft/azuredatastudio/issues/143): Hacer doble clic no selecciona @ en el nombre de variable.
- Corrección del [problema #387](https://github.com/Microsoft/azuredatastudio/issues/387): El icono de la base de datos de la pestaña de SQL está en rojo.
- Corrección del [problema #825](https://github.com/Microsoft/azuredatastudio/issues/825): Solicitud: Conectar automáticamente al servidor actual después de Script como... 
- Corrección del [problema #1278](https://github.com/Microsoft/azuredatastudio/issues/1278): sqlops.desktop [entrada de escritorio] - valor redundante para Nombre y comentario.
- Corrección del [problema #1285](https://github.com/Microsoft/azuredatastudio/issues/1285): La actualización hace que el icono de la aplicación se quite o se reemplace en Windows.
- Corrección del [problema #1317](https://github.com/Microsoft/azuredatastudio/issues/1317): Corrija el separador decimal.
- Corrección del [problema #1474](https://github.com/Microsoft/azuredatastudio/issues/1474): Cancelar el cambio de conexión desconecta la conexión actual.
- Corrección del [problema #1497](https://github.com/Microsoft/azuredatastudio/issues/1497): Las opciones de Ver como gráfico se cortan en la parte inferior.
- Corrección del [problema #1524](https://github.com/Microsoft/azuredatastudio/issues/1524): Shell/Panel: Los iconos principales del viewlet son arrastrables y pueden bloquear la aplicación.
- Corrección del [problema #1578](https://github.com/Microsoft/azuredatastudio/issues/1578): No se puede expandir ni contraer la carpeta del explorador de archivos remotos haciendo clic en el nombre.
- Corrección del [problema #1620](https://github.com/Microsoft/azuredatastudio/issues/1620): Sugerencia de característica: Obtener la cadena de conexión para la conexión existente.
- Corrección del [problema #1624](https://github.com/Microsoft/azuredatastudio/issues/1624): SelectBox no cambia de color cuando se deshabilita.
- Corrección del [problema #1728](https://github.com/Microsoft/azuredatastudio/issues/1728): Guardar como JSON/EXCEL/CSV no funciona.
- Corrección del [problema #1744](https://github.com/Microsoft/azuredatastudio/issues/1744): El panel de resultados pierde las posiciones de desplazamiento al cambiar de pestaña.
- Corrección del [problema #1748](https://github.com/Microsoft/azuredatastudio/issues/1748): Mensaje de error al guardar el archivo de Excel una segunda vez (y en veces posteriores).
- Corrección del [problema #1782](https://github.com/Microsoft/azuredatastudio/issues/1782): Al editar datos, la celda no vuelve al valor original al presionar la tecla Escape.
- Corrección del [problema #1836](https://github.com/Microsoft/azuredatastudio/issues/1836): Archivos .sql no asociados con SQL Operations Studio.
- Corrección del [problema #1850](https://github.com/Microsoft/azuredatastudio/issues/1850): Al escribir N'', se completa automáticamente a N'''.
- Corrección del [problema #1985](https://github.com/Microsoft/azuredatastudio/issues/1985): Desfase de una columna al copiar desde la cuadrícula de resultados de la consulta.
- Corrección del [problema #1998](https://github.com/Microsoft/azuredatastudio/pull/1998): Adición de la versión de VS Code al cuadro de diálogo Acerca de.
- Corrección del [problema #2042](https://github.com/Microsoft/azuredatastudio/pull/2042): Agente: Botón habilitado para importar consultas de archivos SQL.
- Corrección del [problema #2091](https://github.com/Microsoft/azuredatastudio/issues/2091): No se puede usar el método abreviado CTRL + C para copiar desde el panel de resultados.
- Corrección del [problema #2099](https://github.com/Microsoft/azuredatastudio/pull/2099): Se han agregado más opciones de saveAsCsv.
- Corrección del [problema #2107](https://github.com/Microsoft/azuredatastudio/issues/2107): Actualización del icono de los documentos del Panel y de Profiler.
- Corrección del [problema #2129](https://github.com/Microsoft/azuredatastudio/pull/2129): Guardar los datos de edición desplaza la posición al cambiar de pestaña.
- Corrección del [problema #2152](https://github.com/Microsoft/azuredatastudio/issues/2152): El indicador de fila de cuadrícula de resultados se basa en cero.

### <a name="known-issues-august-2018"></a>Problemas conocidos, agosto de 2018

- [Problema #2371](https://github.com/Microsoft/azuredatastudio/issues/2371) Guardar como Excel solo guarda la primera fila de datos.
- [Problema #2150](https://github.com/Microsoft/azuredatastudio/issues/2150): No se puede conectar en Ubuntu 16.04 a SQL en un contenedor.

## <a name="july-2018"></a>Julio de 2018

19 de julio de 2018&nbsp; / &nbsp; versión: 0.31.4 &nbsp; / &nbsp; Versión preliminar pública

La *versión preliminar pública* de julio se centra en los siguientes elementos:

- La versión inicial de los escenarios de configuración del Agente SQL Server.
- Mejoras en la plantilla de vista y sesión de SQL Server Profiler.
- Correcciones de errores continuadas para problemas notificados por los clientes en GitHub.

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Mejoras en la [Extensión Agente SQL Server para SQL Operations Studio](./extensions/sql-server-agent-extension.md). | Se ha agregado la vista de alertas, operadores, proxies e iconos en el panel izquierdo.<br/><br/>Se han agregado los cuadros de diálogo Nuevo trabajo, Nuevo paso de trabajo, Nueva alerta y Nuevo operador.<br/><br/>Se ha agregado Eliminar trabajo, Eliminar alerta y Eliminar operador (clic con el botón derecho).<br/><br/>Se ha agregado la visualización de ejecuciones anteriores.<br/><br/>Se han agregado filtros para cada nombre de columna. |
| Mejoras en la [extensión SQL Server Profiler para SQL Operations Studio](./extensions/sql-server-profiler-extension.md). | Se han agregado 5 plantillas predeterminadas para ver Eventos extendidos.<br/><br/>Se ha agregado el nombre de conexión de Servidor/Base de datos.<br/><br/>Se ha agregado compatibilidad para instancias de Azure SQL Database.<br/><br/>Se ha agregado una sugerencia para salir de Profiler cuando se cierra la pestaña y Profiler todavía se está ejecutando. |
| Versión de la extensión de combinación de scripts. | &nbsp; |
| Se han agregado puntos de extensibilidad de cuadros de diálogo y asistentes para los autores de extensiones. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-july-2018"></a>Correcciones de errores, julio de 2018

- Corrección del [problema 728](https://github.com/Microsoft/azuredatastudio/issues/728): No hay respuesta para Agregar conexión en macOS.
- Corrección del [problema 1612](https://github.com/Microsoft/azuredatastudio/issues/1612): La presentación del texto de la cuadrícula de resultados está desordenada con caracteres internacionales.
- Corrección del [problema 1693](https://github.com/Microsoft/azuredatastudio/issues/1693): Cuadro de diálogo de copia de seguridad - La interfaz de usuario del explorador de archivos está dañada.
- Corrección del [problema 1713](https://github.com/Microsoft/azuredatastudio/issues/1713): Número de filas afectadas.
- Corrección del [problema 1718](https://github.com/Microsoft/azuredatastudio/issues/1718): No se puede conectar con ningún origen de datos.
- Corrección del [problema 1719](https://github.com/Microsoft/azuredatastudio/issues/1719): TypeError al conectar con el servidor.
- Corrección del [problema 1724](https://github.com/Microsoft/azuredatastudio/issues/1724): Los cuadros de diálogo de extensiones han dejado de funcionar.
- Corrección del [problema 1749](https://github.com/Microsoft/azuredatastudio/issues/1749): ERROR - Se interpretan los datos HTML en una columna.
- Corrección del [problema 1789](https://github.com/Microsoft/azuredatastudio/issues/1789): Extensibilidad - Si agrega un proveedor de conexión, la desinstalación nunca lo quitará de la lista.
- Corrección del [problema 1791](https://github.com/Microsoft/azuredatastudio/issues/1791): Extensiones de Sqlops - queryeditor.connect() se conecta a la base de datos de destino, pero la interfaz de usuario no muestra que el editor está conectado.
- Corrección del [problema 1799](https://github.com/Microsoft/azuredatastudio/issues/1799): El gráfico con las 10 bases de datos principales por tamaño no funciona en instancias con distinción de mayúsculas y minúsculas.
- Corrección del [problema 1814](https://github.com/Microsoft/azuredatastudio/issues/1814): error tipográfico de sqlops.d.ts que causa la definición de tipo "any" implícita.
- Corrección del [problema 1817](https://github.com/Microsoft/azuredatastudio/issues/1817): Error de ortografía.
- Corrección del [problema 1830](https://github.com/Microsoft/azuredatastudio/issues/1830): Establecer iconPath en ButtonComponent después de llamar a component() no cambia el icono.
- Corrección del [problema 1843](https://github.com/Microsoft/azuredatastudio/issues/1843): Mejor organización de tablas.

## <a name="june-2018"></a>Junio de 2018

20 de junio de 2018 &nbsp; / &nbsp; versión: 0.30.6 &nbsp; / &nbsp; Versión preliminar pública

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Publicación inicial de la extensión **SQL Server Profiler para SQL Operations Studio, _versión preliminar_**. | &nbsp; |
| La nueva extensión **SQL Data Warehouse** incluye sofisticados widgets de panel personalizables que exponen información al almacenamiento de datos. | Esto desbloquea escenarios clave en torno a la administración y el ajuste del almacenamiento de datos para asegurarse de que está optimizado para un rendimiento coherente. |
| Compatibilidad con **"Filtrar y ordenar" de Editar datos**. | &nbsp; |
| Mejoras de la extensión **Agente SQL Server para SQL Operations Studio _, versión preliminar_**, para las vistas Trabajos e Historial de trabajos. | &nbsp; |
| Se han mejorado las API de extensibilidad del **asistente y del marco de trabajo del generador de interfaz de usuario**. | &nbsp; |
| Actualización del código fuente de la plataforma VS Code. | Se han integrado las siguientes versiones:<br/>&bull; &nbsp; [Marzo de 2018 (1.22)](https://code.visualstudio.com/updates/v1_22)<br/>&bull; &nbsp; [Abril de 2018 (1.23)](https://code.visualstudio.com/updates/v1_23) |
| &nbsp; | &nbsp; |

### <a name="github-issues-fixes-june-2018"></a>Correcciones de problemas de GitHub, junio de 2018

- Solicitud de característica ([problema 1204](https://github.com/Microsoft/azuredatastudio/issues/1204)): Hacer que la cuadrícula de resultados ajuste automáticamente el ancho de columna a los datos y recordar los cambios manuales si se vuelve a ejecutar la misma consulta.
- Corrección del [problema 1398](https://github.com/Microsoft/azuredatastudio/issues/1398): Se debería mostrar el botón Agregar mensaje y Agregar cuenta cuando la cuenta vinculada está vacía.
- Corrección del [problema 1399](https://github.com/Microsoft/azuredatastudio/issues/1399): La pestaña de cuenta vinculada queda dañada cuando se contrae la vista.
- Corrección del [problema 1374](https://github.com/Microsoft/azuredatastudio/issues/1374): El servicio de herramientas de SQL se bloquea al abrir el archivo .sql desde el disco.
- Corrección del [problema 1372](https://github.com/Microsoft/azuredatastudio/issues/1372): Falta la palabra clave de SQL "BETWEEN".
- Corrección del [problema 1395](https://github.com/Microsoft/azuredatastudio/issues/1395): La palabra clave "MATCH" bloquea el servicio de herramientas de SQL.
- Corrección del [problema 1496](https://github.com/Microsoft/azuredatastudio/issues/1496): La opción del menú contextual de nuevo generador de perfiles en el Explorador de objetos no hace nada.
- Corrección del [problema 1495](https://github.com/Microsoft/azuredatastudio/issues/1495): El plan de consulta "Explicar" del editor de consultas está dañado.

## <a name="may-2018"></a>Mayo de 2018

7 de mayo de 2018 &nbsp; / &nbsp; versión: 0.29.3 &nbsp; / &nbsp; Versión preliminar pública

La *versión preliminar pública de mayo* se centra en la estabilización y la corrección de errores.

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Presentación de la extensión Búsqueda de SQL de Redgate disponible en el administrador de extensiones. | &nbsp; |
| Localización de la comunidad disponible para 10 idiomas. | Alemán, español, francés, italiano, japonés, coreano, portugués, ruso, chino simplificado y chino tradicional. |
| Cambios en la colección de telemetría. | &bull; &nbsp; Colección de telemetría reducida.<br/>&bull; &nbsp; Experiencia de exclusión mejorada.<br/>&bull; &nbsp; Vínculos en productos a la declaración de privacidad. |
| El administrador de extensiones ha mejorado la experiencia de Marketplace. | Detección más fácil de las extensiones de la comunidad. |
| Extensión de Agente SQL. | &bull; &nbsp; Trabajos.<br/>&bull; &nbsp; Mejora de la vista del historial de trabajos. |
| Actualizaciones para las extensiones de whoisactive y Server Reports. | &nbsp; |
| Desplazamiento mejorado de la administración de las propiedades del panel. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="fix-github-issues"></a>Corrección de problemas de GitHub

- Corrección del [problema 703](https://github.com/Microsoft/azuredatastudio/issues/703): La escritura de texto similar a HTML en Editar datos hace que el valor se muestre incorrectamente hasta que se actualiza.
- Corrección del [problema 821](https://github.com/Microsoft/azuredatastudio/issues/821): Dependencia del paquete azuredatastudio.deb.
- Corrección del [problema 1260](https://github.com/Microsoft/azuredatastudio/issues/1260): Palabra clave "Distinct" no resaltada.
- Corrección del [problema 1332](https://github.com/Microsoft/azuredatastudio/issues/1332): La reversión de la fila en "Editar datos" no funciona.
- Corrección del [problema 1215](https://github.com/Microsoft/azuredatastudio/issues/1215): Extensión del Agente SQL y la barra de estado.
- Corrección del [problema 1316](https://github.com/Microsoft/azuredatastudio/issues/1316): Agente SQL no cambia de tamaño después de cambiar el tamaño de las ventanas.

## <a name="april-2018"></a>Abril de 2018

25 de abril de 2018 &nbsp; / &nbsp; versión: 0.28.6 &nbsp; / &nbsp; Versión preliminar pública

La *versión preliminar pública de abril* contiene correcciones de errores y mejoras.

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Mejoras en la extensión de la versión preliminar del Agente SQL: | &nbsp; |
| &nbsp; &nbsp; &nbsp; Compatibilidad mejorada con archivos. | &bull; &nbsp; Archivos grandes.<br/>&bull; &nbsp; Archivos protegidos para guardar los protegidos por el administrador.<br/>&bull; &nbsp; Almacenamiento de \>256M archivos en SQL Operations Studio. |
| &nbsp; &nbsp; &nbsp; División de terminal integrado. | Trabajo simultáneo con varios terminales abiertos. |
| &nbsp; &nbsp; &nbsp; Mayor velocidad de instalación y tiempo de inicio. | Se ha reducido la huella del número de archivos en disco dejada por la instalación. |
| &nbsp; | &nbsp; |

### <a name="fix-github-issues-april-2018"></a>Corrección de problemas de GitHub, abril de 2018

- Corrección del [problema 37](https://github.com/Microsoft/azuredatastudio/issues/37): Cuando el visor de gráficos genera un error, se produce un comportamiento inesperado.
- Corrección del [problema 462](https://github.com/Microsoft/azuredatastudio/issues/462): Solicitud de característica - Opción para que los grupos de servidores se expandan de forma predeterminada.
- Corrección del [problema 606](https://github.com/Microsoft/azuredatastudio/issues/606): Intellisense - Sugerencia incorrecta para el comando "update".
- Corrección del [problema 967](https://github.com/Microsoft/azuredatastudio/issues/967): Se espera un plan de consulta cuando se selecciona el plan de presentación XML en la cuadrícula de resultados.
- Corrección del [problema 1023](https://github.com/Microsoft/azuredatastudio/issues/1023): Adición de corchetes para la llamada ms_foreachdb desde flyfishingdba.
- Corrección del [problema 1048](https://github.com/Microsoft/azuredatastudio/issues/1048): Error de protocolo de enlace SSL/TLS previo al inicio de sesión.
- Corrección del [problema 1050](https://github.com/Microsoft/azuredatastudio/issues/1050): Borrar la vista de información antes de mostrar el error.
- Corrección del [problema 1057](https://github.com/Microsoft/azuredatastudio/issues/1057): Las acciones restaurar y nueva consulta en el widget de explorador están dañadas.
- Corrección del [problema 1068](https://github.com/Microsoft/azuredatastudio/issues/1068): Aparecen ventanas de salida del panel con un mensaje de error para Azure SQL Database.
- Corrección del [problema 1069](https://github.com/Microsoft/azuredatastudio/issues/1069): El cuadro de diálogo de conexión muestra el error Servidor obligatorio cuando se muestra inicialmente.
- Corrección del [problema 1070](https://github.com/Microsoft/azuredatastudio/issues/1070): Grupos de servidores ahora requieren un doble clic para expandirse.
- Corrección del [problema 1072](https://github.com/Microsoft/azuredatastudio/issues/1072): El fondo del control Seleccionar es semitransparente.
- Corrección del [problema 1115](https://github.com/Microsoft/azuredatastudio/issues/1115): Corrección de todos los problemas de accesibilidad de contraste alto en SQL Operations Studio.
- Corrección del [problema 1101](https://github.com/Microsoft/azuredatastudio/issues/1101): La extensión no puede actualizar el vínculo "Descargar manualmente"; se dirige a una ubicación incorrecta.
- Corrección del [problema 1103](https://github.com/Microsoft/azuredatastudio/issues/1103): El desplazamiento V no funciona en la pestaña Inicio.
- Corrección del [problema 1104](https://github.com/Microsoft/azuredatastudio/issues/1104): Las pestañas de la extensión de SQL dejaban de funcionar.

### <a name="visual-studio-code-121-platform"></a>Plataforma Visual Studio Code 1.21

Un aspecto destacado de la versión preliminar pública de abril es la actualización del código fuente de la plataforma Visual Studio Code 1.21. Incluye varias actualizaciones en el editor principal y el área de trabajo con respecto al punto de sincronización 1.19 anterior. A continuación se muestran algunos ejemplos:

&nbsp;

| Change | Detalles |
| :----- | :------ |
| [Nueva interfaz de usuario de notificaciones](https://code.visualstudio.com/updates/v1_21#_new-notifications-ui). | Administre y revise fácilmente las notificaciones de SQL Operations Studio. |
| [División de terminal integrado](https://code.visualstudio.com/updates/v1_21#_split-terminals). | Trabajo simultáneo con varios terminales abiertos. |
| [Guardar archivos grandes y protegidos](https://code.visualstudio.com/updates/v1_20#_save-files-that-need-admin-privileges). | Guardar archivos protegidos por el administrador y \>256M archivos en SQL Operations Studio. |
| [Compatibilidad mejorada con archivos grandes](https://code.visualstudio.com/updates/v1_21#_text-buffer-improvements). | Optimizaciones del búfer de texto para archivos grandes. |
| [Búsqueda de configuración mejorada](https://code.visualstudio.com/updates/v1_20#_settings-search). | Detección fácil de la configuración adecuada con búsqueda en lenguaje natural. |
| [Fragmentos de código globales](https://code.visualstudio.com/updates/v1_20#_global-snippets). | Creación de fragmentos de código que pueden usarse en todos los tipos de archivo. |
| [Selección múltiple en el explorador](https://code.visualstudio.com/updates/v1_20#_multi-select-in-the-explorer). | Posibilidad de realizar acciones en varios archivos a la vez. |
| [Errores y advertencias en el explorador](https://code.visualstudio.com/updates/v1_20#_error-indicators-in-the-explorer). | Navegación rápida a los errores en el código base. |
| [Arrastrar y colocar, copiar y pegar entre ventanas](https://code.visualstudio.com/updates/v1_21#_better-drag-and-drop-support). | Movimiento de archivos entre ventanas abiertas de SQL Operations Studio. |
| [Compatibilidad con submódulos de Git](https://code.visualstudio.com/updates/v1_20#_git-submodules). | Posibilidad de realizar operaciones de Git en repositorios de Git anidados. |
| [Compatibilidad con el lector de pantalla de terminal](https://code.visualstudio.com/updates/v1_20#_screen-reader-support). | El terminal integrado ahora tiene un modo **optimizado para lector de pantalla**. |
| [Diseño del editor centrado](https://code.visualstudio.com/updates/v1_21#_centered-editor-layout). | Maximización del espacio real de la pantalla de visualización de código. |
| [Resultados de búsqueda en horizontal (versión preliminar)](https://code.visualstudio.com/updates/v1_21#_horizontal-search). | Ahora puede ver los resultados de la búsqueda en un panel horizontal. |
| &nbsp; | &nbsp; |

Para obtener más información, consulte las [notas de la versión de Visual Studio Code de febrero](https://code.visualstudio.com/updates/v1_21) y las [notas de la versión de Visual Studio Code de enero](https://code.visualstudio.com/updates/v1_20).

Para más información, vea [Registro de cambios](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md).

## <a name="march-2018"></a>Marzo de 2018

28 de marzo de 2018 &nbsp; / &nbsp; versión: 0.27.3 &nbsp; / &nbsp; Versión preliminar pública

La *versión preliminar pública de marzo* continúa solucionando los principales problemas de GitHub y se centra en mejorar nuestro historial de extensibilidad. En concreto, se habilita el administrador de extensiones, se mejora la administración de paneles y se proporcionan extensiones del Agente SQL y de Insights. Esta versión ofrece las siguientes mejoras:

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Mejora del modelo de extensibilidad de paneles para admitir los paneles de configuración y la información con pestañas. | El administrador de extensiones permite la adquisición simple de extensiones.<br/><br/>Extensiones de panel para sp\_whoisactive de [whoisactive.com](http://www.whoisactive.com).<br/><br/>Para obtener más detalles, vea [Ampliación de la funcionalidad de SQL Operations Studio](./extensions/add-extensions.md). |
| Agregue otras [API de extensibilidad para la administración de conexiones y del explorador de objetos](https://github.com/Microsoft/azuredatastudio/wiki/Extensibility-API). | &nbsp; |
| Corrección continua de [problemas de GitHub](https://github.com/Microsoft/azuredatastudio/issues) importantes que afectan a los clientes. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="february-2018"></a>Febrero de 2018

15 de febrero de 2018 &nbsp; / &nbsp; versión: 0.26.7 &nbsp; / &nbsp; Versión preliminar pública

La *versión preliminar pública de febrero* incluye algunas sugerencias de características y correcciones de errores de alta prioridad. Esta versión ofrece las siguientes mejoras:

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Introducción de la instalación de actualización automática, que proporciona una notificación cuando hay una nueva versión disponible para descargar. | &nbsp; |
| El campo **Base de datos** del cuadro de diálogo de conexión es ahora una lista desplegable rellenada dinámicamente que contiene una lista de bases de datos que se han rellenado desde el servidor especificado. | &nbsp; |
| Introducción de la API de extensibilidad de conexiones. | &nbsp; |
| Integración del editor de VS Code 1.19. | &nbsp; |
| Se ha actualizado el componente JustinPealing/html-query-plan para obtener varias mejoras del visor del plan de consulta. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="fixed-issues-february-2018"></a>Problemas corregidos, febrero de 2018

- Corrección del [problema 6](https://github.com/Microsoft/azuredatastudio/issues/6): Mantener la conexión y la base de datos seleccionada al abrir nuevas pestañas de consulta.
- Corrección del [problema 22](https://github.com/Microsoft/azuredatastudio/issues/22): "Nombre de servidor" y "Nombre de base de datos": ¿pueden listas desplegables en lugar de cuadros de texto?
- Corrección del [problema 549](https://github.com/Microsoft/azuredatastudio/issues/549): Tras una instalación silenciosa o muy silenciosa, se abre la aplicación.
- Corrección del [problema 481](https://github.com/Microsoft/azuredatastudio/issues/481): Agregar la opción "Buscar actualizaciones".
- Corrección de la coloración y finalización automática del Editor de SQL:
  - Corrección del [problema 584](https://github.com/Microsoft/azuredatastudio/issues/584): Palabra clave "FULL" no resaltada por IntelliSense.
  - Corrección del [problema 345](https://github.com/Microsoft/azuredatastudio/issues/345): Colorear funciones SQL en el editor.
  - Corrección del [problema 300](https://github.com/Microsoft/azuredatastudio/issues/300): [#tempData] latest "]" mostrará el color verde.
  - Corrección del [problema 225](https://github.com/Microsoft/azuredatastudio/issues/225): Error de coincidencia de color de palabra clave.
  - Corrección del [problema 60](https://github.com/Microsoft/azuredatastudio/issues/60): Resaltado no válido del color de la sintaxis de SQL al usar una tabla temporal INNER en la cláusula.

## <a name="january-2018"></a>Enero de 2018

17 de enero de 2018 &nbsp; / &nbsp; versión: 0.25.4 &nbsp; / &nbsp; Versión preliminar pública

La *versión preliminar pública de enero* incluye algunas sugerencias de características y correcciones de errores de alta prioridad. Esta versión ofrece las siguientes mejoras:

&nbsp;

| Change | Detalles |
| :----- | :------ |
| Las conexiones de servidor guardadas están disponibles en el cuadro de diálogo de conexión. | &nbsp; |
| Habilitar la salida rápida. La salida rápida está desactivada de forma predeterminada. Para habilitarla, vea [Configuración de la salida rápida](settings.md#hot-exit). | &nbsp; |
| Color de pestañas basado en Grupo de servidores. El color de las pestañas está desactivado de forma predeterminada. Para habilitarlo, vea [Configuración del color de las pestañas](settings.md#tab-color). | &nbsp; |
| *Nombre del servidor* ha cambiado a *Servidor* en el cuadro de diálogo de conexión. | &nbsp; |
| Corrección del comando interrumpido de *ejecución de consulta actual*. | &nbsp; |
| Corrección del error de scripting de interrupción de arrastre y colocación. | &nbsp; |
| Corrección del icono del menú Inicio anclado incorrectamente. | &nbsp; |
| Corrección de la ausencia del icono de personalización de marca de cuenta de Azure. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="december-2017"></a>Diciembre de 2017

19 de diciembre de 2017 &nbsp; / &nbsp; versión: 0.24.1 &nbsp; / &nbsp; Versión preliminar pública

La *versión preliminar pública de diciembre* incluye varias correcciones de errores en todas las áreas de características, así como las siguientes mejoras:

&nbsp;

| Change | Detalles |
| :----- | :------ |
| El cuadro de diálogo Crear regla de firewall ahora está disponible para poder conectarse a Azure SQL Database y Azure Synapse Analytics. | &nbsp; |
| Se ha agregado el programa de instalación de Windows y los paquetes de instalación de Linux DEB y RPM. | &nbsp; |
| Administración del editor de objetos visuales del panel. | &nbsp; |
| Comandos *Crear script como ALTER* y *Script como ejecutar*. | &nbsp; |
| Comando *Ejecutar consulta actual con plan real*. | &nbsp; |
| Integración de la plataforma del editor de VS Code 1.18.1. | &nbsp; |
| Habilitación de la instalación de prueba de archivos de extensión VSIX. | &nbsp; |
| Admite la sintaxis de iteración por lotes "GO N". | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="november-2017"></a>Noviembre de 2017

15 de noviembre de 2017 &nbsp; / &nbsp; versión: 0.23.6

- Versión inicial de Azure Data Studio.

## <a name="next-steps"></a>Pasos siguientes

Para empezar, vea una de las siguientes guías de inicio rápido:

- [Conectar y consultar SQL Server](quickstart-sql-server.md)
- [Conectar y consultar Azure SQL Database](quickstart-sql-database.md)
- [Conectar y hacer consultas en Azure Synapse Analytics](quickstart-sql-dw.md)

Colaboración en Azure Data Studio:

- [https://github.com/Microsoft/azuredatastudio](https://github.com/Microsoft/azuredatastudio)

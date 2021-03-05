---
title: Novedades de SSMA para Oracle (OracleToSQL) | Microsoft Docs
description: Obtenga información sobre los cambios en SQL Server Migration Assistant (SSMA) para Oracle (OracleToSQL) para cada versión.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f305ebb6-7393-4a43-abb3-6332b739d690
ms.author: alexiva
ms.openlocfilehash: ef5f8fd634958c75cc67002615e3c4a72dcf3a51
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186566"
---
# <a name="whats-new-in-ssma-for-oracle-oracletosql"></a>Novedades de SSMA para Oracle (OracleToSQL)

En este artículo se enumeran los cambios SQL Server Migration Assistant (SSMA) para Oracle en cada versión.

## <a name="ssma-v817"></a>SSMA v 8.17

La versión v 8.17 de SSMA para Oracle contiene los siguientes cambios:

* Agregar compatibilidad con columnas calculadas
* Corregir problemas del analizador cuando se utilizan caracteres de operador específicos de la referencia cultural en expresiones
* Actualización de informes de evaluación HTML para usar el editor moderno para mostrar texto SQL

## <a name="ssma-v816"></a>SSMA v 8.16

La versión v 8.16 de SSMA para Oracle contiene los siguientes cambios:

* Agregar compatibilidad para columnas de identidad
* Quitar compatibilidad con el analizador heredado
* Corrección del problema con objetos que no se actualizan desde la base de datos

## <a name="ssma-v815"></a>SSMA v 8.15

Además de varias mejoras de accesibilidad, la versión v 8.15 de SSMA para Oracle contiene los cambios siguientes:

* Agregar configuración para usar la especificación de tipo completo para `%type` `%rowtype` los atributos y
* Remodernización de los informes de evaluación para trabajar en exploradores modernos
* Usar autoridad proporcionada por la base de datos para la autenticación de Azure AD
* Mejorar la nomenclatura de las instrucciones cargadas desde archivos

## <a name="ssma-v814"></a>SSMA v 8.14

Además de varias mejoras para garantizar una mayor accesibilidad para las personas con discapacidades, la versión v 8.14 de SSMA para Oracle contiene los cambios siguientes:

* Almacena la versión completa del servidor de origen/destino en los metadatos del proyecto (requiere la actualización del proyecto)
* Usar diccionarios de datos de DBA para la detección de objetos cuando sea posible
* Corregir varios problemas de los analizadores ( `PIVOT` / `UNPIVOT` , `MERGE` , la cotización alternativa)
* Corrección de la conversión de `INSERTING` / `DELETING` / `UPDATING` funciones especiales en desencadenadores

## <a name="ssma-v813"></a>SSMA v 8.13

La versión v 8.13 de SSMA para Oracle contiene los siguientes cambios:

* Corrección de la conversión de `SQLCODE` `SQLERRM` las funciones especiales y en procedimientos locales
* Considerar conversiones de tipo implícitas al convertir llamadas a procedimientos y funciones
* Mejorar el registro de la cadena de conexión de origen para ayudar a solucionar problemas de conexión

## <a name="ssma-v812"></a>SSMA v 8.12

La versión v 8.12 de SSMA para Oracle contiene los siguientes cambios:

* Compatibilidad con cláusulas de factor de subconsulta en `INSERT` / `UPDATE` / `MERGE` / `DELETE` instrucciones
* Mensaje de conversión para la `ON DELETE SET NULL` cláusula en casos de varias rutas o referencias circulares
* Conversión mejorada de cursores creados a partir de una cadena SQL dinámica
* Actualizar ODP.NET a v 19,8

## <a name="ssma-v811"></a>SSMA v 8.11

La versión v 8.11 de SSMA para Oracle contiene los siguientes cambios:

* Compatibilidad con subconsultas en `INSERT ... VALUES` Statement
* Conversión mejorada de la `COMMIT` instrucción
* Corrección de errores en la conversión de la `CONNECT BY LEVEL` cláusula
* La lógica de recuperación de errores del analizador se ha actualizado para ser menos expansiva
* Usar la biblioteca MSAL.NET para la autenticación interactiva Azure Active Directory

## <a name="ssma-v810"></a>SSMA v 8.10

La versión v 8.10 de SSMA para Oracle contiene pequeñas mejoras de rendimiento, así como los siguientes cambios:

* Corrección del problema del evaluador con tablas organizadas por índices
* Corrección de los nombres de procedimientos almacenados extendidos en el paquete de extensiones

## <a name="ssma-v89"></a>SSMA v 8.9

La versión v 8.9 de SSMA para Oracle contiene los siguientes cambios:

* Conversión de literales de cadena SQL dinámicos
* Conversión para `LAG` `FIRST_VALUE` y `LAST_VALUE` funciones analíticas
* Agregar compatibilidad para `ALTER TRIGGER` / `ALTER INDEX` DDL básico (habilitar o deshabilitar, etc.)
* Conversión mejorada para las columnas que coinciden con los nombres de función integrados
* Generar índices únicos filtrados para `NULL` columnas que permitan
* Conversión de declaración de variables mejorada para Azure Synapse Analytics
* Corrección para el problema con caracteres especiales en el nombre del proyecto

## <a name="ssma-v88"></a>SSMA v 8.8

La versión v 8.8 de SSMA para Oracle incluye:

* Mejoras en la estabilidad de sincronización de objetos SQL Server
* Mejoras en el rendimiento de GUI durante la evaluación y conversión
* Conversión mejorada de `OVER PARTITION` cláusulas analíticas
* Nueva conversión para la `LEAD` función de análisis
* Nueva conversión para las cláusulas de factor de subconsulta
* Nueva `REPLICATE` opción de distribución para Azure Synapse Analytics
* Nuevo analizador de sintaxis de Oracle para mejorar aún más el rendimiento de la conversión

## <a name="ssma-v87"></a>SSMA v 8.7

La versión v 8.7 de SSMA para Oracle tiene correcciones y mejoras de rendimiento menores en la interfaz gráfica de usuario.

Además, SSMA para Oracle ahora permite filtrar objetos en función del estado de validez en el cuadro de diálogo "selección avanzada de objetos".

> [!IMPORTANT]
> Con SSMA v 8.5 y versiones posteriores, .NET 4.7.2 es un requisito previo de instalación. Si necesita instalar esta versión, puede descargar el archivo en tiempo de ejecución desde [aquí](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

Además de un conjunto de correcciones diseñado para mejorar la facilidad de uso y el rendimiento, la versión v 8.6 de SSMA para Oracle se ha mejorado agregando una configuración que permite a los usuarios omitir las propiedades extendidas de SSMA en el código convertido.

Para aprovechar este valor, en SSMA para Oracle, vaya a **herramientas**  >  **configuración del proyecto**  >    >  **conversión** general y, a continuación, en **varios**, actualice el valor de la opción **omitir propiedades extendidas** a **sí**.

![Omitir la configuración de propiedades extendidas](../oracle/media/ssma-omit-extended-properties.png)

Además, SSMA para Oracle ofrece ahora un análisis mejorado de la `XMLTABLE` cláusula.

> [!IMPORTANT]
> Con SSMA v 8.5 y versiones posteriores, .NET 4.7.2 es un requisito previo de instalación. Si necesita instalar esta versión, puede descargar el archivo en tiempo de ejecución desde [aquí](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La versión v 8.5 de SSMA para Oracle se ha mejorado con compatibilidad para la autenticación Azure Active Directory y la compatibilidad básica con características JSON en SQL Server, junto con un conjunto de correcciones diseñado para mejorar la facilidad de uso y el rendimiento.

Además, se ha mejorado SSMA para Oracle con compatibilidad para:

* Limitar el número de objetos seleccionados para la detección a 990 (el límite de la cláusula de Oracle `WHERE .. IN (..)` es 1000 elementos).
* Migración de datos de `RAW` a `UNIQUEIDENTIFIER` .
* Analizando la `PARALLEL_ENABLE` cláusula.

Por último, la versión v 8.5 de SSMA para Oracle ahora proporciona:

* Rendimiento mejorado de las constantes empaquetadas convertidas.
* Una actualización para el proveedor de datos de Oracle para .NET a la versión 19.5.0.

> [!IMPORTANT]
> Con SSMA v 8.5, .NET 4.7.2 es un requisito previo de instalación. Si necesita instalar esta versión, puede descargar el archivo en tiempo de ejecución desde [aquí](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La versión v 8.4 de SSMA para Oracle se ha mejorado con correcciones de destino diseñadas para solucionar problemas de accesibilidad y corregir un error relacionado con las columnas de índice máximo (para permitir 32 en lugar de 16) para SQL Server 2016 y versiones posteriores.

Además, esta versión de SSMA para Oracle agrega conversión para `SYS_REFCURSOR` los parámetros de procedimiento almacenado `OUT` .

> [!IMPORTANT]
> Con las versiones de SSMA de 7,4 a 8,4, .NET 4.5.2 es un requisito previo de instalación.

## <a name="ssma-v83"></a>SSMA v 8.3

El lanzamiento de la versión 8.3 de SSMA para Oracle se ha mejorado con correcciones de destino diseñadas para mejorar la calidad y las métricas de conversión. Además, esta versión de SSMA para Oracle proporciona correcciones que:

* Solucione los problemas de accesibilidad.
* Agregue compatibilidad básica para el `hierarchyid` tipo en SQL Server.
* Solucione un problema con un tipo de valor devuelto desconocido para una función llamada a través de sinónimo.
* Actualice ODP.NET a v 19.3.

## <a name="ssma-v82"></a>SSMA v 8.2

La versión v 8.2 de SSMA para Oracle se ha mejorado para:

* Agregue compatibilidad para `DBMS_OUTPUT.ENABLE` / `DISABLE` .
* Quite `CAST AS FLOAT` para `BINARY_FLOAT` `BINARY_DOUBLE` las columnas y en la consulta de migración de datos predeterminada.
* Corregir secuencias actualizar si el valor actual ha cambiado.
* Corregir el error relacionado con la interpretación incorrecta de las pseudo columnas ( `ROWNUM` , etc.) si existe una columna con el mismo nombre.
* Corrección de un bloqueo que se produce `FOR` al convertir bucles con un identificador sin resolver ambiguo.

Además, esta versión incluye un conjunto de correcciones de destino diseñado para mejorar la calidad y las métricas de conversión, así como las correcciones para:

* Un problema con los índices no clúster deshabilitados después de la migración de datos.
* Detección de .NET Framework durante la instalación silenciosa.
* Un bloqueo intermitente que se produce cuando se descarga una nueva versión.

> [!NOTE]
> Un problema conocido con la actualización automática puede producir un error en una actualización de SSMA v 8.1 a v 8.2. Si se produce este error, descargue la nueva versión e instálela manualmente.

## <a name="ssma-v81"></a>SSMA v 8.1

La versión v 8.1 de SSMA para Oracle se ha mejorado con correcciones de destino diseñadas para mejorar la calidad y las métricas de conversión.

> [!NOTE]
> Un problema conocido con la actualización automática puede producir un error en una actualización de SSMA v 8.0 a v 8.1. Si se produce este error, descargue la nueva versión e instálela manualmente.

## <a name="ssma-v80"></a>SSMA v 8.0

La versión v 8.0 de SSMA para Oracle se ha mejorado con las correcciones de destino diseñadas para mejorar la calidad y las métricas de conversión. Esta versión también ofrece las siguientes características nuevas:

* Compatibilidad con **Azure SQL instancia administrada** como destino. Ahora puede crear nuevos proyectos que tengan como destino Azure SQL Instancia administrada:

  ![Proyecto de MI de SQL](../media/ssma-newproject-sqldbmi.png)

  > [!NOTE]
  > SSMA para Oracle Extension Pack también se actualizó para permitir instalaciones remotas en Azure SQL Instancia administrada:
  >
  > ![SSMA para Oracle Extension Pack](../media/ssma-oracle-ext-pack.png)

  Algunas características, como el evaluador y la migración de datos del lado servidor, no se admiten cuando el destino es Azure SQL Instancia administrada. [Aquí encontrará más información](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/migrate-your-oracle-database-to-azure-sql-database-managed-instance-using-ssma-8-0/).

* **Asesor de corrección** posterior a la conversión. Obtenga más información [aquí](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Selección preliminar de base de datos/esquema.

  Al conectarse al origen, el usuario puede seleccionar las bases de datos o los esquemas de interés. Si selecciona solo los esquemas que va a migrar, se ahorrará tiempo durante la conexión inicial y se mejorará el rendimiento global de SSMA.

  ![SSMA (objetos de filtro)](../media/ssma-filter-objects.png)

* La capacidad de usar el controlador .NET oficial administrado para conectarse a Oracle. El controlador OCI ya no es un requisito previo para usar SQL Server Migration Assistant para Oracle.

* La capacidad de asignar `ROWID` y `UROWID` a de `VARCHAR` forma predeterminada. Cambiado de `uniqueidentifier` para dar cabida a la migración de datos para columnas explícitas `ROWID` .

## <a name="ssma-v710"></a>SSMA v 7.10

La versión v 7.10 de SSMA para Oracle contiene los siguientes cambios:

* Soluciones de destino diseñadas para proporcionar seguridad adicional y protección de la privacidad para satisfacer los cambios en los requisitos globales.
* Mejora de la conversión relacionada con las consultas jerárquicas.

## <a name="ssma-v79"></a>SSMA v 7.9

La versión v 7.9 de SSMA para Oracle contiene los siguientes cambios:

* Correcciones dirigidas que mejoran las métricas de calidad y conversión.
* Compatibilidad para migrar instrucciones "Continue" desde Oracle a SQL Server.
* Compatibilidad en la línea de comandos de SSMA para modificar la asignación de tipos de datos y las preferencias del proyecto.
* Compatibilidad para la migración de datos con SQL Server Integration Services (SSIS). Después de convertir el esquema, es posible crear un paquete SSIS mediante el uso de una opción del menú contextual.
* También se ha modificado el cuadro de diálogo de conexión Azure SQL Database en SSMA para especificar el nombre completo del servidor. En versiones anteriores de SSMA, el prefijo Azure SQL Database tenía que mencionarse explícitamente dentro de la configuración de los proyectos.

## <a name="ssma-v78"></a>SSMA v 7.8

La versión v 7.8 de SSMA para Oracle contiene los siguientes cambios:

* Compatibilidad con:
  * Expresión de fila para la `IN` cláusula.
  * Conversiones de tipos implícitas.
  * `UID` conversión para Azure SQL Database.
* Cambiar la asignación de tipos resaltada en la **configuración del proyecto**.
* La capacidad de los usuarios de deshabilitar la telemetría.

## <a name="ssma-v77"></a>SSMA v 7.7

La versión v 7.7 de SSMA para Oracle contiene los siguientes cambios:

* SSMA para Oracle se ha mejorado con correcciones dirigidas que mejoran las métricas de calidad y conversión.
* En función de la demanda popular, la versión de 32 bits de SSMA para Oracle vuelve a ser. En comparación con la implementación anterior (antes de v 7.4), hay dos paquetes de instalador, pero no se pueden instalar en paralelo. Como resultado, debe elegir la versión más adecuada en función de los componentes de conectividad que tenga. Siempre es preferible usar la versión de 64 bits, si es posible.
* La compatibilidad con SQL Server 2017 es ahora oficial con el paquete de extensiones de Oracle admitido en Linux también (nueva opción de instalación remota). Tenga en cuenta que la funcionalidad del paquete de extensiones está limitada cuando se instala en Linux, ya que no se admiten las características de migración de datos del lado servidor y el evaluador.
* SSMA para Oracle permite migrar las vistas materializadas como tablas normales (se pueden configurar a través de los valores de configuración del **proyecto**  ->  **sincronizar**  ->  **detectar tablas de respaldo para vistas materializadas**).

## <a name="ssma-v76"></a>SSMA v 7.6

La versión v 7.6 de SSMA para Oracle se ha mejorado con correcciones específicas que mejoran las métricas de calidad y conversión y admiten SQL Server 2017 (versión preliminar pública). La compatibilidad con SQL Server 2017 en Windows y Linux está en versión preliminar pública y no debe usarse para migraciones de producción.

## <a name="ssma-v75"></a>SSMA v 7.5

La versión v 7.5 de SSMA para Oracle contiene los siguientes cambios:

* Mejorado con varias mejoras para garantizar una mayor accesibilidad para las personas con discapacidades.
* Se actualizó para mejorar la calidad y la métrica de conversión con correcciones específicas, como el control mejorado de los tipos de datos de fecha y flota durante la migración de datos, en función de los comentarios de los clientes.

## <a name="ssma-v74"></a>SSMA v 7.4

La versión v 7.4 de SSMA para Oracle contiene los siguientes cambios:

* SSMA para Oracle ahora es compatible con Azure Synapse Analytics como plataforma de destino para la migración.

  ![Ventana Nuevo proyecto](../media/new-project.png)
  * Admite las opciones de almacenamiento del almacenamiento de datos como se muestra en la siguiente imagen:

  ![Opciones de almacenamiento para el almacenamiento de datos](../media/storage-options_red.png)
  * Admite las opciones de distribución de datos como se muestra en la siguiente imagen:

  ![distribución de datos para el almacenamiento de datos](../media/data-distribution_red.png)

* La opción **tiempo de espera de consulta** ahora está disponible durante la detección de objetos de esquema en el origen y el destino.

  ![opción de tiempo de espera de consulta](../media/query-timeout_red.png)

* La métrica de calidad y conversión se ha mejorado con las correcciones de destino, en función de los comentarios de los clientes.

> [!IMPORTANT]
> .NET 4.5.2 es un requisito previo para instalar SSMA v 7.4. Además, a partir de v 7.4, se suspende la versión de 32 bits de SSMA.

## <a name="ssma-v73"></a>SSMA v 7.3

La versión v 7.3 de SSMA para Oracle contiene los siguientes cambios:

* Métrica de calidad y conversión mejorada con correcciones de destino basadas en los comentarios de los clientes.
* El marco de extensibilidad de SSMA expuesto a través de los siguientes elementos:
  * Exportar funcionalidad a un proyecto SQL Server Data Tools (SSDT).
    * Ahora puede exportar scripts de esquema de SSMA a un proyecto de SSDT. Puede usar los scripts de esquema para realizar cambios de esquema adicionales e implementar la base de datos.

      ![Comando Guardar como proyecto de SSDT](../media/export-schema-scripts_red.png)
  * Bibliotecas que puede usar SSMA para realizar conversiones personalizadas.
    * Ahora puede construir código que pueda controlar las conversiones de sintaxis personalizadas y las conversiones que no se administraron previamente mediante SSMA.
      * En esta entrada de blog se ofrecen instrucciones sobre cómo construir un convertidor personalizado, que [amplían las capacidades de conversión de SQL Server Migration Assistant](https://blogs.msdn.microsoft.com/datamigration/2017/02/21/2185/).
      * Descargue un proyecto de ejemplo para la conversión de esta [entrada de blog](https://blogs.msdn.microsoft.com/datamigration/ssmafororacleconversionsample/).

## <a name="ssma-v72"></a>SSMA v 7.2

La versión v 7.2 de SSMA para Oracle contiene los siguientes cambios:

* Métrica de calidad y conversión mejorada con correcciones de destino basadas en los comentarios de los clientes.
* Mejoras en la telemetría para proporcionar mejores puntos de datos para solucionar problemas de los clientes y mejorar las tasas de conversión de SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La versión v 7.1 de SSMA para Oracle contiene los siguientes cambios:

* SQL Server 2017 en Windows y Linux CTP1 es ahora una plataforma de destino admitida para la migración. Esta característica se encuentra en Technical Preview y permite el movimiento de datos y esquemas a los servidores SQL Server de destino.
* SSMA admite ahora las actualizaciones automáticas para descargar la versión más reciente de SSMA en cuanto esté disponible.
* Los binarios instalables de SSMA se entregan ahora a través de Windows Installer archivos de paquete (. msi).

## <a name="may-2016"></a>Mayo de 2016

La versión de mayo de 2016 de SSMA para Oracle contiene los siguientes cambios:

* Compatibilidad agregada para SQL Server 2016.
* Se ha agregado la conversión de tablas de archivo Oracle Flashback a SQL Server tablas temporales.

  > [!NOTE]
  > SSMA no copia los datos de historial de las tablas de archivo de datos de Oracle Flashback. Como resultado, los datos del historial se deben copiar manualmente durante el proceso de migración. Además, aunque SSMA no muestra la tabla de historial en el explorador de metadatos de SQL Server porque se trata como una tabla del sistema, puede ver la tabla de historial en SQL Server Management Studio.
  >
  > SQL Server 2016 no admite varias características de Oracle Flashback, entre las que se incluyen:
  >
  >   * Consulta de transacciones de Oracle Flashback
  >   * `DBMS_FLASHBACK` Paquete
  >   * Transacción flashback
  >   * Archivo de datos Flashback
  >   * Tabla flashback
  >   * Drop de flashback
  >   * Base de datos Flashback

* Conversión agregada de la Directiva de VPD de Oracle a SQL Server objetos de directiva (Seguridad de nivel de fila para Oracle).
* Menor tiempo de carga inicial para Oracle.
* Se ha mejorado el analizador y la resolución.
* Se quitó la comprobación del instalador para .NET 2,0.
* Dependencia del paquete de extensiones actualizada de .NET 3,5 a .NET 4,0.
* Fixed `save-project` y `open-project` comandos para la consola de SSMA.
* `securepassword`Comando fijo para la consola de SSMA.
* Recuento fijo de objetos para la carga inicial.
* Se ha corregido la conversión de tipos de datos de caracteres para Oracle.
* Se corrigió el error en la configuración global.

## <a name="march-2016"></a>Marzo de 2016

La versión de vista previa de marzo de 2016 de SSMA para Oracle agregó compatibilidad con:

* Migración a SQL Server 2016.
* Migración de Seguridad de nivel de fila de Oracle (con algunas limitaciones).
* Migrar las tablas de Oracle en memoria a SQL Server almacén de columnas.

## <a name="january-2016"></a>Enero de 2016

La versión de mantenimiento de enero de 2014 de SSMA para Oracle contiene los siguientes cambios:

* Compatibilidad agregada para los índices clúster.
* Se han corregido las consultas lentas de esquema de Oracle (RFC 5076207).
* Se corrigió la conexión a Azure desde la consola.
* Se ha agregado el elemento de menú Ver registro a SSMA (RFC 5706203).
* Telemetría agregada.

## <a name="july-2014"></a>Julio de 2014

La versión 2014 de julio de SSMA para Oracle contiene los siguientes cambios:

* Compatibilidad agregada para Azure SQL Database.
* Funcionalidad del paquete de extensión que se ha migrado al esquema para admitir Azure SQL Database.
* Compatibilidad agregada para las vistas materializadas de Oracle.
* Se ha agregado compatibilidad con las tablas con optimización para memoria de SQL Server 2014.
* Se han incluido mejoras de rendimiento probadas para bases de datos con más de 10 000 objetos.
* Se han agregado mejoras de la interfaz de usuario para tratar con un gran número de objetos.
* Se ha agregado el resaltado de esquemas de LOB conocidos.
* Mejoras en la velocidad de conversión incluida.
* Compatibilidad agregada para mostrar los recuentos de objetos en la interfaz de usuario.
* Menor tamaño de informe en más del 25%.
* Mensajes de error mejorados para construcciones sin analizar.

## <a name="april-2014"></a>Abril de 2014

La versión de abril de 2014 de SSMA para Oracle contiene los siguientes cambios:

* Compatibilidad agregada de MS SQL Server 2014.
* Se ha agregado compatibilidad con Oracle 12 y la optimización de consultas.
* Se corrigieron los errores relacionados con la conversión a Azure.
* Se han corregido errores relacionados con las páginas del informe invisibles en IE 10.

## <a name="january-2012"></a>enero de 2012

La versión 2012 de enero de SSMA para Oracle agrega compatibilidad `RowType` con `RecordType` los parámetros de entrada y de forma predeterminada a `NULL` .

## <a name="july-2011"></a>julio de 2011

La versión 2011 de julio de SSMA para Oracle contiene los siguientes cambios:

* Se agregó compatibilidad para la conversión de la secuencia de Oracle al [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] generador de secuencias.
* Informes de errores mejorados durante la migración de datos.
* Conversión mejorada de la instrucción mediante palabras reservadas.
* Conversión implícita mejorada del valor de fecha en una función.

## <a name="april-2011"></a>2011 de abril

La versión de abril de 2011 de SSMA para Oracle contiene los siguientes cambios:

* Producto "SSMA para Oracle" consolidado, que admite [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] , [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] y [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Se ha agregado compatibilidad para conectar y migrar a [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Motor de migración de datos del lado cliente mejorado, que admite la migración en paralelo de datos.
* Rendimiento mejorado de la migración de datos con `Simple` y `Bulk` modelos de recuperación registrados.
* Compatibilidad agregada para la compatibilidad con versiones anteriores de los proyectos creados con versiones anteriores de SSMA (v 4.0 y v 4.2).
* Se ha agregado la posibilidad de instalar SSMA para el producto Oracle v 5.0 en paralelo (SxS) con versiones anteriores de SSMA (v 4.0 y v 4.2).
* Compatibilidad agregada para informes User-Defined tipos (incluye subtipo, `VARRAY` , `NESTED TABLE` , tabla de objetos y vista de objetos) y sus usos en bloques PL/SQL con mensajes de error especiales.

## <a name="july-2010"></a>Julio de 2010

Se ha agregado la versión 2010 de julio de SSMA para Oracle:

* Compatibilidad para migrar a SQL Server 2008 R2.
* Una nueva aplicación de consola SSMA para la ejecución de la línea de comandos.
* Compatibilidad con la migración de datos mediante motores de migración de datos Server-Side y Client-Side.
* Compatibilidad con la instrucción SELECT personalizada en la migración de datos.
* Compatibilidad con la migración desde Oracle 11g R2.

## <a name="june-2008"></a>2008 de junio

La versión 2008 de junio de SSMA para Oracle contiene los siguientes cambios:

* Se han agregado mejoras en el informe de evaluación, incluida información adicional para los sinónimos, el origen sin formato para los objetos que se deben analizar, los paneles y el SQL Server la eliminación del logotipo y la persistencia del diseño.
* Mejoras agregadas en la conversión de objetos:
  * Paquetes `DBMS_LOB` , `DBMS_SQL` conversión agregada.
  * Se ha revisado la conversión.
  * Modificación de la conversión de colecciones y registros; ahora, la conversión de registros en casos simples se libera a través de variables independientes para cada campo.
  * Mejoras en la implementación de registros y colecciones.
  * Funciones de agregación de ventanas agregadas.
  * `ROLLUP`/`CUBE` cláusula agregada.
  * Mejora para `NEXTVAL` / `CURVAL` .
  * Se han agregado la agrupación de columnas en la `SET` cláusula, los conjuntos de agrupación y el ID. de agrupación.
  * `MERGE` instrucción agregada.
  * Compatibilidad con los nuevos tipos de fecha y hora y la conversión de registros y colecciones a medida que se agregan tipos de datos CLR.
* Se han agregado nuevas características de evaluador. Ahora, las tablas se pueden probar como objetos mediante el evaluador, se puede modificar un orden de llamada de varios objetos que se pueden comprobar en el caso de prueba, el usuario puede probar procedimientos y funciones con registros y colecciones como parámetros y valores devueltos, y se ha agregado un analizador de dependencias para comprobar solo las tablas usadas.
  
## <a name="august-2007"></a>2007 de agosto

Se ha agregado la versión 2007 de agosto de SSMA para Oracle:

* Un nuevo componente de evaluador permite crear, administrar y ejecutar casos de prueba para comprobar el código SQL convertido.
* Se ha agregado compatibilidad con la conversión de los subtipos, las colecciones y los módulos locales de Oracle en el convertidor de SQL.
* Una nueva característica de sincronización le permite sincronizar objetos específicos con SQL Server base de datos.
* Nuevas opciones de conversión.
  
## <a name="april-2007"></a>2007 de abril

La versión de abril de 2007 de SSMA para Oracle fue la versión inicial.

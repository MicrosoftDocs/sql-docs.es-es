---
title: Novedades de SSMA para MySQL (MySQLToSql) | Microsoft Docs
description: Obtenga información sobre los cambios en SQL Server Migration Assistant (SSMA) para MySQL (MySQLToSQL) para cada versión.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1451a0b0-6713-4d0c-954f-ea3d8fce1d31
ms.author: alexiva
ms.openlocfilehash: 138cb90558bfb73d0b9e0cd3ba75a7a41fc84229
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186461"
---
# <a name="whats-new-in-ssma-for-mysql-mysqltosql"></a>Novedades de SSMA para MySQL (MySQLToSql)

En este artículo se enumeran los cambios de SQL Server Migration Assistant (SSMA) para MySQL en cada versión.

## <a name="ssma-v817"></a>SSMA v 8.17

La versión v 8.17 de SSMA para MySQL contiene los siguientes cambios:

* Actualización de informes de evaluación HTML para usar el editor moderno para mostrar texto SQL

## <a name="ssma-v816"></a>SSMA v 8.16

La versión v 8.16 de SSMA para MySQL contiene los siguientes cambios:

* Agregar compatibilidad con columnas calculadas
* Corregir problemas al convertir `INSERT` instrucciones para tablas con restricciones UNIQUE y claves principales
* Actualizar el analizador a los `ANSI_QUOTES` modos de cumplimiento y `NO_BACKSLASH_ESCAPES` servidor
* Quitar compatibilidad con el analizador heredado
* Corrección del problema con objetos que no se actualizan desde la base de datos

## <a name="ssma-v815"></a>SSMA v 8.15

Además de varias mejoras de accesibilidad, la versión v 8.15 de SSMA para MySQL contiene los cambios siguientes:

* Remodernización de los informes de evaluación para trabajar en exploradores modernos
* Usar autoridad proporcionada por la base de datos para la autenticación de Azure AD
* Mejorar la nomenclatura de las instrucciones cargadas desde archivos

## <a name="ssma-v814"></a>SSMA v 8.14

Además de varias mejoras para garantizar una mayor accesibilidad para las personas con discapacidades, la versión v 8.14 de SSMA para MySQL requiere una actualización del proyecto, ya que ahora almacena la versión completa del servidor de origen/destino en los metadatos del proyecto.

## <a name="ssma-v813"></a>SSMA v 8.13

La versión v 8.13 de SSMA para MySQL contiene los cambios siguientes:

* Considerar conversiones de tipo implícitas al convertir llamadas a procedimientos y funciones
* Mejorar el registro de la cadena de conexión de origen para ayudar a solucionar problemas de conexión

## <a name="ssma-v812"></a>SSMA v 8.12

La versión v 8.12 de SSMA para MySQL contiene los cambios siguientes:

* Conversión de DDL de tablas temporales

## <a name="ssma-v811"></a>SSMA v 8.11

La versión v 8.11 de SSMA para MySQL contiene los cambios siguientes:

* Usar la biblioteca MSAL.NET para la autenticación interactiva Azure Active Directory

## <a name="ssma-v810"></a>SSMA v 8.10

La versión v 8.10 de SSMA para MySQL contiene pequeñas mejoras de rendimiento y correcciones de errores.

## <a name="ssma-v89"></a>SSMA v 8.9

La versión v 8.9 de SSMA para MySQL contiene los siguientes cambios:

* Corrección para la migración de datos de tipos espaciales
* Corrección para el problema con caracteres especiales en el nombre del proyecto

## <a name="ssma-v88"></a>SSMA v 8.8

La versión v 8.8 de SSMA para MySQL incluye:

* Mejoras en la estabilidad de sincronización de objetos SQL Server
* Mejoras en el rendimiento de GUI durante la evaluación y conversión

## <a name="ssma-v87"></a>SSMA v 8.7

La versión v 8.7 de SSMA para MySQL tiene correcciones y mejoras de rendimiento menores en la interfaz gráfica de usuario.

Además, SSMA para MySQL ahora proporciona conversión para la `LIMIT` cláusula cuando el destino es Azure SQL.

> [!IMPORTANT]
> Con SSMA v 8.5 y versiones posteriores, .NET 4.7.2 es un requisito previo de instalación. Si necesita instalar esta versión, puede descargar el archivo en tiempo de ejecución desde [aquí](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

Además de un conjunto de correcciones diseñado para mejorar la facilidad de uso y el rendimiento, la versión v 8.6 de SSMA para MySQL se ha mejorado agregando una configuración que permite a los usuarios omitir las propiedades extendidas de SSMA en el código convertido.

Para aprovechar esta configuración, en SSMA para MySQL, vaya a **herramientas**  >  **configuración del proyecto**  >    >  **conversión** general y, a continuación, en **varios**, actualice el valor de la opción **omitir propiedades extendidas** a **sí**.

![Omitir la configuración de propiedades extendidas](../mysql/media/ssma-omit-extended-properties.png)

> [!IMPORTANT]
> Con SSMA v 8.5 y versiones posteriores, .NET 4.7.2 es un requisito previo de instalación. Si necesita instalar esta versión, puede descargar el archivo en tiempo de ejecución desde [aquí](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La versión v 8.5 de SSMA para MySQL se ha mejorado con compatibilidad para la autenticación Azure Active Directory y la compatibilidad básica con las características de JSON en SQL Server, junto con un conjunto de correcciones diseñado para mejorar la facilidad de uso y el rendimiento.

> [!IMPORTANT]
> Con SSMA v 8.5, .NET 4.7.2 es un requisito previo de instalación. Si necesita instalar esta versión, puede descargar el archivo en tiempo de ejecución desde [aquí](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La versión v 8.4 de SSMA para MySQL se ha mejorado con correcciones de destino diseñadas para solucionar problemas de accesibilidad y corregir un error relacionado con las columnas de índice máximo (para permitir 32 en lugar de 16) para SQL Server 2016 y versiones posteriores.

> [!IMPORTANT]
> Con las versiones de SSMA 7,4 a 8,4, .NET 4.5.2 es un requisito previo de instalación.

## <a name="ssma-v83"></a>SSMA v 8.3

El lanzamiento de la versión 8.3 de SSMA para MySQL se ha mejorado con correcciones de destino diseñadas para mejorar la calidad y las métricas de conversión. Además, esta versión de SSMA para MySQL proporciona correcciones que:

* Solucione los problemas de accesibilidad.
* Agregue compatibilidad básica para el `hierarchyid` tipo en SQL Server.

## <a name="ssma-v82"></a>SSMA v 8.2

La versión v 8.2 de SSMA para MySQL se ha mejorado con un conjunto de correcciones diseñado para mejorar las métricas de calidad y conversión, así como las correcciones para:

* Un problema con los índices no clúster deshabilitados después de la migración de datos.
* Detección de .NET Framework durante la instalación silenciosa.
* Un bloqueo intermitente que se produce cuando se descarga una nueva versión.

> [!NOTE]
> Un problema conocido con la actualización automática puede producir un error en una actualización de SSMA v 8.1 a v 8.2. Si se produce este error, descargue la nueva versión e instálela manualmente.

## <a name="ssma-v81"></a>SSMA v 8.1

La versión v 8.1 de SSMA para MySQL se ha mejorado con correcciones de destino diseñadas para mejorar la calidad y las métricas de conversión.

> [!NOTE]
> Un problema conocido con la actualización automática puede producir un error en una actualización de SSMA v 8.0 a v 8.1. Si se produce este error, descargue la nueva versión e instálela manualmente.

## <a name="ssma-v80"></a>SSMA v 8.0

La versión v 8.0 de SSMA para MySQL se ha mejorado con las correcciones de destino diseñadas para mejorar la calidad y las métricas de conversión. Esta versión también ofrece las siguientes características nuevas:

* Compatibilidad con **Azure SQL instancia administrada** como destino. Ahora puede crear nuevos proyectos que tengan como destino Azure SQL Instancia administrada:

  ![Proyecto de MI de SQL](../media/ssma-newproject-sqldbmi.png)

* **Asesor de corrección** posterior a la conversión. Obtenga más información [aquí](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Selección preliminar de base de datos/esquema.

  Al conectarse al origen, el usuario puede seleccionar las bases de datos o los esquemas de interés. Si selecciona solo los esquemas que va a migrar, se ahorrará tiempo durante la conexión inicial y se mejorará el rendimiento global de SSMA.

  ![SSMA (objetos de filtro)](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

La versión v 7.10 de SSMA para MySQL contiene los cambios siguientes:

* Soluciones de destino diseñadas para proporcionar seguridad adicional y protección de la privacidad para satisfacer los cambios en los requisitos globales.
* Corrección para la conversión de espacios entre el nombre de función y la lista de argumentos.

## <a name="ssma-v79"></a>SSMA v 7.9

La versión v 7.9 de SSMA para MySQL contiene los siguientes cambios:

* Correcciones dirigidas que mejoran las métricas de calidad y conversión.
* Compatibilidad parcial para la migración de tipos de datos espaciales de MySQL a Azure SQL Database.
* Compatibilidad en la línea de comandos de SSMA para modificar la asignación de tipos de datos y las preferencias del proyecto.
* Compatibilidad para la migración de datos con SQL Server Integration Services (SSIS). Después de convertir el esquema, es posible crear un paquete SSIS mediante el uso de una opción del menú contextual.
* También se ha modificado el cuadro de diálogo de conexión Azure SQL Database en SSMA para especificar el nombre completo del servidor. En versiones anteriores de SSMA, el prefijo Azure SQL Database tenía que mencionarse explícitamente dentro de la configuración de los proyectos.

## <a name="ssma-v78"></a>SSMA v 7.8

La versión v 7.8 de SSMA para MySQL contiene los cambios siguientes:

* Cambiar la asignación de tipos resaltada en la configuración del proyecto.
* La capacidad de los usuarios de deshabilitar la telemetría.

## <a name="ssma-v77"></a>SSMA v 7.7

La versión v 7.7 de SSMA para MySQL contiene los cambios siguientes:

* SSMA para MySQL se ha mejorado con correcciones de destino que mejoran las métricas de calidad y conversión.
* En función de la popular solicitud, se devuelve la versión de 32 bits de SSMA para MySQL. En comparación con la implementación anterior (antes de v 7.4), hay dos paquetes de instalador, pero no se pueden instalar en paralelo. Como resultado, debe elegir la versión más adecuada en función de los componentes de conectividad que tenga. Siempre es preferible usar la versión de 64 bits, si es posible.
* SSMA para MySQL ahora tiene el modo de conexión de cadena de conexión ODBC, que le permite usar cualquier controlador ODBC de terceros que sea compatible con MySQL.

## <a name="ssma-v76"></a>SSMA v 7.6

La versión v 7.6 de SSMA para MySQL se ha mejorado con correcciones específicas que mejoran las métricas de calidad y conversión y admiten SQL Server 2017 (versión preliminar pública). La compatibilidad con SQL Server 2017 en Windows y Linux está en versión preliminar pública y no debe usarse para migraciones de producción.

## <a name="ssma-v75"></a>SSMA v 7.5

La versión v 7.5 de SSMA para MySQL se ha mejorado con varias mejoras para garantizar una mayor accesibilidad para las personas con discapacidades.

## <a name="ssma-v74"></a>SSMA v 7.4

La versión v 7.4 de SSMA para MySQL contiene los siguientes cambios:

* La opción **tiempo de espera de consulta** ahora está disponible durante la detección de objetos de esquema en el origen y el destino.

    ![opción de tiempo de espera de consulta](../media/query-timeout_red.png)
* La métrica de calidad y conversión se ha mejorado con las correcciones de destino, en función de los comentarios de los clientes.

> [!IMPORTANT]
> .NET 4.5.2 es un requisito previo para instalar SSMA v 7.4. Además, a partir de v 7.4, se suspende la versión de 32 bits de SSMA.

## <a name="ssma-v73"></a>SSMA v 7.3

La versión v 7.3 de SSMA para MySQL contiene los siguientes cambios:

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

La versión v 7.2 de SSMA para MySQL contiene los siguientes cambios:

* Métrica de calidad y conversión mejorada con correcciones de destino basadas en los comentarios de los clientes.
* Mejoras en la telemetría para proporcionar mejores puntos de datos para solucionar problemas de los clientes y mejorar las tasas de conversión de SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La versión v 7.1 de SSMA para MySQL contiene los siguientes cambios:

* SQL Server 2017 en Windows y Linux CTP1 es ahora una plataforma de destino admitida para la migración. Esta característica se encuentra en Technical Preview y permite el movimiento de datos y esquemas a los servidores SQL Server de destino.
* SSMA admite ahora las actualizaciones automáticas para descargar la versión más reciente de SSMA en cuanto esté disponible.
* Los binarios instalables de SSMA se entregan ahora a través de Windows Installer archivos de paquete (. msi).

## <a name="may-2016"></a>Mayo de 2016

La versión de mayo de 2016 de SSMA para MySQL contiene los siguientes cambios:

* Compatibilidad agregada para SQL Server 2016.
* Se ha mejorado el analizador y la resolución.
* Se quitó la comprobación del instalador para .NET 2,0.
* Dependencia del paquete de extensiones actualizada de .NET 3,5 a .NET 4,0.
* Asignación de tipo BigInt predeterminada fija para MySql.
* Fixed `save-project` y `open-project` comandos para la consola de SSMA.
* `securepassword`Comando fijo para la consola de SSMA.
* Recuento fijo de objetos para la carga inicial.
* Carga de objetos MsSql corregidos.
* Se corrigió el error en la configuración global.

## <a name="march-2016"></a>Marzo de 2016

La versión de vista previa de marzo de 2016 de SSMA para MySQL agrega compatibilidad para la migración a SQL Server 2016.
  
## <a name="january-2016"></a>Enero de 2016

La versión de mantenimiento de enero de 2016 de SSMA para MySQL contiene los cambios siguientes:

* Se ha agregado el elemento de menú Ver registro a SSMA (RFC 5706203).
* Telemetría agregada.
  
## <a name="july-2014"></a>Julio de 2014

La versión 2014 de julio de SSMA para MySQL contiene los siguientes cambios:
  
* Conversión de código Azure SQL Database mejorada.
* Funcionalidad del paquete de extensión que se ha migrado al esquema para admitir Azure SQL Database.
* Mejoras de rendimiento probadas para bases de datos con más de 10 000 objetos.
* Mejoras de la interfaz de usuario para tratar con un gran número de objetos.
* Resaltado de esquemas de LOB conocidos (por lo que se pueden omitir en la conversión).
* Mejoras en la velocidad de conversión.
* Muestra los recuentos de objetos en la interfaz de usuario.
* Reducción del tamaño de informe en más del 25%.
* Mensajes de error mejorados para construcciones sin analizar.
  
## <a name="april-2014"></a>Abril de 2014

La versión de abril de 2014 de SSMA para MySQL contiene los siguientes cambios:
  
* Compatibilidad agregada para SQL Server 2014.
* Se corrigieron los errores relacionados con la conversión a Azure.
* Se han corregido errores relacionados con las páginas del informe invisibles en IE 10.
  
## <a name="july-2011"></a>julio de 2011

La versión 2011 de julio de SSMA para MySQL contiene los siguientes cambios:
  
* Compatibilidad con la conversión de `LIMIT` a [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] `OFFSET` .
* Informes de errores mejorados durante la migración de datos.
  
## <a name="april-2011"></a>2011 de abril

La versión de abril de 2011 de SSMA para MySQL contiene los siguientes cambios:
  
* Instalación única de "SSMA para MySQL", que admite [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] , [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] y Azure SQL.
* La capacidad de conectarse [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Motor de migración de datos del lado cliente mejorado, que admite la migración en paralelo de datos.
* Rendimiento mejorado de la migración de datos con modelos de recuperación simples y de registro masivo.
* La versión de la consola de SSMA para MySQL admite la compatibilidad con versiones anteriores. Puede abrir los proyectos creados por versiones anteriores a SSMA v 5.0.
* El producto SSMA for MySQL v 5.0 se puede instalar en paralelo (SxS) con versiones anteriores del producto SSMA.
  
## <a name="july-2010"></a>Julio de 2010

La versión 2010 de julio de SSMA para MySQL contiene las siguientes características:
  
**1. mejoras en la interfaz de usuario:**

* Pestaña ' modos SQL ' para objetos de base de datos MySQL
* Pestaña ' configuración ' para objetos de base de datos MySQL
* Pestaña ' datos ' para tablas de MySQL
* Configuración de proyecto actualizada en páginas de conversión y migración
* ' Configuración de migración de datos ' en el nivel de tabla
  
**2. mejoras para conectarse a MySQL y SQL Server:**
  
* Conectividad SSL en MySQL
* Conectividad cifrada en SQL Server
  
**3. mejoras en el explorador de metabase de MySQL:**
  
* Cargar todos los objetos de base de datos MySQL y sus respectivas pestañas.
  
**4. mejoras en la conversión de objetos:**
  
* Conversión de objetos de metabase de MySQL: procedimientos, funciones, vistas, desencadenadores e instrucciones.
* Compatibilidad limitada para los tipos de datos espaciales en las tablas.
* Opción para convertir las funciones de MySQL en SQL Server procedimientos almacenados
* Opción para aplicar los modos SQL y la asignación charset durante la conversión de objetos
  
**5. mejoras en la migración de datos:**  
  
* Compatibilidad con la migración de datos mediante motores de migración de datos Server-Side y Client-Side
* Compatibilidad con la migración de datos espaciales
* SQL personalizado para la migración de datos para tablas
  
**6. SSMA para la consola de MySQL:**  
  
* Característica de la consola de soporte para SSMA para MySQL  
* Compatibilidad con la interconexión de Script-Level  
  
## <a name="january-2010"></a>2010 de enero

La versión de enero de 2010 de SSMA para MySQL fue la versión inicial. Contiene las siguientes características:
  
* Compatibilidad agregada para la migración a la SQL Server local y a Azure SQL.  
  
* **Instantánea de características:** Migración de esquemas y datos de tablas, índices y restricciones de MySQL.

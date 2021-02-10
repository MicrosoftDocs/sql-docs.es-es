---
title: Novedades de SSMA para SAP ASE (SybaseToSQL) | Microsoft Docs
description: Obtenga información sobre los cambios en SQL Server Migration Assistant (SSMA) para Sybase (SybaseToSQL) para cada versión.
author: nahk-ivanov
ms.prod: sql
ms.custom: ''
ms.date: 12/17/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2be0cf8d-6dbe-443a-abbd-036249922205
ms.author: alexiva
ms.openlocfilehash: e63eea932696ca0304443722b40518017ae43485
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065050"
---
# <a name="whats-new-in-ssma-for-sap-ase-sybasetosql"></a>Novedades de SSMA para SAP ASE (SybaseToSQL)

En este artículo se enumeran los cambios de SQL Server Migration Assistant (SSMA) para SAP ASE (anteriormente SSMA para Sybase) en cada versión.

## <a name="ssma-v816"></a>SSMA v 8.16

La versión v 8.16 de SSMA para SAP ASE contiene los siguientes cambios:

* Quitar compatibilidad con el analizador heredado
* Corrección del problema con objetos que no se actualizan desde la base de datos

## <a name="ssma-v815"></a>SSMA v 8.15

Además de varias mejoras de accesibilidad, la versión v 8.15 de SSMA for SAP ASE contiene los siguientes cambios:

* Remodernización de los informes de evaluación para trabajar en exploradores modernos
* Usar autoridad proporcionada por la base de datos para la autenticación de Azure AD
* Mejorar la nomenclatura de las instrucciones cargadas desde archivos

## <a name="ssma-v814"></a>SSMA v 8.14

Además de varias mejoras para garantizar una mayor accesibilidad para las personas con discapacidades, la versión v 8.14 de SSMA for SAP ASE requiere una actualización del proyecto, ya que ahora almacena la versión completa del servidor de origen/destino en los metadatos del proyecto.

## <a name="ssma-v813"></a>SSMA v 8.13

La versión v 8.13 de SSMA para SAP ASE contiene los siguientes cambios:

* Considerar conversiones de tipo implícitas al convertir llamadas a procedimientos y funciones
* Mejorar el registro de la cadena de conexión de origen para ayudar a solucionar problemas de conexión

## <a name="ssma-v812"></a>SSMA v 8.12

La versión v 8.12 de SSMA for SAP ASE contiene pequeñas mejoras de rendimiento y correcciones de errores.

## <a name="ssma-v811"></a>SSMA v 8.11

La versión v 8.11 de SSMA para SAP ASE contiene los siguientes cambios:

* Corrección de la conversión de tablas temporales
* Usar la biblioteca MSAL.NET para la autenticación interactiva Azure Active Directory

## <a name="ssma-v810"></a>SSMA v 8.10

La versión v 8.10 de SSMA for SAP ASE contiene pequeñas mejoras de rendimiento y correcciones de errores.

## <a name="ssma-v89"></a>SSMA v 8.9

La versión v 8.9 de SSMA para SAP ASE contiene los siguientes cambios:

* Mejorar la conversión de formato de fecha y hora
* Corrección para el problema con los caracteres que faltan en las definiciones de SQL para objetos

## <a name="ssma-v88"></a>SSMA v 8.8

La versión v 8.8 de SSMA para SAP ASE incluye:

* Mejoras en la estabilidad de sincronización de objetos SQL Server
* Mejoras en el rendimiento de GUI durante la evaluación y conversión
* Corrección para el problema con los caracteres que faltan en las definiciones de SQL para objetos

## <a name="ssma-v87"></a>SSMA v 8.7

La versión v 8.7 de SSMA for SAP ASE tiene correcciones y mejoras de rendimiento menores en la interfaz gráfica de usuario.

> [!IMPORTANT]
> Con SSMA v 8.5 y versiones posteriores, .NET 4.7.2 es un requisito previo de instalación. Si necesita instalar esta versión, puede descargar el archivo en tiempo de ejecución desde [aquí](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v86"></a>SSMA v 8.6

Además de un conjunto de correcciones diseñado para mejorar la facilidad de uso y el rendimiento, la versión v 8.6 de SSMA for SAP ASE se ha mejorado agregando una configuración que permite a los usuarios omitir las propiedades extendidas de SSMA en el código convertido.

Para aprovechar esta configuración, en SSMA for SAP ase, vaya a **herramientas**  >  **configuración del proyecto**  >    >  **conversión** general y, a continuación, en **varios**, actualice el valor de la opción **omitir propiedades extendidas** a **sí**.

![Omitir la configuración de propiedades extendidas](../sybase/media/ssma-omit-extended-properties.png)

> [!IMPORTANT]
> Con SSMA v 8.5 y versiones posteriores, .NET 4.7.2 es un requisito previo de instalación. Si necesita instalar esta versión, puede descargar el archivo en tiempo de ejecución desde [aquí](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v85"></a>SSMA v 8.5

La versión v 8.5 de SSMA for SAP ASE se ha mejorado con compatibilidad para la autenticación Azure Active Directory y la compatibilidad básica con características JSON en SQL Server, junto con un conjunto de correcciones diseñado para mejorar la facilidad de uso y el rendimiento.

Además, SSMA for SAP ASE le permite ocultar las tablas y vistas del sistema (excluirlas de la conversión).

> [!IMPORTANT]
> Con SSMA v 8.5, .NET 4.7.2 es un requisito previo de instalación. Si necesita instalar esta versión, puede descargar el archivo en tiempo de ejecución desde [aquí](https://dotnet.microsoft.com/download/dotnet-framework/net472).

## <a name="ssma-v84"></a>SSMA v 8.4

La versión v 8.4 de SSMA para SAP ASE se ha mejorado con correcciones de destino diseñadas para solucionar problemas de accesibilidad y corregir un error relacionado con las columnas de índice máximo (para permitir 32 en lugar de 16) para SQL Server 2016 y versiones posteriores.

> [!IMPORTANT]
> Con las versiones de SSMA de 7,4 a 8,4, .NET 4.5.2 es un requisito previo de instalación.

## <a name="ssma-v83"></a>SSMA v 8.3

El lanzamiento de la versión 8.3 de SSMA para SAP ASE se ha mejorado con correcciones de destino diseñadas para mejorar la calidad y las métricas de conversión. Además, esta versión de SSMA para SAP ASE proporciona correcciones que:

* Solucionar problemas de accesibilidad
* Agregar compatibilidad básica para el `hierarchyid` tipo en SQL Server

## <a name="ssma-v82"></a>SSMA v 8.2

La versión v 8.2 de SSMA para SAP ASE se ha mejorado con un conjunto de correcciones diseñado para mejorar la calidad y las métricas de conversión, así como las correcciones para:

* Un problema con los índices no clúster deshabilitados después de la migración de datos.
* Detección de .NET Framework durante la instalación silenciosa.
* Un bloqueo intermitente que se produce cuando se descarga una nueva versión.

> [!NOTE]
> Un problema conocido con la actualización automática puede producir un error en una actualización de SSMA v 8.1 a v 8.2. Si se produce este error, descargue la nueva versión e instálela manualmente.

## <a name="ssma-v81"></a>SSMA v 8.1

La versión v 8.1 de SSMA para SAP ASE se ha mejorado con correcciones de destino diseñadas para mejorar la calidad y las métricas de conversión.

> [!NOTE]
> Un problema conocido con la actualización automática puede producir un error en una actualización de SSMA v 8.0 a v 8.1. Si se produce este error, descargue la nueva versión e instálela manualmente.

## <a name="ssma-v80"></a>SSMA v 8.0

La versión 8.0 de SSMA para SAP ASE se ha mejorado con las correcciones de destino diseñadas para mejorar la calidad y las métricas de conversión. Además, esta versión ofrece las siguientes características nuevas:

* Compatibilidad con **Azure SQL instancia administrada** como destino. Ahora puede crear nuevos proyectos que tengan como destino Azure SQL Instancia administrada:

  ![Proyecto de SQL Database MI](../media/ssma-newproject-sqldbmi.png)

* **Asesor de corrección** posterior a la conversión. Obtenga más información [aquí](https://blogs.msdn.microsoft.com/datamigration/2019/02/17/%20accelerate-your-oracle-migrations-with-new-machine-learning-capabilities-in-ssma/).

* Selección preliminar de base de datos/esquema.

  Al conectarse al origen, el usuario puede seleccionar las bases de datos o los esquemas de interés. Si selecciona solo los esquemas que va a migrar, se ahorrará tiempo durante la conexión inicial y se mejorará el rendimiento global de SSMA.

  ![SSMA (objetos de filtro)](../media/ssma-filter-objects.png)

## <a name="ssma-v710"></a>SSMA v 7.10

La versión v 7.10 de SSMA para SAP ASE se ha mejorado con las correcciones de destino diseñadas para proporcionar protección adicional de seguridad y privacidad para satisfacer los cambios en los requisitos globales.

## <a name="ssma-v79"></a>SSMA v 7.9

La versión v 7.9 de SSMA para SAP ASE contiene los siguientes cambios:

* Correcciones dirigidas que mejoran las métricas de calidad y conversión.
* Compatibilidad en la línea de comandos de SSMA para modificar la asignación de tipos de datos y las preferencias del proyecto.
* Compatibilidad para la migración de datos con SQL Server Integration Services (SSIS). Después de convertir el esquema, es posible crear un paquete SSIS mediante el uso de una opción del menú contextual.
* También se ha modificado el cuadro de diálogo de conexión Azure SQL Database en SSMA para especificar el nombre completo del servidor. En versiones anteriores de SSMA, el prefijo Azure SQL Database tenía que mencionarse explícitamente dentro de la configuración de los proyectos.

## <a name="ssma-v78"></a>SSMA v 7.8

La versión v 7.8 de SSMA para SAP ASE contiene los siguientes cambios:

* Cambiar la asignación de tipos resaltada en la **configuración del proyecto**.
* La capacidad de los usuarios de deshabilitar la telemetría.

## <a name="ssma-v77"></a>SSMA v 7.7

La versión v 7.7 de SSMA para SAP ASE contiene los siguientes cambios:

* SSMA for SAP ASE se ha mejorado con correcciones de destino que mejoran la calidad y las métricas de conversión.
* En función de la demanda popular, la versión de 32 bits de SSMA para SAP ASE vuelve a ser. En comparación con la implementación anterior (antes de v 7.4), hay dos paquetes de instalador, pero no se pueden instalar en paralelo. Como resultado, debe elegir la versión más adecuada en función de los componentes de conectividad que tenga. Siempre es preferible usar la versión de 64 bits, si es posible.

## <a name="ssma-v76"></a>SSMA v 7.6

La versión v 7.6 de SSMA for SAP ASE contiene los siguientes cambios:

* Correcciones de destino que mejoran las métricas de calidad y conversión y admiten SQL Server 2017 (versión preliminar pública). La compatibilidad con SQL Server 2017 en Windows y Linux está en versión preliminar pública y no debe usarse para migraciones de producción.
* Compatibilidad con la conversión de funciones de Sybase.

## <a name="ssma-v75"></a>SSMA v 7.5

La versión v 7.5 de SSMA para SAP ASE (anteriormente SSMA para Sybase) contiene los cambios siguientes:

* Varias mejoras para garantizar una mayor accesibilidad para las personas con discapacidades.
* Compatibilidad con la `CREATE OR REPLACE` Sintaxis de.

## <a name="ssma-v74"></a>SSMA v 7.4

La versión v 7.4 de SSMA para Sybase contiene los siguientes cambios:

* La opción **tiempo de espera de consulta** ahora está disponible durante la detección de objetos de esquema en el origen y el destino.

  ![opción de tiempo de espera de consulta](../media/query-timeout_red.png)
* La métrica de calidad y conversión se ha mejorado con las correcciones de destino, en función de los comentarios de los clientes.

  > [!IMPORTANT]
  > .NET 4.5.2 es un requisito previo para instalar SSMA v 7.4. Además, a partir de v 7.4, se suspende la versión de 32 bits de SSMA.

## <a name="ssma-v73"></a>SSMA v 7.3

La versión v 7.3 de SSMA para Sybase contiene los siguientes cambios:

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

La versión v 7.2 de SSMA para Sybase contiene los siguientes cambios:

* Métrica de calidad y conversión mejorada con correcciones de destino basadas en los comentarios de los clientes.
* Mejoras en la telemetría para proporcionar mejores puntos de datos para solucionar problemas de los clientes y mejorar las tasas de conversión de SSMA.

## <a name="ssma-v71"></a>SSMA v 7.1

La versión v 7.1 de SSMA para Sybase contiene los siguientes cambios:

* SQL Server 2017 en Windows y Linux CTP1 es ahora una plataforma de destino admitida para la migración. Esta característica se encuentra en Technical Preview y admite el movimiento de datos y esquemas para los servidores SQL Server de destino.
* Compatibilidad con las actualizaciones automáticas para descargar la versión más reciente de SSMA en cuanto esté disponible.
* Los binarios instalables de SSMA se entregan ahora a través de Windows Installer archivos de paquete (. msi).

## <a name="may-2016"></a>Mayo de 2016

La versión de mayo de 2016 de SSMA para Sybase contiene los siguientes cambios:

* Compatibilidad agregada para SQL Server 2016.
* Se quitó la comprobación del instalador para .NET 2,0.
* Dependencia del paquete de extensiones actualizada de .NET 3,5 a .NET 4,0.
* Fixed `save-project` y `open-project` comandos para la consola de SSMA.
* `securepassword`Comando fijo para la consola de SSMA.
* Recuento fijo de objetos para la carga inicial.
* Se corrigió el error en la configuración global.

## <a name="march-2016"></a>Marzo de 2016

La versión de vista previa de marzo de 2016 de SSMA para Sybase agrega compatibilidad para la migración a SQL Server 2016.

## <a name="january-2016"></a>Enero de 2016

La versión de mantenimiento de enero de 2016 de SSMA para Sybase contiene los siguientes cambios:

* Se ha agregado el elemento de menú Ver registro a SSMA (RFC 5706203).
* Telemetría agregada.

## <a name="july-2014"></a>Julio de 2014

La versión 2014 de julio de SSMA para Sybase contiene los siguientes cambios:

* Conversión de código Azure SQL Database mejorada.
* Se ha cambiado la funcionalidad del paquete de extensiones al esquema para admitir Azure SQL Database.
* Se han realizado mejoras en el rendimiento de las bases de datos con más de 10 000 objetos.
* Se han agregado mejoras de la interfaz de usuario para tratar con un gran número de objetos.
* Se ha agregado la capacidad de resaltar esquemas de LOB conocidos (por lo que se pueden omitir en la conversión).
* Mejoras en la velocidad de conversión agregada.
* Se ha agregado la capacidad de mostrar recuentos de objetos en la interfaz de usuario.
* Menor tamaño de informe en más del 25%.
* Mensajes de error mejorados para construcciones sin analizar.

## <a name="april-2014"></a>Abril de 2014

La versión de abril de 2014 de SSMA para Sybase contiene los siguientes cambios:

* Compatibilidad agregada de MS SQL Server 2014.
* Se corrigieron los errores relacionados con la conversión a Azure.
* Se han corregido errores relacionados con las páginas del informe invisibles en IE 10.

## <a name="january-2012"></a>enero de 2012

La versión 2012 de enero de SSMA para Sybase contiene los siguientes cambios:

* Compatibilidad agregada para la conversión del desencadenador de reversión.
* Corrección proporcionada para `@@ROWCOUNT` la conversión y `@@ERROR` en la misma `SET` instrucción.

## <a name="july-2011"></a>julio de 2011

La versión 2011 de julio de SSMA para Sybase proporciona informes de errores mejorados durante la migración de datos.

## <a name="april-2011"></a>2011 de abril

La versión de abril de 2011 de SSMA para Sybase contiene los siguientes cambios:

* Producto "SSMA para Sybase" consolidado que admite [!INCLUDE [ssVersion2005](../../includes/ssversion2005-md.md)] , [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)] [!INCLUDE [ssSQL11](../../includes/ssSQL11-md.md)] y Azure SQL.
* Se ha agregado compatibilidad para conectar y migrar a [!INCLUDE [ssSQL11](../../includes/sssql11-md.md)] .
* Se ha agregado una nueva característica para convertir y migrar bases de datos de Sybase a Azure SQL.
* Motor de migración de datos del lado cliente mejorado, que admite la migración en paralelo de datos.
* Rendimiento mejorado de la migración de datos con modelos de recuperación simples y de registro masivo.
* Se ha agregado la capacidad de convertir y migrar bases de datos de Sybase que distinguen mayúsculas de minúsculas a SQL Server que distinguen mayúsculas de minúsculas.
* Se ha agregado compatibilidad para la conversión de instrucciones de combinación no ANSI ASE de Sybase en SQL Server instrucciones de combinación ANSI se ha ampliado para eliminar y actualizar instrucciones.
* Se proporcionan opciones de conectividad adicionales para conectarse a servidores de Sybase ASE mediante el proveedor ODBC de Sybase ASE y proveedores ADO.NET de Sybase ASE.
* Se ha quitado la dependencia de una base de datos independiente denominada `SysDB` , que contiene las funciones de emulación de Sybase (instaladas como parte del paquete de extensiones).
* Se ha agregado la posibilidad de instalar SSMA para Sybase Extension Pack en los [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] clústeres.
* Compatibilidad con versiones anteriores de los proyectos creados con versiones anteriores de SSMA (v 4.0 y v 4.2).
* Se ha agregado la capacidad de instalar el producto SSMA para Sybase v 5.0 (SxS) con versiones anteriores de SSMA (v 4.0 y v 4.2).

## <a name="july-2010"></a>Julio de 2010

Se ha agregado la versión 2010 de julio de SSMA para Sybase:

* Compatibilidad para migrar a SQL Server 2008 R2.
* Una nueva aplicación de consola SSMA para la ejecución de la línea de comandos.
* Compatibilidad con la migración de datos mediante motores de migración de datos Server-Side y Client-Side.
* Compatibilidad con la instrucción SELECT personalizada en la migración de datos.
* Compatibilidad con la migración desde Sybase ASE 15.0.3 y 15,5.

## <a name="june-2008"></a>2008 de junio

La versión 2008 de junio de SSMA para Sybase contiene los siguientes cambios:

* Se ha agregado el comprobador de SSMA, que prueba automáticamente la conversión de objetos de base de datos y la migración de datos realizada por SSMA. Una vez finalizados todos los pasos de migración de SSMA, utilice SSMA Tester para comprobar que los objetos convertidos funcionan de la misma manera y que todos los datos se transfirieron correctamente.
* Conversión anterior a SQL agregada. Ahora, el usuario puede especificar declaraciones de tabla temporal (y de otro objeto) para cada procedimiento de origen que se va a utilizar en la conversión.
* Mejoras agregadas en la conversión de objetos:
  * Se ha revisado la conversión.
  * Agregados y no agregados sin cláusulas/Group by.
  * `IDENTITY`Función con una `SELECT INTO` instrucción.
  * Restricciones y índices en clúster en solo datos bloqueados.
  * Tablas temporales creadas por `SELECT INTO` .
  * Restricciones o índices para las tablas temporales.
  * [!INCLUDE [ssSQL10](../../includes/sssql10-md.md)]Se admiten nuevos tipos de fecha y hora.
  * Compatibilidad con los tipos de datos y conectividad de Sybase 15,0.

## <a name="may-2007"></a>2007 de mayo

Puede Agregar la versión 2007 de SSMA para Sybase:

* La capacidad de cargar el contenido de la base de datos más rápido al guardar un proyecto.
* Compatibilidad con comentarios introducidos por el usuario en el modo SQL con formato SQL Server.
* Mejoras en la conversión de objetos.

## <a name="november-2006"></a>2006 de noviembre

La versión de noviembre de 2006 de SSMA para Sybase contiene los siguientes cambios:

* Nueva configuración global agregada:
  * Puede optar por mostrar los números de línea en las ventanas del editor.
  * Puede configurar SSMA para que solicite reemplazar objetos duplicados, o siempre o nunca reemplazar objetos duplicados durante la conversión del esquema.
* Se ha agregado una nueva opción de conversión que le permite configurar cómo SSMA controla las situaciones siguientes:
  * Una `CAST` `CONVERT` instrucción o que contiene una cadena binaria.
  * Comprueba si hay valores NULL en expresiones de igualdad.
  * Tablas de proxy.
  * Números de error de mensaje de usuario para `RAISERROR` .
  * `UPDATE` instrucciones que contienen identificadores sin resolver.
* Se ha agregado una nueva opción de migración que le permite especificar cómo SSMA debe controlar las fechas que están fuera del [!INCLUDE [ssNoVersion](../../includes/ssnoversion-md.md)] intervalo de fechas.
* Se ha agregado una configuración de **SQL con formato** en la pestaña **SQL** , que da formato al código para mejorar la legibilidad.
* Correcciones de errores, incluidos:
  * SSMA ahora convierte las `LOCK TABLE <table> IN { SHARED | EXCLUSIVE } MODE` instrucciones agregando una `TABLOCK` `TABLOCKX` sugerencia or a la `SELECT` consulta subsiguiente en la tabla.
  * Ahora se agregan las conversiones necesarias cuando se usan tipos binarios en expresiones de caracteres.
  * Mejoras en la memoria y el rendimiento.

## <a name="july-2006"></a>2006 de julio

La versión de julio de 2006 de SSMA para Sybase fue la versión inicial.

## <a name="see-also"></a>Consulte también

[Introducción con SSMA para Sybase &#40;SybaseToSQL&#41;](../../ssma/sybase/getting-started-with-ssma-for-sybase-sybasetosql.md)

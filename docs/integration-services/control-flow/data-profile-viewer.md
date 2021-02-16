---
description: Visor de perfil de datos
title: Visor de perfil de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dataprofileviewer.f1
helpviewer_keywords:
- Data Profile Viewer [Integration Services]
- Data Profiling task [Integration Services], output viewer
ms.assetid: b9043428-ce26-45bb-910c-588d07579565
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aaa5d2276908feccd86aac75012703e093b861d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352129"
---
# <a name="data-profile-viewer"></a>Visor de perfil de datos

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El paso siguiente del proceso de generación de perfiles de datos consiste en ver y analizar los perfiles de datos. Estos perfiles pueden verse después de ejecutar la tarea de generación de perfiles de datos dentro de un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y de calcular los perfiles de datos. Para más información sobre cómo configurar y ejecutar las tareas de generación de perfiles de datos, vea [Configuración de la Tarea de generación de perfiles de datos](../../integration-services/control-flow/setup-of-the-data-profiling-task.md).  
  
> [!IMPORTANT]  
>  El archivo de salida podría contener datos confidenciales acerca de la base de datos y de los datos que contiene. Para conocer sugerencias sobre cómo mejorar la seguridad del archivo, vea [Acceso a los archivos usados por los paquetes](../../integration-services/security/security-overview-integration-services.md#files).  
  
## <a name="data-profiles"></a>Perfiles de datos  
 Para ver los perfiles de datos, se configura la tarea de generación de perfiles de datos de modo que envíe su salida a un archivo y, a continuación, se utiliza el Visor de perfil de datos independiente. Para abrir el Visor de perfil de datos, realice una de las acciones siguientes.  
  
-   Haga clic con el botón derecho en la tarea **Generación de perfiles de datos** en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] y, después, haga clic en **Editar**. Haga clic en **Abrir el visor de perfil** en la página **General** del **Editor de tareas de generación de perfiles de datos**.  
  
-   En la carpeta *\<drive>* :\Archivos de programa (x86) | Archivos de programa\Microsoft SQL Server\110\DTS\Binn, ejecute DataProfileViewer.exe.  
  
 El visor utiliza varios paneles para mostrar los perfiles solicitados y los resultados calculados, y también permite la obtención de detalles:  
  
 Panel **Perfiles**  
 En el panel **Perfiles** se muestran los perfiles que se han pedido en la tarea de generación de perfiles de datos. Para ver los resultados calculados para el perfil, selecciónelo en el panel **Perfiles** y los resultados aparecerán en los otros paneles del visor.  
  
 Panel **Resultados**  
 El panel **Resultados** usa una única fila para resumir los resultados calculados del perfil. Por ejemplo, si se solicita un **Perfil de distribución de longitud de columnas**, esta fila incluye la longitud mínima, la máxima y el recuento de filas. Para la mayoría de los perfiles, es posible seleccionar esta fila en el panel **Resultados** con objeto de ver los detalles adicionales en el panel **Detalles** opcional.  
  
 Panel **Detalles**  
 Para la mayoría de los tipos de perfiles, en el panel **Detalles** se muestra información adicional sobre los resultados del perfil seleccionados en el panel **Resultados** . Por ejemplo, si solicita un **Perfil de distribución de longitud de columnas**, el panel **Detalles** muestra cada una de las longitudes de columna que se han hallado. El panel también muestra el número y el porcentaje de filas cuyo valor de columna tiene esa longitud de columna.  
  
 Para los tres tipos de perfil que se calculan sobre varias columnas (Clave candidata, Dependencia funcional e Inclusión de valores), en el panel **Detalles** se muestran las infracciones de la relación esperada. Por ejemplo, si se solicita un Perfil de claves candidatas, el panel Detalles muestra los valores duplicados que infringen la unicidad de la clave candidata.  
  
 Si el origen de datos que se usa para calcular el perfil está disponible, puede hacer doble clic en una fila en el panel **Detalles** para ver las filas de datos coincidentes en el panel **Obtención de detalles** .  
  
 Panel **Obtención de detalles**  
 Puede hacer doble clic en una fila del panel **Detalles** para ver las filas coincidentes de datos en el panel **Obtención de detalles** cuando se cumplen las condiciones siguientes:  
  
-   El origen de datos que se utiliza para calcular el perfil está disponible.  
  
-   Tiene permiso para ver los datos.  
  
 Para conectarse a la base de datos de origen y realizar una solicitud de obtención de detalles, el Visor de perfil de datos usa la autenticación de Windows y las credenciales del usuario actual. El Visor de perfil de datos no utiliza la información de conexión que está almacenada en el paquete que ejecutó la tarea de generación de perfiles de datos.  
  
> [!IMPORTANT]  
>  La capacidad de detalle, que está disponible en el Visor de perfil de datos, envía las consultas actuales al origen de datos original. Estas consultas pueden tener un efecto desfavorable en el rendimiento del servidor.  
>   
>  Si obtiene los detalles de un archivo de salida que no se ha creado recientemente, las consultas de detalle podrían devolver un conjunto diferente de filas al que se usó para calcular la salida original.  
  
 Para obtener más información sobre la interfaz de usuario del Visor de perfil de datos, vea [Data Profile Viewer F1 Help]().  
  
## <a name="data-profile-viewer-f1-help"></a>Visor de perfiles de datos (Ayuda F1)
  Utilice el Visor de perfil de datos para ver la salida de la Tarea de generación de perfiles de datos.  
  
 Para más información sobre cómo usar el Visor de perfil de datos, vea [Visor de perfil de datos](../../integration-services/control-flow/data-profile-viewer.md). Para más información sobre cómo usar la Tarea de generación de perfiles de datos, que crea la salida del perfil que se analiza en el Visor de perfil de datos, vea [Configuración de la Tarea de generación de perfiles de datos](../../integration-services/control-flow/setup-of-the-data-profiling-task.md).  
  
### <a name="static-options"></a>Opciones estáticas  
 **Abrir**  
 Haga clic para buscar el archivo guardado que contiene la salida de la Tarea de generación de perfiles de datos.  
  
 Panel **Perfiles**  
 Expanda el árbol del panel **Perfiles** para ver los perfiles que están incluidos en la salida. Seleccione un perfil para ver los resultados de ese perfil.  
  
 Panel **Mensaje**  
 Muestra mensajes de estado.  
  
 Panel **Obtención de detalles**  
 Muestra las filas de datos que coinciden con un valor en la salida, si el origen de datos que usa la Tarea de generación de perfiles de datos está disponible.  
  
 Por ejemplo, si está viendo la salida de un perfil Distribución de valores de columna de una columna de estados americanos, el panel **Distribución de valor detallado** podría contener una fila para "WA". Haga doble clic en la fila en el panel **Distribución de valores detallados** para ver las filas de datos en las que el valor de la columna de estado sea "WA" en el panel de obtención de detalles.  
  
### <a name="dynamic-options"></a>Opciones dinámicas  
  
#### <a name="profile-type--column-length-distribution-profile"></a>Tipo de perfil = Perfil de distribución de longitud de columnas  
  
##### <a name="column-length-distribution-profile---column-pane"></a>Panel Perfil de distribución de longitud de columnas- \<column>  
 **Longitud mínima**  
 Muestra la longitud mínima de los valores de esta columna.  
  
 **Longitud máxima**  
 Muestra la longitud máxima de los valores de esta columna.  
  
 **Omitir espacios iniciales**  
 Muestra si este perfil se calculó con un valor de **IgnoreLeadingSpaces** True o False. Esta propiedad se estableció en la página **Solicitudes de perfil** del Editor de tareas de generación de perfiles de datos.  
  
 **Omitir espacios finales**  
 Muestra si este perfil se calculó con un valor de **IgnoreTrailingSpaces** True o False. Esta propiedad se estableció en la página **Solicitudes de perfil** del Editor de tareas de generación de perfiles de datos.  
  
 **Recuento de filas**  
 Muestra el número de filas de la tabla o vista.  
  
##### <a name="detailed-length-distribution-pane"></a>Panel Distribución de longitud detallado  
 **Longitud**  
 Muestra las longitudes de columna encontradas en la columna de perfiles.  
  
 **Recuento**  
 Muestra el número de filas en las que el valor de la columna de perfiles tiene la longitud que se muestra en la columna **Longitud** .  
  
 **Porcentaje**  
 Muestra el porcentaje de filas en las que el valor de la columna de perfiles tiene la longitud que se muestra en la columna **Longitud** .  
  
#### <a name="profile-type--column-null-ratio-profile"></a>Tipo de perfil = Perfil de proporción de columnas nulas  
  
##### <a name="column-null-ratio-profile---column-pane"></a>Panel Perfil de proporción de columnas nulas - \<column>  
 **Recuento nulo**  
 Muestra el número de filas en las que la columna de perfiles tiene el valor null.  
  
 **Porcentaje nulo**  
 Muestra el porcentaje de filas en las que la columna de perfiles tiene el valor null.  
  
 **Recuento de filas**  
 Muestra el número de filas de la tabla o vista.  
  
#### <a name="profile-type--column-pattern-profile"></a>Tipo de perfil = Perfil de patrón de columnas  
  
##### <a name="column-pattern-profile---column-pane"></a>Panel Perfil de patrón de columnas - \<column>  
 **Recuento de filas**  
 Muestra el número de filas de la tabla o vista.  
  
##### <a name="pattern-distribution-pane"></a>Panel Distribución del patrón  
 **Patrón**  
 Muestra los patrones calculados para la columna de perfiles.  
  
 **Porcentaje**  
 Muestra el porcentaje de filas cuyos valores coinciden con el patrón que se muestra en la columna **Patrón** .  
  
#### <a name="profile-type--column-statistics-profile"></a>Tipo de perfil = Perfil de estadísticas de columnas  
  
##### <a name="column-statistics-profile---column-pane"></a>Panel Perfil de estadísticas de columnas - \<column>  
 **Mínimo**  
 Muestra el valor mínimo situado en la columna de perfiles.  
  
 **Máximo**  
 Muestra el valor máximo situado en la columna de perfiles.  
  
 **Promedio**  
 Muestra el promedio de los valores que se encuentran en la columna de perfiles.  
  
 **Desviación estándar**  
 Muestra la desviación estándar de los valores que se encuentran en la columna de perfiles.  
  
#### <a name="profile-type--column-value-distribution-profile"></a>Tipo de perfil = Perfil de distribución de valores de columna  
  
##### <a name="column-value-distribution-profile---column-pane"></a>Panel Perfil de distribución de valores de columna - \<column>  
 **Número de valores distintos**  
 Muestra el recuento de valores distintos que se encuentran en la columna de perfiles.  
  
 **Recuento de filas**  
 Muestra el número de filas de la tabla o vista.  
  
##### <a name="detailed-value-distribution-pane"></a>Panel Distribución de valores detallado  
 **Valor**  
 Muestra los distintos valores que se encuentran en la columna de perfiles.  
  
 **Recuento**  
 Muestra el número de filas en las que la columna de perfiles tiene el valor que se muestra en la columna **Valor** .  
  
 **Porcentaje**  
 Muestra el porcentaje de filas en las que la columna de perfiles tiene el valor que se muestra en la columna **Valor** .  
  
#### <a name="profile-type--candidate-key-profile"></a>Tipo de perfil = Perfil de claves candidatas  
  
##### <a name="candidate-key-profile---table-pane"></a>Panel Perfil de claves candidatas - \<table>  
 **Columnas de clave**  
 Muestra las columnas que se seleccionaron para los perfiles como clave candidata.  
  
 **Nivel de clave**  
 Muestra el nivel (como porcentaje) de la columna de clave candidata o de una combinación de columnas. Un nivel de clave menor del 100% indica que hay valores duplicados.  
  
##### <a name="key-violations-pane"></a>Panel Infracciones de clave  
 **\<column1>, \<column2>, etc.**  
 Muestra los valores duplicados que se encontraron en la columna de perfiles.  
  
 **Recuento**  
 Muestra el número de filas en las que la columna especificada tiene el valor que se muestra en la primera columna.  
  
#### <a name="profile-type--functional-dependency-profile"></a>Tipo de perfil = Perfil de dependencia funcional  
  
##### <a name="functional-dependency-profile-pane"></a>Panel Perfil de dependencia funcional  
 **Columnas determinantes**  
 Muestra la columna o columnas seleccionadas como columna determinante. En el ejemplo en el que el mismo código postal de Estados Unidos siempre debería tener el mismo estado, el código postal es la columna determinante.  
  
 **Columnas dependientes**  
 Muestra la columna o columnas seleccionadas como columna dependiente. En el ejemplo en el que el mismo código postal de Estados Unidos siempre debería tener el mismo estado, el estado es la columna dependiente.  
  
 **Nivel de dependencia funcional**  
 Muestra el nivel (como porcentaje) de la dependencia funcional entre las columnas. Un nivel de clave menor del 100% indica que hay casos en los que el valor determinante no determina el valor dependiente. En el ejemplo en el que el mismo código postal de Estados Unidos siempre debería tener el mismo estado, esto probablemente indica que algunos valores de estado no son válidos.  
  
##### <a name="functional-dependency-violations-pane"></a>Panel Infracciones de dependencia funcional  
  
> [!NOTE]  
>  Un porcentaje alto de valores erróneos en los datos podría provocar resultados inesperados en un perfil Dependencia funcional. Por ejemplo, el 90% de las filas tienen el valor "WI" como estado para el valor "98052" de código postal. El perfil notifica filas que contienen valor de estado correcto de "WA" como infracciones.  
  
 **\<determinant column name>**  
 Muestra el valor de la columna determinante o la combinación de columnas en esta infracción de la dependencia funcional.  
  
 **\<dependent column name>**  
 Muestra el valor de la columna dependiente en esta infracción de la dependencia funcional.  
  
 **Recuento de soporte**  
 Muestra el número de filas en las que el valor de columna determinante determina la columna dependiente.  
  
 **Recuento de infracciones**  
 Muestra el número de filas en las que el valor de columna determinante no determina la columna dependiente. (Estas son las filas en las que el valor dependiente es el que se muestra en la columna **\<dependent column name>** ).  
  
 **Porcentaje admitido**  
 Muestra el porcentaje de filas en las que el valor de columna determinante determina la columna dependiente.  
  
#### <a name="profile-type--value-inclusion-profile"></a>Tipo de perfil = Perfil de inclusión de valores  
  
##### <a name="value-inclusion-profile-pane"></a>Panel Perfil de inclusión de valores  
 **Columnas de subconjunto**  
 Muestra la columna o combinación de columnas que se incluyeron en el perfil para determinar si están en las columnas de superconjunto.  
  
 **Columnas de superconjunto**  
 Muestra la columna o combinación de columnas que se incluyeron en el perfil para determinar si incluyen los valores en las columnas de subconjunto.  
  
 **Nivel de inclusión**  
 Muestra el nivel (como porcentaje) de la superposición entre las columnas. Un nivel de clave menor que 100% indica que hay casos en los que el valor de subconjunto no se encuentra entre los valores del superconjunto.  
  
##### <a name="inclusion-violations-pane"></a>Panel Infracciones de inclusión  
 **\<column1>, \<column2>, etc.**  
 Muestra los valores en la columna o columnas del subconjunto que no se encontraban en la columna o columnas del superconjunto.  
  
 **Recuento**  
 Muestra el número de filas en las que la columna especificada tiene el valor que se muestra en la primera columna.  

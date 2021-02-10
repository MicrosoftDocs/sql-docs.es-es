---
description: Conversión de esquemas DB2 (DB2ToSQL)
title: Conversión de esquemas DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 7947efc3-ca86-4ec5-87ce-7603059c75a0
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: af91274d4e48ce99894316c730a58d68fd633242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062910"
---
# <a name="converting-db2-schemas-db2tosql"></a>Conversión de esquemas DB2 (DB2ToSQL)
Después de conectarse a DB2, conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y establecer las opciones de asignación de datos y de proyecto, puede convertir los objetos de base de datos DB2 en objetos de base de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="the-conversion-process"></a>Proceso de conversión  
La conversión de objetos de base de datos toma las definiciones de objetos de DB2, las convierte en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos similares y, a continuación, carga esta información en los metadatos de SSMA. No carga la información en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . A continuación, puede ver los objetos y sus propiedades mediante el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos.  
  
Durante la conversión, SSMA imprime los mensajes de salida en el panel de salida y los mensajes de error en el panel de Lista de errores. Utilice la información de salida y de error para determinar si tiene que modificar las bases de datos DB2 o el proceso de conversión para obtener los resultados de la conversión deseada.  
  
## <a name="setting-conversion-options"></a>Establecer opciones de conversión  
Antes de convertir objetos, revise las opciones de conversión de proyectos en el cuadro de diálogo **configuración del proyecto** . Mediante este cuadro de diálogo, puede establecer cómo SSMA convierte las funciones y las variables globales. Para obtener más información, vea [configuración del proyecto &#40;conversión&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md).  
  
## <a name="conversion-results"></a>Resultados de la conversión  
En la tabla siguiente se muestra qué objetos DB2 se convierten y los objetos resultantes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Objetos DB2|Objetos SQL Server resultantes|  
|-----------|----------------------------|  
|Tipo de datos|**SSMA asigna todos los tipos, excepto los siguientes:**<br /><br />CLOB: no se admiten algunas funciones nativas para trabajar con este tipo (por ejemplo, CLOB_EMPTY ())<br /><br />BLOB: no se admiten algunas funciones nativas para trabajar con este tipo (por ejemplo, BLOB_EMPTY ())<br /><br />DBLOB: no se admiten algunas funciones nativas para trabajar con este tipo (por ejemplo, DBLOB_EMPTY ())|  
|Tipos definidos por el usuario|**SSMA asigna el siguiente definido por el usuario:**<br /><br />Tipo DISTINCT<br /><br />Tipo estructurado<br /><br />Tipos de datos PL de SQL: Nota: no se admite el tipo de cursor débil.|  
|Registros especiales|**SSMA solo asigna los registros que se enumeran a continuación:**<br /><br />MARCA DE TIEMPO ACTUAL<br /><br />FECHA ACTUAL<br /><br />HORA ACTUAL<br /><br />ZONA HORARIA ACTUAL<br /><br />USUARIO ACTUAL<br /><br />SESSION_USER y usuario<br /><br />SYSTEM_USER<br /><br />CLIENT_APPLNAME ACTUAL<br /><br />CLIENT_WRKSTNNAME ACTUAL<br /><br />TIEMPO DE ESPERA DE BLOQUEO ACTUAL<br /><br />ESQUEMA ACTUAL<br /><br />SERVIDOR ACTUAL<br /><br />AISLAMIENTO ACTUAL<br /><br />Otros registros especiales no se asignan a la semántica de SQL Server.|  
|CREATE TABLE|**SSMA asigna CREATE TABLE con las siguientes excepciones:**<br /><br />Tablas de agrupación en clústeres multidimensionales (MDC)<br /><br />Rango: tablas agrupadas (RCT)<br /><br />Tablas con particiones<br /><br />Tabla separada<br /><br />Cláusula de captura de datos<br /><br />Opción oculta IMPLÍCITAmente<br /><br />VOLATILe, opción|  
|CREATE VIEW|SSMA Maps CREATE VIEW with ' WITH LOCAL CHECK OPTION ' pero otras opciones no se asignan a la semántica de SQL Server|  
|CREATE INDEX|**SSMA asigna CREATE INDEX con las siguientes excepciones:**<br /><br />Índice XML<br /><br />BUSINESS_TIME sin superposiciones, opción<br /><br />Cláusula con PARTIciones<br /><br />Opción solo especificación<br /><br />EXTENDER mediante la opción<br /><br />Opción MINPCTUSED<br /><br />Opción de división de página|  
|Desencadenadores|**SSMA asigna la siguiente semántica de desencadenador:**<br /><br />Desencadenadores AFTER/FOR cada fila<br /><br />AFTER/FOR cada desencadenador de instrucción<br /><br />ANTES/para cada fila y en lugar de/para los desencadenadores de cada fila|  
|Secuencias|Están asignadas.|  
|Instrucción SELECT|**SSMA Maps SELECT con las siguientes excepciones:**<br /><br />Cláusula Data-Change-Table-Reference-asignación parcial, pero no se admiten las tablas finales<br /><br />Cláusula de referencia de tabla: parcialmente asignado, pero solo-Table-Reference, Outer-Table-Reference, analyze_table-Expression, Collection-derived-Table, XMLTABLE-Expression no se asignan a la semántica de SQL Server<br /><br />Cláusula period-Specification-no asignada.<br /><br />Continue: cláusula handler: no asignada.<br /><br />Cláusula de correlación con tipo: no asignada.<br /><br />Cláusula de resolución de acceso simultánea: no asignada.|  
|VALUEs (instrucción)|Está asignado.|  
|Instrucción INSERT|Está asignado.|  
|Instrucción UPDATE|S **SMA asigna Update con las siguientes excepciones:**<br /><br />Cláusula de referencia de tabla: solo la referencia de tabla no está asignada a la semántica de SQL Server<br /><br />Cláusula Period: no está asignada.|  
|Instrucción MERGE|**SSMA asigna MERGE con las siguientes excepciones:**<br /><br />Una sola vez y varias repeticiones de cada cláusula: está asignada a la semántica de SQL Server para las repeticiones limitadas de cada cláusula<br /><br />Cláusula SIGNAL: no se asigna a SQL Server semántica<br /><br />Cláusulas UPDATE y DELETE mixtas: no se asigna a SQL Server semántica<br /><br />Cláusula period-no se asigna a SQL Server semántica|  
|Instrucción DELETE|**SSMA asigna DELETE con las siguientes excepciones:**<br /><br />Cláusula de referencia de tabla: solo la referencia de tabla no está asignada a la semántica de SQL Server<br /><br />Cláusula Period: no se asigna a SQL Server semántica|  
|Nivel de aislamiento y tipo de bloqueo|Está asignado.|  
|Procedimientos (SQL)|Están asignadas.|  
|Procedimientos (externos)|Requerir actualización manual.|  
|Procedimientos (con origen)|No se asignan a la semántica de SQL Server.|  
|Instrucción de asignación|Está asignado.|  
|Instrucción CALL para un procedimiento|Está asignado.|  
|Instrucción CASE|Está asignado.|  
|FOR (instrucción)|Está asignado.|  
|GOTO, instrucción|Está asignado.|  
|Instrucción IF|Está asignado.|  
|ITERAte (instrucción)|Está asignado.|  
|LEAVE (instrucción)|Está asignado.|  
|LOOP (instrucción)|Está asignado.|  
|REPEAT (instrucción)|Está asignado.|  
|Instrucción RESIGNAL|No se admiten condiciones. Los mensajes pueden ser opcionales.|  
|Instrucción return|Está asignado.|  
|SIGNAL (instrucción)|No se admiten condiciones. Los mensajes pueden ser opcionales.|  
|WHILE (instrucción)|Está asignado.|  
|OBTENER la instrucción de diagnóstico|**Las asignaciones de SSMA obtienen DIAGNÓSTICOs con las siguientes excepciones:**<br /><br />ROW_COUNT: está asignado.<br /><br />DB2_RETURN_STATUS: está asignado.<br /><br />MESSAGE_TEXT: está asignado.<br /><br />DB2_SQL_NESTING_LEVEL: no se asigna a la semántica de SQL Server<br /><br />DB2_TOKEN_STRING: no se asigna a la semántica de SQL Server|  
|Cursores|**SSMA asigna CURSOres con las siguientes excepciones:**<br /><br />Instrucción allocate CURSOR: no se asigna a SQL Server semántica<br /><br />Instrucción associator LOCAtions: no se asigna a SQL Server semántica<br /><br />La cláusula DECLARE CURSOR instrucción-RETURNING no está asignada a la semántica de SQL Server<br /><br />Instrucción FETCH: asignación parcial. Solo se admiten variables como destino. El descriptor de SQLDA no está asignado a la semántica de SQL Server|  
|variables|Están asignadas.|  
|Excepciones, controladores y condiciones|**SSMA asigna "control de excepciones" con las excepciones siguientes:**<br /><br />Los controladores de salida se asignan.<br /><br />Los controladores para deshacer: están asignados.<br /><br />Los controladores de continuación no están asignados.<br /><br />Condiciones: no se asigna a la semántica de SQL Server.|  
|SQL dinámico|No asignado.|  
|Alias|Están asignadas.|  
|Alias|Asignación parcial. Se requiere procesamiento manual para el objeto subyacente|  
|Sinónimos|Están asignadas.|  
|Funciones estándar en DB2|SSMA asigna funciones estándar DB2 cuando una función equivalente está disponible en SQL Server:|  
|Autorización|No asignado.|  
|Predicados|Están asignadas.|  
|SELECT INTO, instrucción|No asignado.|  
|VALUEs INTO (instrucción)|No asignado.|  
|Control de transacciones|No asignado.|  
  
## <a name="converting-db2-database-objects"></a>Conversión de objetos de base de datos DB2  
Para convertir objetos de base de datos DB2, primero debe seleccionar los objetos que desea convertir y, a continuación, hacer que SSMA realice la conversión. Para ver los mensajes de salida durante la conversión, en el menú **Ver** , seleccione **salida**.  
  
**Para convertir objetos DB2 en SQL Server sintaxis**  
  
1.  En el explorador de metadatos DB2, expanda el servidor DB2 y, a continuación, expanda **esquemas**.  
  
2.  Seleccionar los objetos que se van a convertir:  
  
    -   Para convertir todos los esquemas, active la casilla situada junto a **esquemas**.  
  
    -   Para convertir u omitir una base de datos, active la casilla situada junto al nombre del esquema.  
  
    -   Para convertir u omitir una categoría de objetos, expanda un esquema y, a continuación, Active o desactive la casilla situada junto a la categoría.  
  
    -   Para convertir u omitir objetos individuales, expanda la carpeta categoría y, a continuación, Active o desactive la casilla situada junto al objeto.  
  
3.  Para convertir todos los objetos seleccionados, haga clic con el botón derecho en **esquemas** y seleccione **convertir esquema**.  
  
    También puede convertir objetos individuales o categorías de objetos; para ello, haga clic con el botón secundario en el objeto o en su carpeta primaria y, a continuación, seleccione **convertir esquema**.  
  
## <a name="viewing-conversion-problems"></a>Ver problemas de conversión  
Es posible que algunos objetos de DB2 no se conviertan. Puede determinar las tasas de éxito de la conversión viendo el informe de conversión de resumen.  
  
**Para ver un informe de Resumen**  
  
1.  En el explorador de metadatos DB2, seleccione **esquemas**.  
  
2.  En el panel derecho, seleccione la pestaña **Informe** .  
  
    Este informe muestra el informe de evaluación de resumen para todos los objetos de base de datos que se han evaluado o convertido. También puede ver un informe de resumen para objetos individuales:  
  
    -   Para ver el informe de un esquema individual, seleccione el esquema en el explorador de metadatos DB2.  
  
    -   Para ver el informe de un objeto individual, seleccione el objeto en el explorador de metadatos DB2. Los objetos que tienen problemas de conversión tienen un icono de error rojo.  
  
En el caso de los objetos que no se pudieron convertir, puede ver la sintaxis que produjo el error de conversión.  
  
**Para ver los problemas de conversión individuales**  
  
1.  En el explorador de metadatos DB2, expanda **esquemas**.  
  
2.  Expanda el esquema que muestra un icono de error rojo.  
  
3.  En el esquema, expanda una carpeta que tenga un icono de error rojo.  
  
4.  Seleccione el objeto que tiene un icono de error rojo.  
  
5.  En el panel derecho, haga clic en la pestaña **Informe** .  
  
6.  En la parte superior de la pestaña **Informe** está una lista desplegable. Si la lista muestra **estadísticas**, cambie la selección a **origen**.  
  
    SSMA mostrará el código fuente y varios botones justo encima del código.  
  
7.  Haga clic en el botón **siguiente problema** . Se trata de un icono de error rojo con una flecha que apunta a la derecha.  
  
    SSMA resaltará el primer código fuente problemático que encuentre en el objeto actual.  
  
Para cada elemento que no se pueda convertir, debe determinar qué desea hacer con ese objeto:  
  
-   Puede modificar el código fuente de los procedimientos en la pestaña **SQL** .  
  
-   Puede modificar el objeto en la base de datos DB2 para quitar o revisar el código problemático. Para cargar el código actualizado en SSMA, tendrá que actualizar los metadatos. Para obtener más información, vea [conectarse a la base de datos DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md).  
  
-   Puede excluir el objeto de la migración. En el explorador de metadatos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el explorador de metadatos DB2, desactive la casilla situada junto al elemento antes de cargar los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y migrar los datos de DB2.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [cargar los objetos convertidos en SQL Server](./loading-converted-database-objects-into-sql-server-db2tosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migración de datos de DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  

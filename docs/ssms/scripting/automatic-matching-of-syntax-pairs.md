---
title: Coincidencia automática de pares en la sintaxis
description: Obtenga información sobre la coincidencia automática de pares de sintaxis en el Editor de consultas (coincidencia de delimitadores), el Editor de consultas XMLA (coincidencia de llaves) y MDX y DMX (coincidencia de paréntesis).
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- IntelliSense [SQL Server], delimiter highlighting
- IntelliSense [SQL Server], syntax pair matching
ms.assetid: bfc54cda-bfd6-4545-a5b9-f9db2ae13769
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cbc0daf1948c72f66f0c4a5b12d4d21af535ca0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354780"
---
# <a name="automatic-matching-of-syntax-pairs"></a>Coincidencia automática de pares en la sintaxis
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La coincidencia automática de pares en la sintaxis informa de manera inmediata sobre si están escritos correctamente los elementos de sintaxis que deben ir emparejados en el código. Esto se conoce como coincidencia de delimitadores en el Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] , coincidencia de llaves en el Editor de consultas XMLA de Analysis Services y coincidencia de paréntesis en los editores MDX y DMX.  
  
## <a name="database-engine-query-editor-delimiter-matching"></a>Coincidencia de delimitadores del Editor de consultas de Database Engine  
 El Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba si coinciden los delimitadores que identifican los límites de los bloques de código. La comprobación de coincidencia se hace de dos maneras:  
  
-   El editor resalta los dos delimitadores de un par cuando se termina de escribir el segundo delimitador del par.  
  
-   Siempre que el cursor está en uno de los delimitadores de un par, puede utilizar el método abreviado de teclado CTRL+] para saltar al delimitador correspondiente.  
  
### <a name="delimiter-pairs"></a>Pares de delimitadores  
 La coincidencia automática de delimitadores reconoce los conjuntos de delimitadores siguientes:  
  
|Delimitador de apertura|Delimitador de cierre|  
|--------------------|-----------------------|  
|**(**|**)**|  
|**BEGIN**|**END**|  
|**BEGIN TRY**|**END TRY**|  
|**BEGIN CATCH**|**END CATCH**|  
  
 La coincidencia automática de delimitadores no reconoce los delimitadores de los identificadores entre corchetes ([ObjectName]) ni los de los identificadores entre comillas ("ObjectName"). La coincidencia de pares no hace coincidir los delimitadores de comillas simples para los literales de cadena ('cadena') porque la codificación en colores ya proporciona una indicación visual de si se ha delimitado la cadena.  
  
### <a name="delimiter-highlighting"></a>Resaltado de delimitadores  
 La coincidencia resalta el elemento de apertura y el de cierre de un par de delimitadores. Esto permite identificar visualmente los bloques de código y comprobar si hay pares de delimitadores que no coinciden.  
  
 Los delimitadores se resaltan cuando se escribe la última letra que completa el par. Por ejemplo, para un par BEGIN END en el que se escribe primero BEGIN seguido de END, el resaltado se activa cuando se escribe la última letra de END. No es necesario escribir el delimitador de apertura seguido del delimitador de cierre para activar el resaltado. Si escribe primero END y, a continuación, se desplaza hacia arriba en el script y escribe BEGIN, el resaltado se activa en cuanto escriba la última letra de BEGIN. No es necesario que la última letra escrita sea la última letra del delimitador. Por ejemplo, si se equivoca y escribe BEIN en lugar de BEGIN, al insertar la G se resaltará el par BEGIN END.  
  
 El par de delimitadores sigue estando resaltado hasta que mueva el cursor. El resaltado se desactiva cuando se mueve el cursor, incluso si la nueva posición del cursor permanece en el mismo delimitador. Puede volver a activar el resaltado si borra y vuelve a escribir cualquier letra de cualquiera de los miembros del par.  
  
## <a name="analysis-services-xmla-query-editor-brace-matching"></a>Coincidencia de llaves del Editor de consultas XMLA de Analysis Services  
 La coincidencia de llaves del Editor de consultas XMLA de Analysis Services resalta las llaves de apertura y de cierre para indicar si se han cerrado correctamente los elementos. También puede utilizar el método abreviado de teclado CTRL+] para saltar de una llave a la llave correspondiente.  
  
 El Editor de consultas XMLA comprueba si coinciden las llaves para los elementos siguientes:  
  
-   Etiquetas de inicio y de cierre coincidentes.  
  
-   Cualquier par de corchetes angulares "\<" and ">".  
  
-   Inicio y fin de comentarios.  
  
-   Inicio y fin de instrucciones de procesamiento.  
  
-   Inicio y fin de bloques CDATA.  
  
-   Inicio y fin de declaraciones DTD.  
  
-   Comillas de apertura y cierre en atributos.  
  
## <a name="mdx-and-dmx-editor-parenthesis-matching"></a>Coincidencia de paréntesis de los editores MDX y DMX  
 El Editor MDX (Expresiones multidimensionales) y el Editor DMX (Expresiones de minería de datos) comprueban automáticamente la coincidencia de los pares de paréntesis en las funciones.

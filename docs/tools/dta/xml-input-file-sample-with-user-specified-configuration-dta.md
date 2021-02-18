---
title: Ejemplo de archivo de entrada XML con configuración especificada por el usuario
description: En este artículo se incluye un ejemplo de archivo de entrada de XML con configuración especificada por el usuario que se utiliza para optimizar las cargas de trabajo que se van a usar con el Asistente para la optimización de motor de base de datos.
titleSuffix: DTA
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: b29c9716-e5c3-4003-9efb-3ade2197b630
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: fc4e0f7e4816a6aec1cdbe41d2d4c84c8af55711
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339998"
---
# <a name="xml-input-file-sample-with-user-specified-configuration-dta"></a>Ejemplo de archivo de entrada XML con configuración especificada por el usuario (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Copie y pegue este ejemplo de archivo de entrada XML que especifica una configuración especificada por el usuario con el elemento **Configuration** en un editor XML o editor de texto. Esto le permitirá realizar análisis de escenarios condicionales. Los análisis de escenarios condicionales implican el uso del elemento **Configuration** para especificar un conjunto de estructuras de diseño físico hipotéticas para la base de datos que se quiere optimizar. A continuación se utiliza el Asistente para la optimización de motor de base de datos para analizar los efectos de la ejecución de una carga de trabajo en esta configuración hipotética con el objetivo de descubrir si mejora el rendimiento a la hora de procesar las consultas. Este tipo de análisis tiene la ventaja de evaluar la nueva configuración sin incurrir en la sobrecarga que supone implementarla realmente. Si la configuración hipotética no proporciona las mejoras de rendimiento que desea, es fácil modificarla y volver a analizarla hasta conseguir la configuración que produzca los resultados necesarios.  
  
 Una vez copiado el ejemplo en la herramienta de edición, sustituya los valores especificados en los elementos **Server**, **Database**, **Schema**, **Table**, **Workload**, **TuningOptions** y **Configuration** por los de la sesión de optimización concreta. Para obtener más información sobre todos los atributos y elementos secundarios que se pueden usar con estos elementos, vea [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md). En el siguiente ejemplo, se utiliza únicamente un subconjunto de opciones de atributos y elementos secundarios disponibles.  
  
## <a name="code"></a>Código  
  
```  
<?xml version="1.0" encoding="utf-16" ?>  
<DTAXML xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="https://schemas.microsoft.com/sqlserver/2004/07/dta">  
  <DTAInput>  
    <Server>  
      <Name>MyServerName</Name>  
<!-- To tune multiple databases, list them and their associated tables in the following section. -->  
      <Database>  
        <Name>MyDatabaseName</Name>  
        <Schema>  
          <Name>MyDatabaseSchemaName</Name>  
<!-- You can list as many tables as necessary in the following section. -->  
          <Table>  
            <Name>MyTableName1</Name>  
          </Table>  
          <Table>  
            <Name>MyTableName2</Name>  
          </Table>  
        </Schema>  
      </Database>  
    </Server>  
    <Workload>  
<!-- The following element specifies a workload file, which can be a trace file or a Transact-SQL script file. -->  
      <File>c:\PathToYourWorkloadFile</File>  
    </Workload>  
    <TuningOptions>  
      <TuningTimeInMin>180</TuningTimeInMin>  
      <StorageBoundInMB>10000</StorageBoundInMB>  
      <FeatureSet>IDX_IV</FeatureSet>  
      <Partitioning>NONE</Partitioning>  
      <KeepExisting>NONE</KeepExisting>  
      <OnlineIndexOperation>OFF</OnlineIndexOperation>  
    </TuningOptions>  
    <Configuration SpecificationMode="Absolute">  
      <Server>  
        <Name>MyServerName</Name>  
          <Database>  
            <Name>MyDatabaseName</Name>  
            <Schema>  
              <Name>MyDatabaseSchemaName</Name>  
                <Table>  
                  <Name>MyTableName1</Name>  
                  <Recommendation>  
                    <Create>  
                      <Index Clustered="true" Unique="false" Online="false" IndexSizeInMB="873.75">  
                        <Name>MyIndexName</Name>  
                        <Column Type="KeyColumn" SortOrder="Ascending">  
                          <Name>MyColumnName1</Name>  
                        </Column>  
                        <Filegroup>MyFileGroupName1</FileGroup>  
                      </Index>  
                    </Create>  
                  </Recommendation>  
                </Table>  
            </Schema>  
          </Database>  
      </Server>  
    </Configuration>  
  </DTAInput>  
</DTAXML>  
```  
  
## <a name="comments"></a>Comentarios  
  
-   Cuando se especifica el modo **Absolute** para el elemento **Configuration** (`Configuration SpecificationMode="Absolute"`), no se permite quitar las estructuras de diseño físico.  
  
-   No es posible crear y quitar la misma estructura de diseño físico en ningún modo (**Relative** o **Absolute**) del elemento **Configuration** .  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar y utilizar el Asistente para la optimización de motor de base de datos](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [Ver y trabajar con la salida del Asistente para la optimización de motor de base de datos](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)   
 [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  

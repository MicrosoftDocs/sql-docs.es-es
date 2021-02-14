---
title: Extensibilidad para reglas de análisis de código de base de datos
description: Familiarícese con los distintos componentes de las reglas de análisis de código de base de datos y cómo interactúan en SQL Server Data Tools. Obtenga información sobre la creación de reglas personalizadas.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 62f5c980-18d5-43fe-b443-c9e149d01fc7
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 24b0224e21f6acec7cc346d35935b0fb0ea692dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080826"
---
# <a name="overview-of-extensibility-for-database-code-analysis-rules"></a>Información general sobre extensibilidad para reglas de análisis de código de base de datos

Las ediciones de Visual Studio que contienen SQL Server Data Tools incluyen reglas de análisis de código que proporcionan información acerca del diseño, nomenclatura y advertencias de rendimiento de Transact\-SQL en el código de la base de datos. Para obtener más información, consulte [Análisis del código de bases de datos para mejorar la calidad del código](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  
  
Si las reglas de análisis de código integradas no abarcan un problema de Transact\-SQL determinado que desee incluir, puede crear reglas de análisis de código de base de datos personalizadas. Por ejemplo, es posible que desee crear una regla personalizada que evite el uso de la instrucción WAITFOR DELAY, como se demuestra en [Tutorial para la creación de un ensamblado de reglas de análisis de código estático personalizadas para SQL Server](../ssdt/walkthrough-author-custom-static-code-analysis-rule-assembly.md). Para crear reglas de análisis de código de base de datos personalizadas, utilice las clases del espacio de nombres [CodeAnalysis](/dotnet/api/microsoft.sqlserver.dac.codeanalysis).  
  
Antes de crear reglas de análisis de código personalizadas, deberá comprender la arquitectura básica de los distintos componentes de las reglas de análisis de código de base de datos.  
  
## <a name="database-code-analysis-rules-components"></a>Componentes de las reglas de análisis de código de base de datos  
En el siguiente diagrama se muestra cómo interactúan los componentes de las reglas de análisis de código de base de datos:  
  
![Componentes de las reglas de análisis de código de base de datos](../ssdt/media/ssdt-database-code-analysis-rules-components.jpg "Componentes de las reglas de análisis de código de base de datos")  
  
Cuando se usa la característica de análisis de código de base de datos, ya sea mediante la ejecución directa de análisis de código estático (para obtener más información, consulte [Cómo: analizar código de Transact-SQL para buscar defectos](/previous-versions/visualstudio/visual-studio-2010/dd172119(v=vs.100))) o mediante la realización de una compilación, todas las reglas se cargan y utilizan según cómo se hayan configurado en el proyecto. Para más información, vea: [Cómo: Habilitar y deshabilitar reglas específicas para el análisis de código estático de base de datos](/previous-versions/visualstudio/visual-studio-2010/dd172131(v=vs.100)). El Administrador de extensiones también cargará los ensamblados de reglas personalizadas que haya creado y registrado. Para más información, vea: [Cómo: Instalar y administrar las extensiones de características](../ssdt/how-to-install-and-manage-feature-extensions.md).  
  
Una clase de reglas de análisis de código personalizadas hereda de [SqlCodeAnalysisRule](/dotnet/api/microsoft.sqlserver.dac.codeanalysis.sqlcodeanalysisrule). La clase de reglas personalizadas puede tener acceso a un número de objetos útiles a través de su contexto de ejecución de regla. Entre ellas se incluyen las siguientes:  
  
-   Metadatos acerca de la propia regla.  
  
-   El Dac.Model.TSqlModel que representa el esquema de la base de datos, incluidos todos los elementos del modelo, las relaciones entre estos y las propiedades de los elementos.  
  
-   Para las reglas que examinan elementos específicos, se incluye en el contexto el Dac.Model.TSqlObject que representa ese elemento del esquema en el modelo.  
  
-   Muchos de los objetos de esquema también tienen una representación [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom) a la que se puede acceder a través de este contexto. Se trata de una representación basada en el AST de un elemento que puede ser útil cuando se intentan detectar posibles problemas de sintaxis, como la presencia de [SelectStarExpression](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.selectstarexpression).  
  
La regla crea un Dac.CodeAnalysis.SqlRuleProblem para representar los problemas encontrados por esta. Al crearla, el Dac.Model.TSqlObject pertinente y posiblemente un elemento de representación [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom) se pasan al constructor y se utilizan para determinar la ubicación del problema en los archivos de código fuente. Al final del análisis, todos estos problemas se pasan al Administrador de errores y se muestran en la lista de errores.  
  
## <a name="see-also"></a>Consulte también  
[Extensión de las características de base de datos](../ssdt/extending-the-database-features.md)  

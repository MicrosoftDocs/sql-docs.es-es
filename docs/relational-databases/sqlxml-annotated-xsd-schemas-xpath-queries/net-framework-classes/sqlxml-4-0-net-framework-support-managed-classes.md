---
title: clases administradas de SQLXML
description: Obtenga información sobre las clases administradas de Microsoft SQLXML que exponen la funcionalidad de SQLXML 4,0 dentro del marco de Microsoft .NET.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- .NET Framework [SQLXML], Managed Classes
- SQL Server .NET Data Provider
- Managed Classes [SQLXML], about managed classes
- providers [SQLXML], SQL Server .NET Data Provider
- data providers [SQLXML], SQL Server .NET Data Provider
- Managed Classes [SQLXML]
- XML [SQLXML]
- SQLXML Managed Classes
- providers [SQLXML], SQLXML Managed Classes
- data providers [SQLXML], SQLXML Managed Classes
- SQLXML, Managed Classes
ms.assetid: 73a5faeb-dabf-4895-acb5-a9651b646065
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f63ac588cf9bdd48ff02343b39d81d250cb6d171
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482976"
---
# <a name="sqlxml-40-net-framework-support---managed-classes"></a>Compatibilidad de SQLXML 4.0 con .NET Framework: clases administradas
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 admite características que le permiten escribir aplicaciones para tener acceso a los datos XML desde una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], introducir los datos en el entorno de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework, procesar los datos y devolver las actualizaciones a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. 
  
  Las clases administradas de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML exponen la funcionalidad de SQLXML 4.0 dentro de [!INCLUDE[msCoName](../../../includes/msconame-md.md)].NET Framework. Con las clases administradas de SQLXML, puede escribir una aplicación C# para tener acceso a los datos XML desde una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], introducir los datos en el entorno .NET Framework, procesar los datos y devolver las actualizaciones a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] como un DiffGram para aplicar las actualizaciones. Debe utilizar un esquema de asignación al aplicar actualizaciones a una base de datos [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilizando clases administradas de SQLXML. Para obtener un ejemplo funcional, vea [obtener acceso a la funcionalidad de SQLXML en el entorno de .net](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md).  
  
 Para utilizar las clases administradas de SQLXML con SQLXML 4.0, debe instalar Microsoft Visual Studio.  
  
> [!NOTE]  
>  .NET Framework incluye el proveedor de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].NET. Este proveedor se puede utilizar para tener acceso a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] desde el entorno .NET; sin embargo, solamente puede administrar consultas SQL tradicionales (es decir, consultas a la base de datos relacional con la excepción de consultas FOR XML). No puede ejecutar plantillas XML o consultas XPath del servidor en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

 Para obtener información sobre el acceso y la modificación de datos en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] el [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework y sobre el uso de DiffGrams para actualizar datos en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tablas, vea [obtener acceso a la funcionalidad de SQLXML en el entorno de .net](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md).  
  
> [!NOTE]  
>  También puede escribir aplicaciones de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Studio para realizar una carga masiva de documentos XML mediante la carga masiva XML. Para obtener más información, vea [realizar la carga masiva de datos XML &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md). Debe agregar una referencia a la DLL de carga masiva XML (Xblkld4.dll) en la aplicación. Ésta es una DLL COM para la que Visual Studio .NET crea automáticamente la biblioteca de contenedores.  
  
  En esta sección se proporcionan aplicaciones de ejemplo que muestran cómo utilizar las [!INCLUDE[msCoName](../../../includes/msconame-md.md)] clases administradas de SQLXML:  
 [Ejecutar consultas SQL &#40;clases administradas de SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-sqlxml-managed-classes.md)  
  [Ejecutar consultas SQL mediante el método ExecuteXMLReader](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-sql-queries-by-using-the-executexmlreader-method.md)  
  [Procesar XML en el lado cliente &#40;clases administradas de SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/processing-xml-on-the-client-side-sqlxml-managed-classes.md)  
  [Ejecutar consultas XPath &#40;clases administradas de SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-sqlxml-managed-classes.md)  
  [Ejecutar consultas XPath con espacios de nombres &#40;clases administradas de SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-xpath-queries-with-namespaces-sqlxml-managed-classes.md)  
  [Ejecutar archivos de plantilla utilizando la propiedad CommandText](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandtext-property.md)  
  [Ejecutar archivos de plantilla utilizando la propiedad CommandStream](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/executing-template-files-by-using-the-commandstream-property.md)  
  [Aplicar una transformación XSL &#40;clases administradas de SQLXML&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/applying-an-xsl-transformation-sqlxml-managed-classes.md)  
  

  
  

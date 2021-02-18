---
title: Novedades de SQL Server 2016
description: Obtenga información sobre las nuevas características de seguridad de SQL Server 2016, las funciones de consulta, la integración de Hadoop y la nube, el análisis de R y mucho más.
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
keywords:
- nuevo sql server
helpviewer_keywords:
- new features [SQL Server]
- SQL Server, what's new
- SQL Server 2008 what's new
- what's new [SQL Server]
ms.assetid: 6a428023-e3cc-4626-a88a-4c13ccbd7db0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c9313877cac9c4e22fdaa4234fc40f0701649934
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271742"
---
# <a name="whats-new-in-sql-server-2016"></a>Novedades de SQL Server 2016
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]    
 Con SQL Server 2016, puede compilar aplicaciones inteligentes críticas mediante una plataforma de base de datos escalable e híbrida con todo integrado, desde rendimiento en memoria y seguridad avanzada hasta análisis en base de datos. La versión de SQL Server 2016 agrega nuevas características de seguridad, funcionalidades de consulta, integración de Hadoop y en la nube, análisis de R y mucho más, junto con numerosas mejoras y ampliaciones. 

En esta página se proporciona información de resumen y vínculos a información más detallada sobre las novedades de SQL Server 2016 para cada componente de SQL Server. 

![SQL Server 2016](../sql-server/media/sql-server-2016.png)

 **Pruebe SQL Server hoy** 
- Descargue la edición **gratuita** [**más reciente de SQL Server**](https://www.microsoft.com/sql-server/sql-server-downloads).
- Descargue la última versión de [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md). 
- ¿Tiene una cuenta de Azure? Ponga en marcha una [máquina virtual con SQL Server 2016 ya instalado](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview).

## <a name="sql-server-2016-database-engine"></a>Motor de base de datos de SQL Server 2016
- Ya puede configurar **varios archivos de base de datos tempDB** durante la instalación y configuración de SQL Server.
- El nuevo **Almacén de consultas** almacena los textos de las consultas, los planes de ejecución y las métricas de rendimiento dentro de la base de datos, lo que permite supervisar y solucionar los problemas de rendimiento con facilidad. Las consultas que consumen más tiempo, memoria o recursos de CPU se muestran en un panel.
- Las **tablas temporales** son tablas de historial en las que se registran todos los cambios de datos, junto con la fecha y hora en que se produjeron.
- La nueva **compatibilidad de JSON** integrada en SQL Server admite importaciones, exportaciones, análisis y almacenamiento de JSON.
- El nuevo motor de consultas de **PolyBase** integra SQL Server con datos externos en Hadoop o Azure Blob Storage. Puede importar y exportar datos, así como ejecutar consultas.
- La nueva característica **Stretch Database** permite archivar datos de forma dinámica y segura desde una base de datos de SQL Server local a una instancia de Azure SQL Database en la nube. SQL Server consulta automáticamente los datos locales y remotos en las bases de datos vinculadas. 
- **OLTP en memoria:** 
    - Ahora admite las restricciones FOREIGN KEY, UNIQUE y CHECK, los procedimientos almacenados compilados nativos OR, NOT, SELECT DISTINCT y OUTER JOIN, así como las subconsultas en SELECT.
    - Admite tablas de hasta 2 TB (a partir de 256 GB). 
    - Presenta mejoras del índice de almacenamiento de columnas para la ordenación, así como compatibilidad para el grupo de disponibilidad AlwaysOn.
- Nuevas características de seguridad:
    - **Always Encrypted:** cuando se habilita, solo la aplicación que tiene la clave de cifrado puede acceder a la información confidencial cifrada en la base de datos de SQL Server 2016. La clave nunca se pasa a SQL Server.
    - **Enmascaramiento dinámico de datos:** si se especifica en la definición de tabla, los datos enmascarados están ocultos para la mayoría de los usuarios, por lo que solo aquellos que dispongan del permiso UNMASK pueden ver la información completa.
    - **Seguridad de nivel de fila:** se puede restringir el acceso a los datos a nivel del motor de base de datos, para que los usuarios solo vean lo que sea pertinente para ellos. 

## <a name="sql-server-2016-analysis-services-ssas"></a>SQL Server 2016 Analysis Services (SSAS)
SQL Server 2016 Analysis Services ofrece mejores prestaciones de rendimiento, creación, administración de base de datos, filtrado, procesamiento y de otras muchas características para bases de datos de modelo tabular en función del **nivel de compatibilidad 1200**.
- **[SQL Server R Services](~/machine-learning/what-s-new-in-sql-server-machine-learning-services.md)** integra el lenguaje de programación R, que se usa para análisis estadísticos, en SQL Server. 
- El nuevo **comprobador de coherencia de la base de datos (DBCC)** se ejecuta internamente para detectar posibles problemas de datos dañados.
- **DirectQuery**, que consulta los datos externos de forma dinámica en lugar de importarlos antes, ahora admite más orígenes de datos, como SQL Azure, Oracle y Teradata. 
- Hay numerosas **funciones de DAX (expresiones de acceso a datos)** nuevas.
- El nuevo espacio de nombres **[Microsoft.AnalysisServices.Tabular](/dotnet/api/microsoft.analysisservices.tabular)** administra modelos e instancias de modo tabular. 
- Se han vuelto a generar los [objetos de administración de Analysis Services (AMO)](/dotnet/api/) para incluir un segundo ensamblado, **Microsoft.AnalysisServices.Core.dll**.

Vea [Motor de Analysis Services (SSAS)](/analysis-services/what-s-new-in-analysis-services). 

## <a name="sql-server-2016-integration-services-ssis"></a>SQL Server 2016 Integration Services (SSIS)
- Compatibilidad con **Grupos de disponibilidad AlwaysOn**
- **Implementación incremental de paquetes**
- Compatibilidad con **Always Encrypted**
- Nuevo rol de base de datos **ssis_logreader**
- Nuevo **nivel de registro personalizado**
- **Nombres de columna para errores** del flujo de datos 
- Nuevos **conectores**
- Compatibilidad con el **sistema de archivos Hadoop (HDFS)**

Vea [Integration Services (SSIS)](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md).

## <a name="sql-server-2016-master-data-services-mds"></a>SQL Server 2016 Master Data Services (MDS)
- **Mejoras de la jerarquía derivada**, incluida la compatibilidad con jerarquías recursivas y de varios a varios
- Filtrado de **atributo basado en dominio**
- **Sincronización de la entidad** para compartir datos de la entidad entre modelos
- Flujos de trabajo de aprobación mediante **conjuntos de cambios**
- **Índices personalizados** para mejorar el rendimiento de consultas
- Nuevos **niveles de permiso** para mejorar la seguridad
- Experiencia rediseñada de **administración de reglas de negocio**

Vea [Master Data Services (MDS)](../master-data-services/what-s-new-in-master-data-services-mds.md).

## <a name="sql-server-2016-reporting-services-ssrs"></a>SQL Server 2016 Reporting Services (SSRS)
Microsoft ha renovado por completo Reporting Services en esta versión. 
- Nuevo **portal de informes web** con la característica KPI
- Nuevo **publicador de informes móviles**
- **Motor de representación del informe rediseñado** que es compatible con HTML5 
- Nuevos **tipos de gráficos** de rectángulos y de proyección solar 

Vea [Reporting Services (SSRS)](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).

## <a name="next-steps"></a>Pasos siguientes   
- [Instalación de SQL Server](../database-engine/install-windows/install-sql-server.md)   
- [Notas de la versión de SQL Server 2016](../sql-server/sql-server-2016-release-notes.md) 
- [Hoja de datos de SQL Server 2016](https://download.microsoft.com/download/C/5/3/C53C3AEF-653C-4598-8721-D522E8AC6A3A/SQL_Server_2016_Everything_Built-In_Datasheet_EN_US.pdf)
- [Características admitidas por las ediciones de SQL Server](./editions-and-components-of-sql-server-2016.md)
- [Requisitos de hardware y software para instalar SQL Server 2016](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
- [Instalar SQL Server 2016 desde el Asistente para la instalación ](../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)
- [Instalación inicial y de servicio](../database-engine/install-windows/install-sql-server-servicing-updates.md)
- [Nuevo módulo de SQL PowerShell](https://blogs.technet.microsoft.com/dataplatforminsider/2016/06/30/sql-powershell-july-2016-update/)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
---
title: SQL Server Migration Assistant | Microsoft Docs
description: Obtenga información sobre SQL Server Migration Assistant, una herramienta que automatiza la migración de bases de datos a SQL Server de Microsoft Access, DB2, MySQL, Oracle y SAP ASE.
ms.custom: ''
ms.date: 10/10/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d0233525-a83b-4279-813e-c554042abd0e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1b715f09cbbcd4dd565e5faa373a23edec50f47f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073349"
---
# <a name="sql-server-migration-assistant"></a>SQL Server Migration Assistant

Microsoft SQL Server Migration Assistant (SSMA) es una herramienta diseñada para automatizar la migración de bases de datos a SQL Server desde Microsoft Access, DB2, MySQL, Oracle y SAP ASE.  
  
## <a name="migration-sources"></a>Orígenes de migración  
  
- [SQL Server Migration Assistant para Access](../ssma/access/sql-server-migration-assistant-for-access-accesstosql.md)  
  
- [SQL Server Migration Assistant para DB2](../ssma/db2/sql-server-migration-assistant-for-db2-db2tosql.md)  
  
- [SQL Server Migration Assistant para MySQL](../ssma/mysql/sql-server-migration-assistant-for-mysql-mysqltosql.md)  
  
- [SQL Server Migration Assistant para Oracle](../ssma/oracle/sql-server-migration-assistant-for-oracle-oracletosql.md)  
  
- [SQL Server Migration Assistant para SAP ASE](../ssma/sybase/sql-server-migration-assistant-for-sybase-sybasetosql.md)  

## <a name="supported-sources-and-target-versions"></a>Orígenes y versiones de destino admitidos

En el caso de los orígenes compatibles, revise la información del centro de descarga de la descarga de SSMA.

Las siguientes versiones de destino son compatibles con SSMA.

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016
- SQL Server 2017 en Windows y Linux
- SQL Server 2019 en Windows y Linux
- Azure SQL Database
- Instancia administrada de Azure SQL
- Azure Synapse Analytics * *

* * Este destino solo es compatible con SSMA para Oracle.

## <a name="downloads"></a>Descargas

- [SSMA para Access](https://aka.ms/ssmaforaccess)
- [SSMA para DB2](https://aka.ms/ssmafordb2)
- [SSMA para MySql](https://aka.ms/ssmaformysql)
- [SSMA para Oracle](https://aka.ms/ssmafororacle)
- [SSMA para SAP ASE](https://aka.ms/ssmaforsybase)
 
## <a name="getting-ssma-support"></a>Obtener compatibilidad con SSMA  

**Ayuda y soporte técnico de Microsoft SQL Server Migration Assistant (SSMA):**  
  
- **Ayuda del producto** : para obtener acceso al servicio de soporte técnico, inicie SSMA y seleccione el menú ayuda o presione la tecla F1.  
  
- **SQL Server** de los foros de la comunidad: formule una pregunta en la comunidad de SQL Server  
  
  - [SQL Server comunidad](../sql-server/index.yml) : grupos de noticias y foros supervisados por la comunidad de SQL Server. En este sitio también se muestran fuentes de información de comunidades como, por ejemplo, blogs y sitios web.  
  
  - [Comunidad de SQL Server Developer Center](../sql-server/index.yml) : grupos de noticias, foros y otros recursos de la comunidad que son útiles para los desarrolladores de SQL Server  
  
- **Soporte técnico asistido** : vaya a [https://support.microsoft.com/assistedsupportproducts](https://support.microsoft.com/assistedsupportproducts) y busque "SQL Server Migration Assistant".  Seleccione su versión y, a continuación, seleccione "iniciar solicitud".  Se incluye soporte técnico asistido con la herramienta SQL Server Migration Assistant.  
  
- **Soporte técnico Premier** : Si tiene un contrato Premier, puede obtener soporte técnico Premier en el [portal Premier online](https://premier.microsoft.com/).  
  
- **Servicios de consultoría** : para migraciones asistidas por asociados, vaya a la [Guía de migración de Azure Database](https://datamigration.microsoft.com/).
  
## <a name="legal-notice-ssma"></a>Aviso legal (SSMA)

Tanto esta documentación como las aplicaciones de ejemplo que contiene, se proporcionan únicamente con fines informativos y sin ninguna garantía, ya sea expresa o implícita. La información que se proporciona en esta documentación, incluidas las direcciones URL y otras referencias a sitios web de Internet, está sujeta a modificaciones sin previo aviso. El usuario asume todos los riesgos derivados del uso de esta documentación.  
  
El objetivo principal de las muestras que se incluyen en esta documentación es ilustrar un concepto o un uso razonable de una instrucción o cláusula determinada. La mayoría de las muestras no incluyen todo el código que se puede encontrar normalmente en un sistema de producción completo, ya que algunos de los controles de errores y de validación de datos habituales se han quitado para centrar el ejemplo en un determinado concepto o instrucción. No hay soporte técnico disponible para estas muestras ni para ningún código fuente incluido.  
  
A menos que se indique lo contrario, los nombres de ejemplos de compañías, organizaciones, productos, nombres de dominio, direcciones de correo electrónico, personas, lugares y acontecimientos aquí mencionados son ficticios, y no deben asociarse en modo alguno ni es intencional ni debe ser inferido o relacionado con ninguna compañía, organización, producto, nombre de dominio, dirección de correo electrónico, persona, lugar o acontecimiento real. Es responsabilidad del usuario el cumplimiento de todas las leyes de derechos de autor aplicables. Sin que se limiten los derechos de autor, ninguna parte de esta documentación puede ser reproducida, almacenada o introducida en un sistema de recuperación, o transmitida de forma alguna, ni por ningún medio (ya sea electrónico, mecánico, por fotocopia, grabación o de otra manera) con ningún propósito, sin la autorización expresa y por escrito de Microsoft Corporation.  
  
Microsoft puede tener patentes, solicitudes de patentes, marcas, derechos de autor y otros derechos de propiedad intelectual sobre la materia objeto de esta documentación. La entrega de esta documentación no le otorga ninguna licencia sobre dichas patentes, marcas, derechos de autor u otros derechos de propiedad intelectual, a menos que así se prevea en un contrato por escrito de licencia de Microsoft a usted.  
  
© 2019 Microsoft Corporation. Todos los derechos reservados.  
  
Microsoft, Windows, Windows NT, Windows Server, Active Directory, ActiveX, BackOffice, bCentral, BizTalk, DirectX, Excel, Hotmail, IntelliSense, J/Direct, Jscript, Microsoft Press, MSDN, MS-DOS, Outlook, PivotChart, PivotTable, PowerPoint, SharePoint, SQL Server, Visual Basic, Visual C#, Visual C++, Visual FoxPro, Visual InterDev, Visual J#, Visual J++, Visual SourceSafe, Visual Studio, Win32, Win32s, Windows Mobile, Windows Server System y WinFX son marcas registradas o marcas comerciales de Microsoft Corporation en los Estados Unidos y/o en otros países o regiones.  
  
SAP NetWeaver es una marca registrada de SAP AG en Alemania y en otros países o regiones.  
  
Las demás marcas comerciales pertenecen a sus respectivos propietarios.  
  
## <a name="documentation-policy-for-sql-server-support-and-upgrade"></a>Directiva de documentación para soporte y actualización de SQL Server

El contenido que aparece en la documentación de SQL Server solo se publica una vez se probó lo suficiente. Documentación del producto: Libros en pantalla de SQL Server, archivos Léame, documentos de problemas conocidos y artículos de Knowledge Base: contiene contenido relacionado con SQL Server características y funcionalidades que es lo suficientemente sólida como para que todos los clientes puedan usarlos con carácter general. Esta directiva se aplica tanto a la documentación de SQL Server como a los archivos Léame de las versiones y los Service Pack; un archivo Léame se considera una extensión de los Libros en pantalla.  
  
En varios casos, es posible que una determinada característica no esté documentada ya que no es algo que los clientes deban usar directamente. A menos que una característica se plasme en la documentación de SQL Server publicada por Microsoft, los contenidos de los libros o sitios web de terceros no serán compatibles con el servicio de asistencia al cliente de Microsoft y no se deberán usar para crear bases de datos o aplicaciones.  
  
Asimismo, los clientes no deberán usar las siguientes API sin documentar, entre otras: procedimientos almacenados, procedimientos almacenados extendidos, funciones, vistas, tablas, columnas, propiedades o metadatos. El servicio de soporte al cliente de Microsoft no admite bases de datos ni aplicaciones que aprovechen o usen puntos de entrada no documentados.  
  
Es posible que las actualizaciones de servidor y de base de datos a versiones futuras de SQL Server no funcionen como es debido en aplicaciones y bases de datos que se aprovechan de o usan puntos de entrada no documentados. Debe limitar el uso de las características y funcionalidades de SQL Server a aquellas que se incluyen en la documentación de Microsoft SQL Server. Si una característica no está documentada en la documentación de Microsoft SQL Server, no será un elemento compatible de SQL Server.
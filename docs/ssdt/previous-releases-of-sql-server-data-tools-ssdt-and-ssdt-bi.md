---
title: Versiones anteriores de SQL Server Data Tools (SSDT)
description: Descubra qué versiones de SSDT y SSDT-BI funcionan con cada versión de Visual Studio. Vea cómo instalar diferentes versiones de SSDT y SSDT-BI.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 5d32e301-0f44-4916-b0db-76e8322c0ab7
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 06/17/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 3e2a3e607b561ede827652f61032b90fc7efa336
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353991"
---
# <a name="previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi"></a>Versiones anteriores de SQL Server Data Tools (SSDT y SSDT-BI)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Data Tools (SSDT) proporciona plantillas de proyecto y superficies de diseño para crear tipos de contenido de SQL Server: bases de datos relacionales, modelos de Analysis Services, informes de Reporting Services y paquetes de Integration Services.

SSDT es compatible con versiones anteriores, por lo que siempre puede usar la [versión más reciente de SSDT](download-sql-server-data-tools-ssdt.md) para diseñar e implementar bases de datos, modelos, informes y paquetes que se ejecutan en versiones anteriores de SQL Server.

Desde que se creó, el shell de Visual Studio que se utiliza para crear tipos de contenido de SQL Server se ha lanzado con diferentes nombres, como **SQL Server Data Tools**, **SQL Server Data Tools - Business Intelligence** y **Business Intelligence Development Studio**. Las versiones anteriores incluían conjuntos específicos de plantillas de proyecto. Para que todas las plantillas de proyecto se encuentren en una misma versión de SSDT, necesita instalar [la más reciente](download-sql-server-data-tools-ssdt.md). De lo contrario, es probable que tenga que instalar varias versiones anteriores para obtener todas las plantillas que se usan en SQL Server. Solo se instala un shell por versión de Visual Studio; si se instala una segunda versión de SSDT solo se agregan las plantillas que faltan.

## <a name="previous-ssdt-releases"></a>Versiones de SSDT anteriores

Para descargar las versiones anteriores de SSDT, seleccione el vínculo de descarga en la sección relacionada.

| Versión de SSDT | Versión de Visual Studio |
|--------------|-----------------------|
| [15.8](#ssdt-for-visual-studio-vs-2017) | 2017 |
| [17.4](#ssdt-for-visual-studio-vs-2015) | 2015 |
| [16.5](#ssdt-for-visual-studio-vs-2013) | 2013 |
| [11.1.50727.1](#ssdt-for-visual-studio-vs-2012) | 2012 |

### <a name="ssdt-for-visual-studio-vs-2017"></a>SSDT para Visual Studio (VS) 2017

**[Descarga de SSDT para Visual Studio 2017 (15.8)](https://go.microsoft.com/fwlink/?linkid=2124319)**

Esta versión de **SSDT para Visual Studio 2017** se puede instalar en los idiomas siguientes:

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40a)

### <a name="ssdt-for-visual-studio-vs-2015"></a>SSDT para Visual Studio (VS) 2015

Para instalar esta versión de SSDT, debe descargar una imagen ISO. El archivo ISO es autocontenido, contiene todos los componentes que necesita SSDT y se puede descargar con un administrador de descargas reiniciable, útil en situaciones con ancho de banda de red limitado o menos confiable. Una vez que se haya descargado, el ISO se puede montar como una unidad.

Pasos para la instalación:

1. **[Descarga de SSDT para Visual Studio 2015 (17.4)](https://go.microsoft.com/fwlink/?linkid=2132817)** .

2. Abra la imagen ISO.

3. Ejecute el archivo *SSDTSetup.exe*.

    ![Imagen ISO](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Esta versión de **SSDT para Visual Studio 2015** se puede instalar en los idiomas siguientes:

| Idioma | Hash SHA256 |
|----------|-------------|
| [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [Francés](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [Alemán](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [Italiano](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [Japonés](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [Coreano](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [Ruso](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [Español](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2013"></a>SSDT para Visual Studio (VS) 2013

Para instalar esta versión de SSDT, debe descargar una imagen ISO. El archivo ISO es autocontenido, contiene todos los componentes que necesita SSDT y se puede descargar con un administrador de descargas reiniciable, útil en situaciones con ancho de banda de red limitado o menos confiable. Una vez que se haya descargado, el ISO se puede montar como una unidad.

Pasos para la instalación:

1. **[Descarga de SSDT para Visual Studio 2013 (16.5)](https://go.microsoft.com/fwlink/?linkid=832312)**

2. Abra la imagen ISO.

3. Ejecute el archivo *SSDTSetup.exe*.

    ![Imagen ISO](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Esta versión de **SSDT para Visual Studio 2013** se puede instalar en los idiomas siguientes:

| Idioma | Hash SHA256 |
|----------|-------------|
| [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [Francés](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [Alemán](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [Italiano](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [Japonés](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [Coreano](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [Ruso](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [Español](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2012"></a>SSDT para Visual Studio (VS) 2012

Para instalar esta versión de SSDT, debe descargar una imagen ISO. El archivo ISO es autocontenido, contiene todos los componentes que necesita SSDT y se puede descargar con un administrador de descargas reiniciable, útil en situaciones con ancho de banda de red limitado o menos confiable. Una vez que se haya descargado, el ISO se puede montar como una unidad.

Pasos para la instalación:

1. **[Descarga de SSDT para Visual Studio 2012 (11.1.50727.1)](https://go.microsoft.com/fwlink/?linkid=518814)**

2. Abra la imagen ISO.

3. Ejecute el archivo *SSDTSetup.exe*.

    ![Imagen ISO](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Esta versión de **SSDT para Visual Studio 2012** se puede instalar en los idiomas siguientes:

| Idioma | Hash SHA256 |
|----------|-------------|
| [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x804) | 78F20325648CFF49D9C58A26186E0AC199D104B3CF634E5663B4B262BEC69C07 |
| [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x404) | A2722380AF2EE1E8BB52B4FA54A61BAB411E5E5FD5B050108F81ED23DC87366D |
| [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x409) | 748EF78D3F9CC6FE360432C378EA690DE51AEB2C747E87D43E08448D26F93A91 |
| [Francés](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40c) | 40E6504169BF618EDC7EB5B62D1215DC96726D6EEC3CA8EC3EEB49044E4B6FB7 |
| [Alemán](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x407) | C45C974E6B8F9611BA2AC1EE90C5C507992BCE5693BF46F6C7C138591ED6A123 |
| [Italiano](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x410) | C1B20CDB41C7B1B5DE76A71F9A091A6FDF5186B12F6AA269DA6338B3CB2D91A6 |
| [Japonés](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x411) | BF56958B904C1C5F28084BD0B16928B6CBFEC83EB3F0C913EC364FA335241943 |
| [Coreano](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x412) | 9023B0656785C357A67E39EB76A2806B923C2BD17342D7226A8ADA384A9F4E28 |
| [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x416) | ACAD9FE03729E289ECE821D92C56CFB1D7FCCEA8423ABF1E164BF3C67ABEFEFB |
| [Ruso](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x419) | E2191D787BA833DF4A85B064C5373DC44099E76214FBF9505728702D4D6B83F0 |
| [Español](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40a) | 6D81FB572A7003C54C29D2ACF076D2CED4A1CA80F329BFF9D41A806920D64EEE |

> [!Note]
> SSDT admite las dos versiones más recientes de Visual Studio. Con el lanzamiento de Visual Studio 2019, ya no se actualizan las versiones de SSDT para Visual Studio 2015 y anteriores. SSDT para Visual Studio 2010 ya no está disponible. Para obtener más información, vea la sección de *preguntas más frecuentes* de [esta entrada del blog del equipo de SSDT](/archive/blogs/ssdt/sql-server-data-tools-17-0-rc-and-ssdt-in-vs2017).

## <a name="sql-bi-analysis-services-reporting-services-integration-services"></a>SQL BI: Analysis Services, Reporting Services e Integration Services

Las plantillas de la edición Business Intelligence (BI) se utilizan para crear modelos de SSAS, informes de SSRS y paquetes SSIS. Los diseñadores de BI están vinculados a versiones específicas de SQL Server. Para utilizar las nuevas características de BI, instale las versiones más recientes de los diseñadores de BI.

## <a name="bi-designers"></a>Diseñadores de BI

[Descargar SSDT-BI para Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313) (SQL Server 2014, SQL Server 2012, SQL Server 2008 y 2008 R2)

[Descargar SSDT-BI para Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843) (SQL Server 2014, SQL Server 2012, SQL Server 2008 y 2008 R2)

Business Intelligence Development Studio (BIDS) se instala mediante el programa de instalación de SQL Server. No hay ninguna descarga web (SQL Server 2008 y 2008 R2).

Para SQL Server 2012 o 2014, puede usar **SSDT-BI para Visual Studio 2012** o **SSDT-BI fo Visual Studio 2013**. La única diferencia entre ambos es la versión de Visual Studio.

## <a name="next-steps"></a>Pasos siguientes

- [Descargar las últimas herramientas de datos SQL Server](../ssdt/download-sql-server-data-tools-ssdt.md)
- [Notas de la versión de SQL Server Data Tools (SSDT)](release-notes-ssdt.md)
- [Descarga de SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [Descarga de Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [Herramientas y utilidades de SQL](../tools/overview-sql-tools.md)
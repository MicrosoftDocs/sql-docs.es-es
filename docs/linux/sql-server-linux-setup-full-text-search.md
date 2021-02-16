---
title: Instalación de Búsqueda de texto completo de SQL Server en Linux
description: Obtenga información sobre cómo instalar Búsqueda de texto completo de SQL Server en Linux. La búsqueda de texto completo permite ejecutar consultas de texto completo en datos basados en caracteres en las tablas de SQL Server.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: bb42076f-e823-4cee-9281-cd3f83ae42f5
ms.openlocfilehash: 406c1a5eb384db2d03514193e0e252ca89c94bd4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346370"
---
# <a name="install-sql-server-full-text-search-on-linux"></a>Instalación de Búsqueda de texto completo de SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Los pasos siguientes sirven para instalar la [Búsqueda de texto completo de SQL Server](../relational-databases/search/full-text-search.md) (**mssql-server-fts**) en Linux. La búsqueda de texto completo permite ejecutar consultas de texto completo en datos basados en caracteres en las tablas de SQL Server. Para ver los problemas conocidos de esta versión, consulte las [Notas de la versión](sql-server-linux-release-notes.md).

> [!NOTE]
> Antes de instalar la búsqueda de texto completo de SQL Server, [instale SQL Server](sql-server-linux-setup.md#platforms). Esto configura las claves y los repositorios que se usan para instalar el paquete **mssql-server-fts**.

Instale la búsqueda de texto completo de SQL Server para su plataforma:

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

## <a name=""></a><a name="RHEL">Instalación en RHEL</a>

Use los comandos siguientes para instalar **mssql-server-fts** en Red Hat Enterprise Linux. 

```bash
sudo yum install -y mssql-server-fts
```

Si ya tiene instalado **mssql-server-fts**, puede actualizar a la versión más reciente con los comandos siguientes:

```bash
sudo yum check-update
sudo yum update mssql-server-fts
```

Si necesita una instalación sin conexión, busque la descarga del paquete Búsqueda de texto completo en las [Notas de la versión](sql-server-linux-release-notes.md). Luego use los mismos pasos de instalación sin conexión descritos en el artículo [Instalar SQL Server](sql-server-linux-setup.md#offline).

## <a name=""></a><a name="ubuntu">Instalación en Ubuntu</a>

Use los comandos siguientes para instalar **mssql-server-fts** en Ubuntu. 

```bash
sudo apt-get update 
sudo apt-get install -y mssql-server-fts
```

Si ya tiene instalado **mssql-server-fts**, puede actualizar a la versión más reciente con los comandos siguientes:

```bash
sudo apt-get update 
sudo apt-get install -y mssql-server-fts 
```

Si necesita una instalación sin conexión, busque la descarga del paquete Búsqueda de texto completo en las [Notas de la versión](sql-server-linux-release-notes.md). Luego use los mismos pasos de instalación sin conexión descritos en el artículo [Instalar SQL Server](sql-server-linux-setup.md#offline).

## <a name=""></a><a name="SLES">Instalación en SLES</a>

Use los comandos siguientes para instalar **mssql-server-fts** en SUSE Linux Enterprise Server. 

```bash
sudo zypper install mssql-server-fts
```

Si ya tiene instalado **mssql-server-fts**, puede actualizar a la versión más reciente con los comandos siguientes:

```bash
sudo zypper refresh
sudo zypper update mssql-server-fts
```

Si necesita una instalación sin conexión, busque la descarga del paquete Búsqueda de texto completo en las [Notas de la versión](sql-server-linux-release-notes.md). Luego use los mismos pasos de instalación sin conexión descritos en el artículo [Instalar SQL Server](sql-server-linux-setup.md#offline).

## <a name="supported-languages"></a>Idiomas compatibles

La búsqueda de texto completo utiliza [separadores de palabras](../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) que determinan cómo identificar palabras individuales en función del idioma. Para obtener una lista de los separadores de palabras registrados, consulte la vista del catálogo **sys.fulltext_languages**. Los separadores de palabras de los siguientes idiomas se instalan con SQL Server:

| Lenguaje | Id. de idioma |
|---|---|
| Neutra | 0 |
| Árabe | 1025 |
| Bengali (India) | 1093 |
| Bokmål | 1044 |
| Portugués (Brasil) | 1046 |
| British English | 2057 |
| Búlgaro | 1026 |
| Catalán | 1027 |
| Chino (Hong Kong ZAE, RPC) | 3076 |
| Chinese (Macao SAR) | 5124 |
| Chinese (Singapore) | 4100 |
| Croata | 1050 |
| Checo | 1029 |
| Danés | 1030 |
| Neerlandés | 1043 |
| Inglés | 3082 |
| Francés | 1036 |
| Alemán | 1031 |
| Griego | 1032 |
| Gujarati | 1095 |
| Hebreo | 1037 |
| Hindi | 1081 |
| Islandés | 1039 |
| Indonesio | 1057 |
| Italiano | 1040 |
| Japonés | 1041 |
| Canarés | 1099 |
| Coreano | 1042 |
| Letón | 1062 |
| Lituano | 1063 |
| Malay - Malaysia | 1086 |
| Malayalam | 1100 |
| Maratí | 1102 |
| Polaco | 1045 |
| Portugués | 2070 |
| Punjabi | 1094 |
| Rumano | 1048 |
| Ruso | 1049 |
| Serbio (cirílico) | 3098 |
| Serbio (latino) | 2074 |
| Chino simplificado | 2052 |
| Eslovaco | 1051 |
| Esloveno | 1060 |
| Español | 3082 |
| Sueco | 1053 |
| Tamil | 1097 |
| Telugu | 1098 |
| Tailandés | 1054 |
| Chino tradicional | 1028 |
| Turco | 1055 |
| Ucraniano | 1058 |
| Urdu | 1056 |
| Vietnamita | 1066 |

## <a name="filters"></a><a id="filters"></a> Filtros

La búsqueda de texto completo también funciona con texto almacenado en archivos binarios. Sin embargo, en este caso se necesita instalar un filtro para procesar el archivo. Para obtener más información sobre los filtros, vea [Configurar y administrar filtros para búsquedas](../relational-databases/search/configure-and-manage-filters-for-search.md).

Para ver una lista de los filtros instalados, llame a **sp_help_fulltext_system_components 'filter'**. Para SQL Server, se instalan los siguientes filtros:

| Nombre de componente | Id. de clase | Versión |
|---|---|---|
|.a | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ans | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.asc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ascx | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.asm | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.asp | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.aspx | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.asx | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.bas | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.bat | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.bcp | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.c | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.cc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.cls | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.cmd | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.cpp | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.cs | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.csa | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.css | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.csv | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.cxx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.dbs | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.def | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.dic | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.dos | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.dsp | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.dsw | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ext | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.faq | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.fky | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.h | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.hhc | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.hpp | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.hta | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htm | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.html | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htt | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htw | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htx | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.hxx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.i | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ibq | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.ics | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.idl | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.idq | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.inc | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.inf | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.ini | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.inl | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.inx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.jav | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.java | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.js | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.kci | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.lgn | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.log | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.lst | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.m3u | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.mak | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.mk | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.odc | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.odh | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.odl | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.pkgdef | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.pkgundef | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.pl | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.prc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.rc | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.rc2 | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.rct | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.reg | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.rgs | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.rtf | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.rul | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.s | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.scc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.shtm | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.shtml | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.snippet | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.sol | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.sor | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.srf | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.stm | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.tab | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.tdl | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.tlh | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.tli | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.trg | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.txt | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.udf | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.udt | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.url | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.usr | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vbs | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.viw | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsct | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsixlangpack | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsixmanifest | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vspscc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsscc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vssscc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.wri | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.wtx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.xml | 41B9BE05-B3AF-460C-BF0B-2CDD44A093B1 | 12.0.9735.0 |

## <a name="semantic-search"></a>Búsqueda semántica
La [búsqueda semántica](../relational-databases/search/semantic-search-sql-server.md) se basa en la característica Búsqueda de texto completo para extraer e indexar *frases clave* estadísticamente relevantes. Esto le permite consultar el significado de los documentos de la base de datos. También ayuda a identificar los documentos que son similares.

Para utilizar la búsqueda semántica, primero debe restaurar la base de datos Estadísticas de lenguaje semántico en su equipo.

1. Use una herramienta, como [sqlcmd](sql-server-linux-setup-tools.md), para ejecutar el siguiente comando de Transact-SQL en la instancia de SQL Server de Linux. Este comando restaura la base de datos de estadísticas de lenguaje.

   ```sql
   RESTORE DATABASE [semanticsdb] FROM
   DISK = N'/opt/mssql/misc/semanticsdb.bak' WITH FILE = 1,
   MOVE N'semanticsdb' TO N'/var/opt/mssql/data/semanticsDB.mdf',
   MOVE N'semanticsdb_log' TO N'/var/opt/mssql/data/semanticsdb_log.ldf', NOUNLOAD, STATS = 5
   GO
   ```

   > [!NOTE]
   > Si es necesario, actualice las rutas de acceso del comando RESTORE anterior para ajustarlas a su configuración.

1. Ejecute el siguiente comando de Transact-SQL para registrar la base de datos Estadísticas de lenguaje semántico.

    ```sql
    EXEC sp_fulltext_semantic_register_language_statistics_db @dbname = N'semanticsdb';  
    GO
    ```

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre la búsqueda de texto completo, vea [Búsqueda de texto completo de SQL Server](../relational-databases/search/full-text-search.md). 

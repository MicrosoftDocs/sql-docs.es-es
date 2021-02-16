---
description: Reversión a la versión anterior de los separadores de palabras usados por Buscar (Búsqueda de SQL Server)
title: Reversión a la versión anterior de los separadores de palabras usados por Buscar
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 29b4488e-4c6a-4bf0-a64d-19e2fdafa7ae
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: 8362788a2bf1092e815a9df43c3b65298935d555
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344467"
---
# <a name="revert-word-breakers-used-by-search-to-previous-version-sql-server-search"></a>Reversión a la versión anterior de los separadores de palabras usados por Buscar (Búsqueda de SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] instala y habilita una versión de los separadores de palabras y lematizadores para todos los idiomas admitidos por búsqueda de texto completo, a excepción del coreano. En este artículo se describe cómo pasar de esta versión de estos componentes a la versión previa o viceversa.  
  
 En este artículo no se explican los siguientes idiomas:  
  
- **Inglés**. Para revertir o restaurar los componentes en inglés, vea [Cambiar el separador de palabras usado para el inglés de Estados Unidos y el del Reino Unido](../../relational-databases/search/change-the-word-breaker-used-for-us-english-and-uk-english.md).  
  
- **Danés, polaco y turco**. Se han reemplazado los separadores de palabras de terceros para danés, polaco y turco que se incluían en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con componentes de [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
- **Checo y griego**. Existen nuevos separadores de palabras para checo y griego. Las versiones anteriores de búsqueda de texto completo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no incluían compatibilidad con estos dos idiomas.  
  
- **Coreano**. En esta versión no se actualiza el separador de palabras y el lematizador para el idioma coreano.  
  
 Para obtener información general sobre los separadores de palabras y lematizadores, vea [Configurar y administrar separadores de palabras y lematizadores para la búsqueda](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
##  <a name="overview-of-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="overview"></a> Información general sobre la reversión y restauración de los separadores de palabras y los lematizadores  
 Las instrucciones para revertir y restaurar los separadores de palabras y lematizadores dependen del idioma. En la tabla siguiente se resumen los tres conjuntos de acciones que pueden ser necesarios para revertir los componentes a la versión anterior.  
  
|Archivo actual|Archivo anterior|Número de idiomas afectados|Acción de archivos|Acción de entradas del Registro|  
|------------------|-------------------|----------------------------------|----------------------|---------------------------------|  
|NaturalLanguage6.dll|NaturalLanguage6.dll|34|Obtenga e instale una versión anterior de NaturalLanguage6.dll, sobrescribiendo la versión actual del archivo.|No es necesaria ninguna acción.<br /><br /> Las claves del Registro y los valores no han cambiado en esta versión.|  
|(Otro nombre de archivo)|NaturalLanguage6.dll|5|Obtenga e instale una versión anterior de NaturalLanguage6.dll, sobrescribiendo la versión actual del archivo.|Cambie un conjunto de entradas del Registro para especificar la versión anterior de los componentes.|  
|(Otro nombre de archivo)|(Otro nombre de archivo)|6|No es necesaria ninguna acción.<br /><br /> [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] copia la versión actual y las anteriores de los componentes en la carpeta Binn.|Cambie un conjunto de entradas del Registro para especificar la versión anterior de los componentes.|  
  
> [!WARNING]  
>  Si reemplaza la versión actual del archivo NaturalLanguage6.dll con una versión distinta, el comportamiento de todos los idiomas que utilizan este archivo se verá afectado.  
  
 Los archivos descritos en este artículo son archivos DLL instalados en la carpeta `MSSQL\Binn` de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La ruta de acceso completa normalmente es:  
  
 `C:\Program Files\Microsoft SQL Server\<instance>\MSSQL\Binn`  
  
##  <a name="languages-for-which-the-file-name-of-both-the-current-and-previous-word-breaker-is-naturallanguage6dll"></a><a name="nl6nl6"></a> Idiomas para los que el nombre de archivo del separador de palabras actual y anterior es NaturalLanguage6.dll  
 Para los idiomas en la siguiente tabla, el nombre de archivo del separador de palabras actual y anterior es NaturalLanguage6.dll. Para revertir o restaurar estos componentes, tiene que sobrescribir NaturalLanguage6.dll con una versión diferente del mismo archivo. No tendrá que cambiar ninguna entrada del Registro, porque las entradas de Registro no han modificado en esta versión.  
  
> [!WARNING]  
>  Si reemplaza la versión actual del archivo NaturalLanguage6.dll con una versión distinta, el comportamiento de todos los idiomas que utilizan este archivo se verá afectado.  
  
 **Lista de idiomas afectados**  
  
|Idioma|Abreviatura<br />utilizada en el<br />Registro|LCID|  
|--------------|---------------------------------------|----------|  
|Bengalí|`ben`|1093|  
|Búlgaro|`bgr`|1026|  
|Catalán|`cat`|1027|  
|Español|`esn`|3082|  
|Francés|`fra`|1036|  
|Gujarati|`guj`|1095|  
|Hebreo|`heb`|1037|  
|Hindi|`hin`|1081|  
|Croata|`hrv`|1050|  
|Indonesio|`ind`|1057|  
|Islandés|`isl`|1039|  
|Italiano|`ita`|1040|  
|Canarés|`kan`|1099|  
|Lituano|`lth`|1063|  
|Letón|`lvi`|1062|  
|Malayalam|`mal`|1100|  
|Maratí|`mar`|1102|  
|Malayo|`msl`|1086|  
|Neutra|`Neutral`|0000|  
|Noruego (Bokmal)|`nor`|1044|  
|Punjabi|`pan`|1094|  
|Portugués (Brasil)|`ptb`|1046|  
|Portugués|`ptg`|2070|  
|Rumano|`rom`|1048|  
|Eslovaco|`sky`|1051|  
|Esloveno|`slv`|1060|  
|Serbio (cirílico)|`srb`|3098|  
|Serbio (latino)|`srl`|2074|  
|Sueco|`sve`|1053|  
|Tamil|`tam`|1097|  
|Telugu|`tel`|1098|  
|Ucraniano|`ukr`|1058|  
|Urdu|`urd`|1056|  
|Vietnamita|`vit`|1066|  
  
 La tabla anterior está ordenada alfabéticamente según la columna Abreviatura.  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="nl6nl6revert"></a> Para revertir a los componentes anteriores  
  
1.  Navegue a la carpeta Binn descrita anteriormente.  
  
2.  Copie la versión de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de NaturalLanguage6.dll en otra ubicación.  
  
3.  Copie la versión anterior de NaturalLanguage6.dll de la carpeta Binn de una instancia de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] o de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] en la carpeta Binn de la instancia de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    > [!WARNING]  
    >  Este cambio afecta a todos los idiomas que utilizan NaturalLanguage6.dll en la versión actual y en la anterior.  
  
4.  Reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

###  <a name="to-restore-the-current-components"></a><a name="nl6nl6restore"></a> Para restaurar los componentes actuales  
  
1.  Navegue a la ubicación en la que realizó la copia de seguridad de la versión de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de NaturalLanguage6.dll.  
  
2.  Copie la versión actual de NaturalLanguage6.dll de la ubicación de la copia de seguridad de la carpeta Binn de la instancia de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    > [!WARNING]  
    >  Este cambio afecta a todos los idiomas que utilizan NaturalLanguage6.dll en la versión actual y en la anterior.  
  
3.  Reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="languages-for-which-the-file-name-of-the-previous-word-breaker-only-is-naturallanguage6dll"></a><a name="newnl6"></a> Idiomas para los que el nombre de archivo del separador de palabras anterior solo es NaturalLanguage6.dll  
 Para los idiomas de la tabla siguiente, el nombre de archivo del separador de palabras anterior es diferente del nombre de la nueva versión. El nombre de archivo anterior es NaturalLanguage6.dll. Para revertir a la versión anterior, tiene que sobrescribir la versión actual de NaturalLanguage6.dll con una versión anterior del mismo archivo. También tiene que cambiar un conjunto de entradas del Registro para especificar la versión anterior o actual de los componentes.  
  
> [!WARNING]  
>  Si reemplaza la versión actual del archivo NaturalLanguage6.dll con una versión distinta, el comportamiento de todos los idiomas que utilizan este archivo se verá afectado.  
  
 **Lista de idiomas afectados**  
  
|Idioma|Abreviatura<br />utilizada en el<br />Registro|LCID|  
|--------------|---------------------------------------|----------|  
|Árabe|ara|1025|  
|Alemán|deu|1031|  
|Japonés|jpn|1041|  
|Neerlandés|nld|1043|  
|Ruso|rus|1049|  
  
 La tabla anterior está ordenada alfabéticamente según la columna Abreviatura.  
  
 Use las siguientes instrucciones junto con la lista de valores de la sección [Nombres de archivos y de valores de Registro para revertir y restaurar los separadores de palabras y los lematizadores](#newnl6values).  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="newnl6revert"></a> Para revertir a los componentes anteriores  
  
1.  Navegue a la carpeta Binn descrita anteriormente.  
  
2.  No elimine los archivos de la versión actual de los componentes de la carpeta Binn.  
  
3.  Copie la versión de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de NaturalLanguage6.dll en otra ubicación.  
  
4.  Copie la versión anterior de NaturalLanguage6.dll de la carpeta Binn de una instancia de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] o [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] en la carpeta Binn de la nueva instancia de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
    > [!WARNING]  
    >  Este cambio afecta a todos los idiomas que utilizan NaturalLanguage6.dll en la versión actual y en la anterior.  
  
5.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
6.  Siga estos pasos para agregar nuevas claves a los COM ClassID de las interfaces de separadores de palabras y lematizadores en el idioma seleccionado:  
  
    1.  Agregue una nueva clave con el valor procedente de la tabla del separador de palabras anterior.  
  
    2.  Actualice los datos (predeterminados) de ese valor de clave con el nombre de archivo del separador de palabras anterior de la tabla.  
  
    3.  Si el idioma seleccionado utiliza un lematizador, agregue una nueva clave con el valor de la tabla del lematizador anterior.  
  
    4.  Si el idioma seleccionado utiliza un lematizador, actualice los datos (predeterminados) de ese valor de clave con el nombre de archivo del lematizador anterior de la tabla.  
  
7.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<clave_idioma>* representa la abreviatura del idioma que se usa en el Registro; por ejemplo, "fra" para francés y "esn" para español.  
  
8.  Actualice el valor de clave de **WBreakerClass** con el valor de la tabla para el separador de palabras actual.  
  
9. Si el idioma seleccionado utiliza un lematizador, actualice el valor de clave de **StemmerClass** con el valor de la tabla del lematizador actual.  
  
10. Reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="to-restore-the-current-components"></a><a name="newnl6restore"></a> Para restaurar los componentes actuales  
  
1.  Navegue a la ubicación en la que realizó la copia de seguridad de la versión de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de NaturalLanguage6.dll.  
  
2.  Copie la versión actual de NaturalLanguage6.dll de la ubicación de la copia de seguridad de la carpeta Binn de la instancia de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    > [!WARNING]  
    >  Este cambio afecta a todos los idiomas que utilizan NaturalLanguage6.dll en la versión actual y en la anterior.  
  
3.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
4.  Si las siguientes claves no existen, siga estos pasos para agregar nueva claves para los ClassID COM de las interfaces de separadores de palabras y lematizadores actuales en el idioma seleccionado:  
  
    1.  Agregue una nueva clave con el valor procedente de la tabla del separador de palabras actual.  
  
    2.  Actualice los datos (predeterminados) de ese valor de clave con el nombre de archivo del separador de palabras actual de la tabla.  
  
    3.  Si el idioma seleccionado utiliza un lematizador, agregue una nueva clave con el valor de la tabla del lematizador actual.  
  
    4.  Si el idioma seleccionado utiliza un lematizador, actualice los datos (predeterminados) de ese valor de clave con el nombre de archivo del lematizador actual de la tabla.  
  
5.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<clave_idioma>* representa la abreviatura del idioma que se usa en el Registro; por ejemplo, "fra" para francés y "esn" para español.  
  
6.  Actualice el valor de clave de **WBreakerClass** con el valor de la tabla para el separador de palabras anterior.  
  
7.  Si el idioma seleccionado utiliza un lematizador, actualice el valor de clave de **StemmerClass** con el valor de la tabla del lematizador anterior.  
  
8.  Reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="file-names-and-registry-values-for-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="newnl6values"></a> Nombres de archivos y de valores de Registro para revertir y restaurar los separadores de palabras y los lematizadores  
 Utilice la siguiente lista de nombres de archivo y de entradas del Registro junto con las instrucciones en la sección anterior. Utilice los valores anteriores para revertir a la versión anterior, o use los valores actuales para restaurar la versión actual de los componentes.  
  
 La siguiente lista está ordenada alfabéticamente según la abreviatura utilizada para cada idioma.  
  
 **Árabe (ara), LCID 1025**  
  
|Componente|Separador de palabras|Lematizador|  
|---------------|------------------|-------------|  
|CLSID anterior|7EFD3C7E-9E4B-4a93-9503-DECD74C0AC6D|483B0283-25DB-4c92-9C15-A65925CB95CE|  
|Nombre de archivo anterior|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID actual|04b37e30-c9a9-4a7d-8f20-792fc87ddf71|None|  
|Nombre de archivo actual|MSWB7.dll|None|  
  
 **Alemán (deu), LCID 1031**  
  
|Componente|Separador de palabras|Lematizador|  
|---------------|------------------|-------------|  
|CLSID anterior|45EACA36-DBE9-4e4a-A26D-5C201902346D|65170AE4-0AD2-4fa5-B3BA-7CD73E2DA825|  
|Nombre de archivo anterior|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID actual|dfa00c33-bf19-482e-a791-3c785b0149b4|8a474d89-6e2f-419c-8dd5-9b50edc8c787|  
|Nombre de archivo actual|MSWB7.dll|MSWB7.dll|  
  
 **Japonés (jpn), LCID 1041**  
  
|Componente|Separador de palabras|Lematizador|  
|---------------|------------------|-------------|  
|CLSID anterior|E1E8F15E-8BEC-45df-83BF-50FF84D0CAB5|3D5DF14F-649F-4cbc-853D-F18FEDE9CF5D|  
|Nombre de archivo anterior|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID actual|04096682-6ece-4e9e-90c1-52d81f0422ed|None|  
|Nombre de archivo actual|MsWb70011.dll|None|  
  
 **Holandés (nld), LCID 1043**  
  
|Componente|Separador de palabras|Lematizador|  
|---------------|------------------|-------------|  
|CLSID anterior|2C9F6BEB-C5B0-42b6-A5EE-84C24DC0D8EF|F7A465EE-13FB-409a-B878-195B420433AF|  
|Nombre de archivo anterior|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID actual|69483c30-a9af-4552-8f84-a0796ad5285b|CF923CB5-1187-43ab-B053-3E44BED65FFA|  
|Nombre de archivo actual|MSWB7.dll|MSWB7.dll|  
  
 **Ruso (rus), LCID 1049**  
  
|Componente|Separador de palabras|Lematizador|  
|---------------|------------------|-------------|  
|CLSID anterior|2CB6CDA4-1C14-4392-A8EC-81EEF1F2E079|E06A0DDD-E81A-4e93-8A8D-F386C3A1B670|  
|Nombre de archivo anterior|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID actual|aaa3d3bd-6de7-4317-91a0-d25e7d3babc3|d42c8b70-adeb-4b81-a52f-c09f24f77dfa|  
|Nombre de archivo actual|MSWB7.dll|MSWB7.dll|  
  
##  <a name="languages-for-which-neither-the-previous-nor-the-current-file-name-is-naturallanguage6dll"></a><a name="newnew"></a> Idiomas para los que ni el nombre de archivo anterior ni el actual es NaturalLanguage6.dll  
 Para los idiomas en la tabla siguiente, los nombres de archivo de los separadores de palabras y los lematizadores anteriores son diferentes de los nombres de archivo de las nuevas versiones. Ni el nombre de archivo anterior ni el actual es NaturalLanguage6.dll. No tendrá que reemplazar los archivos, porque la instalación de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] copia la versión actual y las anteriores de los componentes en la carpeta Binn. Sin embargo, tendrá que cambiar un conjunto de entradas del Registro para especificar la versión anterior o actual de los componentes.  
  
 **Lista de idiomas afectados**  
  
|Idioma|Abreviatura<br />utilizada en el<br />Registro|LCID|  
|--------------|---------------------------------------|----------|  
|Chino simplificado|chs|2052|  
|Chino tradicional|cht|1028|  
|Tailandés|tha|1054|  
|Chino tradicional|zh-HK|3076|  
|Chino tradicional|zh-mo|5124|  
|Chino simplificado|zh-sg|4100|  
  
 La tabla anterior está ordenada alfabéticamente según la columna Abreviatura.  
  
 Use las siguientes instrucciones junto con la lista de valores de la sección [Nombres de archivos y de valores de Registro para revertir y restaurar los separadores de palabras y los lematizadores](#newnewvalues).  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="newnewrevert"></a> Para revertir a los componentes anteriores  
  
1.  No elimine los archivos de la versión actual de los componentes de la carpeta Binn.  
  
2.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
3.  Siga estos pasos para agregar nuevas claves a los COM ClassID de las interfaces de separadores de palabras y lematizadores en el idioma seleccionado:  
  
    1.  Agregue una nueva clave con el valor procedente de la tabla del separador de palabras anterior.  
  
    2.  Actualice los datos (predeterminados) de ese valor de clave con el nombre de archivo del separador de palabras anterior de la tabla.  
  
    3.  Si el idioma seleccionado utiliza un lematizador, agregue una nueva clave con el valor de la tabla del lematizador anterior.  
  
    4.  Si el idioma seleccionado utiliza un lematizador, actualice los datos (predeterminados) de ese valor de clave con el nombre de archivo del lematizador anterior de la tabla.  
  
4.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<clave_idioma>* representa la abreviatura del idioma que se usa en el Registro; por ejemplo, "fra" para francés y "esn" para español.  
  
5.  Actualice el valor de clave de **WBreakerClass** con el valor de la tabla para el separador de palabras actual.  
  
6.  Si el idioma seleccionado utiliza un lematizador, actualice el valor de clave de **StemmerClass** con el valor de la tabla del lematizador actual.  
  
7.  Reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="to-restore-the-previous-components"></a><a name="newnewrestore"></a> Para restaurar los componentes anteriores  
  
1.  No elimine los archivos de la versión anterior de los componentes de la carpeta Binn.  
  
2.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
3.  Si las siguientes claves no existen, siga estos pasos para agregar nueva claves para los ClassID COM de las interfaces de separadores de palabras y lematizadores actuales en el idioma seleccionado:  
  
    1.  Agregue una nueva clave con el valor procedente de la tabla del separador de palabras actual.  
  
    2.  Actualice los datos (predeterminados) de ese valor de clave con el nombre de archivo del separador de palabras actual de la tabla.  
  
    3.  Si el idioma seleccionado utiliza un lematizador, agregue una nueva clave con el valor de la tabla del lematizador actual.  
  
    4.  Si el idioma seleccionado utiliza un lematizador, actualice los datos (predeterminados) de ese valor de clave con el nombre de archivo del lematizador actual de la tabla.  
  
4.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<clave_idioma>* representa la abreviatura del idioma que se usa en el Registro; por ejemplo, "fra" para francés y "esn" para español.  
  
5.  Actualice el valor de clave de **WBreakerClass** con el valor de la tabla para el separador de palabras anterior.  
  
6.  Si el idioma seleccionado utiliza un lematizador, actualice el valor de clave de **StemmerClass** con el valor de la tabla del lematizador anterior.  
  
7.  Reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="file-names-and-registry-values-for-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="newnewvalues"></a> Nombres de archivos y de valores de Registro para revertir y restaurar los separadores de palabras y los lematizadores  
 Utilice la siguiente lista de nombres de archivo y de entradas del Registro junto con las instrucciones en la sección anterior. Utilice los valores anteriores para revertir a la versión anterior, o use los valores actuales para restaurar la versión actual de los componentes.  
  
 La siguiente lista está ordenada alfabéticamente según la abreviatura utilizada para cada idioma.  
  
 **Chino simplificado (chs), LCID 2052**  
  
|Componente|Separador de palabras|  
|---------------|------------------|  
|CLSID anterior|12CE94A0-DEFB-11D2-B31D-00600893A857|  
|Nombre de archivo anterior|chsbrkr.dll|  
|CLSID actual|E0831C90-BAB0-4ca5-B9BD-EA254B538DAC|  
|Nombre de archivo actual|MsWb70804.dll|  
  
 **Chino tradicional (cht), LCID 1028**  
  
|Componente|Separador de palabras|  
|---------------|------------------|  
|CLSID anterior|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Nombre de archivo anterior|chtbrkr.dll|  
|CLSID actual|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Nombre de archivo actual|MsWb70404.dll|  
  
 **Tailandés (tha), LCID 1054**  
  
|Componente|Separador de palabras|Lematizador|  
|---------------|------------------|-------------|  
|CLSID anterior|CCA22CF4-59FE-11D1-BBFF-00C04FB97FDA|CEDC01C7-59FE-11D1-BBFF-00C04FB97FDA|  
|Nombre de archivo anterior|Thawbrkr.dll|Thawbrkr.dll|  
|CLSID actual|F70C0935-6E9F-4ef1-9F06-7876536DB900|None|  
|Nombre de archivo actual|MsWb7001e.dll|None|  
  
 **Chino tradicional (zh-hk), LCID 3076**  
  
|Componente|Separador de palabras|  
|---------------|------------------|  
|CLSID anterior|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Nombre de archivo anterior|chtbrkr.dll|  
|CLSID actual|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Nombre de archivo actual|MsWb70404.dll|  
  
 **Chino tradicional (zh-mo), LCID 5124**  
  
|Componente|Separador de palabras|  
|---------------|------------------|  
|CLSID anterior|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Nombre de archivo anterior|chtbrkr.dll|  
|CLSID actual|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Nombre de archivo actual|MsWb70404.dll|  
  
 **Chino simplificado (zh-sg), LCID 4100**  
  
|Componente|Separador de palabras|  
|---------------|------------------|  
|CLSID anterior|12CE94A0-DEFB-11D2-B31D-00600893A857|  
|Nombre de archivo anterior|chsbrkr.dll|  
|CLSID actual|E0831C90-BAB0-4ca5-B9BD-EA254B538DAC|  
|Nombre de archivo actual|MsWb70804.dll|  
  
## <a name="see-also"></a>Consulte también  
 [Cambiar el separador de palabras usado para el inglés de Estados Unidos y el del Reino Unido](../../relational-databases/search/change-the-word-breaker-used-for-us-english-and-uk-english.md)   
 [Cambios de comportamiento en la búsqueda de texto completo](./full-text-search.md)

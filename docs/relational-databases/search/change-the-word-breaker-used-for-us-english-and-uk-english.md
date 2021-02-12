---
description: Cambiar el separador de palabras usado para el inglés de Estados Unidos y el del Reino Unido
title: Cambio del separador de palabras usado para el inglés de Estados Unidos y el del Reino Unido | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 6b5d2177-db98-47f5-b32e-4b80a2f74ffe
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: de2501c3af1fcc24aed29dc66da455d7e2ff1f2b
ms.sourcegitcommit: 10ae200635b9e8554e6bc6f658125e1a80d4d5ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2021
ms.locfileid: "99589313"
---
# <a name="change-the-word-breaker-used-for-us-english-and-uk-english"></a>Cambiar el separador de palabras usado para el inglés de Estados Unidos y el del Reino Unido

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], el programa de instalación instala una nueva versión del separador de palabras y del lematizador para el idioma inglés, y reemplaza la versión anterior de estos componentes. Para obtener más información sobre el comportamiento modificado de los nuevos componentes, vea [Cambios de comportamiento en la búsqueda de texto completo](./full-text-search.md). En este tema se describe cómo pasar de la nueva versión de estos componentes a la versión previa o viceversa. Para las instalaciones de clúster, estos cambios deben realizarse en todos los nodos principales y pasivos.  
  
 Algunas versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usaban separadores de palabras diferentes representados por CLSID diferentes para el inglés de Estados Unidos (LCID 1033) y el inglés del Reino Unido (LCID 2057). En esta versión, ambos LCID usan los mismos componentes con los mismos CLSID, como se muestra en la siguiente tabla:  
  
|LCID|Separador de palabras instalado por versiones anteriores<br /><br /> versión 12.0.6828.0|Lematizador instalado por versiones anteriores|Separador de palabras instalado por esta versión<br /><br /> versión 14.0.4999.1038|Lematizador instalado por esta versión|  
|----------|-------------------------------------------------------------------------|--------------------------------------------|-----------------------------------------------------------------------|---------------------------------------|  
|3082<br />(inglés de Estados Unidos)|188D6CC5-CB03-4C01-912E-47D21295D77E|EEED4C20-7F1B-11CE-BE57-00AA0051FE20|9faed859-0b30-4434-ae65-412e14a16fb8|e1e5ef84-c4a6-4e50-8188-99aef3de2659|  
|2057<br />(inglés del Reino Unido)|173C97E2-AEBE-437C-9445-01B237ABF2F6|D99F7670-7F1A-11CE-BE57-00AA0051FE20|9faed859-0b30-4434-ae65-412e14a16fb8|e1e5ef84-c4a6-4e50-8188-99aef3de2659|  
  
 Los componentes descritos en este tema son archivos DLL instalados en la carpeta `MSSQL\Binn` para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La ruta de acceso completa es normalmente `C:\Program Files\Microsoft SQL Server\<instance>\MSSQL\Binn`.  
  
 Para obtener más información sobre los separadores de palabras y los lematizadores, vea [Configurar y administrar separadores de palabras y lematizadores para la búsqueda](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
## <a name="switching-from-the-current-english-word-breaker-to-the-previous-english-word-breakers"></a>Pasar del separador de palabras de inglés actual a los separadores de palabras anteriores de inglés  
  
#### <a name="to-switch-from-the-current-version-of-the-us-english-word-breaker-to-the-previous-version"></a>Para pasar de la versión actual del separador de palabras de inglés de Estados Unidos a la versión anterior  
  
1.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID**.  
  
2.  Siga estos pasos para agregar nuevas claves a los COM ClassID de las interfaces en inglés de Estados Unidos de separadores de palabras y lematizadores del LCID 1033:  
  
    1.  Agregue una nueva clave con el valor **{188D6CC5-CB03-4C01-912E-47D21295D77E}** para el separador de palabras anterior.  
  
    2.  Actualice los datos (predeterminados) de ese valor de clave a **langwrbk.dll**.  
  
    3.  Agregue una nueva clave con el valor **{EEED4C20-7F1B-11CE-BE57-00AA0051FE20}** para el lematizador anterior.  
  
    4.  Actualice los datos (predeterminados) de ese valor de clave a **infosoft.dll**.  
  
3.  En el Registro, vaya al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\Language\enu**.  
  
4.  Actualice el valor de clave **WBreakerClass** a **{188D6CC5-CB03-4C01-912E-47D21295D77E}**.  
  
5.  Actualice el valor de clave **StemmerClass** a **{EEED4C20-7F1B-11CE-BE57-00AA0051FE20}**.  
  
6.  Reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

#### <a name="to-switch-from-the-current-version-of-the-uk-english-word-breaker-to-the-previous-version"></a>Para pasar de la versión actual del separador de palabras de inglés del Reino Unido a la versión anterior  
  
1.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID**.  
  
2.  Siga estos pasos para agregar una nueva clave para los ClassID COM de las interfaces inglesas del Reino Unido de separadores de palabras y lematizadores del LCID 2057:  
  
    1.  Agregue una nueva clave con el valor **{173C97E2-AEBE-437C-9445-01B237ABF2F6}** para el separador de palabras anterior.  
  
    2.  Actualice los datos (predeterminados) de ese valor de clave a **langwrbk.dll**.  
  
    3.  Agregue una nueva clave con el valor **{D99F7670-7F1A-11CE-BE57-00AA0051FE20}** para el lematizador anterior.  
  
    4.  Actualice los datos (predeterminados) de ese valor de clave a **infosoft.dll**.  
  
3.  En el Registro, vaya al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\Language\eng**.  
  
4.  Actualice el valor de clave **WBreakerClass** a **{173C97E2-AEBE-437C-9445-01B237ABF2F6}**.  
  
5.  Actualice el valor de clave **StemmerClass** a **{D99F7670-7F1A-11CE-BE57-00AA0051FE20}**.  
  
6.  Reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="switching-back-from-the-previous-english-word-breakers-to-the-current-english-word-breaker"></a>Pasar de los separadores de palabras de inglés anteriores al separador de palabras de inglés actual  
  
#### <a name="to-switch-back-from-the-previous-version-of-the-us-english-word-breaker-to-the-current-version"></a>Para volver a la versión anterior del separador de palabras de inglés de Estados Unidos a la versión actual  
  
1.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID**.  
  
2.  Si las siguientes claves no existen, siga los siguientes pasos para agregar una nueva clave para los ClassID COM de las interfaces inglesas de Estados Unidos de separadores de palabras y lematizadores actuales del LCID 1033:  
  
    1.  Agregue una nueva clave con el valor **{9faed859-0b30-4434-ae65-412e14a16fb8}** para el separador de palabras actual.  
  
    2.  Actualice los datos (predeterminados) de ese valor de clave a **MsWb7.dll**.  
  
    3.  Agregue una nueva clave con el valor **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}** para el lematizador actual.  
  
    4.  Actualice los datos (predeterminados) de ese valor de clave a **MsWb7.dll**.  
  
3.  En el Registro, vaya al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\Language\eng**.  
  
4.  Actualice el valor de clave **WBreakerClass** a **{9faed859-0b30-4434-ae65-412e14a16fb8}**.  
  
5.  Actualice el valor de clave **StemmerClass** a **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}**.  
  
6.  Reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="to-switch-back-from-the-previous-version-of-the-uk-english-word-breaker-to-the-current-version"></a>Para volver a la versión anterior del separador de palabras de inglés del Reino Unido a la versión actual  
  
1.  En el Registro, navegue al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID**.  
  
2.  Si las siguientes claves no existen, siga estos pasos para agregar una nueva clave para los ClassID COM de las interfaces inglesas del Reino Unido de separadores de palabras y lematizadores actuales del LCID 2057:  
  
    1.  Agregue una nueva clave con el valor **{9faed859-0b30-4434-ae65-412e14a16fb8}** para el separador de palabras actual.  
  
    2.  Actualice los datos (predeterminados) de ese valor de clave a **MsWb7.dll**.  
  
    3.  Agregue una nueva clave con el valor **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}** para el lematizador actual.  
  
    4.  Actualice los datos (predeterminados) de ese valor de clave a **MsWb7.dll**.  
  
3.  En el Registro, vaya al siguiente nodo: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\Language\eng**.  
  
4.  Actualice el valor de clave **WBreakerClass** a **{9faed859-0b30-4434-ae65-412e14a16fb8}**.  
  
5.  Actualice el valor de clave **StemmerClass** a **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}**.  
  
6.  Reinicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Consulte también  
 [Revertir los separadores de palabras usados por las búsquedas a la versión anterior](../../relational-databases/search/revert-the-word-breakers-used-by-search-to-the-previous-version.md)   
 [Cambios de comportamiento en la búsqueda de texto completo](./full-text-search.md)  
  

---
description: Configuración del proyecto (conversión) (AccessToSQL)
title: Configuración del proyecto (conversión) (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- conversion, options described
- Project Settings dialog box, Conversion
ms.assetid: bcebc635-c638-4ddb-924c-b9ccfef86388
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a7390c30cd245a3f8a46784ec12e97752173a629
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066490"
---
# <a name="project-settings-conversion-accesstosql"></a>Configuración del proyecto (conversión) (AccessToSQL)
La configuración del proyecto de conversión le permite configurar el modo en que se convierten los objetos de objetos de base de datos de Access en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos o Azure SQL Database.  
  
El panel conversión está disponible en los cuadros de diálogo Configuración del **proyecto** y **configuración predeterminada del proyecto** .  
  
-   Utilice el cuadro de diálogo **configuración del proyecto** para establecer las opciones de configuración del proyecto actual. Para obtener acceso a la configuración de conversión, en el menú **herramientas** , seleccione **configuración del proyecto**, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, seleccione **conversión**.  
  
-   Utilice el cuadro de diálogo **configuración predeterminada del proyecto** para establecer las opciones de configuración de todos los proyectos. Para obtener acceso a la configuración de conversión, en el menú **herramientas** , seleccione **configuración predeterminada del proyecto**, seleccione el tipo de proyecto de migración para el que se deben ver los valores de/Changed en la lista desplegable de la **versión de destino** de la migración, haga clic en **General** en la parte inferior del panel izquierdo y, a continuación, seleccione **conversión**.  
  
## <a name="options"></a>Opciones  
**Agregar clave principal**  
Crea una nueva clave principal en la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tabla o SQL Azure si una tabla de Access no tiene una clave principal o un índice único.  
  
-   **Modo predeterminado**: false  
  
-   **Modo optimista**: false  
  
-   **Modo completo**: true  
  
Cuando se conecta a SQL Azure, es true de forma predeterminada. **Agregar columnas de marca** de tiempo  
Especifica si SSMA debe crear un valor de marca de tiempo si es necesario.  
  
-   **Modo predeterminado**: permitir que SSMA decida  
  
-   **Modo optimista**: nunca  
  
-   **Modo completo**: permitir que SSMA decida  
  
**Incluir un informe de evaluación de datos con informes de evaluación de conversión**  
Incluye una evaluación de datos en el informe de evaluación.  
  
-   **Modo predeterminado**: true  
  
-   **Modo optimista**: false  
  
-   **Modo completo**: true  
  
**Tipo de mensaje cuando una clave principal incluye columnas que aceptan valores NULL**  
Especifica el tipo de mensaje (ADVERTENCIA, error o nada) que SSMA muestra en el panel de salida cuando encuentra las claves principales con columnas que aceptan valores NULL.  
  
-   **Modo predeterminado**: ADVERTENCIA  
  
-   **Modo optimista**: sin mensajes  
  
-   **Modo completo**: error  
  
**Tipo de mensaje cuando las columnas de clave externa tienen tamaños diferentes**  
Especifica el tipo de mensaje (ADVERTENCIA, error o nada) que SSMA muestra en el panel de salida cuando encuentra una clave externa de texto incorrecta.  
  
-   **Modo predeterminado**: ADVERTENCIA  
  
-   **Modo optimista**: sin mensajes  
  
-   **Modo completo**: error  
  
**Tipo de mensaje cuando se indizan las columnas de memorando**  
Especifica el tipo de mensaje (ADVERTENCIA, error o nada) que SSMA muestra en el panel de salida cuando encuentra un índice que contiene una columna de **memorando** .  
  
-   **Modo predeterminado**: ADVERTENCIA  
  
-   **Modo optimista**: sin mensajes  
  
-   **Modo completo**: error  
  
**Advertir cuando una consulta compleja usa un carácter comodín ( \& #42;)**  
Muestra una advertencia en el panel de salida y Lista de errores cuando un nombre de columna en una instrucción SELECT es un carácter comodín (*).  
  
-   **Modo predeterminado**: true  
  
-   **Modo optimista**: false  
  
-   **Modo completo**: true  
  
**Avisar cuando se cambie el nombre del identificador**  
Muestra un mensaje en el informe de evaluación y en el panel de salida cuando SSMA cambia el nombre de un identificador de objeto.  
  
-   **Modo predeterminado**: true  
  
-   **Modo optimista**: false  
  
-   **Modo completo**: true  
  
**Advertir si el identificador se va a entrecomillar**  
Muestra un mensaje en el informe de evaluación y en el panel de salida cuando SSMA va a entrecomillar un nombre de identificador de objeto. Los identificadores de comillas son necesarios cuando el nombre es una palabra clave o contiene caracteres especiales.  
  
-   **Modo predeterminado**: true  
  
-   **Modo optimista**: false  
  
-   **Modo completo**: true  
  
## <a name="see-also"></a>Consulte también  
[Referencia de la interfaz de usuario (acceso)](./user-interface-reference-accesstosql.md)  

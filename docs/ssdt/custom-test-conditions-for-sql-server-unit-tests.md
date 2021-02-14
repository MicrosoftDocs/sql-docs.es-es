---
title: Condiciones de prueba personalizadas para pruebas unitarias de SQL Server
description: Obtenga información sobre cómo instalar condiciones de prueba personalizadas para pruebas unitarias de SQL Server. Vea los riesgos que implica la instalación de condiciones de prueba que no ha creado.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 32a15d61-e908-4ae1-a238-4fd0f988d8c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d7935744497dddce8cb2c9ce75566a9da8e13b42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077737"
---
# <a name="custom-test-conditions-for-sql-server-unit-tests"></a>Condiciones de prueba personalizadas para pruebas unitarias de SQL Server

Pude agregar condiciones de prueba personalizadas para pruebas unitarias de SQL Server. Sin embargo, debe instalar primero la condición de prueba antes de poder usarla, tanto si creó la extensión como si instaló una extensión creada por otra persona.  
  
Antes de instalar una condición de prueba que usted no ha creado, debe entender los riesgos siguientes:  
  
-   El programa de instalación de la condición de prueba puede ser malintencionado y obtener acceso a recursos protegidos según sus permisos de instalación.  
  
-   La propia condición de prueba puede ser malintencionada y obtener control de recursos protegidos si el usuario que emplea la extensión tiene permisos suficientes.  
  
Para minimizar el riesgo, solo debe instalar una condición de prueba personalizada si procede de un origen conocido. Si obtiene una condición de prueba de un origen que no es de confianza, debe inspeccionar el código fuente de la misma y su programa de instalación (si lo tiene) antes de instalarla.  
  
Para instalar una condición de prueba personalizada, copie el ensamblado firmado (.dll) a la carpeta %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions. Si esta carpeta no existe, créela. Necesita privilegios administrativos en el equipo para copiar archivos a este directorio.  
  
> [!NOTE]  
> Deberá instalar las versiones Visual Studio 2010 y Visual Studio 2012 de la condición de prueba si,  
>   
> -   Instala condiciones de prueba personalizadas en un equipo que se puede usar para compilar pruebas unitarias de SQL Server.  
> -   Esas pruebas unitarias se emplean en Visual Studio 2010 y Visual Studio 2012.  
  
Para obtener más información acerca de las condiciones de prueba personalizadas para pruebas unitarias de SQL Server, vea:  
  
-   [Cómo: Crear condiciones de prueba para el Diseñador de pruebas unitarias de SQL Server](../ssdt/how-to-create-test-conditions-for-the-sql-server-unit-test-designer.md)  
  
-   [Cómo: Actualizar una condición de prueba personalizada de Visual Studio 2010 desde una versión anterior a SQL Server Data Tools](../ssdt/how-to-upgrade-visual-studio-2010-custom-test-condition-to-ssdt.md)  
  
-   [Tutorial: Usar una condición de prueba personalizada para comprobar el resultado de un procedimiento almacenado](../ssdt/walkthrough-use-custom-test-condition-to-verify-stored-procedure-results.md)  
  
## <a name="see-also"></a>Consulte también  
[Comprobar el código de base de datos con pruebas unitarias de SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
  

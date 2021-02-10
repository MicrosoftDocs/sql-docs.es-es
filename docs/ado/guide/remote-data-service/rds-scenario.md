---
description: Escenario de RDS
title: Escenario de RDS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: a7dcad87-aaf0-4b02-9660-472f8469761c
author: rothja
ms.author: jroth
ms.openlocfilehash: a61fc0351c7d7b7008ef34d048e66a39908562e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037585"
---
# <a name="rds-scenario"></a>Escenario de RDS
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
 La aplicación de libreta de direcciones es un escenario en el que se muestra cómo usar el servicio de datos remotos (RDS) para crear una sencilla aplicación Web compatible con datos: una libreta de direcciones corporativas en línea. Este escenario es útil para los programadores de Microsoft Visual Basic Scripting Edition (VBScript) y COM que desean obtener información sobre cómo usar los controles ActiveX compatibles con datos con RDS y para los desarrolladores de software más experimentados que desean crear aplicaciones web centradas en datos.  
  
 En este escenario se supone que sabe cómo usar etiquetas de diseño HTML básicas, usar técnicas de enlace de datos DHTML y programar con controles ActiveX.  
  
 Si ha instalado el SDK, el código fuente completo de la aplicación de ejemplo de la libreta de direcciones se puede encontrar en el directorio del SDK en samples\dataaccess\rds\AddressBook\AddressBook.asp. Para ver el escenario de la libreta de direcciones, en Internet Explorer 4,0 o posterior, escriba **https://*WebServer*/RDS/AddressBook/AddressBook.asp** , donde *WebServer* es el nombre asignado al equipo del servidor Web Windows NT 4,0 o Windows 2000 que ejecuta Internet Information Services (IIS) y ASP.  
  
## <a name="introduction-to-address-book"></a>Introducción a la libreta de direcciones  
 La aplicación de ejemplo de libreta de direcciones proporciona una sencilla libreta de direcciones en línea que puede usar para publicar un directorio en el que se pueda buscar a través de una intranet. La libreta de direcciones está diseñada para que un usuario pueda escribir una cadena de búsqueda en uno o varios campos para solicitar información acerca de los empleados. Para mostrar las características básicas del servicio de datos remotos, la aplicación de ejemplo se mantiene involuntariamente pequeña, con un número mínimo de objetos y campos de búsqueda.  
  
 La interfaz de la aplicación consta de las siguientes partes:  
  
-   Un **objeto RDS no visual.** Objeto de enlace de datos del control de datos utilizado por el cliente para conectarse a la base de datos.  
  
-   Cuadros de texto HTML que actúan como campos de entrada para los criterios de búsqueda de atributos de empleado.  
  
-   Botones de comando HTML para generar consultas, borrar campos de búsqueda, actualizar la base de datos con la información de los empleados, cancelar los cambios pendientes y navegar por las filas de datos que se muestran en la cuadrícula.  
  
-   Enlace de datos DHTML para mostrar los datos devueltos por las consultas realizadas en una base de datos back-end (a través de **RDS.** Objeto de enlace de datos DataControl) en una tabla.  
  
-   Rutinas de VBScript que conectan cada uno de los elementos que se han mencionado anteriormente y les permiten interactuar. El código de VBScript también se utiliza para inicializar **RDS. Objeto DataControl** y crear dinámicamente los encabezados de columna en la tabla HTML a partir de los nombres de **RDS.** Campos de conjunto de registros DataControl.  
  
 Siga los vínculos que aparecen en paso a paso para configurar y ejecutar el escenario y obtener más información sobre cómo funciona el escenario.  
  
 Este escenario contiene los siguientes temas.  
  
-   [Requisitos del sistema para la aplicación de la libreta de direcciones](./system-requirements-for-the-address-book-application.md)  
  
-   [Ejecución del script de SQL de la libreta de direcciones](./running-the-address-book-sql-script.md)  
  
-   [Ejecución de la aplicación de ejemplo de la libreta de direcciones](./running-the-address-book-sample-application.md)  
  
-   [Objeto de enlace de datos de la libreta de direcciones](./address-book-data-binding-object.md)  
  
-   [Botones de comando de la libreta de direcciones](./address-book-command-buttons.md)  
  
-   [Botones de navegación de la libreta de direcciones](./address-book-navigation-buttons.md)  
  
## <a name="see-also"></a>Consulte también  
 [Requisitos del sistema para la aplicación de libreta de direcciones](./system-requirements-for-the-address-book-application.md)   
 [Microsoft Objetos de datos ActiveX (ADO)](../../microsoft-activex-data-objects-ado.md)   
 [Aspectos básicos de RDS](./rds-fundamentals.md)   
 [Tutorial de RDS](./rds-tutorial.md)
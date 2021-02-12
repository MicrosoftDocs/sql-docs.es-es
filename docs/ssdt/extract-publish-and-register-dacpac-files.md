---
title: Extraer, publicar y registrar archivos .dacpac
description: Obtenga información sobre las acciones que puede realizar con las aplicaciones de capa de datos (DAC). Entre los ejemplos se incluyen la extracción, publicación y registro de archivos de instantánea (.dacpac).
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.DacTableChooser
- sql.data.tools.DacPublishDialog
- sql.data.tools.DacPropertiesDialog
- sql.data.tools.publishdacproject
- sql.data.tools.DacExtractDialog
ms.assetid: ed900f93-d3df-40f5-8e62-4d722595e041
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: f7d63d6b26d8f5f07b6a4344e84f3d490012c91f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077066"
---
# <a name="extract-publish-and-register-dacpac-files"></a>Extraer, publicar y registrar archivos .dacpac

En este tema se describen cuatro procedimientos que puede realizar haciendo clic con el botón secundario en una base de datos conectada en el Explorador de objetos de SQL Server:  
  
-   Publicar aplicación de capa de datos  
  
-   Extraer aplicación de capa de datos  
  
-   Registrar aplicación de capa de datos  
  
-   Anular registro de aplicación de capa de datos  
  
## <a name="publish-data-tier-application"></a>Publicar aplicación de capa de datos  
Puede publicar un .dacpac en una base de datos. La acción de publicación actualiza de forma incremental un esquema de la base de datos para que coincida con el esquema de un archivo .dacpac de origen. Si no existe la base de datos en el servidor, la operación de publicación la crea.  
  
Esta acción también se puede realizar seleccionando el nodo Bases de datos.  
  
Seleccione el archivo .dacpac. Después de especificar un archivo .dacpac, se habilita el botón **Propiedades de DAC**. El cuadro de diálogo **Propiedades de DAC** muestra información sobre el archivo .dacpac.  
  
Especifique la cadena de conexión con el servidor de bases de datos y, después, especifique el nombre de la base de datos si no está ya en la cadena de conexión.  
  
Los botones **Publicar** y **Generar script** están ahora habilitados. Si genera un script, este se abre en una ventana de documento y se puede guardar cuando sea necesario. Si elige publicar directamente en la base de datos, en la ventana **Operaciones de herramientas de datos** se puede ver un resumen de la actualización y el script real empleado.  
  
Cuando se activa, la casilla **Registrar como aplicación de capa de datos** hace que la base de datos resultante se registre como una aplicación de capa de datos en los metadatos del servidor. Si la base de datos en la que va a publicar está registrada, puede que se produzca un error en la publicación si el esquema de la base de datos es diferente de su archivo dacpac registrado actual.  
  
En el cuadro de diálogo **Configuración de publicación avanzada** hay más opciones de configuración de publicación; se puede obtener acceso a este cuadro de diálogo haciendo clic en el botón **Avanzadas**.  
  
## <a name="extract-data-tier-application"></a>Extraer aplicación de capa de datos  
Puede extraer un archivo .dacpac de una base de datos. La extracción crea un archivo de instantánea de base de datos (.dacpac) a partir de un servidor SQL Server en vivo o una instancia de Azure SQL Database que podría contener datos de tablas de usuario, además del esquema de la base de datos.  
  
Especifique el archivo .dacpac que se va a crear. El botón **Propiedades de DAC** muestra el cuadro de diálogo **Propiedades de DAC**, que le permite especificar propiedades del archivo .dacpac.  
  
Modifique la configuración del proceso de extracción según sea necesario. En la sección **Configuración de extracción**, puede elegir extraer el esquema solamente o extraer el esquema e incluir datos de tabla. Si elige extraer el esquema y los datos, puede seleccionar las tablas cuyos datos desea extraer.  
  
## <a name="register-data-tier-application"></a>Registrar aplicación de capa de datos  
Puede registrar una base de datos como una aplicación de capa de datos (DAC) dentro de la instancia. Esto almacena una representación del estado actual del esquema de la base de datos en los metadatos del sistema.  
  
En el cuadro de diálogo **Registrar aplicación de capa de datos**, especifique las propiedades de la DAC registrada.  
  
## <a name="unregister-data-tier-application"></a>Anular registro de aplicación de capa de datos  
La anulación del registro permite quitar de la instancia los metadatos de una aplicación de capa de datos registrada. La anulación del registro no elimina la base de datos registrada.  
  
## <a name="see-also"></a>Consulte también  
[Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md)  
  

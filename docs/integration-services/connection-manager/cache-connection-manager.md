---
description: Administrador de conexiones de caché
title: Administrador de conexiones de caché | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.cacheconnection.f1
helpviewer_keywords:
- Cache connection manager
ms.assetid: bdc92038-3720-4795-8a5c-79b963f2c952
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fba6ab0ff6c58228f702983b6338c2bd9965e5e7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338441"
---
# <a name="cache-connection-manager"></a>Administrador de conexiones de caché

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El administrador de conexiones de caché lee datos a partir de la transformación de caché o de un archivo caché (.caw) y puede guardar los datos en un archivo caché. Si configura el administrador de conexiones de caché para que utilice un archivo caché, los datos siempre se almacenan en memoria.  
  
 La transformación de caché escribe los datos procedentes de un origen de datos conectado del flujo de datos en un administrador de conexiones de caché. La transformación Búsqueda de un paquete realiza búsquedas en los datos.  
  
> [!NOTE]  
>  El administrador de conexiones de caché no admite los tipos de datos de objetos binarios grandes (BLOB) DT_TEXT, DT_NTEXT y DT_IMAGE. Si el conjunto de datos de referencia contiene un tipo de datos BLOB, se producirá un error en el componente al ejecutar el paquete. Puede utilizar el **Editor del administrador de conexiones de caché** para modificar los tipos de datos de columna. Para obtener más información, vea [Cache Connection Manager Editor]().  
  
> [!NOTE]  
>  El nivel de protección del paquete no se aplica al archivo caché. Si el archivo caché contiene información confidencial, utilice una lista de control de acceso (ACL) para restringir el acceso a la ubicación o carpeta en la que almacena el archivo. Solo debería permitir el acceso a ciertas cuentas. Para más información, vea [Acceso a los archivos usados por los paquetes](../../integration-services/security/security-overview-integration-services.md#files).  
  
## <a name="configuration-of-the-cache-connection-manager"></a>Configuración del administrador de conexiones de caché  
 Puede configurar el administrador de conexiones de caché de las siguientes maneras:  
  
-   Indicar si se ha de utilizar un archivo caché.  
  
     Si configura el administrador de conexiones de caché para utilizar un archivo caché, el administrador de conexiones realizará una de las siguientes acciones:  
  
    -   Guardar los datos en el archivo cuando se haya configurado una transformación de caché para escribir los datos procedentes de un origen de datos del flujo de datos en el administrador de conexiones de caché.  
  
    -   Leer los datos desde el archivo caché.  
  
     Para obtener más información, vea [Cache Transform](../../integration-services/data-flow/transformations/cache-transform.md).  
  
-   Cambiar los metadatos de las columnas almacenadas en la caché.  
  
-   Actualizar el nombre del archivo caché en tiempo de ejecución con una expresión para establecer la propiedad ConnectionString. Para más información, vea [Usar expresiones de propiedad en paquetes](../../integration-services/expressions/use-property-expressions-in-packages.md).  
  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] o mediante programación.  
  
 Para obtener más información sobre cómo configurar un administrador de conexiones mediante programación, vea <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> y [Agregar conexiones mediante programación](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="cache-connection-manager-editor"></a>Editor del administrador de conexiones de caché
  El administrador de conexiones de caché lee un conjunto de datos de referencia a partir de la transformación de caché o de un archivo caché (.caw) y puede guardar los datos en un archivo caché. Los datos siempre se almacenan en memoria.  
  
> [!NOTE]  
>  El administrador de conexiones de caché no admite los tipos de datos de objetos binarios grandes (BLOB) DT_TEXT, DT_NTEXT y DT_IMAGE. Si el conjunto de datos de referencia contiene un tipo de datos BLOB, se producirá un error en el componente al ejecutar el paquete. Puede utilizar el **Editor del administrador de conexiones de caché** para modificar los tipos de datos de columna.  
  
 La transformación de Búsqueda realiza búsquedas en el conjunto de datos de referencia.  
  
 En el cuadro de diálogo **Editor del administrador de conexiones de caché** se incluyen estas pestañas:  
  
###  <a name="general-tab"></a><a name="generaltab"></a> Pestaña General  
 Use la pestaña **General** del cuadro de diálogo **Editor del administrador de conexiones de caché** para indicar si la memoria caché se leerá desde un archivo o se guardará en un archivo.  
  
#### <a name="options"></a>Opciones  
 **Nombre del administrador de conexiones**  
 Especifique un nombre único para la conexión de caché en el flujo de trabajo. El nombre que indique se mostrará en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Descripción**  
 Describe la conexión. Como método recomendado, describa la conexión en función de su propósito, para que los paquetes estén autodocumentados y sean más fáciles de mantener.  
  
 **Utilizar la caché del archivo.**  
 Indicar si se ha de utilizar un archivo caché.  
  
> [!NOTE]  
>  El nivel de protección del paquete no se aplica al archivo caché. Si el archivo caché contiene información confidencial, utilice una lista de control de acceso (ACL) para restringir el acceso a la ubicación o carpeta en la que almacena el archivo. Solo debería permitir el acceso a ciertas cuentas. Para más información, vea [Acceso a los archivos usados por los paquetes](../../integration-services/security/security-overview-integration-services.md#files).  
  
 Si configura el administrador de conexiones de caché para utilizar un archivo caché, el administrador de conexiones realizará una de las siguientes acciones:  
  
-   Guardar los datos en el archivo cuando se haya configurado una transformación de caché para escribir los datos procedentes de un origen de datos del flujo de datos en el administrador de conexiones de caché. Para obtener más información, vea [Cache Transform](../../integration-services/data-flow/transformations/cache-transform.md).  
  
-   Leer los datos desde el archivo caché.  
  
 **Nombre de archivo**  
 Escriba la ruta y el nombre de archivo del archivo caché.  
  
 **Browse**  
 Busque el archivo caché.  
  
 **Actualizar los metadatos**  
 Elimine los metadatos de columna del administrador de conexiones de caché y vuelva a llenar el administrador de conexiones de caché con los metadatos de la columna del archivo caché seleccionado.  
  
###  <a name="columns-tab"></a><a name="columnstab"></a> Pestaña Columnas  
 Utilice la pestaña **Columnas** del cuadro de diálogo **Editor del administrador de conexiones de caché** para configurar las propiedades de cada columna en la caché.  
  
#### <a name="options"></a>Opciones  
 **Columna**  
 Especifique el nombre de la columna.  
  
 **Posición de índice**  
 Define qué columnas son columnas de índice especificando la posición de índice de cada columna. El índice es una colección de una o varias columnas.  
  
 Para las columnas de no índice, la posición de índice es 0.  
  
 Para las columnas de índice, la posición de índice es un número secuencial positivo. Este número indica el orden en el que la transformación Búsqueda compara las filas del conjunto de datos de referencia con las filas del origen de datos de entrada. La columna con el mayor número de valores únicos debería tener la posición de índice más pequeña.  
  
> [!NOTE]  
>  Cuando la transformación Búsqueda se configura para utilizar un administrador de conexiones de caché, a las columnas de entrada solo se les puede asignar las columnas de índice del conjunto de datos de referencia. Asimismo, todas las columnas de índice deben estar asignadas.  
  
 **Tipo**  
 Especifica el tipo de datos de la columna.  
  
 **Longitud**  
 Especifica el tipo de datos de la columna. Si procede en el caso del tipo de datos, puede actualizar la **Length**.  
  
 **Precisión**  
 Especifica la precisión para cierto tipo de datos de columna. La precisión es el número de dígitos de un número. Si procede en el caso del tipo de datos, puede actualizar la **Precision**.  
  
 **Escala**  
 Especifica la escala para cierto tipo de datos de columna. La escala es el número de dígitos situados a la derecha de la coma decimal de un número. Si procede en el caso del tipo de datos, puede actualizar la **Scale**.  
  
 **Página de códigos**  
 Especifica la página de códigos para el tipo de columna. Si procede en el caso del tipo de datos, puede actualizar la **Code Page**.  
  
## <a name="related-tasks"></a>Related Tasks  
 [Implementar una transformación de búsqueda en el modo de caché completa mediante el Administrador de conexiones de caché](lookup-transformation-full-cache-mode-cache-connection-manager.md)  
  

---
title: Extensión Schema Compare
description: Obtenga información sobre cómo instalar y usar la extensión Comparación de esquemas de Azure Data Studio para comparar fácilmente dos bases de datos y cambiar una de forma selectiva para que coincida con la otra.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: 88f96d75150fc1b3e69d9cf158a50e02110dbae0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048482"
---
# <a name="schema-compare-extension"></a>Extensión Schema Compare

La extensión Comparación de esquemas proporciona una experiencia fácil de usar para comparar dos definiciones de bases de datos y aplicar las diferencias del origen al destino.

## <a name="features"></a>Características

* Comparar esquemas de dos archivos o bases de datos dacpac.
* Ver los resultados como un conjunto de acciones que deben tomarse en el destino para que coincida con el origen.
* Excluir selectivamente las acciones enumeradas en los resultados.
* Establecer las opciones que controlan el ámbito de la comparación.
* Aplicar los cambios al destino o generar un script con el mismo efecto.
* Guardar la comparación.

![Comparación de esquemas: Comparación de ejemplo](media/schema-compare-extension/schema-compare.png)

## <a name="why-would-i-use-the-schema-compare-extension"></a>¿Por qué debo usar la extensión Comparación de esquemas?

Puede resultar tedioso administrar y sincronizar manualmente versiones distintas de base de datos. La extensión Comparación de esquemas simplifica el proceso de comparación de las bases de datos y proporciona control total al sincronizarlas &mdash; puede filtrar selectivamente las diferencias y categorías de diferencias concretas antes de aplicar los cambios. La extensión Comparación de esquemas es una herramienta de confianza que le permite ahorrar tiempo y código.

![Comparación de esquemas: Cuadro de diálogo Opciones](media/schema-compare-extension/schema-compare-options.png)

## <a name="install-the-extension"></a>Instalación de la extensión

1. Seleccione el icono de extensiones para ver las extensiones disponibles.

    ![Icono del Administrador de extensiones](media/add-extensions/extension-manager-icon.png)

2. Busque la extensión **Comparación de esquemas** y selecciónela para ver los detalles. Seleccione **Instalar** para agregar la extensión.

3. Una vez instalada, seleccione **Recargar** para habilitar la extensión en Azure Data Studio (solo es necesario cuando se instala una extensión por primera vez).

## <a name="launch-a-schema-compare"></a>Inicio de una Comparación de esquemas

1. Para abrir el cuadro de diálogo de Comparación de esquemas, **haga clic con el botón derecho** en una base de datos en el Explorador de objetos y seleccione **Comparación de esquemas**. La base de datos seleccionada se establece como la base de datos de origen en la comparación.

    ![menú de inicio de la Comparación de esquemas](media/schema-compare-extension/schema-compare-launch.png)

2. Seleccione uno de los puntos suspensivos (...) para cambiar el origen y el destino de Comparación de esquemas y seleccione Aceptar.

    ![selección de origen y destino de la Comparación de esquemas](media/schema-compare-extension/schema-compare-select-source-target.png)

3. Para personalizar la comparación, seleccione el botón **Opciones** de la barra de herramientas.

4. Seleccione **Comparar** para ver los resultados de la comparación.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Comparación de esquemas, consulte [Uso de Comparación de esquemas para comparar distintas definiciones de base de datos](../../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md).
Puede notificar cualquier problema y solicitar nuevas características [aquí](https://github.com/microsoft/azuredatastudio/issues).
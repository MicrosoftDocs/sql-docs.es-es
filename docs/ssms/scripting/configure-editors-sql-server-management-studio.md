---
title: Configurar editores (SQL Server Management Studio)
description: Obtenga información sobre cómo personalizar el funcionamiento de los editores de SQL Server Management Studio mediante el establecimiento de las opciones del cuadro de diálogo Opciones.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: e7c7a8ef-f561-4258-a7b6-c445dba69f87
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 33ddc2599ae030e61059c23a462fb68f9f814966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354743"
---
# <a name="configure-editors-sql-server-management-studio"></a>Configurar editores (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

El funcionamiento de los editores de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] puede personalizarse configurando las opciones de cada editor.  
  
## <a name="setting-editor-options"></a>Configuración de las opciones del editor  
 La mayoría de las opciones del editor se establecen a través del menú **Herramientas**, donde se selecciona **Opciones…** para mostrar un cuadro de diálogo **Opciones**. En el cuadro de diálogo **Opciones** , abra el nodo de **Editor de texto** en el panel izquierdo para configurar las opciones de edición de texto y código. Los nodos situados bajo Editor de texto se corresponden con editores concretos:  
  
1.  **Todos los lenguajes**: las opciones que se establecen a través de este nodo se aplican a todos los editores de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Puede invalidar estos valores estableciendo opciones distintas para un editor concreto en los demás nodos.  
  
2.  **Texto sin formato**: las opciones que se establecen a través de este nodo se aplican a editores MDX, DMX y de texto.  
  
3.  **Transact-SQL**: las opciones que se establecen a través de este nodo se aplican al editor de consultas del motor de base de datos.  
  
4.  **XML**: las opciones que se establecen a través de este nodo se aplican al editor de XML for Analysis.  
  
 Abra los nodos **Ejecución de la consulta** o **Resultados de la consulta** para personalizar la ejecución de consultas y el modo en que se muestran los resultados.  
  
## <a name="editor-configuration-tasks"></a>Tareas de configuración del editor  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Describe cómo se especifica que un editor debe abrirse haciendo doble clic en un archivo con la extensión especificada en el Explorador de Windows.|[Asociar extensiones de archivo a un editor de código](./associate-file-extensions-to-a-code-editor.md)|  
|Describe cómo se personalizan las fuentes para mejorar la legibilidad del código y del texto.|[Cambiar el color, el tamaño y el estilo de la fuente](./change-font-color-size-and-style.md)|  
|Describe cómo se consultan las propiedades.|[Usar la ventana Propiedades en Management Studio](./use-the-properties-window-in-management-studio.md)|  
|Ubicación de las páginas de Ayuda F1 de los cuadros de diálogo de opciones de editores.|[Páginas de Opciones de consulta (Ayuda F1)](../f1-help/f1-help-for-server-connections-sql-server-management-studio.md)|
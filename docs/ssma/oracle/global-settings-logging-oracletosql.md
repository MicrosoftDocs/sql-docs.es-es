---
description: Configuración global (registro) (OracleToSQL)
title: Configuración global (registro) (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 12dbcd77-2b90-4fa1-9cf9-239231ea5773
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: c6b19ddbe9ab7e1bf1e8f88f5c0c4262fa326c56
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068018"
---
# <a name="global-settings-logging-oracletosql"></a>Configuración global (registro) (OracleToSQL)
Utilice el cuadro de diálogo **configuración global** para especificar la configuración de registro para SSMA. Normalmente, esta configuración solo se puede cambiar cuando se trabaja con soporte técnico del producto.  
  
Para obtener acceso a este cuadro de diálogo, en el menú **herramientas** , seleccione **configuración global** y, a continuación, haga clic en el botón **registro** situado en la parte inferior del panel izquierdo.  
  
## <a name="options"></a>Opciones  
**Nivel de mensajes**  
Las siguientes opciones están disponibles en el **nivel de mensajes**:  
  
|Opción|Descripción|  
|----------|---------------|  
|**[todas las categorías]**|Se usa para establecer el nivel de registro para todas las opciones siguientes.|  
|**Recopilador**|Recopila metadatos sobre el esquema de origen y los guarda en el proyecto.|  
|**Converter**|Convierte estructuras de objetos de base de datos de origen, como tablas y procedimientos almacenados, en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] estructuras correspondientes.|  
|**Data Migrator**|Migra datos de la base de datos de origen a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Formateador**|Subcomponente del convertidor que genera scripts para el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] esquema.|  
|**Interfaz gráfica de usuario**|Mensajes que aparecen cuando se usa la herramienta SSMA.|  
|**Enlazador**|Resuelve los identificadores de SQL y proporciona información a otros componentes.|  
|**Otros**|Todos los mensajes que no están en ninguna otra categoría.|  
|**Analizador**|Analiza el esquema de origen.|  
|**Sincronizador**|Carga los objetos de base de datos de origen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**TreeConverter**|Convierte los objetos de los metadatos de origen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] metadatos.|  
|**Evaluador**|Mensajes que aparecen al usar el evaluador de SSMA.|  
  
Para cada opción en **nivel de mensajes**, configure uno de los siguientes niveles de registro para SSMA:  
  
|||  
|-|-|  
|**Error irrecuperable**|Escriba solo mensajes de error irrecuperables en el registro.|  
|**Error**|Escriba mensajes de error y de error grave en el registro.|  
|**Advertencia**|Escriba mensajes de advertencia, error y error grave en el registro.|  
|**Información**|Escriba mensajes informativos, de advertencia, de error y de error grave en el registro.|  
|**Depurar**|Escriba todos los mensajes, incluidos los mensajes de depuración, en el registro.|  
  
**Ruta de acceso al archivo de registro**  
La ruta de acceso del archivo y el nombre de los archivos de registro de SSMA. Para especificar otro nombre, haga clic en la ruta de acceso actual y, a continuación, haga clic en el botón Examinar (**...**).  
  
**Tamaño de archivo de registro**  
Tamaño máximo del archivo de registro en KB. El tamaño mínimo es de 10 KB. El tamaño predeterminado es 10240 KB.  
  
**Número total de archivos de registro**  
Cuando se llena un registro, SSMA cambiará el nombre del archivo de registro y se iniciará uno nuevo. Con esta opción, especifique el número máximo de archivos de registro que se deben conservar. El mínimo es 2.  
  

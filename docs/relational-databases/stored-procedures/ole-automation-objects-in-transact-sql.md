---
title: Objetos de automatización OLE en Transact-SQL | Microsoft Docs
description: Obtenga información sobre la manera en que los objetos de automatización OLE que se ejecutan a través de procedimientos almacenados se ejecutan en el espacio de direcciones de una instancia del Motor de base de datos de SQL Server.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
helpviewer_keywords:
- triggers [SQL Server], OLE Automation
- batches [SQL Server], OLE Automation
- OLE Automation [SQL Server]
- OLE Automation [SQL Server], about OLE Automation
ms.assetid: a887d956-4cd0-400a-aa96-00d7abd7c44b
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1e20fc91b1276a2a7bd8e263c18d6a9da1786ba9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482956"
---
# <a name="ole-automation-objects-in-transact-sql"></a>Objetos de automatización OLE en Transact-SQL
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [!INCLUDE[tsql](../../includes/tsql-md.md)] incluye varios procedimientos almacenados del sistema que permiten hacer referencia a objetos de automatización OLE en los lotes, procedimientos almacenados y desencadenadores de [!INCLUDE[tsql](../../includes/tsql-md.md)] . Estos procedimientos almacenados del sistema se ejecutan como procedimientos almacenados extendidos, y los objetos de automatización OLE que se ejecutan a través de los procedimientos almacenados lo hacen en el espacio de direcciones de una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] de la misma forma que un procedimiento almacenado extendido.  
  
 Los procedimientos almacenados de OLE Automation permiten que los lotes [!INCLUDE[tsql](../../includes/tsql-md.md)] hagan referencia a los objetos SQL-DMO y a los objetos de OLE Automation personalizados, como los objetos que exponen la interfaz **IDispatch**. Un servidor OLE personalizado en proceso creado con [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] debe tener un controlador de errores (especificado con la instrucción **On Error GoTo**) para las subrutinas **Class_Initialize** y **Class_Terminate**. Los errores sin controlar de las subrutinas **Class_Initialize** y **Class_Terminate** pueden generar errores impredecibles, como una infracción de acceso en una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Se recomienda también disponer de identificadores de errores para otras subrutinas.  
  
 El primer paso a la hora de utilizar un objeto de OLE Automation en [!INCLUDE[tsql](../../includes/tsql-md.md)] es llamar al procedimiento almacenado del sistema **sp_OACreate** para crear una instancia del objeto en el espacio de direcciones del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 Una vez creada una instancia del objeto, llame a los siguientes procedimientos almacenados para trabajar con las propiedades, los métodos y la información de error relacionada con el objeto:  
  
-   **sp_OAGetProperty** obtiene el valor de una propiedad.  
  
-   **sp_OASetProperty** establece el valor de una propiedad.  
  
-   **sp_OAMethod** llama a un método.  
  
-   **sp_OAGetErrorInfo** obtiene la información de errores más reciente.  
  
 Cuando el objeto deje de ser necesario, llame a **sp_OADestroy** para cancelar la asignación de la instancia del objeto creado con **sp_OACreate**.  
  
 Los objetos de automatización OLE devuelven datos a través de valores y métodos de propiedad. **sp_OAGetProperty** y **sp_OAMethod** devuelven estos valores de datos en la forma de un conjunto de resultados.  
  
 El ámbito de un objeto de OLE Automation se limita a un lote. Todas las referencias al objeto deben estar contenidas en un único lote, en un procedimiento almacenado o en un desencadenador.  
  
 Cuando se hace referencia a objetos, los objetos de automatización OLE de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permiten atravesar el objeto referenciado para llegar hasta otros objetos contenidos en él. Por ejemplo, cuando se usa el objeto **SQLServer** de SQL-DMO, se puede hacer referencia a las bases de datos y tablas contenidas en ese servidor.  
  
## <a name="related-content"></a>Contenido relacionado  
 [Sintaxis de jerarquía de objetos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/object-hierarchy-syntax-transact-sql.md)  
  
 [Configuración de Área expuesta](../../relational-databases/security/surface-area-configuration.md)  
  
 [Ole Automation Procedures (opción de configuración del servidor)](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)  
  
 [sp_OACreate &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oacreate-transact-sql.md)  
  
 [sp_OAGetProperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oagetproperty-transact-sql.md)  
  
 [sp_OASetProperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oasetproperty-transact-sql.md)  
  
 [sp_OAMethod &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oamethod-transact-sql.md)  
  
 [sp_OAGetErrorInfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oageterrorinfo-transact-sql.md)  
  
 [sp_OADestroy &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-oadestroy-transact-sql.md)  
  
  

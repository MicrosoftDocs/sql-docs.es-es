---
description: MSSQLSERVER_2501
title: MSSQLSERVER_2501 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2501 (Database Engine error)
ms.assetid: 895aafe3-a4e7-4ed8-acc5-93be76ef3664
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3f5f698928b3df5abc1c927ec926b24e2ba853f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208475"
---
# <a name="mssqlserver_2501"></a>MSSQLSERVER_2501
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|2501|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBCC_NO_SUCH_TABLE_NAME|  
|Texto del mensaje|No se encuentra una tabla o un objeto con el nombre 'NAME'. Compruebe el catálogo del sistema.|  
  
## <a name="explanation"></a>Explicación  
No se encuentra el objeto especificado.  
  
### <a name="possible-causes"></a>Causas posibles  
Este error puede deberse a uno de los siguientes problemas:  
  
-   No se ha especificado correctamente el objeto.  
  
-   El objeto no existe o se ha eliminado antes de que una instrucción intentara utilizarlo.  
  
-   Es posible que el objeto exista, pero no ha podido mostrarse al usuario. Por ejemplo, puede que el usuario no tenga permisos para el objeto o que el objeto sea una tabla interna que no puede ver un usuario.  
  
## <a name="user-action"></a>Acción del usuario  
  
-   Compruebe que el contexto de la base de datos activa sea correcto. Para obtener más información, vea [USE &#40;Transact-SQL&#41;](~/t-sql/language-elements/use-transact-sql.md).  
  
-   Compruebe que el nombre de la tabla o del objeto esté correctamente escrito.  
  
-   Compruebe el nombre del esquema que contiene el objeto. Si el objeto pertenece a un esquema que no sea el predeterminado (**dbo**), debe especificar el nombre de la tabla o del objeto usando el formato de dos partes *nombreDeEsquema.nombreDeObjeto*.  
  
-   Compruebe que el objeto exista en las tablas del sistema. Para comprobar si existe una tabla u otro objeto del ámbito de esquema, vea la vista de catálogo [sys.objects](~/relational-databases/system-catalog-views/sys-objects-transact-sql.md). Si el objeto no está en las tablas del sistema, significa que se ha eliminado o que el usuario no tiene permisos para ver los metadatos del objeto. Para obtener más información sobre cómo ver los metadatos de objeto, vea [Configuración de visibilidad de los metadatos](~/relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
[Vistas de catálogo &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  

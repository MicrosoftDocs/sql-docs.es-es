---
description: catalog.check_schema_version
title: catalog.check_schema_version | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e0d5e9f5-59c6-4118-87b5-4aa5c37a7df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aa47e5107551ec2d4ded0832ff52922ffb4bfd55
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247525"
---
# <a name="catalogcheck_schema_version"></a>catalog.check_schema_version 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Determina si el esquema del catálogo de SSISDB y los binarios de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (ensamblado de ISServerExec y SQLCLR) son compatibles.  
  
 ISServerExec.exc registra un mensaje de error cuando el esquema y los archivos binarios son incompatibles.  
  
 La versión de esquema de SSISDB aumenta cuando el esquema se modifica durante la aplicación de revisiones y durante las actualizaciones. Se recomienda ejecutar este procedimiento almacenado después haber restaurado una copia de seguridad de SSISDB para asegurarse de que el esquema y los archivos binarios son compatibles.  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
catalog.check_schema_version [ @use32bitruntime = ] use32bitruntime  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @use32bitruntime= ] *use32bitruntime*  
 Cuando el parámetro se establece en **1**, se llama a la versión de 32 bits de dtexec. *use32bitruntime* es de tipo **int**.  
  
 
## <a name="return-code-value"></a>Valor de código de retorno 
Devuelve 0 si el resultado es correcto. 

## <a name="result-set"></a>Tipo de cursor  

Devuelve una tabla que tiene el siguiente formato:

| Nombre de la columna | Tipo de datos | Descripción |
|---|---|---|
| SERVER_BUILD | **decimal** | Versión de SQL Server. Por ejemplo, un servidor que ejecuta SQL Server 2014 es `14.0.3335.7`. |
| SCHEMA_VERSION | **tinyint** | Número de versión de SQL Server. Por ejemplo, SQL Server 2017 y 2019 son `6` y `7` respectivamente.|
| SCHEMA_BUILD | **string** | Compilación del esquema. |
| ASSEMBLY_BUILD | **string** | Compilación del ensamblado. |
| SHARED_COMPONENT_VERSION | **string** | Versión del componente compartido. | 

## <a name="permissions"></a>Permisos  
 Este procedimiento almacenado necesita el siguiente permiso:  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
  

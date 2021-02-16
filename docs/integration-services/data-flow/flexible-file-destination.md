---
description: Destino de archivo flexible
title: Destino de archivo flexible | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpextfiledest.f1
- sql14.dts.designer.afpextfiledest.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1899538413552e0381f87dd997356e885e435b58
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339753"
---
# <a name="flexible-file-destination"></a>Destino de archivo flexible

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

El componente de **destino de archivo flexible** permite que un paquete SSIS escriba datos en diversos servicios de almacenamiento compatibles.

Los servicios de almacenamiento admitidos actualmente son:

- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-introduction)
   
Arrastre y coloque el **destino de archivo flexible** en el diseñador de flujos de datos y haga doble clic en él para ver el editor.
  
**Destino de archivo flexible** es un componente del [Feature Pack de SQL Server Integration Services (SSIS) para Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  

Las propiedades siguientes están disponibles en el **Editor de destino de archivo flexible**.

- **Tipo de administrador de conexiones de archivos:** especifica el tipo del administrador de conexiones de origen. Luego elija uno existente del tipo especificado o cree uno.
- **Ruta de acceso a la carpeta:** especifica la ruta de acceso a la carpeta de destino.
- **Nombre de archivo:** especifica el nombre de archivo de destino.
- **Formato de archivo:** especifica el formato de archivo de destino. Los formatos admitidos son **Texto**, **Avro**, **ORC** y **Parquet**. Se necesita Java para ORC/Parquet. Vea [aquí](../../integration-services/azure-feature-pack-for-integration-services-ssis.md#dependency-on-java) para obtener más detalles.
- **Carácter delimitador de columna:** especifica el carácter que se va a usar como delimitador de columna (no se admiten delimitadores de varios caracteres).
- **Primera fila como nombre de columna:** especifica si se escriben nombres de columna en la primera fila.
- **Comprimir el archivo:** especifica si se comprime el archivo.
- **Tipo de compresión:** especifica el formato de compresión que se va a usar. Los formatos admitidos son **GZIP**, **DEFLATE** y **BZIP2**.
- **Nivel de compresión:** especifica el nivel de compresión que se va a usar.

Las propiedades siguientes están disponibles en el **Editor avanzado**.

- **rowDelimiter:** carácter utilizado para separar filas en un archivo. Solo se permite un carácter. El valor **predeterminado** es \r\n.
- **escapeChar:** carácter especial utilizado para aplicar una secuencia de escape a un delimitador de columna en el contenido de un archivo de entrada. No puede especificar ambos valores escapeChar y quoteChar para una tabla. Solo se permite un carácter. Ningún valor predeterminado.
- **quoteChar:** carácter utilizado para citar el valor de una cadena. Los delimitadores de columna y fila de dentro de las comillas se tratan como parte del valor de la cadena. Esta propiedad es aplicable tanto al conjunto de datos de entrada como al de salida. No puede especificar ambos valores escapeChar y quoteChar para una tabla. Solo se permite un carácter. Ningún valor predeterminado.
- **nullValue:** uno o varios caracteres empleados para representar un valor nulo. El valor **predeterminado** es \N.
- **encodingName:** permite especificar el nombre de la codificación. Consulte la propiedad [Encoding.EncodingName](/dotnet/api/system.text.encoding).
- **skipLineCount:**  permite indicar el número de filas no vacías que hay que omitir al leer datos de archivos de entrada. Si se especifican ambos valores skipLineCount y firstRowAsHeader, las líneas se omiten primero y, luego, la información del encabezado se lee a partir del archivo de entrada.
- **treatEmptyAsNull:** permite especificar si hay que tratar una cadena nula o vacía como un valor nulo al leer datos de un archivo de entrada. El valor **predeterminado** es true.

Después de especificar la información de conexión, cambie a la página **Columnas** para asignar columnas de origen a columnas de destino para el flujo de datos SSIS.

**Notas sobre la configuración de permisos de la entidad de servicio**

Para que la **conexión de prueba** funcione (ya sea Blob Storage o Data Lake Storage Gen2), se debe asignar al menos el rol **Lector de datos de Storage Blob** a la entidad de servicio en la cuenta de almacenamiento.
Esto se hace con [RBAC](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal).

En el caso del almacenamiento de blobs, el permiso de escritura se concede mediante la asignación de al menos el rol **Colaborador de datos de Storage Blob**.

Para Data Lake Storage Gen2, el permiso lo determina RBAC y las [ACL](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer).
Preste atención a que las ACL se configuran mediante el identificador de objeto (OID) de la entidad de servicio para el registro de la aplicación como se detalla [aquí](/azure/storage/blobs/data-lake-storage-access-control#how-do-i-set-acls-correctly-for-a-service-principal).
Esto es diferente del identificador de aplicación (cliente) que se usa con la configuración de RBAC.
Cuando a una entidad de seguridad se le conceden permisos de datos RBAC a través de un rol integrado, o bien a través de un rol personalizado, estos permisos se evalúan primero tras la autorización de una solicitud.
Si la operación solicitada está autorizada por las asignaciones RBAC de la entidad de seguridad, la autorización se resuelve inmediatamente y no se realiza ninguna comprobación adicional de la ACL.
Como alternativa, si la entidad de seguridad no tiene una asignación RBAC, o la operación de la solicitud no coincide con el permiso asignado, se realizan comprobaciones de ACL para determinar si la entidad de seguridad está autorizada para realizar la operación solicitada.
Para el permiso de escritura, conceda al menos el permiso **Execute** a partir del sistema de archivos de receptor, junto con el permiso **Write** para la carpeta de receptor.
Como alternativa, conceda al menos el rol **Colaborador de datos de Storage Blob** con RBAC.
Vea [este](/azure/storage/blobs/data-lake-storage-access-control) artículo para obtener más información.
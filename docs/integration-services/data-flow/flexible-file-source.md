---
description: Origen de archivo flexible
title: Origen de archivo flexible | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpextfilesrc.f1
- sql14.dts.designer.afpextfilesrc.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 230ada5b116e5789b008a1562ba5e2ba9325a9e0
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123456"
---
# <a name="flexible-file-source"></a>Origen de archivo flexible

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

El componente de **origen de archivo flexible** permite que un paquete SSIS lea datos en diversos servicios de almacenamiento compatibles.
Los servicios de almacenamiento admitidos actualmente son:

- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-introduction)
  
Para ver el editor del origen de archivo flexible, arrastre y coloque el **origen de archivo flexible** en el diseñador de flujos de datos y haga doble clic en él para abrir el editor.
  
[Tarea de archivo flexible](../../integration-services/azure-feature-pack-for-integration-services-ssis.md) es un componente del **Feature Pack de SQL Server Integration Services (SSIS) para Azure**.  
  
Las propiedades siguientes están disponibles en el **Editor de origen de archivo flexible**.

- **Tipo de administrador de conexiones de archivos:** especifica el tipo del administrador de conexiones de origen. Luego elija uno existente del tipo especificado o cree uno.
- **Ruta de acceso a la carpeta:** especifica la ruta de acceso a la carpeta de origen.
- **Nombre de archivo:** especifica el nombre de archivo de origen.
- **Formato de archivo:** especifica el formato de archivo de origen. Los formatos admitidos son **Texto**, **Avro**, **ORC** y **Parquet**. Se necesita Java para ORC/Parquet. Vea [aquí](../../integration-services/azure-feature-pack-for-integration-services-ssis.md#dependency-on-java) para obtener más detalles.
- **Carácter delimitador de columna:** especifica el carácter utilizado como delimitador de columna (no se admiten delimitadores de varios caracteres).
- **Primera fila como nombre de columna:** especifica si se debe tratar la primera línea como nombres de columna.
- **Descomprimir el archivo:** especifica si se descomprime el archivo de origen.
- **Tipo de compresión:** especifica el formato de compresión de archivo de origen. Los formatos admitidos son **GZIP**, **DEFLATE** y **BZIP2**.
  
Las propiedades siguientes están disponibles en el **Editor avanzado**.

- **rowDelimiter:** carácter utilizado para separar filas en un archivo. Solo se permite un carácter. El valor **predeterminado** es \r\n.
- **escapeChar:** carácter especial utilizado para aplicar una secuencia de escape a un delimitador de columna en el contenido de un archivo de entrada. No puede especificar ambos valores escapeChar y quoteChar para una tabla. Solo se permite un carácter. Ningún valor predeterminado.
- **quoteChar:** carácter utilizado para citar el valor de una cadena. Los delimitadores de columna y fila de dentro de las comillas se tratan como parte del valor de la cadena. Esta propiedad es aplicable tanto al conjunto de datos de entrada como al de salida. No puede especificar ambos valores escapeChar y quoteChar para una tabla. Solo se permite un carácter. Ningún valor predeterminado.
- **nullValue:** uno o varios caracteres empleados para representar un valor nulo. El valor **predeterminado** es \N.
- **encodingName:** permite especificar el nombre de la codificación. Consulte la propiedad [Encoding.EncodingName](/dotnet/api/system.text.encoding?view=netframework-4.8).
- **skipLineCount:**  permite indicar el número de filas no vacías que hay que omitir al leer datos de archivos de entrada. Si se especifican ambos valores skipLineCount y firstRowAsHeader, las líneas se omiten primero y, luego, la información del encabezado se lee a partir del archivo de entrada.
- **treatEmptyAsNull:** permite especificar si hay que tratar una cadena nula o vacía como un valor nulo al leer datos de un archivo de entrada. El valor **predeterminado** es true.

Después de especificar la información de conexión, cambie a la página **Columnas** para asignar columnas de origen a columnas de destino para el flujo de datos SSIS.

**Notas sobre la configuración de permisos de la entidad de servicio**

Para que la **conexión de prueba** funcione (ya sea Blob Storage o Data Lake Storage Gen2), se debe asignar al menos el rol **Lector de datos de Storage Blob** a la entidad de servicio en la cuenta de almacenamiento.
Esto se hace con [RBAC](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal).

En el caso del almacenamiento de blobs, el permiso de lectura se concede mediante la asignación de al menos el rol **Lector de datos de Storage Blob**.

Para Data Lake Storage Gen2, el permiso lo determina RBAC y las [ACL](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer).
Preste atención a que las ACL se configuran mediante el identificador de objeto (OID) de la entidad de servicio para el registro de la aplicación como se detalla [aquí](/azure/storage/blobs/data-lake-storage-access-control#how-do-i-set-acls-correctly-for-a-service-principal).
Esto es diferente del identificador de aplicación (cliente) que se usa con la configuración de RBAC.
Cuando a una entidad de seguridad se le conceden permisos de datos RBAC a través de un rol integrado, o bien a través de un rol personalizado, estos permisos se evalúan primero tras la autorización de una solicitud.
Si la operación solicitada está autorizada por las asignaciones RBAC de la entidad de seguridad, la autorización se resuelve inmediatamente y no se realiza ninguna comprobación adicional de la ACL.
Como alternativa, si la entidad de seguridad no tiene una asignación RBAC, o la operación de la solicitud no coincide con el permiso asignado, se realizan comprobaciones de ACL para determinar si la entidad de seguridad está autorizada para realizar la operación solicitada.
Para el permiso de lectura, conceda al menos el permiso **Execute** a partir del sistema de archivos de origen, junto con el permiso **Read** para los archivos que se van a leer.
Como alternativa, conceda al menos el rol **Lector de datos de Storage Blob** con RBAC.
Vea [este](/azure/storage/blobs/data-lake-storage-access-control) artículo para obtener más información.
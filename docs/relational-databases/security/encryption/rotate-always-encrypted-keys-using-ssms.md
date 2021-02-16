---
title: Rotación de claves de Always Encrypted mediante SQL Server Management Studio | Microsoft Docs
description: Obtenga información sobre la rotación de claves maestras de columna y claves de cifrado de columna de Always Encrypted mediante SQL Server Management Studio.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
- sql13.SWB.COLUMNMASTERKEY.CLEANUP.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 249e0d2bc53083839787543723fc16dc11a4ff57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340130"
---
# <a name="rotate-always-encrypted-keys-using-sql-server-management-studio"></a>Rotación de claves de Always Encrypted mediante SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

En este artículo se describen las tareas para rotar claves maestras de columna y claves de cifrado de columna de Always Encrypted mediante [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md).

Para obtener información general sobre la administración de claves de Always Encrypted, incluidos algunos procedimientos recomendados y consideraciones de seguridad importantes, vea [Información general de administración de claves de Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

<a name="rotatecmk"></a>
## <a name="rotate-column-master-keys"></a>Rotación de claves maestras de columna 

La rotación de una clave maestra de columna es el proceso por el cual se reemplaza una clave maestra de columna existente por otra nueva. Puede que necesite rotar una clave si está en peligro, o bien para cumplir las directivas o los reglamentos de la organización que exigen que se roten las claves criptográficas de forma regular. La rotación de claves maestras de columna implica descifrar las claves de cifrado de columnas que están protegidas con la clave maestra de columna actual, volver a cifrarlas con la nueva clave maestra de columna y actualizar los metadatos de clave. 

### <a name="step-1-provision-a-new-column-master-key"></a>Paso 1: Aprovisionar una nueva clave maestra de columna

Siga los pasos descritos en [Aprovisionamiento de claves maestras de columna con el cuadro de diálogo Nueva clave maestra de columna](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog).

### <a name="step-2-encrypt-column-encryption-keys-with-the-new-column-master-key"></a>Paso 2: Cifrar las claves de cifrado de columna con la nueva clave maestra de columna

Por lo general, una clave maestra de columna protege una o más claves de cifrado de columnas. Cada clave de cifrado de columnas tiene un valor cifrado, almacenado en la base de datos, que es el resultado de haber cifrado la clave de cifrado de columnas con la clave maestra de columna.
En este paso, todas las claves de cifrado de columna, que están protegidas con la clave maestra de columna que se va a rotar, se cifrarán con la nueva clave maestra de columna, y el nuevo valor cifrado se almacenará en la base de datos. Como resultado, cada clave de cifrado de columnas afectada por la rotación tendrá dos valores cifrados: uno cifrado con la clave maestra de columna antigua y otro reciente, cifrado con la nueva.

1.  En el **Explorador de objetos**, vaya a la carpeta **Security > Always Encrypted Keys > Column Master Keys** y encuentre la clave maestra de columna que va a rotar.
2.  Haga clic con el botón derecho en ella y seleccione **Rotar**.
3.  En el cuadro de diálogo **Rotación de clave maestra de columna** , seleccione el nombre de la nueva clave maestra de columna que creó en el paso 1 en el campo **Destino** .
4.  Revise la lista de claves de cifrado de columna, protegidas por las claves maestras de columna existentes. Estas claves se verán afectadas por la rotación.
5.  Haga clic en **OK**.

SQL Server Management Studio obtendrá los metadatos de las claves de cifrado de columnas que están protegidas con la clave maestra de columna antigua y los metadatos de las claves maestras de columna antigua y nueva. Después, SSMS usará los metadatos de la clave maestra de columna para tener acceso al almacén de claves que contiene la clave maestra de columna antigua y descifrar las claves de cifrado de columnas. Posteriormente, SSMS obtendrá acceso al almacén de claves que contiene la nueva clave maestra de columna para generar un conjunto de valores cifrados de las claves de cifrado de columnas y, después, agregará los valores nuevos a los metadatos, para lo que generará y emitirá instrucciones [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) .

> [!NOTE]
> Asegúrese de que ninguna de las claves de cifrado de columnas, cifradas con la clave maestra de columna antigua, esté cifrada con otra clave maestra de columna. Dicho de otro modo, cada clave cifrada de columna afectada por la rotación debe tener exactamente un valor cifrado en la base de datos. Si alguna de las claves de cifrado de columnas afectadas tiene más de un valor cifrado, tendrá que quitarlo para poder continuar con la rotación (consulte el *paso 4* , en el que se explica cómo quitar un valor cifrado de una clave de cifrado de columna).

### <a name="step-3-configure-your-applications-with-the-new-column-master-key"></a>Paso 3: Configurar las aplicaciones con la nueva clave maestra de columna

En este paso, debe asegurarse de que todas sus aplicaciones cliente que consultan columnas de la base de datos protegidas con la clave maestra de columna que va a rotar puedan acceder a la nueva clave maestra de columna (es decir, las columnas de la base de datos cifradas con una clave de cifrado de columna que, a su vez, está cifrada con la clave maestra de columna, que se va a rotar). Este paso depende del tipo de almacén de claves en el que se encuentre la nueva clave maestra de columna. Por ejemplo:

- Si la nueva clave maestra de columna es un certificado guardado en el Almacén de certificados de Windows, tendrá que implementarlo en la misma ubicación del almacén de certificados (*Usuario actual* o *Equipo local*) que la especificada en la ruta de acceso de la clave de su clave maestra de columna en la base de datos. La aplicación debe poder acceder al certificado:
  - Si el certificado se guarda en la ubicación del almacén de certificados *Usuario actual*, será necesario importarlo en el almacén Usuario actual de la identidad de Windows (el usuario) de la aplicación.
  - Si el certificado se guarda en la ubicación del almacén de certificados *Equipo local*, la identidad de Windows de la aplicación deberá tener permiso para acceder al certificado.
- Si la nueva clave maestra de columna se guarda en el Almacén de claves de Microsoft Azure, la aplicación se debe implementar de forma que se pueda autenticar en Azure y tenga permiso para obtener acceso a la clave.

Para más información, vea [Creación y almacenamiento de claves maestras de columna para Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

> [!NOTE]
> En este momento de la rotación, tanto la clave maestra de columna antigua como la nueva son válidas y se pueden usar para obtener acceso a los datos.

### <a name="step-4-clean-up-column-encryption-key-values-encrypted-with-the-old-column-master-key"></a>Paso 4: Limpiar los valores de clave de cifrado de columna cifrados con la clave maestra de columna antigua

Una vez que haya configurado todas las aplicaciones para que usen la nueva clave maestra de columna, quite de la base de datos los valores de las claves de cifrado de columnas que estén cifradas con la clave maestra de columna *antigua* . Al quitar los valores antiguos, garantizará que está preparado para la siguiente rotación (recuerde que cada clave de cifrado de columna protegida con una clave maestra de columna que se vaya a rotar debe tener exactamente un único valor cifrado).

Existe otro motivo para limpiar el valor antiguo antes de archivar o quitar la clave maestra de columna antigua, y está relacionado con el rendimiento: al realizar una consulta en una columna cifrada, es posible que un controlador cliente habilitado para Always Encrypted intente descifrar dos valores (el antiguo y el nuevo). El controlador no sabe cuál de las dos claves maestras de columna es válida en el entorno de la aplicación, por lo que recuperará los dos valores cifrados del servidor. Si no se puede descifrar uno de los valores, porque está protegido con la clave maestra de columna que no está disponible (por ejemplo, se trata de la clave maestra de columna que se ha quitado del almacén), el controlador tratará de descifrar otro valor con la clave nueva.

> [!WARNING]
> Si quita el valor de una clave de cifrado de columnas antes de que su clave maestra de columna correspondiente se haya puesto a disposición de una aplicación, esta última no podrá descifrar la columna de la base de datos.

1.  En el **Explorador de objetos**, vaya a la carpeta **Security > Always Encrypted Keys** y encuentre la clave maestra de columna existente que quiere reemplazar.
2.  Haga clic con el botón derecho en la clave maestra de columna existente y seleccione **Limpieza**.
3.  Revise la lista de valores de claves de cifrado de columna que se van a quitar.
4.  Haga clic en **OK**.

SQL Server Management Studio emitirá instrucciones [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) para quitar los valores cifrados de las claves de cifrado de columnas que están cifradas con la clave maestra de columna antigua.

### <a name="step-5-delete-metadata-for-your-old-column-master-key"></a>Paso 5: Eliminar los metadatos de la clave maestra de columna antigua

Si decide quitar la definición de la clave maestra de columna antigua de la base de datos, siga los pasos que se indican a continuación.

1. En el **Explorador de objetos**, vaya a la carpeta **Seguridad > Siempre claves cifradas > Claves maestras de columna** y encuentre la clave maestra de columna antigua que quiere quitar de la base de datos.
2. Haga clic con el botón derecho en la clave maestra de columna antigua y seleccione **Eliminar**. De este modo, se generará y emitirá una instrucción [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md) para quitar los metadatos de la clave maestra de columna.
3. Haga clic en **OK**.

> [!NOTE]
> Es muy recomendable no eliminar permanentemente la antigua clave maestra de columna después de la rotación. Debería conservarla en su almacén de claves actual o archivarla en otra ubicación segura. Si restaura la base de datos desde un archivo de copia de seguridad a un punto en el tiempo anterior a la configuración de la nueva clave maestra de columna, necesitará la clave antigua para tener acceso a los datos.

### <a name="permissions-for-rotating-column-master-key"></a>Permisos para la rotación de la clave maestra de columna

La rotación de una clave maestra de columna requiere los siguientes permisos de base de datos:

- **ALTER ANY COLUMN MASTER KEY**: es necesario para crear los metadatos de la nueva clave maestra de columna y eliminar los metadatos de la clave maestra de columna antigua.
- **ALTER ANY COLUMN ENCRYPTION KEY**: es necesario para modificar los metadatos de la clave de cifrado de columnas (agregar nuevos valores cifrados).

También debe tener acceso a las claves maestras de columna antigua y nueva en sus almacenes de claves. Para tener acceso a un almacén de claves y usar una clave maestra de columna, es posible que necesite permisos en el almacén de claves o en la clave:
- **Almacén de certificados: equipo local**: debe tener acceso de lectura al certificado que se usa como clave maestra de columna, o bien ser el administrador del equipo.
- **Azure Key Vault**: necesita los permisos *create*, *get*, *unwrapKey*, *wrapKey*, *sign* y *verify* en el almacén que contiene las claves maestras de columna.
- **Proveedor de almacén de claves (CNG)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del KSP.
- **Proveedor de servicios criptográficos (CAPI)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del CSP.

Para más información, vea [Creación y almacenamiento de claves maestras de columna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

<a name="rotatecek"></a> 
## <a name="rotate-column-encryption-keys"></a>Rotación de claves de cifrado de columna

La rotación de una clave de cifrado de columnas implica descifrar los datos de todas las columnas cifrados con la clave que se va a rotar y volver a cifrarlos con la nueva clave de cifrado de columnas.

>[!NOTE]
> La rotación de una clave de cifrado de columnas puede tardar mucho tiempo si las tablas que contienen columnas cifradas con la clave que se va a rotar son grandes. Mientras se vuelven a cifrar los datos, las aplicaciones no pueden escribir en las tablas afectadas. Por lo tanto, la organización tiene que planear muy cuidadosamente cualquier rotación de claves de cifrado de columnas.
Para realizar la rotación de una clave de cifrado de columnas, use el Asistente para Always Encrypted.

1.  Para abrir el asistente para la base de datos, haga clic con el botón derecho en la base de datos, seleccione **Tareas** y, después, haga clic en **Cifrar columnas**.
2.  Revise la página **Introduction** y haga clic en **Next**.
3.  En la página **Selección de columna** , expanda las tablas y busque todas las columnas que quiera reemplazar que actualmente estén cifradas con la clave de cifrado de columnas antigua.
4.  Para cada columna cifrada con la clave de cifrado de columna antigua, establezca **Clave de cifrado** en una nueva clave generada automáticamente. **Nota:** También puede crear una clave de cifrado de columnas antes de ejecutar el asistente. Vea [Aprovisionamiento de claves de cifrado de columnas con el cuadro de diálogo Nueva clave de cifrado de columnas](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog).
5.  En la página **Configuración de la clave maestra** , seleccione una ubicación para almacenar la clave nueva y seleccione un origen de clave maestra. Después, haga clic en **Siguiente**. **Nota:** Si usa una clave de cifrado de columna existente (no una clave generada automáticamente), no debe realizar ninguna acción en esta página.
6.  En la página **Validación**, elija si quiere ejecutar inmediatamente el script o crear un script de PowerShell y, después, haga clic en **Siguiente**.
7.  En la página **Resumen**, revise las opciones que ha seleccionado, haga clic en **Finalizar** y cierre el asistente cuando acabe.
8.  En el **Explorador de objetos**, vaya a la carpeta **Seguridad &gt; Siempre claves cifradas&gt; Claves de cifrado de columna** y encuentre la clave de cifrado de columnas antigua que quiere quitar de la base de datos. Haga clic con el botón derecho en la clave y seleccione **Eliminar**.

### <a name="permissions-for-rotating-column-encryption-keys"></a>Permisos para la rotación de las claves de cifrado de columna

La rotación de una clave de cifrado de columnas requiere los permisos de base de datos siguientes: **ALTER ANY COLUMN MASTER KEY**: es obligatorio si se usa una nueva clave de cifrado de columna generada de forma automática (también se generará una nueva clave maestra de columna y sus nuevos metadatos).
**ALTER ANY COLUMN ENCRYPTION KEY**: es necesario para agregar metadatos a la nueva clave de cifrado de columnas.

También debe tener acceso a las claves maestras de columna de las claves de cifrado de columnas antigua y nueva. Para tener acceso a un almacén de claves y usar una clave maestra de columna, es posible que necesite permisos en el almacén de claves o en la clave:
- **Almacén de certificados, equipo local**: debe tener acceso de lectura al certificado que se usa como clave maestra de columna o ser el administrador del equipo.
- **Azure Key Vault**: necesita los permisos get, unwrapKey y verify en el almacén que contiene la clave maestra de columna.
- **Proveedor de almacén de claves (CNG)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del KSP.
- **Proveedor de servicios criptográficos (CAPI)** : se le podrían solicitar el permiso y las credenciales necesarios al usar una clave o un almacén de claves, en función del almacén y de la configuración del CSP.

Para más información, vea [Creación y almacenamiento de claves maestras de columna (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="next-steps"></a>Pasos siguientes
- [Consulta de columnas mediante Always Encrypted con SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Desarrollo de aplicaciones con Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Consulte también
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Información general sobre la administración de claves de Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
- [Configuración de Always Encrypted con SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
- [Configurar Always Encrypted con PowerShell](configure-always-encrypted-using-powershell.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)

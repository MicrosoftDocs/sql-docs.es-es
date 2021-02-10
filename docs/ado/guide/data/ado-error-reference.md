---
description: Errores de ADO
title: Referencia de error de ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], number reference
- errors [ADO], ErrorValueEnum
- ErrorValueEnum enumeration [ADO]
ms.assetid: f653393e-d4b0-4c34-ad5f-2bdf56bc1305
author: rothja
ms.author: jroth
ms.openlocfilehash: cfb6a2ede665c939aa5824810c692bdbe02df6c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028377"
---
# <a name="ado-errors"></a>Errores de tiempo de ejecución de ADO
La constante **ErrorValueEnum** describe los valores de error de ADO. Para obtener una lista completa de estas constantes enumeradas, incluidos los valores, vea el [Apéndice B: errores de ADO](../appendixes/appendix-b-ado-errors.md). En esta sección se examinan algunos de los errores más interesantes y se explican algunas situaciones específicas que pueden generarlos, o soluciones para solucionar el problema. Se enumeran la constante **ErrorValueEnum** y el número decimal positivo corto.

|Number|ErrorValueEnum (constante)|Descripción/causas posibles|
|------------|-----------------------------|----------------------------------|
|**3000**|**adErrProviderFailed**|El proveedor no pudo realizar la operación solicitada.|
|**3001**|**adErrInvalidArgument**|Los argumentos son del tipo incorrecto, están fuera del intervalo aceptable o están en conflicto entre sí. Este error suele deberse a un error tipográfico en una instrucción SELECT de SQL. Por ejemplo, un nombre de campo o un nombre de tabla mal escrito puede generar este error. Este error también puede producirse cuando un campo o una tabla denominada en una instrucción SELECT no existe en el almacén de datos.|
|**3002**|**adErrOpeningFile**|No se pudo abrir el archivo. Se especificó un nombre de archivo mal escrito, o se ha cambiado el nombre de un archivo o se ha eliminado. A través de una red, es posible que la unidad no esté disponible temporalmente o que el tráfico de red impida una conexión.|
|**3003**|**adErrReadFile**|No se pudo leer el archivo. El nombre del archivo se ha especificado de forma incorrecta, es posible que se haya eliminado o eliminado el archivo o que el archivo se haya dañado.|
|**3004**|**adErrWriteFile**|No se pudo escribir en el archivo. Es posible que haya cerrado un archivo e intentado escribir en él, o que el archivo esté dañado. Si el archivo se encuentra en una unidad de red, es posible que las condiciones de red transitorias impidan escribir en una unidad de red.|
|**3021**|**adErrNoCurrentRecord**|**BOF** o **EOF** es true, o se ha eliminado el registro actual. La operación solicitada requiere un registro actual.<br /><br /> Se intentó actualizar registros mediante **Buscar** o buscar para desplace el puntero del **registro al registro** deseado. Si no se encuentra el registro, **EOF** será true. Este error también puede producirse después de un error de **AddNew** o **Delete** porque no hay ningún registro actual cuando se produce un error en estos métodos.|
|**3219**|**adErrIllegalOperation**|La operación no se permite en este contexto.|
|**3220**|**adErrCantChangeProvider**|El proveedor proporcionado es diferente del que ya está en uso.|
|**3246**|**adErrInTransaction**|El objeto de **conexión** no se puede cerrar explícitamente mientras se está en una transacción. No se puede cerrar un **conjunto de registros** o un objeto de **conexión** que actualmente participa en una transacción. Llame a **RollbackTrans** o a **CommitTrans** antes de cerrar el objeto.|
|**3251**|**adErrFeatureNotAvailable**|El objeto o proveedor no es capaz de realizar la operación solicitada. Algunas operaciones dependen de una versión de proveedor determinada.|
|**3265**|**adErrItemNotFound**|No se encuentra el elemento en la colección que corresponde al nombre o el ordinal solicitados. Se ha especificado un nombre de tabla o campo incorrecto.|
|**3367**|**adErrObjectInCollection**|El objeto ya está en la colección. No se puede anexar. Un objeto no se puede agregar dos veces a la misma colección.|
|**3420**|**adErrObjectNotSet**|El objeto ya no es válido.|
|**3421**|**adErrDataConversion**|La aplicación usa un valor de tipo incorrecto para la operación actual. Es posible que haya proporcionado una cadena a una operación que espera una secuencia, por ejemplo.|
|**3704**|**adErrObjectClosed**|No se permite la operación cuando el objeto está cerrado. Se ha cerrado la **conexión** o el **conjunto de registros** . Por ejemplo, otra rutina podría haber cerrado un objeto global. Puede evitar este error comprobando la propiedad **State** antes de intentar una operación.|
|**3705**|**adErrObjectOpen**|La operación no se permite cuando el objeto está abierto. No se puede abrir un objeto abierto. Los campos no se pueden anexar a un **conjunto de registros** abierto.|
|**3706**|**adErrProviderNotFound**|No se encuentra el proveedor. Puede que no esté instalado correctamente.<br /><br /> Es posible que el nombre del proveedor esté especificado incorrectamente, que el proveedor especificado no esté instalado en el equipo en el que se ejecuta el código o que la instalación se haya dañado.|
|**3707**|**adErrBoundToCommand**|No se puede cambiar la propiedad **ActiveConnection** de un objeto de **conjunto de registros** , que tiene un objeto de **comando** como origen. La aplicación intentó asignar un nuevo objeto de **conexión** a un **conjunto de registros** que tiene un objeto de **comando** como su origen.|
|**3708**|**adErrInvalidParamInfo**|El objeto de **parámetro** no está definido correctamente. Se proporcionó información incoherente o incompleta.|
|**3709**|**adErrInvalidConnection**|No se puede usar la conexión para realizar esta operación. Está cerrado o no es válido en este contexto.|
|**3710**|**adErrNotReentrant**|No se puede realizar la operación al procesar el evento. Una operación no se puede realizar en un controlador de eventos que provoque que el evento se active de nuevo. Por ejemplo, no se debe llamar a los métodos de navegación desde dentro de un controlador de eventos **WillMove** .|
|**3711**|**adErrStillExecuting**|No se puede realizar la operación mientras se ejecuta de forma asincrónica.|
|**3712**|**adErrOperationCancelled**|El usuario ha cancelado la operación. La aplicación ha llamado al método **CancelUpdate** o **CancelBatch** y se ha cancelado la operación actual.|
|**3713**|**adErrStillConnecting**|No se puede realizar la operación mientras se conecta de forma asincrónica.|
|**3714**|**adErrInvalidTransaction**|La coordinación de la transacción no es válida o no se ha iniciado.|
|**3715**|**adErrNotExecuting**|No se puede realizar la operación mientras no se ejecuta.|
|**3716**|**adErrUnsafeOperation**|La configuración de seguridad de este equipo prohíbe el acceso a un origen de datos en otro dominio.|
|**3717**|**adWrnSecurityDialog**|Solo para uso interno. No use. (La entrada se incluyó por razones de integridad. Este error no debería aparecer en el código).|
|**3718**|**adWrnSecurityDialogHeader**|Solo para uso interno. No use. (Entrada que se incluye en aras de la integridad. Este error no debería aparecer en el código).|
|**3719**|**adErrIntegrityViolation**|El valor de los datos entra en conflicto con las restricciones de integridad del campo. Un nuevo valor para un **campo** produciría una clave duplicada. Un valor que constituye un lado de una relación entre dos registros podría no ser actualizable.|
|**3720**|**adErrPermissionDenied**|Los permisos insuficientes evitan escribir en el campo. El usuario con el nombre en la cadena de conexión no tiene los permisos adecuados para escribir en un **campo**.|
|**3721**|**adErrDataOverflow**|El valor de los datos es demasiado grande para representarlo en el tipo de datos del campo. Se asignó un valor numérico demasiado grande para el campo previsto. Por ejemplo, un valor entero largo se asignó a un campo entero corto.|
|**3722**|**adErrSchemaViolation**|El valor de los datos entra en conflicto con el tipo de datos o con las restricciones del campo. El almacén de datos tiene restricciones de validación que difieren del valor del **campo** .|
|**3723**|**adErrSignMismatch**|Error de conversión porque el valor de los datos tenía signo y el tipo de datos del campo utilizado por el proveedor no tenía signo.|
|**3724**|**adErrCantConvertvalue**|No se puede convertir el valor de los datos por motivos distintos a un error de coincidencia de signos o desbordamiento de datos. Por ejemplo, la conversión tendría datos truncados.|
|**3725**|**adErrCantCreate**|No se puede establecer ni recuperar el valor de los datos porque el tipo de datos del campo era desconocido o el proveedor no tenía suficientes recursos para realizar la operación.|
|**3726**|**adErrColumnNotOnThisRow**|El registro no contiene este campo. Se especificó un nombre de campo incorrecto o se hizo referencia a un campo que no se encontraba en la colección de **campos** del registro actual.|
|**3727**|**adErrURLDoesNotExist**|La dirección URL de origen o el elemento primario de la dirección URL de destino no existen. Hay un error tipográfico en la dirección URL de origen o de destino. Es posible que tenga que `https://mysite/photo/myphoto.jpg` realmente tenga `https://mysite/photos/myphoto.jpg` en su lugar. El error tipográfico en la dirección URL principal (en este caso, *Photo* en lugar de *fotos*) ha provocado el error.|
|**3728**|**adErrTreePermissionDenied**|Los permisos son insuficientes para tener acceso al árbol o al subárbol. El usuario con el nombre en la cadena de conexión no tiene los permisos adecuados.|
|**3729**|**adErrInvalidURL**|La dirección URL contiene caracteres no válidos. Asegúrese de que la dirección URL está escrita correctamente. La dirección URL sigue el esquema registrado en el proveedor actual (por ejemplo, el proveedor de publicación en Internet está registrado para http).|
|**3730**|**adErrResourceLocked**|El objeto representado por la dirección URL especificada está bloqueado por uno o más procesos. Espere hasta que el proceso haya finalizado e intente la operación de nuevo. Otro usuario u otro proceso de la aplicación ha bloqueado el objeto al que está intentando obtener acceso. Lo más probable es que se produzca en un entorno de varios usuarios.|
|**3731**|**adErrResourceExists**|No se puede realizar la operación de copia. El objeto denominado por la dirección URL de destino ya existe. Especifique **adCopyOverwrite** para reemplazar el objeto. Si no especifica **adCopyOverwrite** al copiar los archivos en un directorio, se produce un error en la copia al intentar copiar un elemento que ya existe en la ubicación de destino.|
|**3732**|**adErrCannotComplete**|El servidor no puede completar la operación. Esto puede deberse a que el servidor está ocupado con otras operaciones o a recursos insuficientes.|
|**3733**|**adErrVolumeNotFound**|El proveedor no encuentra el dispositivo de almacenamiento indicado por la dirección URL. Asegúrese de que la dirección URL está escrita correctamente. Es posible que la dirección URL del dispositivo de almacenamiento sea incorrecta, pero este error puede producirse por otros motivos. El dispositivo puede estar sin conexión o un gran volumen de tráfico de red puede impedir que se realice la conexión.|
|**3734**|**adErrOutOfSpace**|No se puede realizar la operación. El proveedor no puede obtener suficiente espacio de almacenamiento. Puede que no haya suficiente memoria RAM o espacio en disco duro para los archivos temporales en el servidor.|
|**3735**|**adErrResourceOutOfScope**|La dirección URL de origen o de destino está fuera del ámbito del registro actual.|
|**3736**|**adErrUnavailable**|No se pudo completar la operación y el estado no está disponible. El campo puede no estar disponible o no se intentó realizar la operación. Es posible que otro usuario haya cambiado o eliminado el campo al que está intentando obtener acceso.|
|**3737**|**adErrURLNamedRowDoesNotExist**|El registro nombrado por esta dirección URL no existe. Al intentar abrir un archivo con un objeto de **registro** , el nombre de archivo o la ruta de acceso al archivo no se ha escrito correctamente.|
|**3738**|**adErrDelResOutOfScope**|La dirección URL del objeto que se va a eliminar está fuera del ámbito del registro actual.|
|**3747**|**adErrCatalogNotSet**|La operación requiere un **ParentCatalog** válido.|
|**3748**|**adErrCantChangeConnection**|Se denegó la conexión. La nueva conexión que ha solicitado tiene características diferentes de la que ya está en uso.|
|**3749**|**adErrFieldsUpdateFailed**|Error al actualizar los campos. Para obtener más información, examine la propiedad **status** de los objetos de campo individuales. Este error puede producirse en dos situaciones: al cambiar el valor de un objeto de **campo** en el proceso de cambiar o agregar un registro a la base de datos; y al cambiar las propiedades del propio objeto de **campo** .<br /><br /> No se pudo actualizar el **registro** o el **conjunto de registros** debido a un problema con uno de los campos del registro actual. Enumere la colección **Fields** y Compruebe la propiedad **status** de cada campo para determinar la causa del problema.|
|**3750**|**adErrDenyNotSupported**|El proveedor no admite restricciones de uso compartido. Se intentó restringir el uso compartido de archivos y el proveedor no admite el concepto.|
|**3751**|**adErrDenyTypeNotSupported**|El proveedor no admite el tipo solicitado de restricción de uso compartido. Se intentó establecer un tipo determinado de restricción de uso compartido de archivos que no es compatible con el proveedor. Consulte la documentación del proveedor para determinar qué restricciones de uso compartido de archivos se admiten.|
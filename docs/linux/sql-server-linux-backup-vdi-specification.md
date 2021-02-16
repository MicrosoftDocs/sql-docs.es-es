---
title: 'Especificación de copia de seguridad de VDI: SQL Server en Linux'
description: Obtenga información sobre las interfaces proporcionadas por el SDK de cliente de la interfaz de dispositivo virtual (VDI) de SQL Server en Linux.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 0250ba2b-8cdd-450e-9109-bf74f70e1247
ms.openlocfilehash: dcdbd7ba3a00048b63836e52092835fe6f7a618f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343182"
---
# <a name="sql-server-on-linux-vdi-client-sdk-specification"></a>Especificación del SDK de cliente VDI de SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este documento se describen las interfaces proporcionadas por el SDK de cliente de la interfaz de dispositivo virtual (VDI) de SQL Server en Linux. Los fabricantes de software independientes (ISV) pueden usar la interfaz de programación de aplicaciones (API) del dispositivo de copia de seguridad virtual para integrar SQL Server en sus productos. En general, VDI en Linux se comporta de forma similar a VDI en Windows, con los cambios siguientes:

- La memoria compartida de Windows se convierte en la memoria compartida de POSIX.
- Los semáforos de Windows se convierten en semáforos de POSIX.
- Los tipos de Windows como HRESULT y DWORD se cambian a números enteros equivalentes.
- Las interfaces COM se quitan y se reemplazan por un par de clases de C++.
- SQL Server en Linux no admite instancias con nombre, por lo que se han quitado las referencias al nombre de instancia. 
- La biblioteca compartida se implementa en el libsqlvdi.so instalado en /opt/mssql/lib/libsqlvdi.so

Este documento es un anexo a **vbackup.chm** que detalla las especificaciones de VDI de MS SQL Server en Windows. Descargue la [especificación de VDI de SQL en Windows](https://www.microsoft.com/download/details.aspx?id=17282).

Revise también la solución de copia de seguridad de VDI en el [repositorio de GitHub de ejemplos de SQL Server](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sqlvdi-linux).

## <a name="user-permissions-setup"></a>Configuración de permisos de usuario

En Linux, los primitivos de POSIX son propiedad del usuario que los crea y su grupo predeterminado. En el caso de los objetos creados por SQL Server, estos serán de forma predeterminada propiedad del usuario de MSSQL y el grupo MSSQL. Para permitir el uso compartido entre SQL Server y el cliente VDI, se recomienda uno de los dos métodos siguientes:

1. Ejecución del cliente VDI como usuario de MSSQL
   
   Ejecute el siguiente comando para cambiar al usuario de MSSQL:
   
   ```bash
   sudo su mssql
   ```

2. Agregue el usuario de MSSQL al grupo de vdiuser, y vdiuser al grupo MSSQL.
   
   Ejecute los siguientes comandos:

   ```bash
   sudo useradd vdiuser
   sudo usermod -a -G mssql vdiuser
   sudo usermod -a -G vdiuser mssql
   ```

   Reinicie el servidor para seleccionar nuevos grupos de SQL Server y vdiuser

## <a name="client-functions"></a>Funciones de cliente

Este capítulo contiene descripciones de cada una de las funciones de cliente. Las descripciones incluyen la información siguiente:

- Propósito de la función
- Sintaxis de la función
- Lista de parámetros
- Valores devueltos
- Observaciones

## <a name="clientvirtualdevicesetcreate"></a>ClientVirtualDeviceSet::Create

**Propósito** Esta función crea el conjunto de dispositivos virtuales.

**Sintaxis**
   ```
   int ClientVirtualDeviceSet::Create (
   char *   name,       // name for the set
   VDConfig   * cfg     // configuration for the set
   );
   ```

| Parámetros | Argumento | Explicación
| ----- | ----- | ------ |
| | **name** | Identifica el conjunto de dispositivos virtuales. Deben seguirse las reglas para los nombres usados por CreateFileMapping(). Cualquier carácter excepto la barra diagonal inversa (puede usarse \)). Esta es una cadena de caracteres. Se recomienda agregar un prefijo a la cadena con nombre del producto o la compañía del usuario y el nombre de la base de datos. |
| |**cfg** | Esta es la configuración del conjunto de dispositivos virtuales. Para obtener más información, consulte la sección "Configuración" más adelante en este documento.

| Valores devueltos | Argumento | Explicación
| ----- | ----- | ------ |
| |**NOERROR** | La función se ha realizado correctamente. |
| |**VD_E_NOTSUPPORTED** |Uno o varios de los campos de la configuración no eran válidos o no se admiten. |
| |**VD_E_PROTOCOL** | El conjunto de dispositivos virtuales ya existe.

**Notas** El método Create solo debe llamarse una vez por operación BACKUP o RESTORE. Después de invocar el método Close, el cliente puede volver a usar la interfaz para crear otro conjunto de dispositivos virtuales.

## <a name="clientvirtualdevicesetgetconfiguration"></a>ClientVirtualDeviceSet::GetConfiguration

**Propósito** Esta función se usa para esperar a que el servidor configure el conjunto de dispositivos virtuales.
**Sintaxis**
   ```
   int ClientVirtualDeviceSet::GetConfiguration (
   time_t       timeout,    // in milliseconds
   VDConfig *       cfg // selected configuration
   );
   ```

| Parámetros | Argumento | Explicación
| ----- | ----- | ------ |
| | **timeout** | Se trata del tiempo de espera, en milisegundos. Use INFINITE o cualquier entero negativo para evitar el tiempo de espera.
| | **cfg** | Tras la ejecución correcta, contiene la configuración seleccionada por el servidor. Para obtener más información, consulte la sección "Configuración" más adelante en este documento.

| Valores devueltos | Argumento | Explicación
| ----- | ----- | ------ |
| |**NOERROR** | Se devolvió la configuración.
| |**VD_E_ABORT** |Se invocó SignalAbort.
| |**VD_E_TIMEOUT** |Se agotó el tiempo de espera de la función.

**Notas** Esta función se bloquea en un estado de alerta. Después de la invocación correcta, se pueden abrir los dispositivos del conjunto de dispositivos virtuales.


## <a name="clientvirtualdevicesetopendevice"></a>ClientVirtualDeviceSet::OpenDevice
**Propósito** Esta función abre uno de los dispositivos del conjunto de dispositivos virtuales.
**Sintaxis**
   ```
   int ClientVirtualDeviceSet::OpenDevice (
   char *           name,       // name for the set
   ClientVirtualDevice **       ppVirtualDevice // returns interface to device
   );
   ```

| Parámetros | Argumento | Explicación
| ----- | ----- | ------ |
| | **name** |Identifica el conjunto de dispositivos virtuales.
| | **ppVirtualDevice** |Cuando la función se ejecuta correctamente, se devuelve un puntero al dispositivo virtual. Este dispositivo se usa con GetCommand y CompleteCommand.

| Valores devueltos | Argumento | Explicación
| ----- | ----- | ------ |
| |**NOERROR** |La función se ha realizado correctamente.
| |**VD_E_ABORT** | Se solicitó Abort.
| |**VD_E_OPEN** |  Todos los dispositivos están abiertos.
| |**VD_E_PROTOCOL** |  El conjunto no está en el estado de inicialización o este dispositivo en concreto ya está abierto.
| |**VD_E_INVALID** |El nombre del dispositivo no es válido. No es uno de los nombres que se sabe que forman el conjunto.

**Notas** Es posible que se devuelva VD_E_OPEN sin problemas. El cliente puede llamar a OpenDevice por medio de un bucle hasta que se devuelva este código.
Si se configura más de un dispositivo, por ejemplo *n* dispositivos, el conjunto de dispositivos virtuales devolverá *n* interfaces de dispositivo únicas.

Se puede usar la función `GetConfiguration` para esperar hasta que se puedan abrir los dispositivos.
Si esta función no se realiza correctamente, se devuelve un valor NULL a través de ppVirtualDevice.
 
## <a name="clientvirtualdevicegetcommand"></a>ClientVirtualDevice::GetCommand

**Propósito** Esta función se usa para obtener el siguiente comando en cola en un dispositivo. Cuando se solicita, esta función espera el siguiente comando.

**Sintaxis**
   ```
   int ClientVirtualDevice::GetCommand (
   time_t       timeout,    // time-out in milliseconds
   VDC_Command**    ppCmd   // returns the next command
   );
   ```

| Parámetros | Argumento | Explicación
| ----- | ----- | ------ |
| |**timeout** |Se trata del tiempo de espera, en milisegundos. Use INFINTE para esperar indefinidamente. Use 0 para sondear un comando. Se devuelve VD_E_TIMEOUT si no hay ningún comando disponible actualmente. Si se agota el tiempo de espera, el cliente decide la siguiente acción.
| |**Tiempo de espera** |Se trata del tiempo de espera, en milisegundos. Use INFINTE o un valor negativo para esperar indefinidamente. Use 0 para sondear un comando. Se devuelve VD_E_TIMEOUT si no hay ningún comando disponible antes de que el tiempo de expiración se agote. Si se agota el tiempo de expiración, el cliente decide la siguiente acción.
| |**ppCmd** |Cuando un comando se devuelve correctamente, el parámetro devuelve la dirección de un comando que se va a ejecutar. La memoria devuelta es de solo lectura. Cuando se completa el comando, este puntero se pasa a la rutina CompleteCommand. Para obtener más información sobre cada comando, vea "Comandos" más adelante en este documento.
        
| Valores devueltos | Argumento | Explicación
| ----- | ----- | ------ |
| |**NOERROR** |Se ha capturado un comando.
| |**VD_E_CLOSE** |El servidor ha cerrado el dispositivo.
| |**VD_E_TIMEOUT** |No había ningún comando disponible y se agotó el tiempo de espera.
| |**VD_E_ABORT** |El cliente o el servidor han usado el elemento SignalAbort para forzar un apagado.

**Notas** Cuando se devuelve VD_E_CLOSE, SQL Server ha cerrado el dispositivo. Esto forma parte del apagado normal. Una vez cerrados todos los dispositivos, el cliente invoca ClientVirtualDeviceSet::Close para cerrar el conjunto de dispositivos virtuales.
Cuando esta rutina debe bloquearse para esperar un comando, el subproceso se deja en una condición de alerta.

## <a name="clientvirtualdevicecompletecommand"></a>ClientVirtualDevice::CompleteCommand

**Propósito** Esta función se usa para notificar a SQL Server que un comando ha finalizado. Se debe devolver la información de finalización adecuada para el comando. Para obtener más información, consulte la sección "Comandos" más adelante en este documento.

**Sintaxis** 

   ```
   int ClientVirtualDevice::CompleteCommand (
   VDC_Command pCmd,        // the command
   int  completionCode,     // completion code
   unsigned long    bytesTransferred,   // bytes transferred
   int64_t  position        // current position
   );
   ```

| Parámetros | Argumento | Explicación
| ----- | ----- | ------ |
| |**pCmd** |Se trata de la dirección de un comando devuelto anteriormente desde ClientVirtualDevice::GetCommand.
| |**completionCode** |Se trata de un código de estado que indica el estado de finalización. Este parámetro debe devolverse para todos los comandos. El código devuelto debe ser adecuado para el comando que se va a realizar. ERROR_SUCCESS se usa en todos los casos para indicar un comando ejecutado correctamente. Para obtener una lista completa de los posibles códigos, consulte el archivo vdierror.h. Una lista de códigos de estado típicos para cada comando aparece en "Comandos" más adelante en este documento.
| |**bytesTransferred** |Se trata del número de bytes transferidos correctamente. Solo se devuelve para los comandos de transferencia de datos de lectura y escritura.
| |**position** |Esta es solo una respuesta al comando GetPosition.
        
| Valores devueltos | Argumento | Explicación
| ----- | ----- | ------ |
| |**NOERROR** |La finalización se anotó correctamente.
| |**VD_E_INVALID** |pCmd no era un comando activo.
| |**VD_E_ABORT** |Se señaló Abort.
| |**VD_E_PROTOCOL** |El dispositivo no está abierto.

**Notas**  Ninguna

## <a name="clientvirtualdevicesetsignalabort"></a>ClientVirtualDeviceSet::SignalAbort

**Propósito** Esta función se usa para indicar que debe producirse una finalización anómala.

**Sintaxis** 

   ```
   int ClientVirtualDeviceSet::SignalAbort ();
   ```

| Parámetros | Argumento | Explicación
| ----- | ----- | ------ |
| |None | No aplicable
        
| Valores devueltos | Argumento | Explicación
| ----- | ----- | ------ |
| |**NOERROR**|La notificación de Abort se envió correctamente.

**Notas** En cualquier momento, el cliente puede optar por anular la operación BACKUP o RESTORE. Esta rutina indica que todas las operaciones deben cesar. El estado del conjunto de dispositivos virtuales completo entra en un estado de finalización anómala. No se devuelven más comandos en ningún dispositivo. Todos los comandos incompletos se completan automáticamente y devuelven ERROR_OPERATION_ABORTED como código de finalización. El cliente debe llamar a ClientVirtualDeviceSet::Close una vez que haya finalizado de forma segura cualquier uso pendiente de los búferes proporcionados al cliente. Para obtener más información, consulte "Terminación anómala" anteriormente en este documento.

## <a name="clientvirtualdevicesetclose"></a>ClientVirtualDeviceSet::Close

**Propósito** Esta función cierra el conjunto de dispositivos virtuales creado por ClientVirtualDeviceSet::Create. Como resultado, se liberan todos los recursos asociados al conjunto de dispositivos virtuales.

**Sintaxis** 

   ```
   int ClientVirtualDeviceSet::Close ();
   ```

| Parámetros | Argumento | Explicación
| ----- | ----- | ------ |
| |None |No aplicable
        
| Valores devueltos | Argumento | Explicación
| ----- | ----- | ------ |
| |**NOERROR** |Se devuelve cuando el conjunto de dispositivos virtuales se ha cerrado correctamente.
| |**VD_E_PROTOCOL** |No se realizó ninguna acción porque el conjunto de dispositivos virtuales no estaba abierto.
| |**VD_E_OPEN** |Los dispositivos aún estaban abiertos.

**Notas** La invocación de Close es una declaración de cliente que indica que deben liberarse todos los recursos usados por el conjunto de dispositivos virtuales. El cliente debe asegurarse de que toda actividad que implique búferes de datos y dispositivos virtuales haya finalizado antes de invocar Close. Todas las interfaces de dispositivo virtual devueltas por OpenDevice se invalidan con Close.
El cliente puede emitir una llamada a Create en la interfaz del conjunto de dispositivos virtuales después de que se devuelva la llamada a Close. Esta llamada crearía un nuevo conjunto de dispositivos virtuales para una operación BACKUP o RESTORE posterior.
Si se llama a Close cuando uno o más dispositivos virtuales están todavía abiertos, se devuelve VD_E_OPEN. En este caso, SignalAbort se desencadena internamente para garantizar un apagado adecuado si es posible. Se publican los recursos de VDI. El cliente debe esperar una indicación VD_E_CLOSE en cada dispositivo antes de invocar ClientVirtualDeviceSet::Close. Si el cliente sabe que el conjunto de dispositivos virtuales ya está en un estado de finalización anómala, no debería esperar una indicación VD_E_CLOSE de GetCommand y puede invocar ClientVirtualDeviceSet::Close tan pronto como se termine la actividad en los búferes compartidos.
Para obtener más información, consulte "Terminación anómala" anteriormente en este documento.

## <a name="clientvirtualdevicesetopeninsecondary"></a>ClientVirtualDeviceSet::OpenInSecondary

**Propósito** Esta función abre el conjunto de dispositivos virtuales en un cliente secundario. El cliente principal ya debe haber usado Create y GetConfiguration para configurar el conjunto de dispositivos virtuales.

**Sintaxis** 
   
   ```
   int ClientVirtualDeviceSet::OpenInSecondary (
   char *   setName         // name of the set
   );
   ```

| Parámetros | Argumento | Explicación
| ----- | ----- | ------ |
| |**setName** |Identifica el conjunto. Este nombre distingue entre mayúsculas y minúsculas y debe coincidir con el nombre usado por el cliente principal cuando invocó ClientVirtualDeviceSet::Create.

| Valores devueltos | Argumento | Explicación
| ----- | ----- | ------ |
| |**NOERROR** |La función se ha realizado correctamente.
| |**VD_E_PROTOCOL** |No se ha creado el conjunto de dispositivos virtuales, ya se ha abierto en este cliente o el conjunto de dispositivos virtuales no está listo para aceptar solicitudes abiertas de clientes secundarios.
| |**VD_E_ABORT** |La operación se va a anular.

**Notas** Cuando se usa un modelo de varios procesos, el cliente principal es responsable de detectar la finalización normal y anómala de clientes secundarios.

## <a name="clientvirtualdevicesetgetbufferhandle"></a>ClientVirtualDeviceSet::GetBufferHandle

**Propósito** Es posible que algunas aplicaciones requieran más de un proceso para operar en los búferes devueltos por ClientVirtualDevice::GetCommand. En tales casos, el proceso que recibe el comando puede usar GetBufferHandle para obtener un identificador independiente del proceso que identifica el búfer. Este identificador se puede comunicar a cualquier otro proceso que también tenga abierto el mismo conjunto de dispositivos virtuales. Ese proceso utilizaría entonces ClientVirtualDeviceSet::MapBufferHandle para obtener la dirección del búfer. Es probable que la dirección que obtenga sea distinta a la de su asociado, ya que cada proceso puede asignar búferes en direcciones diferentes.

**Sintaxis** 

   ```
   int ClientVirtualDeviceSet::GetBufferHandle (
   uint8_t*     pBuffer,        // in: buffer address
   unsigned int*        pBufferHandle   // out: buffer handle
   );
   ```

| Parámetros | Argumento | Explicación
| ----- | ----- | ------ |
| |**pBuffer** |Se trata de la dirección de un búfer que se obtiene de un comando de lectura o escritura.
| |**BufferHandle** |Devuelve un identificador único para el búfer.

| Valores devueltos | Argumento | Explicación
| ----- | ----- | ------ |
| |**NOERROR** |La función se ha realizado correctamente.
| |**VD_E_PROTOCOL** |El conjunto de dispositivos virtuales no está abierto actualmente.
| |**VD_E_INVALID** |pBuffer no es una dirección válida.

Notas El proceso que invoca la función GetBufferHandle es responsable de invocar ClientVirtualDevice::CompleteCommand cuando se complete la transferencia de datos.

## <a name="clientvirtualdevicesetmapbufferhandle"></a>ClientVirtualDeviceSet::MapBufferHandle

**Propósito** Esta función se usa para obtener una dirección de búfer válida de un identificador de búfer obtenido de algún otro proceso. 

**Sintaxis** 

   ```
   int ClientVirtualDeviceSet::MapBufferHandle (
   i        nt  dwBuffer,   // in: buffer handle
   uint8_t**    ppBuffer        // out: buffer address
   );
   ```

| Parámetros | Argumento | Explicación
| ----- | ----- | ------ |
| |**dwBuffer** |Identificador devuelto por ClientVirtualDeviceSet::GetBufferHandle.
| |**ppBuffer** |Dirección del búfer que es válido en el proceso actual.

| Valores devueltos | Argumento | Explicación
| ----- | ----- | ------ |
| |**NOERROR** |La función se ha realizado correctamente.
| |**VD_E_PROTOCOL** |El conjunto de dispositivos virtuales no está abierto actualmente.
| |**VD_E_INVALID** |ppBuffer es un identificador no válido.

**Notas** Hay que tener cuidado de comunicar correctamente los identificadores. Los identificadores son locales para un único conjunto de dispositivos virtuales. Los procesos de asociados que comparten un identificador deben asegurarse de que los identificadores de búfer solo se usan dentro del ámbito del conjunto de dispositivos virtuales del que se obtuvo originalmente el búfer.



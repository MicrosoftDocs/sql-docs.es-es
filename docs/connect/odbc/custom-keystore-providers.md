---
title: Proveedores de almacén de claves personalizados
description: Obtenga información sobre cómo implementar un proveedor de almacén de claves personalizado para usarlo con ODBC Driver for SQL Server y la característica Always Encrypted.
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a6166d7d-ef34-4f87-bd1b-838d3ca59ae7
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: f1f075c7ea2e80ec1760012c33ed9f719837c036
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837436"
---
# <a name="custom-keystore-providers"></a>Proveedores de almacén de claves personalizados
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Información general

La característica de cifrado de columnas de SQL Server 2016 requiere que el cliente recupere las claves de cifrado de columnas cifradas (ECEK) que se almacenan en el servidor y que, luego, se descifren en claves de cifrado de columnas (CEK) para acceder a los datos almacenados en columnas cifradas. Las ECEK se cifran mediante claves maestras de columna (CMK), y la seguridad de la CMK es importante para la seguridad del cifrado de columnas. Por lo tanto, la CMK debe almacenarse en una ubicación segura; la finalidad de un proveedor de almacén de claves de cifrado de columnas es proporcionar una interfaz que permita que el controlador ODBC tenga acceso a estas CMK almacenadas de forma segura. En el caso de usuarios con su propio almacenamiento seguro, la interfaz del proveedor de almacén de claves personalizado proporciona un marco para implementar el acceso a almacenamiento seguro de la CMK del controlador ODBC, que se puede usar para realizar el cifrado y el descifrado de CEK.

Cada proveedor de almacén de claves contiene y administra una o varias CMK, que se identifican mediante rutas de acceso de la clave: cadenas de un formato definido por el proveedor. Esta CMK, junto con el algoritmo de cifrado, también una cadena definida por el proveedor, se pueden usar para realizar el cifrado de una CEK y el descifrado de una ECEK. El algoritmo, junto con ECEK y el nombre del proveedor, se almacenan en los metadatos de cifrado de la base de datos. Para más información, vea [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) y [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md). Por lo tanto, las dos operaciones fundamentales de administración de claves son:

```
CEK = DecryptViaCEKeystoreProvider(CEKeystoreProvider_name, Key_path, Key_algorithm, ECEK)

-and-

ECEK = EncryptViaCEKeystoreProvider(CEKeyStoreProvider_name, Key_path, Key_algorithm, CEK)
```

donde `CEKeystoreProvider_name` se usa para identificar el proveedor de almacén de claves de cifrado de columnas específico (CEKeystoreProvider), y CEKeystoreProvider usa los demás argumentos para cifrar o descifrar la (E)CEK. El nombre y la ruta de acceso de la clave se proporcionan en los metadatos de CMK, mientras que el algoritmo y el valor de ECEK se proporcionan en los metadatos de CEK. Pueden existir varios proveedores de almacén de claves junto con los proveedores integrados predeterminados. Tras realizar la operación que requiere la CEK, el controlador usa los metadatos de CMK para buscar el proveedor de almacén de claves adecuado por el nombre y ejecuta su operación de descripción, que se puede expresar como:

```
CEK = CEKeyStoreProvider_specific_decrypt(Key_path, Key_algorithm, ECEK)
```

Aunque el controlador no necesita cifrar las CEK, puede que una herramienta de administración de claves deba hacerlo para implementar operaciones como creación y rotación de CMK. Estas acciones requieren realizar la operación inversa:

```
ECEK = CEKeyStoreProvider_specific_encrypt(Key_path, Key_algorithm, CEK)
```

### <a name="cekeystoreprovider-interface"></a>Interfaz CEKeyStoreProvider

En este documento se describe de forma detallada la interfaz CEKeyStoreProvider. Microsoft ODBC Driver for SQL Server puede usar un proveedor de almacén de claves que implemente esta interfaz. Los implementadores de CEKeyStoreProvider pueden usar esta guía para desarrollar proveedores de almacenes de claves personalizados que pueda usar el controlador.

Una biblioteca de proveedores de almacenes de claves ("biblioteca de proveedores") es una biblioteca de vínculos dinámicos que se puede cargar con el controlador ODBC y que contiene uno o varios proveedores de almacenes de claves. Una biblioteca de proveedores debe exportar el símbolo `CEKeystoreProvider` y ser este la dirección de una matriz de punteros con terminación NULL a estructuras `CEKeystoreProvider`, uno por cada proveedor de almacén de claves de la biblioteca.

Una estructura `CEKeystoreProvider` define los puntos de entrada de un único proveedor de almacén de claves:

```
typedef struct CEKeystoreProvider {
    wchar_t *Name;
    int (*Init)(CEKEYSTORECONTEXT *ctx, errFunc *onError);
    int (*Read)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int *len);
    int (*Write)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len);
    int (*DecryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *ecek,
                        unsigned short ecekLen,
                        unsigned char **cekOut,
                        unsigned short *cekLen);
    int (*EncryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *cek,
                        unsigned short cekLen,
                        unsigned char **ecekOut,
                        unsigned short *ecekLen);
    void (*Free)();
} CEKEYSTOREPROVIDER;
```

|Nombre del campo|Descripción|
|:--|:--|
|`Name`|Nombre del proveedor de almacén de claves. No debe ser igual al de cualquier otro proveedor de almacén de claves cargado anteriormente por el controlador o que exista en esta biblioteca. Cadena de caracteres anchos* terminada en NULL.|
|`Init`|Función de inicialización. Si no se necesita una función de inicialización, este campo puede ser NULL.|
|`Read`|Función de lectura del proveedor. Puede ser NULL si no es necesaria.|
|`Write`|Función de escritura del proveedor. Necesaria si Read no es NULL. Puede ser NULL si no es necesaria.|
|`DecryptCEK`|Función de descifrado de ECEK. Esta función es el motivo de la existencia de un proveedor de almacén de claves y no debe ser NULL.|
|`EncryptCEK`|Función de cifrado de CEK. El controlador no llama a esta función, pero se proporciona para permitir el acceso mediante programación a la creación de ECEK mediante las herramientas de administración de claves. Puede ser NULL si no es necesaria.|
|`Free`|Función de terminación. Puede ser NULL si no es necesaria.|

A excepción de Free, las funciones de esta interfaz tienen un par de parámetros, **ctx** y **onError**. El primero identifica el contexto en el que se llama a la función, mientras que el último se usa para informar de errores. Para más información, vea [Contextos](#context-association) y [Control de errores](#error-handling) a continuación.

```
int Init(CEKEYSTORECONTEXT *ctx, errFunc onError);
```
Nombre del marcador de posición de una función de inicialización definida por el proveedor. El controlador llama a esta función una vez, después de que se haya cargado un proveedor, pero antes de la primera vez que se necesita para realizar las solicitudes Read()/Write() o de descifrado de ECEK. Use esta función para realizar cualquier inicialización que se necesite.

|Argumento|Descripción|
|:--|:--|
|`ctx`|[Input] Contexto de la operación.|
|`onError`|[Input] Función de informe de errores.|
|`Return Value`|Devuelve un valor distinto de cero para indicar que se ha realizado correctamente, o cero para indicar un error.|

```
int Read(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int *len);
```

Nombre del marcador de posición de una función de comunicación definida por el proveedor. El controlador llama a esta función cuando la aplicación solicita la lectura de datos de un proveedor (en el que se ha escrito previamente) mediante el atributo de conexión SQL_COPT_SS_CEKEYSTOREDATA, lo que permite a la aplicación leer datos arbitrarios del proveedor. Para más información, vea [Comunicación con proveedores de almacén de claves](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers).

|Argumento|Descripción|
|:--|:--|
|`ctx`|[Input] Contexto de la operación.|
|`onError`|[Input] Función de informe de errores.|
|`data`|[Output] Puntero a un búfer en el que el proveedor escribe datos que va a leer la aplicación. Este búfer corresponde al campo de datos de la estructura CEKEYSTOREDATA.|
|`len`|[InOut] Puntero a un valor de longitud; tras la entrada, esta es la longitud máxima del búfer de datos, y el proveedor no escribirá más de *len bytes en él. Tras la devolución, el proveedor debe actualizar *len con el número de bytes escrito.|
|`Return Value`|Devuelve un valor distinto de cero para indicar que se ha realizado correctamente, o cero para indicar un error.|

```
int Write(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int len);
```
Nombre del marcador de posición de una función de comunicación definida por el proveedor. El controlador llama a esta función cuando la aplicación solicita la escritura de datos en un proveedor mediante el atributo de conexión SQL_COPT_SS_CEKEYSTOREDATA, lo que permite a la aplicación escribir datos arbitrarios en el proveedor. Para más información, vea [Comunicación con proveedores de almacén de claves](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers).

|Argumento|Descripción|
|:--|:--|
|`ctx`|[Input] Contexto de la operación.|
|`onError`|[Input] Función de informe de errores.|
|`data`|[Input] Puntero a un búfer que contiene los datos que va a leer el proveedor. Este búfer corresponde al campo de datos de la estructura CEKEYSTOREDATA. El proveedor no debe leer más de len bytes de este búfer.|
|`len`|[Input] El número de bytes disponible en los datos. Corresponde al campo dataSize de la estructura CEKEYSTOREDATA.|
|`Return Value`|Devuelve un valor distinto de cero para indicar que se ha realizado correctamente, o cero para indicar un error.|

```
int (*DecryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen);
```
Nombre del marcador de posición de una función de descifrado de ECEK definida por el proveedor. El controlador llama a esta función para descifrar una ECEK cifrada mediante una CMK asociada a este proveedor en una CEK.

|Argumento|Descripción|
|:--|:--|
|`ctx`|[Input] Contexto de la operación.|
|`onError`|[Input] Función de informe de errores.|
|`keyPath`|[Input] El valor del atributo de metadatos [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md) de la CMK a la que hace referencia la ECEK dada. Cadena de caracteres anchos* terminada en NULL. Este valor pretende identificar una CMK administrada por este proveedor.|
|`alg`|[Input] El valor del atributo de metadatos [ALGORITHM](../../t-sql/statements/create-column-encryption-key-transact-sql.md) de la ECEK dada. Cadena de caracteres anchos* terminada en NULL. Este valor pretende identificar el algoritmo de cifrado usado para cifrar la ECEK dada.|
|`ecek`|[Input] Puntero a la ECEK que se va a descifrar.|
|`ecekLen`|[Input] Longitud de la ECEK.|
|`cekOut`|[Output] El proveedor asignará memoria para la ECEK descifrada y escribirá su dirección en el puntero al que apunta mediante cekOut. Debe ser posible liberar este bloque de memoria mediante la función [LocalFree](/windows/desktop/api/winbase/nf-winbase-localfree) (Windows) o free (Linux/macOS). Si no se asigna memoria debido a un error o algo diferente, el proveedor establecerá *cekOut en un puntero NULL.|
|`cekLen`|[Output] El proveedor escribirá en la dirección a la que apunta mediante cekLen la longitud de la ECEK descifrada que ha escrito en **cekOut.|
|`Return Value`|Devuelve un valor distinto de cero para indicar que se ha realizado correctamente, o cero para indicar un error.|

```
int (*EncryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *cek,unsigned short cekLen, unsigned char **ecekOut, unsigned short *ecekLen);
```
Nombre del marcador de posición de una función de cifrado de CEK definida por el proveedor. El controlador no llama a esta función ni expone su funcionalidad mediante la interfaz de ODBC, pero se proporciona para permitir acceso mediante programación a la creación de ECEK mediante las herramientas de administración de claves.

|Argumento|Descripción|
|:--|:--|
|`ctx`|[Input] Contexto de la operación.|
|`onError`|[Input] Función de informe de errores.|
|`keyPath`|[Input] El valor del atributo de metadatos [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md) de la CMK a la que hace referencia la ECEK dada. Cadena de caracteres anchos* terminada en NULL. Tiene como fin identificar una CMK administrada por este proveedor.|
|`alg`|[Input] El valor del atributo de metadatos [ALGORITHM](../../t-sql/statements/create-column-encryption-key-transact-sql.md) de la ECEK dada. Cadena de caracteres anchos* terminada en NULL. Este valor pretende identificar el algoritmo de cifrado usado para cifrar la ECEK dada.|
|`cek`|[Input] Puntero a la CEK que se va a cifrar.|
|`cekLen`|[Input] Longitud de la CEK.|
|`ecekOut`|[Output] El proveedor asignará memoria para la CEK cifrada y escribirá su dirección en el puntero al que apunta mediante ecekOut. Debe ser posible liberar este bloque de memoria mediante la función [LocalFree](/windows/desktop/api/winbase/nf-winbase-localfree) (Windows) o free (Linux/macOS). Si no se asigna memoria debido a un error o algo diferente, el proveedor establecerá *ecekOut en un puntero nulo.|
|`ecekLen`|[Output] El proveedor escribirá en la dirección a la que apunta mediante ecekLen la longitud de la CEK cifrada que ha escrito en **ecekOut.|
|`Return Value`|Devuelve un valor distinto de cero para indicar que se ha realizado correctamente, o cero para indicar un error.|

```
void (*Free)();
```
Nombre del marcador de posición de una función de terminación definida por el proveedor. El controlador puede llamar a esta función tras la terminación normal del proceso.

> [!NOTE]
> *Las cadenas de caracteres anchos son caracteres de 2 bytes (UTF-16) debido a cómo las almacena SQL Server.*


### <a name="error-handling"></a>Tratamiento de errores

Dado que se pueden producir errores durante el procesamiento de un proveedor, se proporciona un mecanismo para permitirle notificar los errores de vuelta al controlador con detalles más específicos que un valor booleano de acierto o error. Muchas de las funciones tienen un par de parámetros, **ctx** y **onError**, que se usan juntos con este fin, además del valor de devolución de acierto o error.

El parámetro **ctx** identifica el contexto en el que tiene lugar una operación del proveedor.

El parámetro **onError** apunta a una función de informe de errores, con el siguiente prototipo:

`typedef void errFunc(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...);`

|Argumento|Descripción|
|:--|:--|
|`ctx`|[Input] El contexto en el que se informa del error.|
|`msg`|[Input] El mensaje de error del que se debe informar. Cadena de caracteres anchos terminada en NULL. Para permitir que exista información parametrizada, esta cadena puede contener secuencias de formato de inserción de la forma aceptada por la función [FormatMessage](/windows/desktop/api/winbase/nf-winbase-formatmessage). Se puede especificar funcionalidad extendida mediante este parámetro, como se describe a continuación.|
|...|[Input] Parámetros variádicos adicionales para ajustar especificadores de formato en MSG, según sea adecuado.|

Para informar de un error que se ha producido, el proveedor llama a onError, y proporciona el parámetro de contexto pasado a la función del proveedor por el controlador y un mensaje de error con parámetros adicionales opcionales a los que se aplica formato en él. El proveedor puede llamar a esta función varias veces para publicar varios mensajes de error de forma consecutiva en una invocación de función del proveedor. Por ejemplo:

```
    if (!doSomething(...))
    {
        onError(ctx, L"An error occurred in doSomething.");
        onError(ctx, L"Additional error message with more details.");
        return 0;
    }
```


El parámetro `msg` es normalmente una cadena de caracteres anchos, pero están disponibles extensiones adicionales:

Usando uno de los valores predefinidos especiales con la macro IDS_MSG, se pueden utilizar mensajes de error ya existentes y en una forma localizada en el controlador. Por ejemplo, si un proveedor no puede asignar memoria, se puede usar el mensaje `IDS_S1_001` "Error de asignación de memoria":

`onError(ctx, IDS_MSG(IDS_S1_001));`

Para que el controlador reconozca el error, la función del proveedor debe devolver un error. Cuando un error ocurre en el contexto de una operación de ODBC, los errores publicados se volverán accesibles en el controlador de conexiones o instrucciones mediante el mecanismo de diagnóstico de ODBC estándar (`SQLError`, `SQLGetDiagRec` y `SQLGetDiagField`).


### <a name="context-association"></a>Asociación de contexto

La estructura `CEKEYSTORECONTEXT`, además de proporcionar contexto para la devolución de llamada de error, se puede usar también para determinar el contexto de ODBC en el que se ejecuta una operación del proveedor. Este contexto permite a un proveedor asociar datos a cada uno de estos contextos, por ejemplo, para implementar la configuración por conexión. Con esta finalidad, la estructura contiene tres punteros opacos correspondientes al entorno, la conexión y el contexto de la instrucción:

```
typedef struct CEKeystoreContext
{
void *envCtx;
void *dbcCtx;
void *stmtCtx;
} CEKEYSTORECONTEXT;
```

|Campo|Descripción|
|:--|:--|
|`envCtx`|Contexto de entorno.|
|`dbcCtx`|Contexto de conexión.|
|`stmtCtx`|Contexto de instrucción.|

Cada uno de estos contextos es un valor opaco que, aunque no es igual que el controlador ODBC correspondiente, se puede usar como identificador único para el manipulador: si el manipulador *X* está asociado con el valor de contexto *Y*, entonces no hay otros manipuladores de entorno, conexión o instrucción que existan al mismo tiempo, ya que *X* tendrá un valor de contexto de *Y*, y no se asociará ningún otro valor de contexto al manipulador *X*. Si la operación del proveedor que se va a realizar no tiene un contexto de manipulador determinado (por ejemplo, llamadas SQLSetConnectAttr para cargar y configurar proveedores, en los que no hay ningún manipulador de instrucción), el valor de contexto correspondiente en la estructura es NULL.


## <a name="example"></a>Ejemplo

### <a name="keystore-provider"></a>Proveedor de almacén de claves

El código siguiente es un ejemplo de una implementación mínima de proveedor de almacén de claves.

```
/* Custom Keystore Provider Example

Windows:   compile with cl MyKSP.c /LD /MD /link /out:MyKSP.dll
Linux/macOS: compile with gcc -fshort-wchar -fPIC -o MyKSP.so -shared MyKSP.c

 */

#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#endif

#include <stdlib.h>
#include <sqltypes.h>
#include "msodbcsql.h"
#include <sql.h>
#include <sqlext.h>

int __stdcall KeystoreInit(CEKEYSTORECONTEXT *ctx, errFunc *onError) {
    printf("KSP Init() function called\n");
    return 1;
}

static unsigned char *g_encryptKey;
static unsigned int g_encryptKeyLen;

int __stdcall KeystoreWrite(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len) {
    printf("KSP Write() function called (%d bytes)\n", len);
    if (len) {
        if (g_encryptKey)
            free(g_encryptKey);
        g_encryptKey = malloc(len);
        if (!g_encryptKey) {
            onError(ctx, L"Memory Allocation Error");
            return 0;
        }
        memcpy(g_encryptKey, data, len);
        g_encryptKeyLen = len;
    }
    return 1;
}

// Very simple "encryption" scheme - rotating XOR with the key
int __stdcall KeystoreDecrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg,
    unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen) {
    unsigned int i;
    printf("KSP Decrypt() function called (keypath=%S alg=%S ecekLen=%u)\n", keyPath, alg, ecekLen);
    if (wcscmp(keyPath, L"TheOneAndOnlyKey")) {
        onError(ctx, L"Invalid key path");
        return 0;
    }
    if (wcscmp(alg, L"none")) {
        onError(ctx, L"Invalid algorithm");
        return 0;
    }
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
#ifndef _WIN32
    *cekOut = malloc(ecekLen);
#else
    *cekOut = LocalAlloc(LMEM_FIXED, ecekLen);
#endif
    if (!*cekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *cekLen = ecekLen;
    for (i = 0; i < ecekLen; i++)
        (*cekOut)[i] = ecek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

// Note that in the provider interface, this function would be referenced via the CEKEYSTOREPROVIDER
// structure. However, that does not preclude keystore providers from exporting their own functions,
// as illustrated by this example where the encryption is performed via a separate function (with a
// different prototype than the one in the KSP interface.)
#ifdef _WIN32
__declspec(dllexport)
#endif
int KeystoreEncrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError,
    unsigned char *cek, unsigned short cekLen,
    unsigned char **ecekOut, unsigned short *ecekLen) {
    unsigned int i;
    printf("KSP Encrypt() function called (cekLen=%u)\n", cekLen);
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
    *ecekOut = malloc(cekLen);
    if (!*ecekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *ecekLen = cekLen;
    for (i = 0; i < cekLen; i++)
        (*ecekOut)[i] = cek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

CEKEYSTOREPROVIDER MyCustomKSPName_desc = {
    L"MyCustomKSPName",
    KeystoreInit,
    0,
    KeystoreWrite,
    KeystoreDecrypt,
    0
};

#ifdef _WIN32
__declspec(dllexport)
#endif
CEKEYSTOREPROVIDER *CEKeystoreProvider[] = {
    &MyCustomKSPName_desc,
    0
};
```

### <a name="odbc-application"></a>Aplicación ODBC

El código siguiente es una aplicación de demostración que usa el proveedor de almacén de claves anterior. Al ejecutarlo, asegúrese de que la biblioteca de proveedores está en el mismo directorio que el archivo binario de la aplicación y que la cadena de conexión especifica el valor de `ColumnEncryption=Enabled` o un DNS que lo contiene.

```
/*
 Example application for demonstration of custom keystore provider usage

Windows:   compile with cl /MD kspapp.c /link odbc32.lib
Linux/macOS: compile with gcc -o kspapp -fshort-wchar kspapp.c -lodbc -ldl
 
 usage: kspapp connstr

 */

#define KSPNAME L"MyCustomKSPName"
#define PROV_ENCRYPT_KEY "JHKCWYT06N3RG98J0MBLG4E3"

#include <stdio.h>
#include <stdlib.h>
#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#include <dlfcn.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include "msodbcsql.h"

/* Convenience functions */

int checkRC(SQLRETURN rc, char *msg, int ret, SQLHANDLE h, SQLSMALLINT ht) {
    if (rc == SQL_ERROR) {
        fprintf(stderr, "Error occurred upon %s\n", msg);
        if (h) {
            SQLSMALLINT i = 0;
            SQLSMALLINT outlen = 0;
            char errmsg[1024];
            while ((rc = SQLGetDiagField(
                ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
                || rc == SQL_SUCCESS_WITH_INFO) {
                fprintf(stderr, "Err#%d: %s\n", i, errmsg);
            }
        }
        if (ret)
            exit(ret);
        return 0;
    }
    else if (rc == SQL_SUCCESS_WITH_INFO && h) {
        SQLSMALLINT i = 0;
        SQLSMALLINT outlen = 0;
        char errmsg[1024];
        printf("Success with info for %s:\n", msg);
        while ((rc = SQLGetDiagField(
            ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
            || rc == SQL_SUCCESS_WITH_INFO) {
            fprintf(stderr, "Msg#%d: %s\n", i, errmsg);
        }
    }
    return 1;
}

void postKspError(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...) {
    if (msg > (wchar_t*)65535)
        wprintf(L"Provider emitted message: %s\n", msg);
    else
        wprintf(L"Provider emitted message ID %d\n", msg);
}

int main(int argc, char **argv) {
    char sqlbuf[1024];
    SQLHENV env;
    SQLHDBC dbc;
    SQLHSTMT stmt;
    SQLRETURN rc;
    unsigned char CEK[32];
    unsigned char *ECEK;
    unsigned short ECEKlen;
#ifdef _WIN32
    HMODULE hProvLib;
#else
    void *hProvLib;
#endif
    CEKEYSTORECONTEXT ctx = {0};
    CEKEYSTOREPROVIDER **ppKsp, *pKsp;
    int(__stdcall *pEncryptCEK)(CEKEYSTORECONTEXT *, errFunc *, unsigned char *, unsigned short, unsigned char **, unsigned short *);
    int i;
    if (argc < 2) {
        fprintf(stderr, "usage: kspapp connstr\n");
        return 1;
    }

    /* Load the provider library */
#ifdef _WIN32
    if (!(hProvLib = LoadLibrary("MyKSP.dll"))) {
#else
    if (!(hProvLib = dlopen("./MyKSP.so", RTLD_NOW))) {
#endif
        fprintf(stderr, "Error loading KSP library\n");
        return 2;
    }
#ifdef _WIN32
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)GetProcAddress(hProvLib, "CEKeystoreProvider"))) {
#else
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)dlsym(hProvLib, "CEKeystoreProvider"))) {
#endif
        fprintf(stderr, "The export CEKeystoreProvider was not found in the KSP library\n");
        return 3;
    }
    while (pKsp = *ppKsp++) {
        if (!memcmp(KSPNAME, pKsp->Name, sizeof(KSPNAME)))
            goto FoundProv;
    }
    fprintf(stderr, "Could not find provider in the library\n");
    return 4;
FoundProv:
    if (pKsp->Init && !pKsp->Init(&ctx, postKspError)) {
        fprintf(stderr, "Could not initialize provider\n");
        return 5;
    }
#ifdef _WIN32
    if (!(pEncryptCEK = (LPVOID)GetProcAddress(hProvLib, "KeystoreEncrypt"))) {
#else
    if (!(pEncryptCEK = dlsym(hProvLib, "KeystoreEncrypt"))) {
#endif
        fprintf(stderr, "The export KeystoreEncrypt was not found in the KSP library\n");
        return 6;
    }
    if (!pKsp->Write) {
        fprintf(stderr, "Provider does not support configuration\n");
        return 7;
    }

    /* Configure the provider with the key */
    if (!pKsp->Write(&ctx, postKspError, PROV_ENCRYPT_KEY, strlen(PROV_ENCRYPT_KEY))) {
        fprintf(stderr, "Error writing to KSP\n");
        return 8;
    }

    /* Generate a CEK and encrypt it with the provider */
    srand(time(0) ^ getpid());
    for (i = 0; i < sizeof(CEK); i++)
        CEK[i] = rand();

    if (!pEncryptCEK(&ctx, postKspError, CEK, sizeof(CEK), &ECEK, &ECEKlen)) {
        fprintf(stderr, "Error encrypting CEK\n");
        return 9;
    }

    /* Connect to Server */
    rc = SQLAllocHandle(SQL_HANDLE_ENV, NULL, &env);
    checkRC(rc, "allocating environment handle", 2, 0, 0);
    rc = SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0);
    checkRC(rc, "setting ODBC version to 3.0", 3, env, SQL_HANDLE_ENV);
    rc = SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc);
    checkRC(rc, "allocating connection handle", 4, env, SQL_HANDLE_ENV);
    rc = SQLDriverConnect(dbc, 0, argv[1], strlen(argv[1]), NULL, 0, NULL, SQL_DRIVER_NOPROMPT);
    checkRC(rc, "connecting to data source", 5, dbc, SQL_HANDLE_DBC);
    rc = SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt);
    checkRC(rc, "allocating statement handle", 6, dbc, SQL_HANDLE_DBC);

    /* Create a CMK definition on the server */
    {
        static char cmkSql[] = "CREATE COLUMN MASTER KEY CustomCMK WITH ("
            "KEY_STORE_PROVIDER_NAME = 'MyCustomKSPName',"
            "KEY_PATH = 'TheOneAndOnlyKey')";
        printf("Create CMK: %s\n", cmkSql);
        SQLExecDirect(stmt, cmkSql, SQL_NTS);
    }

    /* Create a CEK definition on the server */
    {
        const char cekSqlBefore[] = "CREATE COLUMN ENCRYPTION KEY CustomCEK WITH VALUES ("
            "COLUMN_MASTER_KEY = CustomCMK,"
            "ALGORITHM = 'none',"
            "ENCRYPTED_VALUE = 0x";
        char *cekSql = malloc(sizeof(cekSqlBefore) + 2 * ECEKlen + 2); /* 1 for ')', 1 for null terminator */
        strcpy(cekSql, cekSqlBefore);
        for (i = 0; i < ECEKlen; i++)
            sprintf(cekSql + sizeof(cekSqlBefore) - 1 + 2 * i, "%02x", ECEK[i]);
        strcat(cekSql, ")");
        printf("Create CEK: %s\n", cekSql);
        SQLExecDirect(stmt, cekSql, SQL_NTS);
        free(cekSql);
#ifdef _WIN32
        LocalFree(ECEK);
#else
        free(ECEK);
#endif
    }

#ifdef _WIN32
    FreeLibrary(hProvLib);
#else
    dlclose(hProvLib);
#endif

    /* Create a table with encrypted columns */
    {
        static char *tableSql = "CREATE TABLE CustomKSPTestTable ("
            "c1 int,"
            "c2 varchar(255) COLLATE Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = CustomCEK, ENCRYPTION_TYPE = DETERMINISTIC, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'))";
        printf("Create table: %s\n", tableSql);
        SQLExecDirect(stmt, tableSql, SQL_NTS);
    }

    /* Load provider into the ODBC Driver and configure it */
    {
        unsigned char ksd[sizeof(CEKEYSTOREDATA) + sizeof(PROV_ENCRYPT_KEY) - 1];
        CEKEYSTOREDATA *pKsd = (CEKEYSTOREDATA*)ksd;
        pKsd->name = L"MyCustomKSPName";
        pKsd->dataSize = sizeof(PROV_ENCRYPT_KEY) - 1;
        memcpy(pKsd->data, PROV_ENCRYPT_KEY, sizeof(PROV_ENCRYPT_KEY) - 1);
#ifdef _WIN32
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "MyKSP.dll", SQL_NTS);
#else
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "./MyKSP.so", SQL_NTS);
#endif
        checkRC(rc, "Loading KSP into ODBC Driver", 7, dbc, SQL_HANDLE_DBC);
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREDATA, (SQLPOINTER)pKsd, SQL_IS_POINTER);
        checkRC(rc, "Configuring the KSP", 7, dbc, SQL_HANDLE_DBC);
    }

    /* Insert some data */
    {
        int c1;
        char c2[256];
        rc = SQLBindParameter(stmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 0, 0, &c1, 0, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        rc = SQLBindParameter(stmt, 2, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_VARCHAR, 255, 0, c2, 255, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        for (i = 0; i < 10; i++) {
            c1 = i * 10 + i + 1;
            sprintf(c2, "Sample data %d for column 2", i);
            rc = SQLExecDirect(stmt, "INSERT INTO CustomKSPTestTable (c1, c2) values (?, ?)", SQL_NTS);
            checkRC(rc, "Inserting rows query", 10, stmt, SQL_HANDLE_STMT);
        }
        printf("(Encrypted) data has been inserted into the [CustomKSPTestTable]. You may inspect the data now.\n"
            "Press Enter to continue...\n");
        getchar();
    }

    /* Retrieve the data */
    {
        int c1;
        char c2[256];
        rc = SQLBindCol(stmt, 1, SQL_C_LONG, &c1, 0, 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLBindCol(stmt, 2, SQL_C_CHAR, c2, sizeof(c2), 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLExecDirect(stmt, "SELECT c1, c2 FROM CustomKSPTestTable", SQL_NTS);
        checkRC(rc, "Retrieving rows query", 12, stmt, SQL_HANDLE_STMT);
        while (SQL_SUCCESS == (rc = SQLFetch(stmt)))
            printf("Retrieved data: c1=%d c2=%s\n", c1, c2);
        SQLFreeStmt(stmt, SQL_CLOSE);
        printf("Press Enter to clean up and exit...\n");
        getchar();
    }

    /* Clean up */
    {
        SQLExecDirect(stmt, "DROP TABLE CustomKSPTestTable", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN ENCRYPTION KEY CustomCEK", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN MASTER KEY CustomCMK", SQL_NTS);
        printf("Removed table, CEK, and CMK\n");
    }
    SQLDisconnect(dbc);
    SQLFreeHandle(SQL_HANDLE_DBC, dbc);
    SQLFreeHandle(SQL_HANDLE_ENV, env);
    return 0;
}

```

## <a name="see-also"></a>Consulte también

[Uso de Always Encrypted con el controlador ODBC](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)

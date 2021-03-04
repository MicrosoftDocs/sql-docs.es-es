---
description: Uso de la clasificación de datos con Microsoft ODBC Driver for SQL Server
title: Uso de la clasificación de datos con Microsoft ODBC Driver for SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: v-makouz
ms.author: v-makouz
manager: kenvh
ms.openlocfilehash: 3b97e6407f541657acee2b15097d3fbb877854a9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837420"
---
# <a name="data-classification"></a>Clasificación de datos
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Información general
Con el fin de administrar datos confidenciales, SQL Server y Azure SQL Server presentaron la capacidad de proporcionar columnas de base de datos con metadatos confidenciales que permiten a la aplicación cliente administrar diferentes tipos de datos confidenciales (como datos de mantenimiento, financieros, etc.) según las directivas de protección de datos.

Para obtener más información sobre cómo asignar una clasificación a las columnas, consulte [Clasificación y detección de datos de SQL](../../relational-databases/security/sql-data-discovery-and-classification.md).

Microsoft ODBC Driver 17.2 permite la recuperación de estos metadatos a través de SQLGetDescField mediante el identificador de campo SQL_CA_SS_DATA_CLASSIFICATION.

## <a name="format"></a>Formato
SQLGetDescField tiene la siguiente sintaxis:

```  
SQLRETURN SQLGetDescField(  
     SQLHDESC        DescriptorHandle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     FieldIdentifier,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```
*DescriptorHandle*  
 [Entrada] Identificador de IRD (descriptor de fila de implementación). Se puede recuperar mediante una llamada a SQLGetStmtAttr con el atributo de instrucción SQL_ATTR_IMP_ROW_DESC
  
 *RecNumber*  
 [Input] 0
  
 *FieldIdentifier*  
 [Entrada] SQL_CA_SS_DATA_CLASSIFICATION
  
 *ValuePtr*  
 [Salida] Búfer de salida
  
 *BufferLength*  
 [Entrada] Longitud del búfer de salida en bytes

 *StringLengthPtr* [Salida] Puntero al búfer en el que se va a devolver el número total de bytes disponibles para devolver en *ValuePtr*.
 
> [!NOTE]
> Si no se conoce el tamaño del búfer, puede determinarse llamando a SQLGetDescField con *ValuePtr* como NULL y examinando el valor de *StringLengthPtr*.
 
Si la información de clasificación de datos no está disponible, se devolverá un error *Campo descriptor no válido*.

Tras una llamada correcta a SQLGetDescField, el búfer señalado por *ValuePtr* contendrá los siguientes datos:

 `nn nn [n sensitivitylabels] tt tt [t informationtypes] cc cc [c columnsensitivitys]`

> [!NOTE]
> `nn nn`, `tt tt` y `cc cc` son enteros multibyte, que se almacenan con el byte menos significativo en la dirección inferior.

*`sensitivitylabel`* y *`informationtype`* tienen el formato

 `nn [n bytes name] ii [i bytes id]`

*`columnsensitivity`* tiene el formato

 `nn nn [n sensitivityprops]`

En cada una de las columnas *(c)* , *`sensitivityprops`* de 4 bytes *n* están presentes:

 `ss ss tt tt`

Índice s - en la matriz *`sensitivitylabels`* , `FF FF` si no se etiqueta

Índice t - en la matriz *`informationtypes`* , `FF FF` si no se etiqueta


<br><br>
El formato de los datos se puede expresar como las siguientes pseudoestructuras:

```
struct IDnamePair {
 BYTE nameLen;
 USHORT name[nameLen];
 BYTE idLen;
 USHORT id[idLen];
};

struct SensitivityProp {
 USHORT labelIdx;
 USHORT infoTypeIdx;
};

USHORT nLabels;
struct IDnamePair labels[nLabels];
USHORT nInfoTypes;
struct IDnamePair infotypes[nInfoTypes];
USHORT nColumns;
struct {
 USHORT nProps;
 struct SensitivityProp[nProps];
} columnClassification[nColumns];
```


## <a name="code-sample"></a>Código de ejemplo
Aplicación de prueba que muestra cómo leer los metadatos de clasificación de datos. En Windows, se puede compilar mediante `cl /MD dataclassification.c /I (directory of msodbcsql.h) /link odbc32.lib` y ejecutar con una cadena de conexión, y una consulta SQL (que devuelve columnas clasificadas) como parámetros:

```
#ifdef _WIN32
#include <windows.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include <msodbcsql.h>
#include <stdio.h>
SQLHANDLE env, dbc, stmt;
void checkRC_exit(SQLRETURN rc, SQLHANDLE hand, SQLSMALLINT htype, int retcode, char *action)
{
    if ((rc == SQL_ERROR || rc == SQL_SUCCESS_WITH_INFO) && hand)
    {
        char msg[1024], state[6];
        int i = 0;
        SQLRETURN rc2;
        SQLINTEGER err;
        SQLSMALLINT lenout;
        while ((rc2 = SQLGetDiagRec(htype, hand, ++i, state, &err, msg, sizeof(msg), &lenout)) == SQL_SUCCESS ||
            rc2 == SQL_SUCCESS_WITH_INFO)
            printf("%d (%d)[%s]%s\n", i, err, state, msg);
    }
    if (rc == SQL_ERROR && retcode)
    {
        printf("Error occurred%s%s\n", action ? " upon " : "", action ? action : "");
        exit(retcode);
    }
}
void printLabelInfo(char *type, char **pptr)
{
    char *ptr = *pptr;
    unsigned short nlabels;
    printf("----- %s(%u) -----\n", type, nlabels = *(unsigned short*)ptr);
    ptr += sizeof(unsigned short);
    while (nlabels--)
    {
        int namelen, idlen;
        char *nameptr, *idptr;
        namelen = *ptr++;
        nameptr = ptr;
        ptr += namelen * 2;
        idlen = *ptr++;
        idptr = ptr;
        ptr += idlen * 2;
        wprintf(L"Name: \"%.*s\" Id: \"%.*s\"\n", namelen, nameptr, idlen, idptr);
    }
    *pptr = ptr;
}
int main(int argc, char **argv)
{
    unsigned char *dcbuf;
    unsigned int dclen = 0;
    SQLRETURN rc;
    SQLHANDLE ird;
    if (argc < 3)
    {
        fprintf(stderr, "usage: dataclassification connstr query\n");
        return 1;
    }
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_ENV, 0, &env), 0, 0,
        2, "allocate environment");
    checkRC_exit(SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0), env, SQL_HANDLE_ENV,
        3, "set ODBC version");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc), env, SQL_HANDLE_ENV,
        4, "allocate connection");
    checkRC_exit(SQLDriverConnect(dbc, 0, argv[1], SQL_NTS, 0, 0, 0, SQL_DRIVER_NOPROMPT), dbc, SQL_HANDLE_DBC,
        5, "connect to server");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt), dbc, SQL_HANDLE_DBC,
        6, "allocate statement");
    checkRC_exit(SQLExecDirect(stmt, argv[2], SQL_NTS), stmt, SQL_HANDLE_STMT,
        7, "execute query");
    checkRC_exit(SQLGetStmtAttr(stmt, SQL_ATTR_IMP_ROW_DESC, (SQLPOINTER)&ird, SQL_IS_POINTER, 0), stmt, SQL_HANDLE_STMT,
        8, "get IRD handle");
    rc = SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, 0, &dclen);
    if (rc != SQL_SUCCESS && rc != SQL_SUCCESS_WITH_INFO)
    {
        checkRC_exit(rc, ird, SQL_HANDLE_DESC, 0, 0);
        printf("Error reading SQL_CA_SS_DATA_CLASSIFICATION IRD field. Ensure that the driver and\n"
            "server both support the Data Classification feature, and that the query returns columns\n"
            "with classification information.\n");
    }
    else
    {
        SQLINTEGER dclenout;
        unsigned char *dcptr;
        unsigned short ncols;
        printf("Data Classification information (%u bytes):\n", dclen);
        if (!(dcbuf = malloc(dclen)))
        {
            printf("Memory Allocation Error");
            return 9;
        }
        checkRC_exit(SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, dclen, &dclenout),
            ird, SQL_HANDLE_DESC, 10, "reading SQL_CA_SS_DATA_CLASSIFICATION");
        dcptr = dcbuf;
        printLabelInfo("Labels", &dcptr);
        printLabelInfo("Information Types", &dcptr);
        printf("----- Column Sensitivities(%u) -----\n", ncols = *(unsigned short*)dcptr);
        dcptr += sizeof(unsigned short);
        while (ncols--)
        {
            unsigned short nprops = *(unsigned short*)dcptr;
            dcptr += sizeof(unsigned short);
            while (nprops--)
            {
                unsigned short labelidx, typeidx;
                labelidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                typeidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                printf(labelidx == 0xFFFF ? "(none) " : "%u ", labelidx);
                printf(typeidx == 0xFFFF ? "(none)\n" : "%u\n", typeidx);
            }
            printf("-----\n");
        }
        if (dcptr != dcbuf + dclen)
        {
            printf("Error: unexpected parse of DATACLASSIFICATION data\n");
            return 11;
        }
        free(dcbuf);
    }
    return 0;
}
```

## <a name="supported-version"></a><a name="bkmk-version"></a>Versión admitida
Microsoft ODBC Driver 17.2 permite la recuperación de información de clasificación de datos a través de `SQLGetDescField` si `FieldIdentifier` se establece en `SQL_CA_SS_DATA_CLASSIFICATION` (1237). 

A partir de Microsoft ODBC Driver 17.4.1.1, es posible recuperar la versión de clasificación de datos compatible con un servidor a través de `SQLGetDescField` mediante el identificador de campo `SQL_CA_SS_DATA_CLASSIFICATION_VERSION` (1238). En 17.4.1.1, la versión de clasificación de datos compatible se establece en "2".

 

A partir de 17.4.2.1, se presentó la versión predeterminada de clasificación de datos que se establece en "1" y es la versión notificada por el controlador a SQL Server como compatible. El nuevo atributo de conexión `SQL_COPT_SS_DATACLASSIFICATION_VERSION` (1400) puede permitir que la aplicación cambie la versión compatible de clasificación de datos de "1" hasta el máximo admitido.  

Ejemplo: 

Para establecer la versión, debe realizarse esta llamada justo antes de la llamada a SQLConnect o SQLDriverConnect:
```
ret = SQLSetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)2, SQL_IS_INTEGER);
```

El valor de la versión de clasificación de datos admitida actualmente se puede recuperar a través de la llamada a SQlGetConnectAttr: 
```
ret = SQLGetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)&dataClassVersion, SQL_IS_INTEGER, 0);
```
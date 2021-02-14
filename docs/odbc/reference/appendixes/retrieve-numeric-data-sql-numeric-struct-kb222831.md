---
title: Recuperar datos numéricos con SQL_NUMERIC_STRUCT | Microsoft Docs
description: C/C++ con ODBC recupera el tipo de datos numérico SQL Server mediante SQL_NUMERIC_STRUCT, relacionado con SQL_C_NUMERIC.
editor: ''
ms.prod: sql
ms.technology: connectivity
ms.devlang: cpp
ms.topic: reference
ms.custom: ''
ms.date: 07/14/2017
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: 2acdc83e4dd85a2663becaee11ec8ca5f3b5bcbb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079316"
---
# <a name="retrieve-numeric-data-with-sql_numeric_struct"></a>Recuperar datos numéricos con \_ struct numérico de SQL \_

En este artículo se describe cómo recuperar datos numéricos del controlador ODBC de SQL Server en una estructura numérica. También se describe cómo obtener los valores correctos usando valores de precisión y escala concretos.

Este tipo de datos permite que las aplicaciones controlen directamente los datos numéricos. En torno al año 2003, ODBC 3,0 presentó un nuevo tipo de datos ODBC C, identificado por **SQL \_ c \_ Numeric**. Este tipo de datos sigue siendo relevante a partir de 2017.

El búfer de C que se utiliza tiene la definición de tipo de **\_ \_ struct numérico de SQL**. Esta estructura tiene campos para almacenar la precisión, la escala, el signo y el valor de los datos numéricos. El propio valor se almacena como un entero con escala con el byte menos significativo que comienza en la posición más a la izquierda. 

En el artículo [tipos de datos de C](c-data-types.md) se proporciona más información sobre el formato y el uso de la \_ estructura numérica de SQL \_ . Por lo general, el [Apéndice D](appendix-d-data-types.md) de la referencia del programador de ODBC 3,0 describe los tipos de datos.


## <a name="sql_numeric_struct-overview"></a>\_ \_ Información general sobre el struct numérico de SQL


La \_ estructura numérica \_ de SQL se define en el archivo de encabezado SqlTypes. h de la siguiente manera:


```c
#define SQL_MAX_NUMERIC_LEN    16
typedef struct tagSQL_NUMERIC_STRUCT
{
   SQLCHAR    precision;
   SQLSCHAR   scale;
   SQLCHAR    sign;   /* 1 if positive, 0 if negative */
   SQLCHAR    val[SQL_MAX_NUMERIC_LEN];
} SQL_NUMERIC_STRUCT;
```

            
Los campos de precisión y escala de la estructura numérica nunca se usan para la entrada de una aplicación, solo para la salida del controlador a la aplicación.

El controlador utiliza la precisión predeterminada (definida por el controlador) y la escala predeterminada (0) cada vez que se devuelven datos a la aplicación. A menos que la aplicación especifique valores para la precisión y la escala, el controlador asume el valor predeterminado y trunca la parte decimal de los datos numéricos.

## <a name="sql_numeric_struct-code-sample"></a>\_Ejemplo de código de struct numérico de SQL \_

En este ejemplo de código se muestra cómo:

- Establezca la precisión.
- Establezca la escala.
- Recupere los valores correctos. 

> [!Note]
> CUALQUIER USO QUE USTED REALICE DEL CÓDIGO QUE SE PROPORCIONA EN ESTE ARTÍCULO ES BAJO SU RESPONSABILIDAD. 
>
> Microsoft proporciona estos ejemplos de código "tal cual" sin garantía de ningún tipo, ya sea expresa o implícita, incluidas, entre otras, las garantías implícitas de comerciabilidad o idoneidad para un propósito determinado.

```c
#include <stdio.h>
#include <string.h>
#include <conio.h>
#include <stdlib.h>
#include <ctype.h>
#include <windows.h>
#include <sql.h>
#include <sqlext.h>

#define MAXDSN       25
#define MAXUID       25
#define MAXAUTHSTR   25
#define MAXBUFLEN   255

SQLHENV     henv   = SQL_NULL_HENV;
SQLHDBC     hdbc1  = SQL_NULL_HDBC;     
SQLHSTMT    hstmt1 = SQL_NULL_HSTMT;
SQLHDESC    hdesc  = NULL;


SQL_NUMERIC_STRUCT NumStr;

int main()
{
   RETCODE retcode;

//Change the values below as appropriate to make a successful connection.
//szDSN: DataSourceName, szUID=userid, szAuthStr: password

UCHAR szDSN[MAXDSN+1] = "sql33",szUID[MAXUID+1]="sa", szAuthStr[MAXAUTHSTR+1] = "";
SQLINTEGER strlen1;
SQLINTEGER a;
int i,sign =1;
long myvalue, divisor;
float final_val;
   
// Allocate the Environment handle. Set the Env attribute, allocate the
//connection handle, connect to the database and allocate the statement //handle.

retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);
retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION,(SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);
retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);
retcode = SQLConnect(hdbc1, szDSN,SQL_NTS,szUID,SQL_NTS,szAuthStr,SQL_NTS);
retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);

// Execute the select statement. Here it is assumed that numeric_test
//table is created using the following statements:

// Create table numeric_test (col1 numeric(5,3))
//insert into numeric_test values (25.212)

retcode = SQLExecDirect(hstmt1,(UCHAR *)"select * from numeric_test",SQL_NTS);

// Use SQLBindCol to bind the NumStr to the column that is being retrieved.

retcode = SQLBindCol(hstmt1,1,SQL_C_NUMERIC,&NumStr,19,&strlen1);

// Get the application row descriptor for the statement handle using
//SQLGetStmtAttr.

retcode = SQLGetStmtAttr(hstmt1, SQL_ATTR_APP_ROW_DESC,&hdesc, 0, NULL);

// You can either use SQLSetDescRec or SQLSetDescField when using
// SQLBindCol. However, if you prefer to call SQLGetData, you have to
// call SQLSetDescField instead of SQLSetDescRec. For more information on
// descriptors, please refer to the ODBC 3.0 Programmers reference or
// your Online documentation.

//Used when using SQLSetDescRec
//a=b=sizeof(NumStr);

// Set the datatype, precision and scale fields of the descriptor for the 
//numeric column. Otherwise the default precision (driver defined) and 
//scale (0) are returned.

// In this case, the table contains only one column, hence the second 
//parameter contains one. Zero applies to bookmark columns. Please check 
//the programmers guide for more information.

//retcode=SQLSetDescRec(hdesc,1,SQL_NUMERIC,NULL,sizeof(NumStr),5,3,&NumStr,&a,&b);

retcode = SQLSetDescField (hdesc,1,SQL_DESC_TYPE,(VOID*)SQL_C_NUMERIC,0);
retcode = SQLSetDescField (hdesc,1,SQL_DESC_PRECISION,(VOID*) 5,0);
retcode = SQLSetDescField (hdesc,1,SQL_DESC_SCALE,(VOID*) 3,0);
    
// Initialize the val array in the numeric structure.

memset(NumStr.val,0,16);
   
// Call SQLFetch to fetch the first record.

while((retcode =SQLFetch(hstmt1)) != SQL_NO_DATA)
  {
// Notice that the TargetType (3rd Parameter) is SQL_ARD_TYPE, which  
//forces the driver to use the Application Row Descriptor with the 
//specified scale and precision.

   retcode = SQLGetData(hstmt1, 1, SQL_ARD_TYPE, &NumStr, 19, &a); 

// Check for null indicator.

   if ( SQL_NULL_DATA == a )
   {
   printf( "The final value: NULL\n" );
   continue;
   }

// Call to convert the little endian mode data into numeric data.

   myvalue = strtohextoval();

// The returned value in the above code is scaled to the value specified
//in the scale field of the numeric structure. For example 25.212 would
//be returned as 25212. The scale in this case is 3 hence the integer 
//value needs to be divided by 1000.

   divisor = 1;
   if(NumStr.scale > 0)
     {
    for (i=0;i< NumStr.scale; i++)   
         divisor = divisor * 10;
     }
   final_val =  (float) myvalue /(float) divisor;

// Examine the sign value returned in the sign field for the numeric
//structure.
//NOTE: The ODBC 3.0 spec required drivers to return the sign as 
//1 for positive numbers and 2 for negative number. This was changed in the
//ODBC 3.5 spec to return 0 for negative instead of 2.

      if(!NumStr.sign) sign = -1;
      else sign  =1;

   final_val *= sign;
   printf("The final value: %f\n",final_val);
    }

   while ( ( retcode = SQLMoreResults(hstmt1) ) != SQL_NO_DATA_FOUND);

   /* clean up */ 
   SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);
   SQLDisconnect(hdbc1);
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);
   SQLFreeHandle(SQL_HANDLE_ENV, henv);
   return(0);
}
```


### <a name="interim-results"></a>Resultados provisionales:


```console
//  C  ==> 12 * 1    =     12
//  7  ==> 07 * 16   =    112
//  2  ==> 02 * 256  =    512
//  6  ==> 06 * 4096 =  24576
===============================
                 Sum =  25212
```


En la estructura numérica, el campo Val es una matriz de caracteres de 16 elementos. Por ejemplo, 25,212 se escala a 25212 y la escala es 3. En formato hexadecimal, este número sería 627C.

El controlador devuelve los elementos siguientes:

- Carácter equivalente de 7C, que es ' | ' (canalización) en el primer elemento de la matriz de caracteres.
- Equivalente a 62, que es "b" en el segundo elemento.
- Los restos de los elementos de la matriz contienen ceros, por lo que el búfer contiene ' | B\0 '.

Ahora, el reto es construir el entero escalado fuera de esta matriz de cadenas. Cada carácter de la cadena se corresponde con dos dígitos hexadecimales, por ejemplo, el dígito menos significativo (LSD) y el dígito más significativo (MSD). El valor entero escalado se podría generar multiplicando cada dígito (LSD & MSD) con un múltiplo de 16, comenzando por 1.

Código que implementa la conversión del modo little endian al entero escalado. Depende del desarrollador de la aplicación implementar esta funcionalidad. El siguiente ejemplo de código es solo una de las muchas maneras posibles.


```c
long strtohextoval()
{
    long val=0,value=0;
    int i=1,last=1,current;
    int a=0,b=0;

        for(i=0;i<=15;i++)
        {
         current = (int) NumStr.val[i];
         a= current % 16; //Obtain LSD
         b= current / 16; //Obtain MSD
            
         value += last* a;   
         last = last * 16;   
         value += last* b;
         last = last * 16;   
        }
     return value;
}
```


### <a name="applies-to-versions"></a>Se aplica a las versiones


La información anterior sobre la \_ estructura numérica de SQL \_ se aplica a las siguientes versiones del producto:

- Microsoft ODBC driver for Microsoft SQL Server 3,7
- Microsoft Data Access Components 2,1
- Microsoft Data Access Components 2,5
- Microsoft Data Access Components 2,6
- Microsoft Data Access Components 2,7


## <a name="sql_c_numeric-overview"></a>\_ \_ Información general sobre SQL C numérica


En el siguiente programa de ejemplo se muestra el uso de SQL \_ C \_ Numeric, insertando 123,45 en una tabla. En la tabla, la columna se define como un valor numérico o decimal, con una precisión de 5 y una escala 2.

El controlador ODBC que se utiliza para ejecutar este programa debe admitir la funcionalidad ODBC 3,0.


```c
#include <windows.h>
#include <sql.h>
#include <sqlext.h>

void main() {

   SQLHENV    henv  = NULL;
   SQLHDBC    hdbc  = NULL;
   SQLHSTMT   hstmt = NULL;

   SQL_NUMERIC_STRUCT   NumStr;
   SQLINTEGER           cbNumStr = sizeof (NumStr);

   SQLAllocHandle(SQL_HANDLE_ENV, NULL, &henv);

   /* Set the ODBC behavior version. */ 
   SQLSetEnvAttr(henv,
         SQL_ATTR_ODBC_VERSION,
         (SQLPOINTER) SQL_OV_ODBC3,
         SQL_IS_INTEGER);

   SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);

   /* Substitute your own connection information */ 
   SQLConnect(hdbc,
      (SQLCHAR *) "MyDSN", 5,
      (SQLCHAR *) "UserID", 6,
      (SQLCHAR *) "Password", 8);

   SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);

   /*
      Set up the SQL_NUMERIC_STRUCT, NumStr, to hold "123.45".

      First, we need to scale 123.45 to an integer: 12345
      One way to switch the bytes is to convert 12345 to Hex:  0x3039
      Since the least significant byte will be stored starting from the
      leftmost byte, "0x3039" will be stored as "0x3930".

      The precision and scale fields are not used for input to the driver,
      only for output from the driver. The precision and scale will be set
      in the application parameter descriptor later.
   */ 

   NumStr.sign = 1;   /* 1 if positive, 2 if negative */ 

   memset (NumStr.val, 0, 16);
   NumStr.val [0] = 0x39;
   NumStr.val [1] = 0x30;

   /* SQLBindParameter needs to be called before SQLSetDescField */ 
   SQLBindParameter(hstmt,
          1,
          SQL_PARAM_INPUT,
          SQL_C_NUMERIC,
          SQL_NUMERIC,
          5,
          2,
          &NumStr,
          0,
          (SQLINTEGER *) &cbNumStr);

   /* Modify the fields in the implicit application parameter descriptor */ 
   SQLHDESC   hdesc = NULL;

   SQLGetStmtAttr(hstmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);
   SQLSetDescField(hdesc, 1, SQL_DESC_TYPE, (SQLPOINTER) SQL_C_NUMERIC, 0);
   SQLSetDescField(hdesc, 1, SQL_DESC_PRECISION, (SQLPOINTER) 5, 0);
   SQLSetDescField(hdesc, 1, SQL_DESC_SCALE, (SQLPOINTER) 2, 0);
   SQLSetDescField(hdesc, 1, SQL_DESC_DATA_PTR, (SQLPOINTER) &NumStr, 0);

   SQLExecDirect(hstmt,
         (SQLCHAR *) "INSERT INTO table (numeric_column) VALUES (?)",
         SQL_NTS);

   SQLFreeHandle(SQL_HANDLE_STMT, hstmt);

   SQLDisconnect (hdbc);

   SQLFreeHandle(SQL_HANDLE_DBC, hdbc);
   SQLFreeHandle(SQL_HANDLE_ENV, henv);
}
```


<!--
GeneMi historical notes, 2017/July/12. Per Jason.C

https://go.microsoft.com/fwlink/?LinkId=147596

https://support.microsoft.com/help/222831

https://web.archive.org/web/20140319133434/http:/support.microsoft.com:80/kb/222831

https://web.archive.org/web/20080505073901/http:/support.microsoft.com:80/kb/181254

https://docs.microsoft.com/sql/odbc/reference/appendixes/c-data-types
-->


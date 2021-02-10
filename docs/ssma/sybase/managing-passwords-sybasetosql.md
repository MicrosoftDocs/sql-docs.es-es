---
description: Administración de contraseñas (SybaseToSQL)
title: Administración de contraseñas (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/07/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Exporting or Importing Encrypted Passwords
- Sybase Console,Managing Passwords
- Sybase Console,Securing Password
ms.assetid: 9b6a70f9-6840-4140-a059-bb7bd7ccc67c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: ef87698106dfd3bec7fd0c53434a115dda73efe6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058440"
---
# <a name="managing-passwords-sybasetosql"></a>Administración de contraseñas (SybaseToSQL)
Esta sección trata sobre la protección de contraseñas de base de datos y el procedimiento para importarlas o exportarlas entre servidores.

## <a name="securing-password"></a>Protección de contraseñas  
SSMA le permite proteger la contraseña de una base de datos.  
  
Use el procedimiento siguiente para implementar una conexión segura:  
  
Especifique una contraseña válida con uno de los tres métodos siguientes:  
  
1.  **Texto no cifrado:** Escriba la contraseña de la base de datos en el atributo de valor del nodo ' contraseña '. Se encuentra en el nodo definición del servidor en la sección servidor del archivo de script o el archivo de conexión de servidor.  
  
    Las contraseñas en texto no cifrado no son seguras. Por lo tanto, se mostrará el siguiente mensaje de advertencia en la salida de la consola: *"la &lt; contraseña del ID. del servidor del servidor &gt; se proporciona en forma de texto no cifrado no seguro, la aplicación de consola SSMA proporciona una opción para proteger la contraseña a través del cifrado, consulte la opción-securepassword en el archivo de ayuda de SSMA para obtener más información".*  
  
    **Contraseñas cifradas:** La contraseña especificada, en este caso, se almacena en un formato cifrado en el equipo local en ProtectedStorage. SSMA.  
  
    -   **Protección de contraseñas**  
  
        -   Ejecute `SSMAforSybaseConsole.exe` con la `-securepassword` línea de comandos y Add switch at pasando la conexión de servidor o el archivo de script que contiene el nodo de contraseña en la sección de definición de servidor.  
  
        -   En el símbolo del sistema, se pide al usuario que escriba la contraseña de la base de datos y la confirme.  
  
            Los identificadores de definición de servidor y sus contraseñas cifradas correspondientes se almacenan en un archivo en el equipo local.  
            
            Ejemplo 1:  
            
            1. Especificar contraseña
                
            2. `C:\SSMA\SSMAforSybaseConsole.EXE -securepassword -add all -s "D:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "D:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ VariableValueFileSample.xml"`
                
            3. Escriba la contraseña de server_id ' XXX_1 ': xxxxxxx
                
            4. Vuelva a escribir la contraseña de server_id ' XXX_1 ': xxxxxxx
            
            Ejemplo 2:
            
            1. `C:\SSMA\SSMAforSybaseConsole.EXE -securepassword -add "source_1,target_1" -c "D:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ServersConnectionFileSample.xml" - v "D:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ VariableValueFileSample.xml" -o`
                
            2. Escriba la contraseña de server_id ' source_1 ': xxxxxxx
                
            3. Vuelva a escribir la contraseña de server_id ' source_1 ': xxxxxxx
                
            4. Escriba la contraseña de server_id ' target_1 ': xxxxxxx
                
            5. Vuelva a escribir la contraseña para server_id ' Target _ 1 ': xxxxxxx  
    
    -   **Quitar contraseñas cifradas**  
  
        Ejecute `SSMAforSybaseConsole.exe` con el `-securepassword` `-remove` modificador y en la línea de comandos pasando los identificadores de servidor para quitar las contraseñas cifradas del archivo de almacenamiento protegido presente en el equipo local.  
  
        Ejemplo:  
        
        ```console
            C:\SSMA\SSMAforSybaseConsole.EXE -securepassword -remove all
            C:\SSMA\SSMAforSybaseConsole.EXE -securepassword -remove "source_1,target_1"  
        ```
  
    -   **Enumerar los identificadores de servidor cuyas contraseñas están cifradas**  
  
        Ejecute `SSMAforSybaseConsole.exe` con la `-securepassword` línea de `-list` comandos y en la línea de comandos para enumerar todos los identificadores de servidor cuyas contraseñas se han cifrado.  
  
        Ejemplo:  

        ```console
            C:\SSMA\SSMAforSybaseConsole.EXE -securepassword -list  
        ```
  
    > [!NOTE]  
    > 1.  La contraseña en texto no cifrado mencionada en el archivo de conexión de script o servidor tiene prioridad sobre la contraseña cifrada en un archivo protegido.  
    > 2.  Cuando no existe ninguna contraseña en la sección servidor del archivo de conexión del servidor o el archivo de script o si no se ha protegido en el equipo local, la consola de le pide que escriba la contraseña.  
  
## <a name="exporting-or-importing-encrypted-passwords"></a>Exportar o importar contraseñas cifradas  
La aplicación de consola SSMA permite exportar contraseñas de base de datos cifradas presentes en un archivo del equipo local a un archivo protegido y viceversa. Ayuda a que las contraseñas cifradas sean independientes de la máquina. La funcionalidad de exportación lee el identificador y la contraseña del servidor desde el almacenamiento protegido local y guarda la información en un archivo cifrado. Se solicita al usuario que escriba la contraseña del archivo protegido. Asegúrese de que la contraseña especificada tiene una longitud de 8 caracteres o más. Este archivo protegido es portátil entre diferentes equipos. La funcionalidad de importación lee el ID. de servidor y la información de contraseña del archivo protegido. Se solicita al usuario que escriba la contraseña del archivo protegido y anexa la información al almacenamiento protegido local.  
  
### <a name="export-example"></a>Ejemplo de exportación:  

1. Exportar contraseña
    
2. Escriba la contraseña para proteger el archivo exportado
    
3. `C:\SSMA\SSMAforSybaseConsole.EXE -securepassword -export all "machine1passwords.file"`
    
4. Escriba la contraseña para proteger el archivo exportado: xxxxxxxx
    
5. Confirme la contraseña: xxxxxxxx
    
6. `C:\SSMA\SSMAforSybaseConsole.EXE -p -e "SybaseDB_1_1,Sql_1" "machine2passwords.file"`
    
7. Escriba la contraseña para proteger el archivo exportado: xxxxxxxx
    
8. Confirme la contraseña: xxxxxxxx  
  
### <a name="import-example"></a>Ejemplo de importación:  

1. Importar una contraseña cifrada
    
2. Escriba la contraseña para proteger el archivo importado.
    
3. `C:\SSMA\SSMAforSybaseConsole.EXE -securepassword -import all "machine1passwords.file"`
    
4. Escriba la contraseña para importar los servidores del archivo cifrado: xxxxxxxx
    
5. Confirme la contraseña: xxxxxxxx
    
6. `C:\SSMA\SSMAforSybaseConsole.EXE -p -i "SybaseDB_1,Sql_1" "machine2passwords.file"`
    
7. Escriba la contraseña para importar los servidores del archivo cifrado: xxxxxxxx
    
8. Confirme la contraseña: xxxxxxxx  
  
## <a name="see-also"></a>Consulte también  
[Ejecutar la consola SSMA (Sybase)](./executing-the-ssma-console-sybasetosql.md)  

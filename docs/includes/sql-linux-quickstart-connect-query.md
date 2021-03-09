## <a name="connect-locally"></a>Conexión local

En los pasos siguientes se usa **sqlcmd** para conectarse localmente a la nueva instancia de SQL Server.

1. Ejecute **sqlcmd** con parámetros para el nombre de SQL Server (-S), el nombre de usuario (-U) y la contraseña (-P). En este tutorial se conecta de forma local, por lo que el nombre del servidor es `localhost`. El nombre de usuario es `SA` y la contraseña es la que proporcionó para la cuenta SA durante la configuración.

   ```bash
   sqlcmd -S localhost -U SA -P '<YourPassword>'
   ```

   > [!TIP]
   > Puede omitir la contraseña en la línea de comandos para que se le solicite escribirla.

   > [!TIP]
   > Si más adelante decide conectarse de forma remota, especifique el nombre de la máquina o la dirección IP del parámetro **-S** y asegúrese de que el puerto 1433 esté abierto en el firewall.

1. Si se realiza correctamente, debe ver un símbolo de sistema de **sqlcmd**: `1>`.

1. Si recibe un error de conexión, intente primero diagnosticar el problema a partir del mensaje de error. Luego revise las [recomendaciones para solucionar problemas de conexión](../linux/sql-server-linux-troubleshooting-guide.md#connection).

## <a name="create-and-query-data"></a>Creación y consulta de datos
Las secciones siguientes le guían en el uso de **sqlcmd** para crear una base de datos, agregar datos y ejecutar una consulta simple.

### <a name="create-a-new-database"></a>Creación de una base de datos

En los pasos siguientes se crea una base de datos denominada `TestDB`.

1. En el símbolo del sistema de **sqlcmd**, pegue el comando Transact-SQL siguiente para crear una base de datos de prueba:

   ```sql
   CREATE DATABASE TestDB
   ```

1. En la línea siguiente, escriba una consulta para devolver el nombre de todas las bases de datos del servidor:

   ```sql
   SELECT Name from sys.Databases
   ```

1. Los dos comandos anteriores no se ejecutaron de inmediato. Debe escribir `GO` en una línea nueva para ejecutar los comandos anteriores:

   ```sql
   GO
   ```

> [!TIP]
> Para más información sobre cómo escribir consultas e instrucciones Transact-SQL, vea [Tutorial: Escribir instrucciones Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md).

### <a name="insert-data"></a>Insertar datos

Luego cree una tabla, `Inventory`, e inserte dos filas nuevas.

1. En el símbolo del sistema de **sqlcmd**, cambie el contexto a la nueva base de datos `TestDB`:

   ```sql
   USE TestDB
   ```

1. Cree una tabla llamada `Inventory`:

   ```sql
   CREATE TABLE Inventory (id INT, name NVARCHAR(50), quantity INT)
   ```

1. Inserte datos en la nueva tabla:

   ```sql
   INSERT INTO Inventory VALUES (1, 'banana', 150); INSERT INTO Inventory VALUES (2, 'orange', 154);
   ```

1. Escriba `GO` para ejecutar los comandos anteriores:

   ```sql
   GO
   ```

### <a name="select-data"></a>Selección de datos

Ahora ejecute una consulta para devolver datos desde la tabla `Inventory`.

1. En el símbolo del sistema **sqlcmd**, escriba una consulta que devuelva filas desde la tabla `Inventory` donde la cantidad sea mayor que 152:

   ```sql
   SELECT * FROM Inventory WHERE quantity > 152;
   ```

1. Ejecute el comando:

   ```sql
   GO
   ```

### <a name="exit-the-sqlcmd-command-prompt"></a>Salida del símbolo del sistema de sqlcmd

Para finalizar la sesión de **sqlcmd**, escriba `QUIT`:

```sql
QUIT
```

## <a name="performance-best-practices"></a>Procedimientos recomendados de rendimiento

Después de instalar SQL Server en Linux, revise los procedimientos recomendados para configurar Linux y SQL Server con el fin de mejorar el rendimiento de los escenarios de producción. Para obtener más información, consulte [Procedimientos recomendados e instrucciones de configuración de SQL Server en Linux](../linux/sql-server-linux-performance-best-practices.md).

## <a name="cross-platform-data-tools"></a>Herramientas de datos multiplataforma

Además de **sqlcmd**, puede usar las siguientes herramientas multiplataforma para administrar SQL Server:

| Herramienta | Descripción |
| ---- | ----------- |
| [Azure Data Studio](../azure-data-studio/index.yml) | Una utilidad de administración de bases de datos GUI multiplataforma. |
| [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md) | Un editor de código GUI multiplataforma que ejecuta instrucciones Transact-SQL con la extensión mssql. |
| [PowerShell Core](../linux/sql-server-linux-manage-powershell-core.md) | Una herramienta de configuración y automatización multiplataforma basada en cmdlets. |
| [mssql-cli](https://github.com/dbcli/mssql-cli/tree/master/doc) | Una interfaz de línea de comandos multiplataforma para ejecutar comandos Transact-SQL. |

## <a name="connecting-from-windows"></a>Conexión desde Windows

Las herramientas de SQL Server en Windows se conectan a instancias de SQL Server en Linux del mismo modo en que se conectarían a cualquier instancia remota de SQL Server.

Si tiene una máquina Windows que se puede conectar a la máquina Linux, pruebe con los mismos pasos de este tema desde un símbolo del sistema Windows mediante la ejecución de **sqlcmd**. Simplemente compruebe que usa el nombre o la dirección IP de la máquina Linux de destino en lugar del host local y asegúrese de que el puerto TCP 1433 esté abierto. Si tiene problemas para conectarse desde Windows, consulte las [recomendaciones para solucionar problemas de conexión](../linux/sql-server-linux-troubleshooting-guide.md#connection).

Para las otras herramientas que se ejecutan en Windows pero se conectan a SQL Server en Linux, consulte:

- [SQL Server Management Studio (SSMS)](../linux/sql-server-linux-manage-ssms.md)
- [Windows PowerShell](../linux/sql-server-linux-manage-powershell.md)
- [SQL Server Data Tools (SSDT)](../linux/sql-server-linux-develop-use-ssdt.md)

## <a name="other-deployment-scenarios"></a>Otros escenarios de implementación

Para otros escenarios de instalación, vea los siguientes recursos:

* [Actualización](../linux/sql-server-linux-setup.md#upgrade): Obtenga información sobre cómo actualizar una instalación existente de SQL Server en Linux
* [Uninstall](../linux/sql-server-linux-setup.md#uninstall). Desinstalación de SQL Server en Linux
* [Instalación desatendida](../linux/sql-server-linux-setup.md#unattended): Obtenga información sobre cómo crear un script para la instalación sin pedir confirmación
* [Instalación sin conexión](../linux/sql-server-linux-setup.md#offline): Obtenga información sobre cómo descargar manualmente los paquetes de instalación sin conexión

> [!TIP]
> Para obtener respuesta a las preguntas más frecuentes, vea [Preguntas más frecuentes sobre SQL Server en Linux](../linux/sql-server-linux-faq.yml).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Explore los tutoriales de SQL Server en Linux](../linux/sql-server-linux-migrate-restore-database.md)
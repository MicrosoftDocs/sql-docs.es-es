1. **En todos los servidores SQL Server, cree un inicio de sesión de servidor para Pacemaker**. La siguiente instrucción Transact-SQL crea un inicio de sesión:

   ```Transact-SQL
   USE [master]
   GO
   CREATE LOGIN [pacemakerLogin] with PASSWORD= N'ComplexP@$$w0rd!'
    
   ALTER SERVER ROLE [sysadmin] ADD MEMBER [pacemakerLogin]
   ```

  En el momento de crear el grupo de disponibilidad, el usuario de Pacemaker necesitará permisos ALTER, CONTROL y VIEW DEFINITION en el grupo de disponibilidad, una vez creado, pero antes de que se le agreguen nodos.

1. **En todos los servidores SQL Server, guarde las credenciales del inicio de sesión de SQL Server**.

   ```bash
   echo 'pacemakerLogin' >> ~/pacemaker-passwd
   echo 'ComplexP@$$w0rd!' >> ~/pacemaker-passwd
   sudo mv ~/pacemaker-passwd /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/secrets/passwd
   sudo chmod 400 /var/opt/mssql/secrets/passwd # Only readable by root
   ```

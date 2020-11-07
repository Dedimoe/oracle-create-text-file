# oracle-create-text-file
Oracle creates a text file

With sqlplus: (in oracle 11G on Linux OS, using command line)

1. Create a database user: user1 and give grant to user1:
```
sqlplus /nolog
conn /as sysdba
create user user1 identified by user1pwd default tablespace users temporary tablespace temp;
grant connect,resource to user1;
grant create any directory to user1;
grant execute on utl_file to user1;
exit;
```

2. Create a database directory:
```
sqlplus /nolog
conn user1;
create directory testdir as '/tmp';
exit;
```

3. Write text file to ```/tmp``` directory:
```
sqlplus /nolog
set serveroutput on
conn user1;
declare
  f  UTL_FILE.FILE_TYPE;
begin
  f := UTL_FILE.FOPEN('testdir', 'testfile.txt' , 'W');

  UTL_FILE.PUT_LINE(f , 'Content file');
  UTL_FILE.FCLOSE(f);
exception
      when OTHERS then
           DBMS_OUTPUT.PUT_LINE
                ('ERROR ' || TO_CHAR(SQLCODE) || SQLERRM);
end;
/
exit;
```

4. Check the result:
```
cat /tmp/testfile.txt
```
and the output:
```
Content file
```

5. Done.


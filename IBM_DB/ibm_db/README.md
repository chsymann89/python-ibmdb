#Installing ibm_db and ibm_db_dbi module 

We are assuming that you have Python already installed. In Linux you may need the python-dev package (you can install python-dev package through "$yum install python-devel" if yum doesn't work then you can also install it through "$apt-get install python-dev")

Note:The minimum python version supported by driver is python 2.7 and the latest version supported is python 3.6.
     
 
## Installation 
```
  easy_install ibm_db
```
This will install *ibm_db* and *ibm_db_dbi* module.


###IBM_DB and DB-API wrapper (ibm_db_dbi) sanity test 

```python
$ python
Python 3.6.5 (default, May 10 2018, 00:54:55)
[GCC 4.3.4 [gcc-4_3-branch revision 152973]] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import ibm_db
>>> #For connecting to local database named pydev for user db2inst1 and password secret, use below example
>>> #ibm_db_conn = ibm_db.connect('pydev', 'db2inst1', 'secret')
>>> #For connecting to remote database named pydev for uid db2inst and pwd secret on host host.test.com, use below example
>>> conn_str='database=pydev;hostname=host.test.com;port=portno;protocol=tcpip;uid=db2inst1;pwd=secret'
>>> ibm_db_conn = ibm_db.connect(conn_str,'','')
>>> import ibm_db_dbi
>>> conn = ibm_db_dbi.Connection(ibm_db_conn)
>>> conn.tables('SYSCAT', '%')
```
More examples can be found under 'tests' folder.

### Issues with MAC OS X
* If you run into errors for libdb2.dylib as below:

```python
>>> import ibm_db
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: dlopen(/usr/local/lib/python3.5/site-packages/ibm_db.cpython-35m-darwin.so, 2): Library not loaded: libdb2.dylib
  Referenced from: /usr/local/lib/python3.5/site-packages/ibm_db.cpython-35m-darwin.so
  Reason: image not found
  
```

You would need to set DYLD_LIBRARY_PATH to point to lib folder as per the installation location of clidriver in your environment. Assuming the driver is installed at /usr/local/lib/python3.5/site-packages/clidriver, you can set the path as:

```
export DYLD_LIBRARY_PATH=/usr/local/lib/python3.5/site-packages/clidriver/lib:$DYLD_LIBRARY_PATH

```

* Resolving SQL1042C error

If you hit following error while attempting to connect to a database:

```python
>>> import ibm_db
>>> ibm_db.connect("my_connection_string", "", "")
 Traceback (most recent call last):
   File "<stdin>", line 1, in <module>
 Exception: [IBM][CLI Driver] SQL1042C An unexpected system error occurred. SQLSTATE=58004 SQLCODE=-1042   
```
Set DYLD_LIBRARY_PATH to point to icc folder as per the installation location of clidriver in your environment.

```
export DYLD_LIBRARY_PATH=/usr/local/lib/python3.5/site-packages/clidriver/lib/icc:$DYLD_LIBRARY_PATH
```

##Supported databases
 * **Minimum Supported version of IBM DB2 is V9fp2 for Linux, UNIX, and Windows**
 * Informix 11.10 **(Cheetah)**
 * Remote connections to i5/OS (iSeries)
 * Remote connections to z/OS (DB2 for z/OS)

##Feedback

**Your feedback is very much appreciated and expected through project ibm-db:**
  * ibm-db issues reports: **https://github.com/ibmdb/python-ibmdb/issues**
  * ibm_db discuss: **http://groups.google.com/group/ibm_db**
  * ibm-db developers: **opendev@us.ibm.com**


#Testing

Tests displaying Python ibm_db driver code examples are located in the tests 
directory. A valid config.py will need to be created to configure your DB2
settings. A config.py.sample exists that can be copied and modified for your
environment.

The config.py should look like this:

```python
test_dir =      'tests'         # Location of testsuite file (relative to current directory)

database =      'test'          # Database to connect to
user     =      'db2inst1'      # User ID to connect with
password =      'password'      # Password for given User ID
hostname =      'localhost'     # Hostname
port     =      50000           # Port Number
```

Point the database to mydatabase as created by the following command.

The tests that ibm_db driver uses depends on a UTF-8 database.  This can be 
created by running:
```
  CREATE DATABASE mydatabase USING CODESET UTF-8 TERRITORY US
```    
Some of the tests utilize XML functionality only available in version 9 or 
later of DB2.  While DB2 v8.x is fully supported, two of the tests 
(test_195.py and test_52949.py) utilize XML functionality.  These tests will 
fail on version 8.x of DB2.

##Running the driver testsuite on Linux
  In order to run the entire python driver testsuite on Linux, run this 
  command at the command prompt:
  ```
    python tests.py
  ```  
  To run a single test, set the environment variable, **SINGLE_PYTHON_TEST**, to 
  the test filename you would like to run, followed by the previous command.
    
##Running the driver testsuite on Windows
  In order to run the entire python driver testsuite on Windows, run this 
  command at the command prompt:
  ```
    tests.py
  ```
  To run a single test, set the environment variable, **SINGLE_PYTHON_TEST**, to 
  the test filename you would like to run, followed by the previous command.


#Known Limitations for the Python driver

If trusted context is not set up, there will be two failures related to trusted context. When thick client has been used than additioanl three failures related to create, recreate DB.


#Known Limitations for the Python wrapper

1. The rowcount for select statements can not be generated.
2. Some warnings from the drivers are not caught by the wrapper.
   As such these might go unnoticed.

# APIs

For more information on the APIs supported by ibm_db, please refer to below link:

https://github.com/ibmdb/python-ibmdb/wiki/APIs

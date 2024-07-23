## Display available SAS/ACCESS Interfaces for SAS Viya Workbench

**1. Save the SAS Log to a file**
```sas
*  1. Save the SAS Log to a file ;
filename logfile '/workspaces/workspace/logfile.txt' ;
proc printto log = logfile ; run ;
proc setinit; run ;
proc printto ; run ;
```

**2. Read the log file and extract ACCESS information**
```sas
* 2. Read the log file and extract ACCESS information ;
data log_data ;
    infile '/workspaces/workspace/logfile.txt' truncover ;
    input Workbench_SAS_Access_Info $char200. ;
    if index(upcase(Workbench_SAS_Access_Info), 'ACCESS') > 0 ;
run ;
```

**3. Display ACCESS filtered result**
```sas
* 3. Display ACCESS filtered result ;
proc print data = log_data noobs ;
run ;
```

**4. Delete created dataset and log file**
```sas
* 4. Delete created dataset and log file ;
proc datasets library = work nolist ;
    delete log_data ;
quit ;
data _null_ ;
    rc = fdelete('logfile') ;
run ;
filename logfile clear ;
```

# utl-reading-a-sas-table-using-macro-language-statements
Reading a sas table using macro language statements 
    Reading a sas table using macro language statements                                                        
                                                                                                               
    I have a very simple request. Loop through a dataset, turning each observation into a macro variables      
                                                                                                               
    github                                                                                                     
    https://tinyurl.com/y6nzflvj                                                                               
    https://github.com/rogerjdeangelis/utl-reading-a-sas-table-using-macro-language-statements                 
                                                                                                               
    Related to                                                                                                 
    https://stackoverflow.com/questions/57417256/sas-macro-do-loop-issues                                      
                                                                                                               
    *_                   _                                                                                     
    (_)_ __  _ __  _   _| |_                                                                                   
    | | '_ \| '_ \| | | | __|                                                                                  
    | | | | | |_) | |_| | |_                                                                                   
    |_|_| |_| .__/ \__,_|\__|                                                                                  
            |_|                                                                                                
    ;                                                                                                          
                                                                                                               
    data have;                                                                                                 
     set sashelp.class(keep=name sex where=(name=:"A"));                                                       
    run;quit;                                                                                                  
                                                                                                               
    WORK.HAVE total obs=2                                                                                      
                                                                                                               
        NAME     SEX                                                                                           
                                                                                                               
       Alfred     M                                                                                            
       Alice      F                                                                                            
                                                                                                               
    *            _               _                                                                             
      ___  _   _| |_ _ __  _   _| |_                                                                           
     / _ \| | | | __| '_ \| | | | __|                                                                          
    | (_) | |_| | |_| |_) | |_| | |_                                                                           
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                          
                    |_|                                                                                        
    ;                                                                                                          
                                                                                                               
    %put &=name1 &=sex1;                                                                                       
    %put &=name2 &=sex2;                                                                                       
                                                                                                               
      NAME1=Alfred  SEX1=M                                                                                     
      NAME2=Alice   SEX2=F                                                                                     
                                                                                                               
    *                                                                                                          
     _ __  _ __ ___   ___ ___  ___ ___                                                                         
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                        
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                        
    | .__/|_|  \___/ \___\___||___/___/                                                                        
    |_|                                                                                                        
    ;                                                                                                          
                                                                                                               
                                                                                                               
    %macro macsd1;                                                                                             
                                                                                                               
       %let dsid=%sysfunc(open(sashelp.class(keep=name sex where=(name=:"A")),i));                             
                                                                                                               
       /* No leading ampersand with %SYSCALL */                                                                
       %syscall set(dsid);                                                                                     
                                                                                                               
       %do i=1 %to 2;                                                                                          
         %let rc=%sysfunc(fetchobs(&dsid,&i));                                                                 
         %let name&i=&name;                                                                                    
         %let sex&i=&sex;                                                                                      
       %end;                                                                                                   
                                                                                                               
       %let rc=%sysfunc(close(&dsid));                                                                         
                                                                                                               
       %put &=name1 &=sex1;                                                                                    
       %put &=name2 &=sex2;                                                                                    
                                                                                                               
    %mend macsd1;                                                                                              
                                                                                                               
    %macsd1;                                                                                                   
                                                                                                               
                                                                                                               

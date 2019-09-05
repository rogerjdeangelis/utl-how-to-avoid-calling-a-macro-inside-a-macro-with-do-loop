# utl-how-to-avoid-calling-a-macro-inside-a-macro-with-do-loop
How to avoid calling a macro inside a macro with do loop 

    SAS Forum: How to avoid calling a macro inside a macro with do loop                                   
                                                                                                          
    I realize you do not need to run eight proc freqs, however,                                           
    more realistic examples are possible.                                                                 
                                                                                                          
     Problem: Avoid this code?                                                                            
                                                                                                          
      %macro test1(s);                                                                                    
        %do i=1 %to %sysfunc(countw(&s,%str( ),q));                                                       
          %let varname =%scan(&s,&i.);                                                                    
          call execute('%freq(DATASET = b2,VAR=&varname.,_,OUTDS=d_&varname.)');                          
        %end;                                                                                             
      %mend test1;                                                                                        
                                                                                                          
    github                                                                                                
    https://tinyurl.com/y3ajwd8g                                                                          
    https://github.com/rogerjdeangelis/utl-how-to-avoid-calling-a-macro-inside-a-macro-with-do-loop       
                                                                                                          
    SAS forum                                                                                             
    https://tinyurl.com/y2h3s4a5                                                                          
    https://communities.sas.com/t5/SAS-Programming/call-a-macro-inside-a-macro-with-do-loop/m-p/586337    
                                                                                                          
                                                                                                          
      Two Solutions                                                                                       
      =============                                                                                       
                                                                                                          
          a. Ted Clay's array and do_over statements (we need these statements in base SAS)               
          b. DOSUBL                                                                                       
                                                                                                          
    *_                   _                                                                                
    (_)_ __  _ __  _   _| |_                                                                              
    | | '_ \| '_ \| | | | __|                                                                             
    | | | | | |_) | |_| | |_                                                                              
    |_|_| |_| .__/ \__,_|\__|                                                                             
            |_|                                                                                           
    ;                                                                                                     
                                                                                                          
    data have;                                                                                            
       array ltrs[7] a b c d e f g;                                                                       
       do rec=1 to 10;                                                                                    
         do idx=1 to dim(ltrs);                                                                           
            ltrs[idx] = int(10*uniform(314));                                                             
         end;                                                                                             
         output;                                                                                          
      end;                                                                                                
      drop rec idx;                                                                                       
    run;quit;                                                                                             
                                                                                                          
                                                                                                          
    WORK.HAVE total obs=10                                                                                
                                                                                                          
      A    B    C    D    E    F    G                                                                     
                                                                                                          
      0    6    5    4    3    3    4                                                                     
      9    6    9    2    4    9    0                                                                     
      5    3    6    6    7    1    1                                                                     
      6    3    1    3    3    2    3                                                                     
      9    3    7    0    4    7    1                                                                     
      3    7    3    3    7    2    5                                                                     
      8    4    7    5    5    5    5                                                                     
      0    2    8    1    1    3    6                                                                     
      4    8    1    5    0    8    1                                                                     
      8    2    0    3    8    4    0                                                                     
                                                                                                          
    *            _               _                                                                        
      ___  _   _| |_ _ __  _   _| |_                                                                      
     / _ \| | | | __| '_ \| | | | __|                                                                     
    | (_) | |_| | |_| |_) | |_| | |_                                                                      
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                     
                    |_|                                                                                   
    ;                                                                                                     
                                                                                                          
    WORK.LOG total obs=7                                                                                  
                                                                                                          
     DSNS    RC                 STATUS                                                                    
                                                                                                          
      A       0    Freq table for table A Completed                                                       
      B       0    Freq table for table B Completed                                                       
      C       0    Freq table for table C Completed                                                       
      D       0    Freq table for table D Completed                                                       
      E       0    Freq table for table E Completed                                                       
      F       0    Freq table for table F Completed                                                       
      G       0    Freq table for table G Completed                                                       
                                                                                                          
                                                                                                          
    Eight proc freqs                                                                                      
    ================                                                                                      
                                                                                                          
    WORK.D_A total obs=7                                                                                  
                                                                                                          
      A    COUNT    PERCENT                                                                               
                                                                                                          
      0      2         20                                                                                 
      3      1         10                                                                                 
      4      1         10                                                                                 
      5      1         10                                                                                 
      6      1         10                                                                                 
      8      2         20                                                                                 
      9      2         20                                                                                 
                                                                                                          
    ...                                                                                                   
    ...                                                                                                   
    ...                                                                                                   
                                                                                                          
    WORK.D_G total obs=6                                                                                  
                                                                                                          
      G    COUNT    PERCENT                                                                               
                                                                                                          
      0      2         20                                                                                 
      1      3         30                                                                                 
      3      1         10                                                                                 
      4      1         10                                                                                 
      5      2         20                                                                                 
      6      1         10                                                                                 
                                                                                                          
    *          _       _   _                                                                              
     ___  ___ | |_   _| |_(_) ___  _ __  ___                                                              
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|                                                             
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \                                                             
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/                                                             
      __ _      __ _ _ __ _ __ __ _ _   _                                                                 
     / _` |    / _` | '__| '__/ _` | | | |                                                                
    | (_| |_  | (_| | |  | | | (_| | |_| |                                                                
     \__,_(_)  \__,_|_|  |_|  \__,_|\__, |                                                                
                                    |___/                                                                 
    ;                                                                                                     
                                                                                                          
    %array(ltrs,values=a b c d e f g);                                                                    
                                                                                                          
    %do_over(ltrs,phrase=%str(                                                                            
        proc freq data=have;                                                                              
           tables ? / missing out=d_?;                                                                    
        run;quit;                                                                                         
        ));                                                                                               
                                                                                                          
    *_            _                 _     _                                                               
    | |__      __| | ___  ___ _   _| |__ | |                                                              
    | '_ \    / _` |/ _ \/ __| | | | '_ \| |                                                              
    | |_) |  | (_| | (_) \__ \ |_| | |_) | |                                                              
    |_.__(_)  \__,_|\___/|___/\__,_|_.__/|_|                                                              
                                                                                                          
    ;                                                                                                     
                                                                                                          
    %symdel dsn / nowarn;                                                                                 
    data log ;                                                                                            
                                                                                                          
      /* do dsn="A","B","C","D","E","F","G" */                                                            
      do dsns=%utl_varlist(have,keep=a b c d e f g,qstyle=DOUBLE,od=%str(,));                             
                                                                                                          
         call symputx("dsn",dsns);                                                                        
                                                                                                          
         rc=dosubl('                                                                                      
             proc freq data=have;                                                                         
                tables &dsn / missing out=want_&dsn;                                                      
             run;quit;                                                                                    
             %let cc=&syserr;                                                                             
         ');                                                                                              
                                                                                                          
         if symgetn("cc")=0 then status=catx(" ","Freq table for table",dsns,"Completed");                
         else status=catx(" ","Freq table for table",dsns,"Failed");                                      
         output;                                                                                          
                                                                                                          
      end;                                                                                                
                                                                                                          
    run;quit;                                                                                             
                                                                                                          

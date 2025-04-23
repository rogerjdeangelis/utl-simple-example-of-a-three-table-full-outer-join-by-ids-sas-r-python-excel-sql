# utl-simple-example-of-a-three-table-full-outer-join-by-ids-sas-r-python-excel-sql
Simple example of a three table full outer join by ids sas r python excel sql
    %let pgm=utl-simple-example-of-a-three-table-full-outer-join-by-ids-sas-r-python-excel-sql;

    %stop_submission;

    Simple example of a three table full outer join by ids sas r python excel sql

            CONTENTS

               1 sas sql
               2 r sql     (cut and paste sas code)
               3 python sql(cut and paste sas code)
               4 excel sql (cut and paste sas code)

    github
    https://tinyurl.com/ptn3a9fx
    https://github.com/rogerjdeangelis/utl-simple-example-of-a-three-table-full-outer-join-by-ids-sas-r-python-excel-sql

    communities.sas
    https://tinyurl.com/4vtskwx3
    https://communities.sas.com/t5/SAS-Data-Management/Full-join-between-3-tables-SAS-Data-Integration-Studio/m-p/825053#M20487

    SOAPBOX ON

    Building the code in Data Integration Studio's Join transformation is a different matter.
    It is more complicated, and it is harder to debug and maintain afterwards.
    So my advice is: Dont do it. Use two joins instead and make a full
    join of two tables, and then join the result with the third table.

    see sas communities for data integration studio join

    SOAPBOX OFF


    /**************************************************************************************************************************/
    /*                                          |                                               |                             */
    /*           INPUT                          |       PROCESS                                 |      OUTPUT                 */
    /*        THREE TABLES                      |       =======                                 |      ======                */
    /*        ============                      |                                               |                             */
    /*                                          |                                               |                             */
    /*   SD1.A      SD1.B      SD1.C            | 1 SAS SQL                                     |  SAS                        */
    /* =======    =======    =======            | =========                                     |  ID    A    B    C          */
    /* ID    A    ID    B    ID    C            |                                               |                             */
    /*                                          | proc sql;                                     |   1    X         Z          */
    /*  1    X     4    Y     1    Z            | create                                        |   2    X         Z          */
    /*  2    X     5    Y     2    Z            |    table want as                              |   3    X                    */
    /*  3    X     6    Y     8    Z            | select                                        |   4    X    Y               */
    /*  4    X     7    Y     9    Z            |   coalesce(a.id,b.id,c.id) as id              |   5    X    Y               */
    /*  5    X     8    Y    10    Z            |  ,a.a                                         |   6    X    Y               */
    /*  6    X     9    Y    11    Z            |  ,b.b                                         |   7         Y               */
    /* options                                  |  ,c.c                                         |   8         Y    Z          */
    /*  validvarname=upcase;                    | from sd1.a                                    |   9         Y    Z          */
    /* libname sd1 "d:/sd1";                    |  full outer join sd1.b                        |  10              Z          */
    /*                                          | on (a.id=b.id)                                |  11              Z          */
    /* data sd1.a;  data sd1.b; data sd1.c      |  full outer join sd1.c                        |                             */
    /*  input        input       input          | on (a.id=c.id) or (b.id=c.id)                 |                             */
    /*   Id  A $1.;   Id B $1.;   Id C $1.      | order                                         |                             */
    /* cards4;      cards4;     cards4;         |  by coalesce(a.id,b.id,c.id)                  |                             */
    /* 1 X          4 Y         1 Z             | ;                                             |                             */
    /* 2 X          5 Y         2 Z             | quit;                                         |                             */
    /* 3 X          6 Y         8 Z             |                                               |                             */
    /* 4 X          7 Y         9 Z             | ----------------------------------------------|-----------------------------*/
    /* 5 X          8 Y         10 Z            |                                               |                             */
    /* 6 X          9 Y         11 Z            | 2 R SQL (EXACTLY THE SAME CODE)               | R                           */
    /* ;;;;         ;;;;        ;;;;            |                                               |                             */
    /* run;quit;    run;quit;   run;quit;       | %utl_rbeginx;                                 | id    A    B    C           */
    /*                                          | parmcards4;                                   |  1    X <NA>    Z           */
    /*                                          | library(haven)                                |  2    X <NA>    Z           */
    /*                                          | library(sqldf)                                |  3    X <NA> <NA>           */
    /*                                          | source("c:/oto/fn_tosas9x.R")                 |  4    X    Y <NA>           */
    /*                                          | a<-read_sas("d:/sd1/a.sas7bdat")              |  5    X    Y <NA>           */
    /*                                          | b<-read_sas("d:/sd1/b.sas7bdat")              |  6    X    Y <NA>           */
    /*                                          | c<-read_sas("d:/sd1/c.sas7bdat")              |  7 <NA>    Y <NA>           */
    /*                                          | want<-sqldf("                                 |  8 <NA>    Y    Z           */
    /*                                          |  select                                       |  9 <NA>    Y    Z           */
    /*                                          |     coalesce(a.id,b.id,c.id) as id            | 10 <NA> <NA>    Z           */
    /*                                          |    ,a.a                                       | 11 <NA> <NA>    Z           */
    /*                                          |    ,b.b                                       |                             */
    /*                                          |    ,c.c                                       | SAS                         */
    /*                                          |  from a                                       | ROWNAMES ID  A  B  C        */
    /*                                          |    full outer join b                          |                             */
    /*                                          |  on (a.id=b.id)                               |     1     1  X     Z        */
    /*                                          |    full outer join c                          |     2     2  X     Z        */
    /*                                          |  on (a.id=c.id) or (b.id=c.id)                |     3     3  X              */
    /*                                          |  order                                        |     4     4  X  Y           */
    /*                                          |    by coalesce(a.id,b.id,c.id)                |     5     5  X  Y           */
    /*                                          |  ")                                           |     6     6  X  Y           */
    /*                                          | want                                          |     7     7     Y           */
    /*                                          | fn_tosas9x(                                   |     8     8     Y  Z        */
    /*                                          |       inp    = want                           |     9     9     Y  Z        */
    /*                                          |      ,outlib ="d:/sd1/"                       |    10    10        Z        */
    /*                                          |      ,outdsn ="want"                          |    11    11        Z        */
    /*                                          |      )                                        |                             */
    /*                                          | ;;;;                                          |                             */
    /*                                          | %utl_rendx;                                   |                             */
    /*                                          |                                               |                             */
    /*                                          | ----------------------------------------------|-----------------------------*/
    /*                                          |                                               |                             */
    /*                                          |                                               | R     id     A     B     C  */
    /*                                          | 3 PYTHON SQL                                  | 0    1.0     X  None     Z  */
    /*                                          | (EXACTLY THE SAME CODE)                       | 1    2.0     X  None     Z  */
    /*                                          |                                               | 2    3.0     X  None  None  */
    /*                                          | proc datasets lib=sd1 nolist nodetails;       | 3    4.0     X     Y  None  */
    /*                                          |  delete pywant;                               | 4    5.0     X     Y  None  */
    /*                                          | run;quit;                                     | 5    6.0     X     Y  None  */
    /*                                          |                                               | 6    7.0  None     Y  None  */
    /*                                          | %utl_pybeginx;                                | 7    8.0  None     Y     Z  */
    /*                                          | parmcards4;                                   | 8    9.0  None     Y     Z  */
    /*                                          | exec(open('c:/oto/fn_python.py').read())      | 9   10.0  None  None     Z  */
    /*                                          | a,meta=ps.read_sas7bdat('d:/sd1/a.sas7bdat')  | 10  11.0  None  None     Z  */
    /*                                          | b,meta=ps.read_sas7bdat('d:/sd1/b.sas7bdat')  |                             */
    /*                                          | c,meta=ps.read_sas7bdat('d:/sd1/c.sas7bdat')  |                             */
    /*                                          | want=pdsql('''                                | SAS                         */
    /*                                          |  select                            \          | ID    A    B    C           */
    /*                                          |     coalesce(a.id,b.id,c.id) as id \          |                             */
    /*                                          |    ,a.a                            \          |  1    X         Z           */
    /*                                          |    ,b.b                            \          |  2    X         Z           */
    /*                                          |    ,c.c                            \          |  3    X                     */
    /*                                          |  from a                            \          |  4    X    Y                */
    /*                                          |    full outer join b               \          |  5    X    Y                */
    /*                                          |  on (a.id=b.id)                    \          |  6    X    Y                */
    /*                                          |    full outer join c               \          |  7         Y                */
    /*                                          |  on (a.id=c.id) or (b.id=c.id)     \          |  8         Y    Z           */
    /*                                          |  order                             \          |  9         Y    Z           */
    /*                                          |    by coalesce(a.id,b.id,c.id)     \          | 10              Z           */
    /*                                          |    ''');                                      | 11              Z           */
    /*                                          | print(want);                                  |                             */
    /*                                          | fn_tosas9x(         \                         |                             */
    /*                                          |   want              \                         |                             */
    /*                                          |  ,outlib='d:/sd1/'  \                         |                             */
    /*                                          |  ,outdsn='pywant'   \                         |                             */
    /*                                          |  ,timeest=3)                                  |                             */
    /*                                          | ;;;;                                          |                             */
    /*                                          | %utl_pyendx;                                  |                             */
    /*                                          |                                               |                             */
    /*                                          | proc print data=sd1.pywant;                   |                             */
    /*                                          | run;quit;                                     |                             */
    /*                                          |                                               |                             */
    /*                                          | ----------------------------------------------|-----------------------------*/
    /*                                          |                                               |                             */
    /*                                          | 4 excel sql                                   |                             */
    /*                                          |                                               |  d:/xls/wantxl.xlsx         */
    /*                                          |                                               |                             */
    /*                                          | %utlfkil(d:/xls/wantxl.xlsx);                 |  ---------------+           */
    /*                                          |                                               |  | A1| fx  | ID |           */
    /*                                          | %utl_rbeginx;                                 |  -----------------------+   */
    /*                                          | parmcards4;                                   |  [_] |   A |  B | C | D |   */
    /*                                          | library(openxlsx)                             |  -----------------------|   */
    /*                                          | library(sqldf)                                |   1  |  ID |  A | B | C |   */
    /*                                          | library(haven)                                |   -- |-----+----+---+---|   */
    /*                                          | a<-read_sas("d:/sd1/a.sas7bdat")              |   2  |1    | X  |   | Z |   */
    /*                                          | b<-read_sas("d:/sd1/b.sas7bdat")              |   -- |-----+----+---+---|   */
    /*                                          | c<-read_sas("d:/sd1/c.sas7bdat")              |   3  |2    | X  |   | Z |   */
    /*                                          | wb <- createWorkbook()                        |   -- |-----+----+---+---|   */
    /*                                          | addWorksheet(wb, "a")                         |   4  |3    | X  |   |   |   */
    /*                                          | writeData(wb, sheet = "a", x = a)             |   -- |-----+----+---+---|   */
    /*                                          | addWorksheet(wb, "b")                         |   5  |4    | X  | Y |   |   */
    /*                                          | writeData(wb, sheet = "b", x = b)             |   -- |-----+----+---+---|   */
    /*                                          | addWorksheet(wb, "c")                         |   6  |5    | X  | Y |   |   */
    /*                                          | writeData(wb, sheet = "c", x = c)             |   -- |-----+----+---+---|   */
    /*                                          | saveWorkbook(                                 |   7  |6    | X  | Y |   |   */
    /*                                          |     wb                                        |   -- |-----+----+---+---|   */
    /*                                          |    ,"d:/xls/wantxl.xlsx"                      |   8  |7    |    | Y |   |   */
    /*                                          |    ,overwrite=TRUE)                           |   -- |-----+----+---+---|   */
    /*                                          | ;;;;                                          |   9  |8    |    | Y | Z |   */
    /*                                          | %utl_rendx;                                   |   -- |-----+----+---+---|   */
    /*                                          |                                               |  10  |9    |    | Y | Z |   */
    /*                                          | %utl_rbeginx;                                 |   -- |-----+----+---+---|   */
    /*                                          | parmcards4;                                   |  11  |10   |    |   | Z |   */
    /*                                          | library(openxlsx)                             |   -- |-----+----+---+---|   */
    /*                                          | library(sqldf)                                |  12  |11   |    |   | Z |   */
    /*                                          | source("c:/oto/fn_tosas9x.R")                 |   -- |-----+----+---+---|   */
    /*                                          | wb<-loadWorkbook("d:/xls/wantxl.xlsx")        |  [WANT]                     */
    /*                                          | a<-read.xlsx(wb,"a")                          |                             */
    /*                                          | b<-read.xlsx(wb,"b")                          |                             */
    /*                                          | c<-read.xlsx(wb,"c")                          |                             */
    /*                                          | addWorksheet(wb, "want")                      |                             */
    /*                                          | want<-sqldf('                                 |                             */
    /*                                          | select                                        |                             */
    /*                                          |    coalesce(a.id,b.id,c.id) as id             |                             */
    /*                                          |   ,a.a                                        |                             */
    /*                                          |   ,b.b                                        |                             */
    /*                                          |   ,c.c                                        |                             */
    /*                                          | from a                                        |                             */
    /*                                          |   full outer join b                           |                             */
    /*                                          | on (a.id=b.id)                                |                             */
    /*                                          |   full outer join c                           |                             */
    /*                                          | on (a.id=c.id) or (b.id=c.id)                 |                             */
    /*                                          | order                                         |                             */
    /*                                          |   by coalesce(a.id,b.id,c.id)                 |                             */
    /*                                          |   ')                                          |                             */
    /*                                          |  print(want)                                  |                             */
    /*                                          |  writeData(wb,sheet="want",x=want)            |                             */
    /*                                          |  saveWorkbook(                                |                             */
    /*                                          |      wb                                       |                             */
    /*                                          |     ,"d:/xls/wantxl.xlsx"                     |                             */
    /*                                          |     ,overwrite=TRUE)                          |                             */
    /*                                          | fn_tosas9x(                                   |                             */
    /*                                          |       inp    = want                           |                             */
    /*                                          |      ,outlib ="d:/sd1/"                       |                             */
    /*                                          |      ,outdsn ="want"                          |                             */
    /*                                          |      )                                        |                             */
    /*                                          | ;;;;                                          |                             */
    /*                                          | %utl_rendx;                                   |                             */
    /*                                          |                                               |                             */
    /*                                          | proc print data=sd1.want;                     |                             */
    /*                                          | run;quit;                                     |                             */
    /**************************************************************************************************************************/

    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    options
     validvarname=upcase;
    libname sd1 "d:/sd1";

    data sd1.a;
     input
      Id  A $1.;
    cards4;
    1 X
    2 X
    3 X
    4 X
    5 X
    6 X
    ;;;;
    run;quit;

    data sd1.b;
     input
      Id B $1.;
    cards4;
    4 Y
    5 Y
    6 Y
    7 Y
    8 Y
    9 Y
    ;;;;
    run;quit;

    data sd1.c;
     input
      Id C $1.;
    cards4;
    1 Z
    2 Z
    8 Z
    9 Z
    10 Z
    11 Z
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*   SD1.A      SD1.B      SD1.C                                                                                          */
    /* =======    =======    =======                                                                                          */
    /* ID    A    ID    B    ID    C                                                                                          */
    /*                                                                                                                        */
    /*  1    X     4    Y     1    Z                                                                                          */
    /*  2    X     5    Y     2    Z                                                                                          */
    /*  3    X     6    Y     8    Z                                                                                          */
    /*  4    X     7    Y     9    Z                                                                                          */
    /*  5    X     8    Y    10    Z                                                                                          */
    /*  6    X     9    Y    11    Z                                                                                          */
    /**************************************************************************************************************************/

    /*                             _
    / |  ___  __ _ ___   ___  __ _| |
    | | / __|/ _` / __| / __|/ _` | |
    | | \__ \ (_| \__ \ \__ \ (_| | |
    |_| |___/\__,_|___/ |___/\__, |_|
                                |_|
    */

    proc sql;
    create
       table want as
    select
      coalesce(a.id,b.id,c.id) as id
     ,a.a
     ,b.b
     ,c.c
    from sd1.a
     full outer join sd1.b
    on (a.id=b.id)
     full outer join sd1.c
    on (a.id=c.id) or (b.id=c.id)
    order
     by coalesce(a.id,b.id,c.id)
    ;
    quit;

    /**************************************************************************************************************************/
    /*   ID    A    B    C                                                                                                    */
    /*                                                                                                                        */
    /*    1    X         Z                                                                                                    */
    /*    2    X         Z                                                                                                    */
    /*    3    X                                                                                                              */
    /*    4    X    Y                                                                                                         */
    /*    5    X    Y                                                                                                         */
    /*    6    X    Y                                                                                                         */
    /*    7         Y                                                                                                         */
    /*    8         Y    Z                                                                                                    */
    /*    9         Y    Z                                                                                                    */
    /*   10              Z                                                                                                    */
    /*   11              Z                                                                                                    */
    /**************************************************************************************************************************/

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    a<-read_sas("d:/sd1/a.sas7bdat")
    b<-read_sas("d:/sd1/b.sas7bdat")
    c<-read_sas("d:/sd1/c.sas7bdat")
    want<-sqldf("
     select
        coalesce(a.id,b.id,c.id) as id
       ,a.a
       ,b.b
       ,c.c
     from a
       full outer join b
     on (a.id=b.id)
       full outer join c
     on (a.id=c.id) or (b.id=c.id)
     order
       by coalesce(a.id,b.id,c.id)
     ")
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /*___                     _
    |___ \   _ __   ___  __ _| |
      __) | | `__| / __|/ _` | |
     / __/  | |    \__ \ (_| | |
    |_____| |_|    |___/\__, |_|
                           |_|
    */

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    a<-read_sas("d:/sd1/a.sas7bdat")
    b<-read_sas("d:/sd1/b.sas7bdat")
    c<-read_sas("d:/sd1/c.sas7bdat")
    want<-sqldf("
     select
        coalesce(a.id,b.id,c.id) as id
       ,a.a
       ,b.b
       ,c.c
     from a
       full outer join b
     on (a.id=b.id)
       full outer join c
     on (a.id=c.id) or (b.id=c.id)
     order
       by coalesce(a.id,b.id,c.id)
     ")
    want
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="rwant"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.rwant;
    run;quit;


    /**************************************************************************************************************************/
    /*  R                     |  SAS                                                                                          */
    /*     id    A    B    C  |  ROWNAMES    ID    A    B    C                                                                */
    /*                        |                                                                                               */
    /*  1   1    X <NA>    Z  |      1        1    X         Z                                                                */
    /*  2   2    X <NA>    Z  |      2        2    X         Z                                                                */
    /*  3   3    X <NA> <NA>  |      3        3    X                                                                          */
    /*  4   4    X    Y <NA>  |      4        4    X    Y                                                                     */
    /*  5   5    X    Y <NA>  |      5        5    X    Y                                                                     */
    /*  6   6    X    Y <NA>  |      6        6    X    Y                                                                     */
    /*  7   7 <NA>    Y <NA>  |      7        7         Y                                                                     */
    /*  8   8 <NA>    Y    Z  |      8        8         Y    Z                                                                */
    /*  9   9 <NA>    Y    Z  |      9        9         Y    Z                                                                */
    /*  10 10 <NA> <NA>    Z  |     10       10              Z                                                                */
    /*  11 11 <NA> <NA>    Z  |     11       11              Z                                                                */
    /**************************************************************************************************************************/

    /*____               _   _                             _
    |___ /   _ __  _   _| |_| |__   ___  _ __    ___  __ _| |
      |_ \  | `_ \| | | | __| `_ \ / _ \| `_ \  / __|/ _` | |
     ___) | | |_) | |_| | |_| | | | (_) | | | | \__ \ (_| | |
    |____/  | .__/ \__, |\__|_| |_|\___/|_| |_| |___/\__, |_|
            |_|    |___/                                |_|
    */

    proc datasets lib=sd1 nolist nodetails;
     delete pywant;
    run;quit;

    %utl_pybeginx;
    parmcards4;
    exec(open('c:/oto/fn_python.py').read())
    a,meta=ps.read_sas7bdat('d:/sd1/a.sas7bdat')
    b,meta=ps.read_sas7bdat('d:/sd1/b.sas7bdat')
    c,meta=ps.read_sas7bdat('d:/sd1/c.sas7bdat')
    want=pdsql('''
     select                            \
        coalesce(a.id,b.id,c.id) as id \
       ,a.a                            \
       ,b.b                            \
       ,c.c                            \
     from a                            \
       full outer join b               \
     on (a.id=b.id)                    \
       full outer join c               \
     on (a.id=c.id) or (b.id=c.id)     \
     order                             \
       by coalesce(a.id,b.id,c.id)     \
       ''');
    print(want);
    fn_tosas9x(         \
      want              \
     ,outlib='d:/sd1/'  \
     ,outdsn='pywant'   \
     ,timeest=3)
    ;;;;
    %utl_pyendx;

    proc print data=sd1.pywant;
    run;quit;

    /**************************************************************************************************************************/
    /* R                            |   SAS                                                                                   */
    /*       id     A     B     C   |   ID    A    B    C                                                                     */
    /* 0    1.0     X  None     Z   |                                                                                         */
    /* 1    2.0     X  None     Z   |    1    X         Z                                                                     */
    /* 2    3.0     X  None  None   |    2    X         Z                                                                     */
    /* 3    4.0     X     Y  None   |    3    X                                                                               */
    /* 4    5.0     X     Y  None   |    4    X    Y                                                                          */
    /* 5    6.0     X     Y  None   |    5    X    Y                                                                          */
    /* 6    7.0  None     Y  None   |    6    X    Y                                                                          */
    /* 7    8.0  None     Y     Z   |    7         Y                                                                          */
    /* 8    9.0  None     Y     Z   |    8         Y    Z                                                                     */
    /* 9   10.0  None  None     Z   |    9         Y    Z                                                                     */
    /* 10  11.0  None  None     Z   |   10              Z                                                                     */
    /**************************************************************************************************************************/


    /*  _                       _             _
    | || |     _____  _____ ___| |  ___  __ _| |
    | || |_   / _ \ \/ / __/ _ \ | / __|/ _` | |
    |__   _| |  __/>  < (_|  __/ | \__ \ (_| | |
       |_|    \___/_/\_\___\___|_| |___/\__, |_|
                                           |_|
    */

    %utlfkil(d:/xls/wantxl.xlsx);

    %utl_rbeginx;
    parmcards4;
    library(openxlsx)
    library(sqldf)
    library(haven)
    a<-read_sas("d:/sd1/a.sas7bdat")
    b<-read_sas("d:/sd1/b.sas7bdat")
    c<-read_sas("d:/sd1/c.sas7bdat")
    wb <- createWorkbook()
    addWorksheet(wb, "a")
    writeData(wb, sheet = "a", x = a)
    addWorksheet(wb, "b")
    writeData(wb, sheet = "b", x = b)
    addWorksheet(wb, "c")
    writeData(wb, sheet = "c", x = c)
    saveWorkbook(
        wb
       ,"d:/xls/wantxl.xlsx"
       ,overwrite=TRUE)
    ;;;;
    %utl_rendx;

    %utl_rbeginx;
    parmcards4;
    library(openxlsx)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    wb<-loadWorkbook("d:/xls/wantxl.xlsx")
    a<-read.xlsx(wb,"a")
    b<-read.xlsx(wb,"b")
    c<-read.xlsx(wb,"c")
    addWorksheet(wb, "want")
    want<-sqldf('
    select
       coalesce(a.id,b.id,c.id) as id
      ,a.a
      ,b.b
      ,c.c
    from a
      full outer join b
    on (a.id=b.id)
      full outer join c
    on (a.id=c.id) or (b.id=c.id)
    order
      by coalesce(a.id,b.id,c.id)
      ')
     print(want)
     writeData(wb,sheet="want",x=want)
     saveWorkbook(
         wb
        ,"d:/xls/wantxl.xlsx"
        ,overwrite=TRUE)
    fn_tosas9x(
          inp    = want
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print data=sd1.want;
    run;quit;

    /**************************************************************************************************************************/
    /*                              |                                                                                          */
    /*  d:/xls/wantxl.xlsx          |   SAS                                                                                    */
    /*                              |                                                                                          */
    /*  ---------------+            |    ID    A    B    C                                                                     */
    /*  | A1| fx  | ID |            |                                                                                          */
    /*  -----------------------+    |     1    X         Z                                                                     */
    /*  [_] |   A |  B | C | D |    |     2    X         Z                                                                     */
    /*  -----------------------|    |     3    X                                                                               */
    /*   1  |  ID |  A | B | C |    |     4    X    Y                                                                          */
    /*   -- |-----+----+---+---|    |     5    X    Y                                                                          */
    /*   2  |1    | X  |   | Z |    |     6    X    Y                                                                          */
    /*   -- |-----+----+---+---|    |     7         Y                                                                          */
    /*   3  |2    | X  |   | Z |    |     8         Y    Z                                                                     */
    /*   -- |-----+----+---+---|    |     9         Y    Z                                                                     */
    /*   4  |3    | X  |   |   |    |    10              Z                                                                     */
    /*   -- |-----+----+---+---|    |    11              Z                                                                     */
    /*   5  |4    | X  | Y |   |    |                                                                                          */
    /*   -- |-----+----+---+---|    |                                                                                          */
    /*   6  |5    | X  | Y |   |    |                                                                                          */
    /*   -- |-----+----+---+---|    |                                                                                          */
    /*   7  |6    | X  | Y |   |    |                                                                                          */
    /*   -- |-----+----+---+---|    |                                                                                          */
    /*   8  |7    |    | Y |   |    |                                                                                          */
    /*   -- |-----+----+---+---|    |                                                                                          */
    /*   9  |8    |    | Y | Z |    |                                                                                          */
    /*   -- |-----+----+---+---|    |                                                                                          */
    /*  10  |9    |    | Y | Z |    |                                                                                          */
    /*   -- |-----+----+---+---|    |                                                                                          */
    /*  11  |10   |    |   | Z |    |                                                                                          */
    /*   -- |-----+----+---+---|    |                                                                                          */
    /*  12  |11   |    |   | Z |    |                                                                                          */
    /*   -- |-----+----+---+---|    |                                                                                          */
    /*  [WANT]                      |                                                                                          */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

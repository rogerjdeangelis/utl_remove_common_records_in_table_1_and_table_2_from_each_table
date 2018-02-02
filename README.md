# utl_remove_common_records_in_table_1_and_table_2_from_each_table
Remove common records in both table 1 and table 2 from each table. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverfl SAS community.
    Remove common records in both table 1 and table 2 from each table;

    github
    https://goo.gl/BRMizc
    https://github.com/rogerjdeangelis/utl_remove_common_records_in_table_1_and_table_2_from_each_table

    This solution is a nice example of a SAS/WPS merge complementing SAS/WPS SQL.

    Whats interesting is you need a merge without by. This construct
    is not easily done in WPS/SAS but not SQL. For two reasons
       1. SQL cannot easily do a one to one merge
       2. SQL requires the join use an 'on'  or 'where'.

    see
    https://goo.gl/AT3eWJ
    https://communities.sas.com/t5/Base-SAS-Programming/Retrieve-unique-observations/m-p/433471

    KSharp profile
    https://communities.sas.com/t5/user/viewprofilepage/user-id/18408

    INPUT

      ALGORITHM

         Remove common records in table 1 and table 2 from each table


      WORK.T1 total obs=6

        TEXTE1

        T1_DISJOINT1
        T1_DISJOINT2
        T1_DISJOINT3
        COMMON1
        COMMON2
        COMMON3


      WORK.T2 total obs=5

        TEXTE2

        T2_DISJOINT1
        T2_DISJOINT2
        COMMON1
        COMMON2
        COMMON3

    RULES  would yeild

    WORK.WANT total obs=3

         TEXTE1          TEXTE2

      T1_DISJOINT1    T2_DISJOINT1
      T1_DISJOINT2    T2_DISJOINT2
      T1_DISJOINT3


    PROCESS
    =======

       proc sql;
       create view x1 as
        select texte1 from t1
        except
        select texte2 from t2 ;

       create view x2 as
        select texte2 from t2
        except
        select texte1 from t1 ;
       quit;

       data want;
        merge x1 x2;
       run;

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    Data t1 ;
    Input texte1 $13.;
    Cards4 ;
    T1_DISJOINT1
    T1_DISJOINT2
    T1_DISJOINT3
    COMMON1
    COMMON2
    COMMON3
    ;;;;
    Run;

    Data t2 ;
    Input texte2 $13. ;
    Cards4 ;
    T2_DISJOINT1
    T2_DISJOINT2
    COMMON1
    COMMON2
    COMMON3
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname wrk "%sysfunc(pathname(work))";

       proc sql;
       create view x1 as
        select texte1 from wrk.t1
        except
        select texte2 from wrk.t2 ;

       create view x2 as
        select texte2 from wrk.t2
        except
        select texte1 from wrk.t1 ;
       quit;

       data wrk.want;
        merge x1 x2;
       run;quit;

    run;submit;
    ');


# utl-create-a-crosstab-dataset-instead-of-a-listing-from-proc-tabulate
Create a crosstab dataset instead of a listing from proc tabulate 
    %let pgm=utl-create-a-crosstab-dataset-instead-of-a-listing-from-proc-tabulate;

    Create a crosstab dataset instead of a listing from proc tabulate

    Original work by

    Bartosz Jablonski
    yabwon@gmail.com

    Github
    https://tinyurl.com/5n8yef2t
    https://github.com/rogerjdeangelis/utl-create-a-crosstab-dataset-instead-of-a-listing-from-proc-tabulate

    Macros on end( I have 'ODS' macros for Report and Freq on end)
    for other related repos
    https://github.com/rogerjdeangelis?tab=repositories&q=tabulate&type=&language=&sort=

    variation on
    https://tinyurl.com/yc2jjjx3
    https://stackoverflow.com/questions/71326738/combining-a-proc-freq-statement-with-proc-means-statement-sas
                     _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|

    I AM NOT INTERESTED IN A STATIC PROC TABULATE LISTING. I WANT A DATASET THAT LOOKS LIKE THIS

    ---------------------------------------------------------------------------------------------------------------
    |     |           SEX           |                                      |                                      |
    |     |-------------------------|                                      |                                      |
    |     |     F      |     M      |                HEIGHT                |                WEIGHT                |
    |     |------------+------------+--------------------------------------+--------------------------------------|
    |     |     N      |     N      |    Mean    |   QRange   |   Median   |    Mean    |   QRange   |   Median   |
    |-----+------------+------------+------------+------------+------------+------------+------------+------------|
    |AGE  |            |            |            |            |            |            |            |            |
    |-----|            |            |            |            |            |            |            |            |
    |11   |        1.00|        1.00|       54.40|        6.20|       54.40|       67.75|       34.50|       67.75|
    |-----+------------+------------+------------+------------+------------+------------+------------+------------|
    |12   |        2.00|        3.00|       59.44|        2.50|       59.00|       94.40|       16.50|       84.50|
    |-----+------------+------------+------------+------------+------------+------------+------------+------------|
    |13   |        2.00|        1.00|       61.43|        8.80|       62.50|       88.67|       14.00|       84.00|
    |-----+------------+------------+------------+------------+------------+------------+------------+------------|
    |14   |        2.00|        2.00|       64.90|        3.50|       63.90|      101.88|       11.25|      102.50|
    |-----+------------+------------+------------+------------+------------+------------+------------+------------|
    |15   |        2.00|        2.00|       65.63|        2.25|       66.50|      117.38|       10.75|      112.25|
    |-----+------------+------------+------------+------------+------------+------------+------------+------------|
    |16   |           0|        1.00|       72.00|        0.00|       72.00|      150.00|        0.00|      150.00|
    ---------------------------------------------------------------------------------------------------------------
                         _
    __      ____ _ _ __ | |_
    \ \ /\ / / _` | `_ \| __|
     \ V  V / (_| | | | | |_
      \_/\_/ \__,_|_| |_|\__|

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*  THIS IS WHAT I WANT A DATASET                                                                                         */
    /*                                                                                                                        */
    /*  Up to 40 obs WORK.WANT_ODSRPT total obs=6 04MAR2022:13:55:11                                                          */
    /*                                                                                                                        */
    /*  Obs    AGE    FN    MN    HMEAN    HQRANGE    HMEDIAN     WMEAN    WQRANGE    WMEDIAN                                 */
    /*                                                                                                                        */
    /*   1      11     1     1    54.40      6.20       54.4      67.75     34.50       67.75                                 */
    /*   2      12     2     3    59.44      2.50       59.0      94.40     16.50       84.50                                 */
    /*   3      13     2     1    61.43      8.80       62.5      88.67     14.00       84.00                                 */
    /*   4      14     2     2    64.90      3.50       63.9     101.88     11.25      102.50                                 */
    /*   5      15     2     2    65.63      2.25       66.5     117.38     10.75      112.25                                 */
    /*   6      16     0     1    72.00      0.00       72.0     150.00      0.00      150.00                                 */
    /*                                                                                                                        */
    /*               Variables in Creation Order                                                                              */
    /*                                                                                                                        */
    /*   #    Variable    Type    Len    Format     Informat                                                                  */
    /*                                                                                                                        */
    /*   1    AGE         Num       8    BEST12.    BEST32.                                                                   */
    /*   2    FN          Num       8    BEST12.    BEST32.                                                                   */
    /*   3    MN          Num       8    BEST12.    BEST32.                                                                   */
    /*   4    HMEAN       Num       8    BEST12.    BEST32.                                                                   */
    /*   5    HQRANGE     Num       8    BEST12.    BEST32.                                                                   */
    /*   6    HMEDIAN     Num       8    BEST12.    BEST32.                                                                   */
    /*   7    WMEAN       Num       8    BEST12.    BEST32.                                                                   */
    /*   8    WQRANGE     Num       8    BEST12.    BEST32.                                                                   */
    /*   9    WMEDIAN     Num       8    BEST12.    BEST32.                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
     _                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                                                                                                                        */
    /*  Up to 40 obs from SASHELP.CLASS total obs=19 04MAR2022:13:58:06                                                       */
    /*                                                                                                                        */
    /*  Obs    NAME       SEX    AGE    HEIGHT    WEIGHT                                                                      */
    /*                                                                                                                        */
    /*    1    Alfred      M      14     69.0      112.5                                                                      */
    /*    2    Alice       F      13     56.5       84.0                                                                      */
    /*    3    Barbara     F      13     65.3       98.0                                                                      */
    /*    4    Carol       F      14     62.8      102.5                                                                      */
    /*    5    Henry       M      14     63.5      102.5                                                                      */
    /*                                                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/
               _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| `_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|


    options ls=255;

    options FORMCHAR='|----|+|---+=|-/\<>*'; * use this;
    %utl_odstab(setup);
    proc tabulate data=sashelp.class;
      title "|AGE|FN|MN|HMean|hQRange|hMedian|wMean|wQRange|wMedian|";
      class age sex;
      var height weight;
      tables age,(sex*n (height weight)*(mean qrange median));
    run;quit;
    %utl_odstab(outdsn=want_odsrpt,datarow=6); * you may have to try a couple of datarow numbers.

    * restore formchar;
    options FORMCHAR='|----|+|---+=|-/\<>*';
    run;quit;

    /*
     _ __ ___   __ _  ___ _ __ ___  ___
    | `_ ` _ \ / _` |/ __| `__/ _ \/ __|
    | | | | | | (_| | (__| | | (_) \__ \
    |_| |_| |_|\__,_|\___|_|  \___/|___/

    */

    %macro utl_odstab(outdsn,datarow=1);

       %if %qupcase(&outdsn)=SETUP %then %do;

            filename _tmp1_ clear;  * just in case;

            %utlfkil(%sysfunc(pathname(work))/_tmp1_.txt);

            filename _tmp1_ "%sysfunc(pathname(work))/_tmp1_.txt";

            %let _ps_= %sysfunc(getoption(ps));
            %let _fc_= %sysfunc(getoption(formchar));

            OPTIONS ls=max ps=32756  FORMCHAR='|'  nodate nocenter;

            title; footnote;

            proc printto print=_tmp1_;
            run;quit;

       %end;
       %else %do;

            /* %let outdsn=tst; %let datarow=3; */

            proc printto;
            run;quit;

            %utlfkil(%sysfunc(pathname(work))/_tmp2_.txt);

            *filename _tmp2_  "%sysfunc(pathname(work))/_tmp2_.txt";

            proc datasets lib=work nolist;  *just in case;
             delete &outdsn;
            run;quit;

            proc printto print="%sysfunc(pathname(work))/_tmp2_.txt";
            run;quit;

            data _null_;
              retain n 0;
              infile _tmp1_ length=l;
              input lyn $varying32756. l;
              if _n_=1 then do;
                  file print titles;
                  putlog lyn;
                  *put lyn;
              end;
              else do;
                 if countc(lyn,'|')>2;
                 n=n+1;
                 if n ge %eval(&datarow + 1) then do;
                    file print;
                    putlog lyn;
                    put lyn;
                 end;
              end;
            run;quit;

            proc printto;
            run;quit;

            proc import
               datafile="%sysfunc(pathname(work))/_tmp2_.txt"
               dbms=dlm
               out=&outdsn(drop=var:)
               replace;
               delimiter='|';
               getnames=yes;
            run;quit;

            filename _tmp1_ clear;
            filename _tmp2_ clear;

            %utlfkil(%sysfunc(pathname(work))/_tmp1_.txt);
            %utlfkil(%sysfunc(pathname(work))/_tmp2_.txt);

       %end;

    %mend utl_odstab;



    %macro utl_odsrpt(outdsn);


       %if %qupcase(&outdsn)=SETUP %then %do;

            %put @@@@ &=sysindex.;

            %let _tmp1_=a&sysindex.;

            %put xxxx &=_tmp1_;

            filename &_tmp1_ clear;  * just in case;

            %utlfkil(%sysfunc(pathname(work))/&_tmp1_..txt);

            filename &_tmp1_ "%sysfunc(pathname(work))/&_tmp1_..txt";

            %let _ps_= %sysfunc(getoption(ps));
            %let _fc_= %sysfunc(getoption(formchar));

            OPTIONS ls=max ps=32756  FORMCHAR='|'  nodate nocenter;

            title; footnote;

            proc printto print=&_tmp1_;
            run;quit;

       %end;
       %else %do;

            /* %let outdsn=tst;  */
            %put @@@ &=sysindex.;

            %let _tmp2_=b&sysindex.;
            %let _tmp1_=a%eval(&sysindex - 2);

            %put xxxx  &=_tmp1_;
            %put xxxx  &=_tmp2_;

            proc printto;
            run;quit;

            filename &_tmp2_ clear;

            %utlfkil(%sysfunc(pathname(work))/&_tmp2_.txt);

            proc datasets lib=work nolist;  *just in case;
             delete &outdsn;
            run;quit;

            filename &_tmp2_ "%sysfunc(pathname(work))/&_tmp2_.txt";

            data _null_;
              infile &_tmp1_ length=l;
              input lyn $varying32756. l;
              if countc(lyn,'|')>1;
              lyn=compress(lyn);
              putlog lyn;
              file &_tmp2_;
              put lyn;
            run;quit;

            proc import
               datafile=&_tmp2_
               dbms=dlm
               out=&outdsn(drop=VAR:)
               replace;
               delimiter='|';
               getnames=yes;
            run;quit;

            filename &_tmp1_ clear;
            filename &_tmp2_ clear;

            %utlfkil(%sysfunc(pathname(work))/&_tmp1_.txt);
            %utlfkil(%sysfunc(pathname(work))/&_tmp2_.txt);

       %end;

    %mend utl_odsrpt;

    %macro utl_odsfrq(outdsn);

       %if %qupcase(&outdsn)=SETUP %then %do;

            filename _tmp1_ clear;  * just in case;

            %utlfkil(%sysfunc(pathname(work))/_tmp1_.txt);

            filename _tmp1_ "%sysfunc(pathname(work))/_tmp1_.txt";

            %let _ps_= %sysfunc(getoption(ps));
            %let _fc_= %sysfunc(getoption(formchar));

            OPTIONS ls=max ps=32756  FORMCHAR='|'  nodate nocenter;

            title; footnote;

            proc printto print=_tmp1_;
            run;quit;

       %end;
       %else %do;

            proc printto;
            run;quit;

            filename _tmp2_ clear;

            %utlfkil(%sysfunc(pathname(work))/_tmp2_.txt);

            filename _tmp2_ "%sysfunc(pathname(work))/_tmp2_.txt";

            proc datasets lib=work nolist;  *just in case;
             delete &outdsn;
            run;quit;

            data _null_;
              infile _tmp1_ length=l;
              input lyn $varying32756. l;
              if index(lyn,'Col Pct')>0 then substr(lyn,1,7)='LEVELN   ';
              lyn=compbl(lyn);
              if countc(lyn,'|')>1;
              putlog lyn;
              file _tmp2_;
              put lyn;
            run;quit;

            proc import
               datafile=_tmp2_
               dbms=dlm
               out=_temp_
               replace;
               delimiter='|';
               getnames=yes;
            run;quit;

            data &outdsn(rename=(_total=TOTAL));
              length rowNam $8 level $64;
              retain rowNam level ;
              set _temp_;
              select (mod(_n_-1,4));
                when (0) do; level=cats(leveln); rowNam="COUNT";end;
                when (1) rowNam="PERCENT";
                when (2) rowNam="ROW PCT";
                when (3) rowNam="COL PCT";
              end;
              drop leveln;
            run;quit;

            filename _tmp1_ clear;
            filename _tmp2_ clear;

            %utlfkil(%sysfunc(pathname(work))/_tmp1_.txt);
            %utlfkil(%sysfunc(pathname(work))/_tmp2_.txt);

       %end;

    %mend utl_odsfrq;


    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

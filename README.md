# utl_3D_scatter_plots_in_SAS_Python_and_R
3D scatter plots in SAS Python and R.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    3D scatter plots in SAS Python and R (multiple packages in R and Python?)

      Three Solutions (The perspectives are defaults and may be different)

           1. SAS proc g3d
           2. WPS/Python (matplotlib)
           3. WPS/Proc R scatterplot3d


    R 3D
    https://tinyurl.com/y9mfa4jk
    https://github.com/rogerjdeangelis/utl_3D_scatter_plots_in_SAS_Python_and_R/blob/master/utl_3D_scatter_plots_in_SAS_Python_and_R.pdf

    SAS 3D
    https://tinyurl.com/ycwst6ss
    https://github.com/rogerjdeangelis/utl_3D_scatter_plots_in_SAS_Python_and_R/blob/master/utl_3D_scatter_plots_in_SAS_Python_and_R_sas.pd

    PYTHON 3D (expects you to click and rotate the image?)
    https://tinyurl.com/ycwst6ss
    https://github.com/rogerjdeangelis/utl_3D_scatter_plots_in_SAS_Python_and_R/blob/master/utl_3D_scatter_plots_in_SAS_Python_and_R_python

    githb project
    https://github.com/rogerjdeangelis/utl_3D_scatter_plots_in_SAS_Python_and_R

    Three Solutions (The perspectives are defaults and may be different)

        1. SAS proc g3d
        2. Python (matplotlib)
        3. R scatterplot3d

    It is my understanding that 3D scatter plots are not supported in SG graphics.
    University edition only supports SG graphics, here si a solution using R or Python.

    see
    https://stackoverflow.com/questions/50454098/python3-create-a-3-d-plot-from-a-dictionary-value-tuple

    Sacul profile
    https://stackoverflow.com/users/6671176/sacul


    INPUT
    =====

     SD1.HAVE total obs=31

       Obs     X     Y     Z

         1     0     7     2
         2     1    11     2
         3     2     7     2
         4     3     1     3
         5     4    13     4
         6     5     1    13
         7     6     2    27
         8     7    29    22
         9     8    12    11
       ...
        29    28    10     8
        30    29    21    27
        31    30    18    21


    PROCESS
    =======

      1. SAS

          filename outfile "d:/pdf/&pgm._sas.pdf";
          options orientation=landscape;
          goptions reset=all gsfmode = replace gsfname = outfile device=pdf
             vsize=6in hsize=6in;
          proc g3d data=sd1.have;
             scatter x*y=z / noneedle grid shape="ballon";
          run;quit;
          filename outfile clear;

      2. WPS/PROC R  (working code0

          scatterplot3d(have[,1:3]);

      3. WPS/Python

          ax = fig.gca(projection='3d');
          ax.scatter(have['X'], have['Y'], have['Z']);

    OUTPUT

    *                _              _       _
     _ __ ___   __ _| | _____    __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \  / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/ | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|  \__,_|\__,_|\__\__,_|

    ;
    libname sd1 'd:/sd1';
    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
     do x=0 to 30;
      y=int(30*uniform(1234));
      z=int(30*uniform(1234));
      output;
     end;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __  ___
    / __|/ _ \| | | | | __| |/ _ \| '_ \/ __|
    \__ \ (_) | | |_| | |_| | (_) | | | \__ \
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|___/

    ;
    *SAS;

    filename outfile "d:/pdf/&pgm._sas.pdf";
    run;quit;
    options orientation=landscape;
    goptions reset=all gsfmode = replace gsfname = outfile device=pdf
       vsize=6in hsize=6in;
    proc g3d data=sd1.have;
       scatter x*y=z / noneedle grid shape="ballon";
    run;quit;
    filename outfile clear;

    * PYTHON;
    %utl_submit_wps64("
    options set=PYTHONHOME 'c:\progra~1\python~1.5\';
    options set=PYTHONPATH 'c:\progra~1\python~1.5\lib\';
    libname sd1 sas7bdat   'd:/sd1';
    libname wrk sas7bdat   '%sysfunc(pathname(work))';
    proc python;
    submit;
    import numpy as np;
    import pandas as pd;
    from matplotlib import pyplot;
    from mpl_toolkits.mplot3d import Axes3D;
    from sas7bdat import SAS7BDAT;
    have = pandas.read_sas('d:/sd1/have.sas7bdat',encoding='ascii');
    fig = pyplot.figure();
    ax = fig.gca(projection='3d');
    ax.scatter(have['X'], have['Y'], have['Z']);
    pyplot.savefig('d:/pdf/&pgm._python.pdf');
    endsubmit;
    run;quit;
    ");

    * R;
    %utl_submit_wps64('
    libname sd1 "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk  sas7bdat "%sysfunc(pathname(work))";
    libname hlp  sas7bdat "C:\Progra~1\SASHome\SASFoundation\9.4\core\sashelp";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.1/etc/Rprofile.site", echo=T);
    library(haven);
    library(scatterplot3d);
    have<-read_sas("d:/sd1/have.sas7bdat");
    head(have);
    pdf("d:/pdf/&pgm..pdf");
    scatterplot3d(have[,1:3]);
    pdf();
    endsubmit;
    run;quit;
    ');


    *SAS;
    filename outfile "d:/pdf/draft.pdf";
    run;quit;
    goptions reset=all gsfmode = replace gsfname = outfile  device=pdf rotate=portrait
       vsize=6in
        hsize=6in
    ;
    run;quit;
    proc g3d data=sd1.have;
       scatter x*y=z
             / noneedle
               grid shape="ballon";
    run;quit;
    filename outfile clear;


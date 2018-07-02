# utl_extract_a_particular_value_from_an_xml_document
Extract a particular value from an xml document.  Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.

    Extract a particular value from an xml document

     WPS / PROC R (probably as easy in Python or Perl and faster?)

    This demonstrates the power of lists.
    Especially with xml, html and json files.
    The nested data structure is mapped to the R list.

    github
    https://github.com/rogerjdeangelis/utl_extract_a_particular_value_from_an_xml_document

    see
    https://tinyurl.com/y9gw6dzl
    https://stackoverflow.com/questions/51077430/extract-a-particular-value-from-an-xml-document-in-r


    INPUT
    =====

    <contrib-group>
        <contrib contrib-type="author" xlink:type="simple">
           <string-name>
              <given-names>Anagha K.</given-names>
              <x xml:space="preserve"> </x>
              <surname>Matapurkar</surname>
           </string-name>
           <x xml:space="preserve"/>
        </contrib>
        <contrib contrib-type="author" xlink:type="simple">
           <string-name>
              <given-names>Milind G.</given-names>
              <x xml:space="preserve"> </x>
              <surname>Watve</surname>
           </string-name>
           <x xml:space="preserve"/>
        </contrib>
        <aff id="aff_1">Life Research Foundation, 10 Pranav, 1000/6-c, Navi Peth, Pune 411 030, India</aff>
        <x xml:space="preserve"/>
    </contrib-group>


    EXAMPLE OUTPUT
    --------------

      WORK.WANT total obs=1
                                            AFF

       Life Research Foundation, 10 Pranav, 1000/6-c, Navi Peth, Pune 411 030, India


    RULES

      Here in the map that R autmatically generates

      List of 1
       $ contrib-group:List of 4
        ..$ contrib:List of 2
        .. ..$ string-name:List of 3
        .. .. ..$ given-names:List of 1
        .. .. .. ..$ : chr "Anagha K."
        .. .. ..$ x          :List of 1
        .. .. .. ..$ : chr " "
        .. .. .. ..- attr(*, "space")= chr "preserve"
        .. .. ..$ surname    :List of 1
        .. .. .. ..$ : chr "Matapurkar"
        .. ..$ x          : list()
        .. .. ..- attr(*, "space")= chr "preserve"
        .. ..- attr(*, "contrib-type")= chr "author"
        .. ..- attr(*, "xlink:type")= chr "simple"
        ..$ contrib:List of 2
        .. ..$ string-name:List of 3
        .. .. ..$ given-names:List of 1
        .. .. .. ..$ : chr "Milind G."
        .. .. ..$ x          :List of 1
        .. .. .. ..$ : chr " "
        .. .. .. ..- attr(*, "space")= chr "preserve"
        .. .. ..$ surname    :List of 1
        .. .. .. ..$ : chr "Watve"
        .. ..$ x          : list()
        .. .. ..- attr(*, "space")= chr "preserve"
        .. ..- attr(*, "contrib-type")= chr "author"
        .. ..- attr(*, "xlink:type")= chr "simple"

     **   THIS IS THE ELEMENT WE WANT
     **   THIS IS HOW IT IS REFERENCES [["contrib-group"]][["aff"]][[1]]

        ..$ aff    :List of 1
        .. ..$ : chr "Life Research Foundation, 10 Pranav, 1000/6-c, Navi Peth, Pune 411 030, India"
        .. ..- attr(*, "id")= chr "aff_1"
        ..$ x      : list()
        .. ..- attr(*, "space")= chr "preserve"


    PROCESS (WORKING CODE)
    =======================

      xml_as_list <- as_list(read_xml("d:/xml/utl_extract_a_particular_value_from_an_xml_document.xml"));
      want<- as.data.frame(xml_as_list[["contrib-group"]][["aff"]][[1]]);


    OUTPUT
    ======


      WORK.WANT total obs=1
                                            AFF

       Life Research Foundation, 10 Pranav, 1000/6-c, Navi Peth, Pune 411 030, India

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

    data _null_;
     file "d:/xml/utl_extract_a_particular_value_from_an_xml_document.xml";
     input;
     put _infile_;
    cards4;
    <contrib-group>
        <contrib contrib-type="author" xlink:type="simple">
           <string-name>
              <given-names>Anagha K.</given-names>
              <x xml:space="preserve"> </x>
              <surname>Matapurkar</surname>
           </string-name>
           <x xml:space="preserve"/>
        </contrib>
        <contrib contrib-type="author" xlink:type="simple">
           <string-name>
              <given-names>Milind G.</given-names>
              <x xml:space="preserve"> </x>
              <surname>Watve</surname>
           </string-name>
           <x xml:space="preserve"/>
        </contrib>
        <aff id="aff_1">Life Research Foundation, 10 Pranav, 1000/6-c, Navi Peth, Pune 411 030, India</aff>
        <x xml:space="preserve"/>
    </contrib-group>
    ;;;;
    run;quit;

    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_submit_wps64('
    libname sd1 "d:/sd1";
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk  sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(xml2);
    xml_as_list <- as_list(read_xml("d:/xml/utl_extract_a_particular_value_from_an_xml_document.xml"));
    want<- as.data.frame(xml_as_list[["contrib-group"]][["aff"]][[1]]);
    colnames(want)<-"aff";
    endsubmit;
    import r=want data=wrk.want;
    run;quit;
    ');


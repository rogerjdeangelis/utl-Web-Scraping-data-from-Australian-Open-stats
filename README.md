# utl-Web-Scraping-data-from-Australian-Open-stats
Web Scraping data from Australian Open stats
    Web Scraping data from Australian Open stats

    GitHub
    https://tinyurl.com/3z7737vv
    https://github.com/rogerjdeangelis/utl-Web-Scraping-data-from-Australian-Open-stats

    RELATED REPOS
    https://github.com/rogerjdeangelis?tab=repositories&q=web+scra&type=&language=
    https://github.com/rogerjdeangelis?tab=repositories&q=jsonlite++in%3Areadme&type=&language=

    Inspired by
    https://stackoverflow.com/questions/66702595/scraping-data-from-australian-open-stats

    Solution by ronak-shah
    https://stackoverflow.com/users/3962914/ronak-shah

    See  S-JDBC Driver
    https://dullesresearch.com/

    *_                   _
    (_)_ __  _ __  _   _| |_
    | | '_ \| '_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    ;

    NOTE THIS IS A JSON FILE

    https://prod-scores-api.ausopen.com/year/2021/stats

    {"statistics":[{"name":"Aces","ordering":1,"rankings":
    [{"event_id":"a6bfdfaf-e000-4fd9-96b3-b3b2c423af40","players":
    [{"player_id":"7b04e056-88b3-4e0a-9431-84f2fe039993","matches":7,
    "value":103,"eventId":"a6bfdfaf-e000-4fd9-96b3-b3b2c423af40","rank":1},
    {"player_id":"4244ca05-50be-4d41-b214-7f69ebb0e77d","matches":5,
    "value":86,"eventId":"a6bfdfaf-e000-4fd9-96b3-b3b2c423af40","rank":2},
    ....

    *
     _ __  _ __ ___   ___ ___  ___ ___
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    ;

    * Note list 'dat' has a lot of information;

    * you need to do yhis if you rerun S-JDBC will not overwrite;
    %utlfkil(d:/sd1/want.sas7bdat);

    %utl_submit_r64(%tslit(
    library(RJDBC);
    library(dplyr);
    library(jsonlite);
    library(data.table);
    dat <- jsonlite::fromJSON("https://prod-scores-api.ausopen.com/year/2021/stats");
    aces <- bind_rows(dat$statistics$rankings[[1]]$players);
    want<-as.data.table(dat$players %>%
      inner_join(aces, by = c("uuid" = "player_id")) %>%
      select(full_name, value) %>%
      arrange(-value));
    drv<- JDBC("com.dullesopen.jdbc.Driver","d:/carolina/carolina-jdbc-2.4.3.jar");
    conn <- dbConnect(drv, "jdbc:carolina:bulk:libnames=(dir='d:/sd1')", "", "");
    head(want);
    rc<- dbWriteTable(conn,"want",want);
    ));

    libname sd1 "d:/sd1";
    proc print data=sd1.want;
    run;quit;

    *            _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| '_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    ;

    SD1.WANT total obs=40

    Obs    FULL_NAME                VALUE

      1    Novak Djokovic            103
      2    Alexander Zverev           86
      3    Milos Raonic               82
      4    Daniil Medvedev            80
      5    Nick Kyrgios               69
      6    Alexander Bublik           66
      7    Reilly Opelka              61
      8    Jiri Vesely                58
      9    Andrey Rublev              57
    ....

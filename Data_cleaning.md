Data cleaning
================
Granát Marcell

# Setup

``` r
library(tidyverse)
```

``` r
survey_untidied <- readxl::read_excel("Egyetemistak elkepzelesei a jovo munkahelyerol-adat-2020-04-16.xlsx")
```

``` r
df_names %>% set_names(c("variable", "Question in Hungarian", "Question in English")) %>% knitr::kable(caption = "variable names, the original Q in Hungarian & its translation to English", align = c("l", "c", "c"))
```

| variable                  |                                                                                                                                                 Question in Hungarian                                                                                                                                                 |                                                                                                                             Question in English                                                                                                                             |
| :------------------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| sex                       |                                                                                                                                                          Nem                                                                                                                                                          |                                                                                                                                     Sex                                                                                                                                     |
| adress                    |                                                                                                                                              Hol van az állandó lakcíme?                                                                                                                                              |                                                                                                                      Where is your permanent address?                                                                                                                       |
| parents\_edu              |                                                                                                                                   A szülei végzettségei közül melyik a legmagasabb?                                                                                                                                   |                                                                                                        Which is the highest level of the education of your parents?                                                                                                         |
| uni                       |                                                                                                                                       Melyik felsőoktatási intézményben tanul?                                                                                                                                        |                                                                                                                      At which university do you study?                                                                                                                      |
| level\_edu                |                                                                                                      Milyen képzésre jár ezen az egyetemen? (Ha többfélére is, akkor a legfontosabbnak tartottat jelölje meg\!)                                                                                                       |                                                                         What kind of education do you attend at this university? (If more than one, mark the one you consider the most important\!)                                                                         |
| area\_edu                 |                                                                                                               Milyen képzésterületre jár? (Ha többre is, akkor a legfontosabbnak tartottat adja meg\!)                                                                                                                |                                                                                   What is the area of your education? (If more than one, give the one you consider the most important\!)                                                                                    |
| continue\_edu             |                                                                                                                          Tervezi-e, hogy jelenlegi képzése(i) után folytatja a tanulmányait?                                                                                                                          |                                                                                                     Do you plan to continue your education after your current studies?                                                                                                      |
| english\_knowledge        |                                                                                                                                          Milyen szintű az angol nyelvtudása?                                                                                                                                          |                                                                                                                What is the level of your English knowledge?                                                                                                                 |
| german\_knowledge         |                                                                                                                                          Milyen szintű a német nyelvtudása?                                                                                                                                           |                                                                                                                 What is the level of your German knowledge?                                                                                                                 |
| employed                  |                                                                                                                                               Volt-e már munkaviszonya?                                                                                                                                               |                                                                                                                        Have you ever been employed?                                                                                                                         |
| pro\_employed             |                                                                                                                  A munkaviszonya az Ön által legfontosabbnak tartott szakterületéhez kapcsolódott-e?                                                                                                                  |                                                                                             Did this job connected to the field you consider the most appropriate for yourself?                                                                                             |
| time\_employed            |                                                                                                                           Milyen hosszan tartott? (Ha több is volt, összesítve adja meg\!)                                                                                                                            |                                                                                  How much time did you spend in that position? (In case you’ve worked at several places, summarize them\!)                                                                                  |
| company\_type\_SME        |                                                                                                                                        Milyen típusú cégnél dolgozott? \> KKV                                                                                                                                         |                                                                                                              What kind of company have you worked for? \> SME                                                                                                               |
| company\_type\_local      |                                                                                                                                Milyen típusú cégnél dolgozott? \> magyar nagyvállalat                                                                                                                                 |                                                                                                       What kind of company have you worked for? \> local corporation                                                                                                        |
| company\_type\_multi      |                                                                                                                              Milyen típusú cégnél dolgozott? \> multinacionális vállalat                                                                                                                              |                                                                                                   What kind of company have you worked for? \> multinational corporation                                                                                                    |
| company\_type\_public     |                                                                                                                                     Milyen típusú cégnél dolgozott? \> közszféra                                                                                                                                      |                                                                                                         What kind of company have you worked for? \> public sector                                                                                                          |
| company\_type\_other      |                                                                                                                                       Milyen típusú cégnél dolgozott? \> egyéb                                                                                                                                        |                                                                                                             What kind of company have you worked for? \> other                                                                                                              |
| wpi\_wage                 |                                                                                   Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Magas bér és juttatások                                                                                    |                                                                How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> High wages and benefits                                                                |
| wpi\_career               |                                                                                    Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Előmenetel lehetősége                                                                                     |                                                                  How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Career advancement                                                                   |
| wpi\_improvement          |                                                                                       Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Szakmai fejlődés                                                                                       |                                                                 How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Personal improvement                                                                  |
| wpi\_challenge            |                                                                          Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Változatos munkavégzés, szakmai kihívások                                                                           |                                                       How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Interesting work, professional challenges                                                       |
| wpi\_enviroment           |                                                                         Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Munkahelyi környezet (iroda, felszereltség)                                                                          |                                                         How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Work environment (office, equipment)                                                          |
| wpi\_prestige             |                                                                          Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Vállalat hírneve, presztízse, tevékenysége                                                                          |                                                        How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Company reputation, prestige, activity                                                         |
| wpi\_corporate            |                                                                     Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Vállalati struktúra (KKV, multinacionális vállalat)                                                                      |                                                            How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Corporate structure (SME, MNE)                                                             |
| wpi\_housing              |                                                                                     Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Lakhatási támogatás                                                                                      |                                                                   How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Housing allowance                                                                   |
| wpi\_abroad               |                                                                            Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Külföldi tapasztalatszerzés lehetősége                                                                            |                                                         How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Opportunity to gain experience abroad                                                         |
| wpi\_team                 |                                                                                        Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> A csapatmunka                                                                                         |                                                                       How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Team work                                                                       |
| wpi\_friends              |                                                                          Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Hogy kollégái között barátokat is találjon                                                                          |                                                         How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> To find friends among your collegues                                                          |
| wpi\_values               |                                                             Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Hogy Ön és kollégái azonosuljanak a cég céljaival és értékrendjével                                                              |                                        How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> To help you and your colleagues accept your company’s goals and values                                         |
| wpi\_develop              |                                                                              Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Hogy kollégáival együtt fejlődjön                                                                               |                                                            How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> To develop with your colleagues                                                            |
| wpi\_well\_being          |                                          Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Hogy legyenek olyan kezdeményezések, amelyek sikeresen biztosítják a munkavállalók testi és lelki jólétét                                           |                             How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> To have opportunities that succesfully ensure the physical and mental well-being of employees                             |
| wpi\_manageable\_wl       | Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Hogy a munkaterhelés a munkaidő alatt kezelhető legyen, így a munkavállalók teljes mértékben kihasználhassák a pihenőidőt, és munkahelyi nyomás nélkül lazíthassanak esténként és hétvégeken | How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Workload should be manageable during working hours, so that employees can take full advantage of their rest and relax in the evenings and on weekends |
| wpi\_realxation           |                  Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Olyan munkakörnyezet megléte, amely elősegíti a jólétet (például a pihenésre, a felüdülésre alkalmas terek), és alkalmazkodik a különféle munkastílusokhoz                  |             How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> A work environment ensuring well-being (for example, relaxation and recreation spaces) and adapting to different work styles              |
| wpi\_ho                   |                                    Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Hogy a munkahely aktívan támogassa a táv- és a virtuális munkát mindazoknak, akiknek a munkalehetősége lehetővé teszi                                     |                                                 How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Workplace should support home office and virtual work                                                 |
| wpi\_free\_timeing        |                                                      Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Hogy a munkavállaló szabadon dönthessen arról, hogy hogyan strukturálja a munkáját                                                      |                                                 How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Employee is free to decide how to structure his work                                                  |
| wpi\_platform             |                                                    Mennyire tartja fontosnak az alábbi szempontokat egy munkahely kapcsán? (1: egyáltalán nem fontos - 5: nagyon fontos) \> Hogy legyen egy virtuális platform, amely elősegíti a kollégákkal való együttműködést                                                     |                                     How important do you consider the following aspects for a workplace? (1: not important at all - 5: very important) \> Existence of a virtual platform to facilitate the cooperation with colleagues                                     |
| quit\_respect             |                                                                                   Melyik tényezők miatt mondana fel egy munkahelyen? (1: emiatt nem mondanék fel - 5: emiatt mindenképpen felmondanék) \> A munkámat nem ismerik el                                                                                   |                                                    Which factors would cause you to quit at a workplace? (1: I would not quit because of this - 5: I would definitely quit because of this) \> My work is not respected                                                     |
| quit\_wageincrease        |                                                                            Melyik tényezők miatt mondana fel egy munkahelyen? (1: emiatt nem mondanék fel - 5: emiatt mindenképpen felmondanék) \> A béremelés kicsiny mértéke, elmaradása                                                                            |                                           Which factors would cause you to quit at a workplace? (1: I would not quit because of this - 5: I would definitely quit because of this) \> The small rate or the lack of wage increase                                           |
| quit\_promotion           |                                                                   Melyik tényezők miatt mondana fel egy munkahelyen? (1: emiatt nem mondanék fel - 5: emiatt mindenképpen felmondanék) \> Ha az előrejutás nem szakmai szempontok alapján történik                                                                    |                                      Which factors would cause you to quit at a workplace? (1: I would not quit because of this - 5: I would definitely quit because of this) \> Promotion is not based on professional considerations                                      |
| quit\_workload            |                                                                                 Melyik tényezők miatt mondana fel egy munkahelyen? (1: emiatt nem mondanék fel - 5: emiatt mindenképpen felmondanék) \> Sok túlóra, nagy leterheltség                                                                                 |                                               Which factors would cause you to quit at a workplace? (1: I would not quit because of this - 5: I would definitely quit because of this) \> Many overtime hours, heavy workload                                               |
| quit\_overtime            |                                                                                    Melyik tényezők miatt mondana fel egy munkahelyen? (1: emiatt nem mondanék fel - 5: emiatt mindenképpen felmondanék) \> Túlórák ki nem fizetése                                                                                    |                                                     Which factors would cause you to quit at a workplace? (1: I would not quit because of this - 5: I would definitely quit because of this) \> No payment of overtime                                                      |
| quit\_enviroment          |                                                                              Melyik tényezők miatt mondana fel egy munkahelyen? (1: emiatt nem mondanék fel - 5: emiatt mindenképpen felmondanék) \> Szakmailag nem megfelelő környezet                                                                               |                                            Which factors would cause you to quit at a workplace? (1: I would not quit because of this - 5: I would definitely quit because of this) \> Professionally inappropriate environment                                             |
| quit\_monotnous           |                                                                                 Melyik tényezők miatt mondana fel egy munkahelyen? (1: emiatt nem mondanék fel - 5: emiatt mindenképpen felmondanék) \> Monoton munka, kevés kihívás                                                                                  |                                                 Which factors would cause you to quit at a workplace? (1: I would not quit because of this - 5: I would definitely quit because of this) \> Monotonous work, few challenges                                                 |
| quit\_salaryoffer         |                                                                        Melyik tényezők miatt mondana fel egy munkahelyen? (1: emiatt nem mondanék fel - 5: emiatt mindenképpen felmondanék) \> Legalább 20%-kal magasabb bérajánlat máshonnan                                                                         |                                           Which factors would cause you to quit at a workplace? (1: I would not quit because of this - 5: I would definitely quit because of this) \> At least 20% higher salary offer elsewhere                                            |
| value\_added              |                                                                                                                       Melyiket tartja fontosabbnak? (Hozzáadott érték vagy munkaidő kitöltése)                                                                                                                        |                                                                                             Which one do you consider more important? (Value added or Filling in working time)                                                                                              |
| up\_or\_out               |                                                                                                                          Melyiket tartja fontosabbnak? (Multiple career path vagy Up or out)                                                                                                                          |                                                                                             Which one do you consider more important? (“Up or out” or “Multiple career path” )                                                                                              |
| prioritize\_wl\_balance   |                                                                                           Állítsa fontosság szerint sorrendbe az alábbiakat\! Előre kerüljön az Ön szerint legfontosabb\! \> Munka és magánélet egyensúlya                                                                                            |                                                                                     Prioritize the following\! Get ahead of what you think is the most important\! \> Work-life balance                                                                                     |
| prioritize\_salary        |                                                                                                      Állítsa fontosság szerint sorrendbe az alábbiakat\! Előre kerüljön az Ön szerint legfontosabb\! \> Fizetés                                                                                                       |                                                                                          Prioritize the following\! Get ahead of what you think is the most important\! \> Salary                                                                                           |
| prioritize\_pro\_progress |                                                                                                Állítsa fontosság szerint sorrendbe az alábbiakat\! Előre kerüljön az Ön szerint legfontosabb\! \> Szakmai előrehaladás                                                                                                |                                                                                   Prioritize the following\! Get ahead of what you think is the most important\! \> Professional progress                                                                                   |
| prioritize\_realtions     |                                                                                               Állítsa fontosság szerint sorrendbe az alábbiakat\! Előre kerüljön az Ön szerint legfontosabb\! \> Szociális kapcsolatok                                                                                                |                                                                                     Prioritize the following\! Get ahead of what you think is the most important\! \> Social relations                                                                                      |
| ppl\_room                 |                                                                                                                              Mit tartana ideálisnak, hányan dolgozzanak egy helyiségben?                                                                                                                              |                                                                                                    What do you consider ideal, how many people should work in one room?                                                                                                     |
| meeting                   |                                                                                                                              Mit tart ideálisnak a meetingek gyakoriságára vonatkozóan?                                                                                                                               |                                                                                                          What do you think about the ideal frequency for meetings?                                                                                                          |
| online\_contact           |                                                                                                                             Igényelné-e az állandó online kapcsolattartást a kollégákkal?                                                                                                                             |                                                                                                       Would you require constant online contact with your colleagues?                                                                                                       |
| atypical\_platform        |                                                                                                                   Az alábbi atipikus foglalkozási formák közül melyik érdekelné? \> platform alapú                                                                                                                    |                                                                                       Which of the following atypical employment forms would you be interested in? \> platform based                                                                                        |
| atypical\_telework        |                                                                                                                      Az alábbi atipikus foglalkozási formák közül melyik érdekelné? \> távmunka                                                                                                                       |                                                                                          Which of the following atypical employment forms would you be interested in? \> telework                                                                                           |
| atypical\_part\_time      |                                                                                                                      Az alábbi atipikus foglalkozási formák közül melyik érdekelné? \> részmunka                                                                                                                      |                                                                                        Which of the following atypical employment forms would you be interested in? \> part-time job                                                                                        |
| atypical\_startup         |                                                                                                        Az alábbi atipikus foglalkozási formák közül melyik érdekelné? \> saját vállalkozás indítása (startup)                                                                                                         |                                                                                           Which of the following atypical employment forms would you be interested in? \> startup                                                                                           |
| atypical\_non\_fixed      |                                                                                                           Az alábbi atipikus foglalkozási formák közül melyik érdekelné? \> nem helyhez kötött munkavégzés                                                                                                            |                                                                                    Which of the following atypical employment forms would you be interested in? \> non-fixed employment                                                                                     |
| comf\_salary              |                                                                                                                                 Mekkora az a havi nettó bér, amivel elégedett lenne?                                                                                                                                  |                                                                                          What is your monthly net salary that you would be comfortable with? (In Hungarian Forint)                                                                                          |
| real\_salary              |                                                                                                          Ön szerint mekkora az a havi nettó bér, amely az Ön szakterületén pályakezdőként reálisan elérhető?                                                                                                          |                                                                    What do you think is the net monthly salary that is realistically available in your field as a career beginner? (In Hungarian Forint)                                                                    |
| fivey\_salary             |                                                                                                                            5 év elteltével a kezdőfizetése hányszorosával lenne elégedett?                                                                                                                            |                                                                                                After 5 years, how many times of your starting salary would you be satisfied?                                                                                                |
| fivey\_position           |                                                                                                                                   5 év elteltével milyen beosztást szeretne elérni?                                                                                                                                   |                                                                                                            After 5 years, what position do you want to achieve?                                                                                                             |
| fiveyy\_development       |                                                                                                   Mennyire fontos önnek, hogy 5 év alatt jelentős szakmai fejlődést produkáljon (anyagiakon és előremenetelen túl)?                                                                                                   |                                                                     How important is it to you to produce significant professional development (beyond material and hierarchical position) in 5 years?                                                                      |
| work\_abroad              |                                                                                                                         A végzettség megszerzését követően tervez-e külföldi munkavállalást?                                                                                                                          |                                                                                                                Do you plan to work abroad after graduation?                                                                                                                 |
| work\_abroad\_c           |                                                                                                                                                   Melyik országban?                                                                                                                                                   |                                                                                                                              In which country?                                                                                                                              |
| work\_abroad\_p           |                                                                                                                                  A szakterületén tervezi a külföldi munkavállalást?                                                                                                                                   |                                                                                                          Do you plan on working abroad in your professional field?                                                                                                          |
| country\_stay             |                                                                                                                                         Ha Magyarországon maradna, akkor hol?                                                                                                                                         |                                                                                                                      If you stayed in Hungary, where?                                                                                                                       |
| stay\_enviroment          |                                                                                                    Az előző kérdésre adott válasz tekintetében mely tényezőket tartja relevánsnak? \> Szellemi és kulturális közeg                                                                                                    |                                                                           Which factors do you consider relevant to the answer to the previous question? \> intellectual and cultural environment                                                                           |
| stay\_material            |                                                                                                    Az előző kérdésre adott válasz tekintetében mely tényezőket tartja relevánsnak? \> Magasabb anyagi megbecsülés                                                                                                     |                                                                               Which factors do you consider relevant to the answer to the previous question? \> higher material appreciation                                                                                |
| stay\_housing             |                                                                                                 Az előző kérdésre adott válasz tekintetében mely tényezőket tartja relevánsnak? \> Lakhatási és megélhetési kiadások                                                                                                  |                                                                                Which factors do you consider relevant to the answer to the previous question? \> housing and living expenses                                                                                |
| stay\_family              |                                                                                                        Az előző kérdésre adott válasz tekintetében mely tényezőket tartja relevánsnak? \> Családi kapcsolatok                                                                                                         |                                                                                   Which factors do you consider relevant to the answer to the previous question? \> family relationships                                                                                    |
| stay\_friends             |                                                                                                         Az előző kérdésre adott válasz tekintetében mely tényezőket tartja relevánsnak? \> Baráti kapcsolatok                                                                                                         |                                                                                   Which factors do you consider relevant to the answer to the previous question? \> friends relationships                                                                                   |
| stay\_development         |                                                                                               Az előző kérdésre adott válasz tekintetében mely tényezőket tartja relevánsnak? \> Szakmai kihívás és fejlődés lehetősége                                                                                               |                                                                 Which factors do you consider relevant to the answer to the previous question? \> professional challanges and opportunitiy for development                                                                  |
| scholarship               |                                                                                  Állami ösztöndíjas, részösztöndíjas vagy önköltséges hallgató? (Ha többfajta képzésre is jár, akkor az erősebb ösztöndíj kategóriát jelölje meg\!)                                                                                   |                                                        State scholarship, part-scholarship or self-financing student? (If more than one type of training is involved, indicate the stronger scholarship category\!)                                                         |
| mobility\_restrictions    |                                                                                                                Befolyásolja-e a röghöz kötés abban a döntésben, hogy tervez-e külföldi munkavállalást?                                                                                                                |                                                                                                Do mobility restrictions affect your future plans concerning working abroad?                                                                                                 |
| sabbatical\_know          |                                                                                                                                       Tudja-e, mit fed a sabbatical kifejezés?                                                                                                                                        |                                                                                                                 Do you know what the term sabbatical means?                                                                                                                 |
| ho                        |                                                                                                                                             Van-e igénye home office-ra?                                                                                                                                              |                                                                                                                        Would you prefer home office?                                                                                                                        |
| work\_kind                |                                                                                                                                    Milyen munkavégzés felel meg Önnek a legjobban?                                                                                                                                    |                                                                                                                     What kind of work is best for you?                                                                                                                      |
| change\_job               |                                                                                                                                           Milyen gyakran váltana munkakört?                                                                                                                                           |                                                                                                                      How often would you change jobs?                                                                                                                       |
| flexibility               |                                                                                                                         Mennyire rugalmas a munkavégzés helyét illetően? Hol vállalna munkát?                                                                                                                         |                                                                                                    How flexible are you about your place of work? Where would you work?                                                                                                     |
| baby                      |                                                                                                                 Segítene-e a gyermekvállalásban, ha tudná, hogy a munkahely rugalmasan kezeli azt (pl                                                                                                                 |                                                                           Would it be helpful if the workplace was flexible (eg providing temporary part-time work) when you were having a baby?                                                                            |
| goal\_confidence          |                                                                                                                     Mennyire bízik abban, hogy eléri a következő 10 évre kitűzött karriercéljait?                                                                                                                     |                                                                                                 How confident are you in achieving your career goals for the next 10 years?                                                                                                 |
| ws\_high\_payment         |                                                             Értékelje 0-tól 10-ig a következő munkahelyi szituációkat\! \> Kiemelt fizetés, napi 10-12 óra munka, akár hétvégeken is, nemzetközi munkakörnyezet, sok stressz, állandó rendelkezésre állás                                                             |                                                   Rate the following workplace situations from 0 to 10 \> High payment, 10-12 hours of work a day, sometimes even at weekends, international environment, stressful work                                                    |
| ws\_stable                |                                                                                Értékelje 0-tól 10-ig a következő munkahelyi szituációkat\! \> Biztos állás, állandó feladatok, napi 8 óra munka, pozíciónak megfelelő átlagos fizetés                                                                                 |                                                                                 Rate the following workplace situations from 0 to 10 \> Stable job, 8 hours of work a day, average payment                                                                                  |
| ws\_flexible              |                                                 Értékelje 0-tól 10-ig a következő munkahelyi szituációkat\! \> Rugalmas munkaidő, szakmai önállóság, nem hierarchikus munkaszervezet, horizontális karrierpályák, munkavégzéssel azonos bérezés, nincs fix jövedelem                                                  |                         Rate the following workplace situations from 0 to 10 \> Flexible working hours, professional independence, non-hierarchical work organization, horizontal career path, payment based on the working hours, no fixed income                          |
| ws\_inspiring             |                                                      Értékelje 0-tól 10-ig a következő munkahelyi szituációkat\! \> Szakmailag inspiráló munkakörnyezet, szakmai fejlődés folyamatos kényszere, változatos munka folyamatos kihívásokkal, nem kiemelkedő bérezés                                                      |                                              Rate the following workplace situations from 0 to 10 \> Inspiring professional environment, permanent urge of professional advancement, varied, challenging work, average payment                                              |
| ws\_management            |                                                                               Értékelje 0-tól 10-ig a következő munkahelyi szituációkat\! \> Felsővezetői pozíció, kiemelkedő bérezés, hatékony döntéselőkészítő csapat, stabil jövőkép                                                                               |                                                                  Rate the following workplace situations from 0 to 10 \> Top management position, high payment, effective decision-making team, stable job                                                                  |
| ws\_large\_company        |                                                             Értékelje 0-tól 10-ig a következő munkahelyi szituációkat\! \> Nagyvállalat, minimális előrelépési lehetőség, hierarchikus szervezet, pozíciónak megfelelő átlagos fizetés, fizetetlen túlóra                                                             |                                               Rate the following workplace situations from 0 to 10 \> Large company, low chance of professional advancement, hierarchical work organization, average payment, unpaid overtime                                               |
| wb\_life                  |                                                                                                                 Értékelje 0-tól 10-ig a következőket\! \> Mennyire elégedett az életével mostanában?                                                                                                                  |                                                                                              Rate the following from 0 to 10\! \> How satisfied are you with your life lately?                                                                                              |
| wb\_finance               |                                                                                                                  Értékelje 0-tól 10-ig a következőket\! \> Mennyire elégedett az anyagi helyzetével?                                                                                                                  |                                                                                          Rate the following from 0 to 10\! \> How satisfied are you with your financial situation?                                                                                          |
| wb\_study                 |                                                                                                         Értékelje 0-tól 10-ig a következőket\! \> Mennyire elégedett a jelenlegi munkájával / tanulmányaival?                                                                                                         |                                                                                          Rate the following from 0 to 10\! \> How satisfied are you with your current study/work?                                                                                           |
| wb\_conditions            |                                                                                                        Értékelje 0-tól 10-ig a következőket\! \> Mennyire elégedett a munkába (iskolába) járás körülményeivel?                                                                                                        |                                                                                   Rate the following from 0 to 10\! \> How satisfied are you with the conditions of going to work/school?                                                                                   |
| wb\_time\_spent           |                                                                                          Értékelje 0-tól 10-ig a következőket\! \> Mennyire elégedett azon idő mennyiségével, amelyet az Ön által kedvelt dolgokkal tölthet?                                                                                          |                                                                                Rate the following from 0 to 10\! \> How satisfied are you with the amount of time spent on things you like?                                                                                 |
| wb\_meaningful            |                                                                                                 Értékelje 0-tól 10-ig a következőket\! \> Összességében mennyire érzi tartalmasnak azokat a dolgokat, amiket csinál?                                                                                                  |                                                                                    Rate the following from 0 to 10\! \> Overall, how meaningful do you feel to the things you are doing?                                                                                    |
| wb\_hopefull              |                                                                       Értékelje 0-tól 10-ig a következőket\! \> Mennyire néz reményvesztetten vagy bizakodóan a jövőbe? (0: teljes mértékben reményvesztetten, 10: teljes mértékben bizakodóan)                                                                       |                                                              Rate the following from 0 to 10\! \> How hopelessly or hopefully do you look to the future? (0: completely hopelessly, 10: completely hopefully)                                                               |
| feel\_happy               |                                                                                                                                        Mostanában milyen gyakran volt boldog?                                                                                                                                         |                                                                                                                 Nowadays, how often did you feel \> happy?                                                                                                                  |
| feel\_stress              |                                                                                                                                       Mostanában milyen gyakran volt stresszes?                                                                                                                                       |                                                                                                               Nowadays, how often did you feel \> stressful?                                                                                                                |
| feel\_lonely              |                                                                                                                                  Mostanában milyen gyakran érezte magát magányosnak?                                                                                                                                  |                                                                                                                 Nowadays, how often did you feel \> lonely?                                                                                                                 |
| trust\_ppl                |                                                                                                         Értékelje 0-tól 10-ig a következőket\! \> Ön szerint mennyire lehet megbízni az emberekben általában?                                                                                                         |                                                                                     Rate the following from 0 to 10\! \> What do you think how much do you can trust people in general?                                                                                     |
| trust\_politicals         |                                                                                                       Értékelje 0-tól 10-ig a következőket\! \> Mennyire bízik meg Ön személy szerint a politikai rendszerben?                                                                                                        |                                                                                         Rate the following from 0 to 10\! \> How much do you personally trust the political system?                                                                                         |
| trust\_legals             |                                                                                                           Értékelje 0-tól 10-ig a következőket\! \> Mennyire bízik meg Ön személy szerint a jogrendszerben?                                                                                                           |                                                                                             Rate the following from 0 to 10\! \> How much do you personally trust legal system?                                                                                             |
| trust\_police             |                                                                                                            Értékelje 0-tól 10-ig a következőket\! \> Mennyire bízik meg Ön személy szerint a rendőrségben?                                                                                                            |                                                                                              Rate the following from 0 to 10\! \> How much do you personally trust the police?                                                                                              |
| trust\_military           |                                                                                                            Értékelje 0-tól 10-ig a következőket\! \> Mennyire bízik meg Ön személy szerint a honvédségben?                                                                                                            |                                                                                             Rate the following from 0 to 10\! \> How much do you personally trust the military?                                                                                             |

variable names, the original Q in Hungarian & its translation to English

## General cleaning

``` r
survey_untidied <-
  survey_untidied[-(duplicated(survey_untidied[2]) == T) * (is.na(survey_untidied[2]) == F) * seq(nrow(survey_untidied)), ] %>% # remove the answer from participants, who answered for 2nd time
  select_if(~ sum(!is.na(.)) > 0) %>% # remove empty columns
  .[, ] %>%
  select(-c(1, 2)) %>%  # remove user rank & e-mail ---> not relevant 
  setNames(df_names$variable)
```

## Hungarian

### Factors

``` r
survey_W_outliers_hun <- # before cleaning from outliers at v59 & v60
  survey_untidied %>%
  set_names(str_c("v", seq_along(survey_untidied))) %>% 
  mutate_each(funs(as.factor)) %>%
  mutate_at(c(18:44, 47:50, 63, 82:95, 99:103), factor, ordered = T) %>% 
  mutate(
    v59 = as.integer(survey_untidied$comf_salary),
    v60 = as.integer(survey_untidied$real_salary),
    # factors which has to be sorted
    v3 = factor(v3, levels = c("legfeljebb általános iskola", "szakiskola vagy szakmunkásképző", "középiskola érettségivel", "főiskola", "egyetem"), ordered = T),
    v5 = factor(v5, levels = c("alapképzés", "mesterképzés", "osztatlan képzés", "PHD képzés", "egyéb")),
    v8 = factor(v8, levels = c("nincs", "alap", "közép", "felső"), ordered = T),
    v9 = factor(v9, levels = c("nincs", "alap", "közép", "felső"), ordered = T),
    v12 = factor(v12, levels = c("3 hónapnál rövidebb ideig", "3 hónap és fél év között", "fél évnél hosszabb ideig"), ordered = T),
    v52 = factor(v52, levels = c("még ritkábban", "havonta", "havonta kétszer", "hetente egyszer", "hetente többször", "napi"), ordered = T),
    v62 = factor(v62, levels = c("beosztott", "középvezető", "felsővezető", "független szakértő / saját cégen belüli munkavégzés"), ordered = T),
    v77 = factor(v77, levels = c("nem", "igen, heti pár órára", "igen, heti egy napra", "igen, hetente több napra"), ordered = T),
    v78 = factor(v78, levels = c("fix idejű", "bizonyos keretek között rugalmas", "teljesen rugalmas"), ordered = T),
    v80 = factor(v80, levels = c("egyáltalán nem", "a környező települések még szóba jöhetnek", "távolabbi települések is szóba jöhetnek, de csak Magyarországon", "külföldön is vállalnék munkát"), ordered = T),
    v96 = factor(v96, levels = c("soha", "ritkán", "időnként", "többnyire", "mindig"), ordered = T),
    v97 = factor(v96, levels = c("soha", "ritkán", "időnként", "többnyire", "mindig"), ordered = T),
    v98 = factor(v96, levels = c("soha", "ritkán", "időnként", "többnyire", "mindig"), ordered = T)
  )
```

### Remove outliers

``` r
outlier_remove <- function(v) {
  # replace outliers w NAs, detection eqvivalent w the method of a boxplot
  ifelse(
    v < quantile(v, probs = .25, na.rm = T) - 1.5 * IQR(v, na.rm = T) |
      v > quantile(v, probs = .75, na.rm = T) + 1.5 * IQR(v, na.rm = T),
    NA, v
  )
}
```

``` r
survey_hun <- survey_W_outliers_hun %>% mutate(
  v59 = ifelse(v59 < 10000, v59 * 1000, v59), # manage commit execution error by participant ---> probably thougth to '000 Ft or wrote '.' instead ','
  v60 = ifelse(v60 < 10000, v60 * 1000, v60),
  v59 = outlier_remove(v59),
  v60 = outlier_remove(v60)
) %>% set_names(df_names$variable)

survey_W_outliers_hun <- survey_W_outliers_hun %>% set_names(df_names$variable)
```

## English

### Translate factors

``` r
library(plyr) # ---> revalue() f
survey_W_outliers <- survey_W_outliers_hun %>% mutate(
  sex = revalue(sex, c(
    'Férfi' = 'Male',
    'Nő' = 'Female'
  )),
  parents_edu = revalue(parents_edu, c(
    "legfeljebb általános iskola" = "at most elementary school",
    "szakiskola vagy szakmunkásképző" = "vocational (specialized) school",
    "középiskola érettségivel" = "high school",
    "főiskola" = "BSc",
    "egyetem" = "MSc"
  )),
  uni = revalue(uni, c(
    "Állatorvostudományi Egyetem (ATE)" = "University of Veterinary Medicine Budapest",
    "Budapesti Corvinus Egyetem (BCE)" = "Corvinus University of Budapest",
    "Budapesti Gazdasági Egyetem (BGE)" = "Budapest Business School",
    "Budapesti Műszaki és Gazdaságtudományi Egyetem (BME)" = "Budapest University of Technology and Economics",
    "Debreceni Egyetem (DE)" = "University of Debrecen",
    "Eötvös Loránd Tudományegyetem (ELTE)" = "Eötvös Loránd University",
    "Közép-európai Egyetem (CEU)" = "Central European University",
    "Miskolci Egyetem (ME)" = "Miskolci Egyetem (ME)", # no answer already
    "Nemzeti Közszolgálati Egyetem (NKE)" = "National University of Public Service",
    "Óbudai Egyetem (OE)" = "Óbudai University",
    "Pannon Egyetem (PE)" = "University of Pannonia",
    "Pázmány Péter Katolikus Egyetem (PPKE)" = "Pázmány Péter Catholic University",
    "Pécsi Tudományegyetem (PPKE)" = "University of Pécs",
    "Semmelweis Egyetem (SE)" = "Semmelweis University",
    "Szent István Egyetem (SZIE)" = "Szent István University",
    "Szegedi Tudományegyetem (SZTE)" = "University of Szeged",
    "egyéb külföldi intézmény" = "other foreign institution", # no answer already
    "egyéb magyarországi intézmény" = "other Hungarian institution"
  ), warn_missing = F),
  level_edu = revalue(level_edu, c(
    "alapképzés"    = 'BSc',
    "mesterképzés"  = 'MSc',
    "osztatlan képzés"  = 'undived (BSc and MSc together)',
    "PHD képzés"    = 'PhD',
    "egyéb" = 'other'   
  )),
  area_edu = revalue(area_edu, c(
    "agrár" = "agriculture",
    "államtudományi" = "public science",
    "bölcsészettudományi" = "humanities",
    "gazdaságtudományi" = "economics",
    "informatika" = "IT",
    "jogi" = "law",
    "műszaki" = "technology",
    "művészeti / művészetközvetítési" = "artistic",
    "orvos- és egészségtudományi" = "medical and health sciences",
    "pedagógusképzési" = "teacher training",
    "sporttudományi" = "sport sciences",
    "társadalomtudományi" = "social sciences",
    "természettudományi" = "natural sciences",
    "egyéb" = "other"
  )),
  continue_edu = revalue(continue_edu, c(
    "igen, Magyarországon" = "yes, in Hungary",
    "igen, külföldön" = "yes, in another country",
    "nem" = "no"
  )),
  english_knowledge = revalue(english_knowledge, c(
    "alap" = "basic",
    "közép" = "medium",
    "felső" = "upper",
    "nincs" = "none"
  )),
  german_knowledge = revalue(german_knowledge, c(
    "alap" = "basic",
    "közép" = "medium",
    "felső" = "upper",
    "nincs" = "none"
  )),
  employed = revalue(employed, c(
    "nem" = "no",
    "igen" = "yes"
  )),
  pro_employed = revalue(pro_employed, c(
    "nem" = "no",
    "igen" = "yes"
  )),
  time_employed = revalue(time_employed, c(
    "3 hónapnál rövidebb ideig" = "less than three months",
    "3 hónap és fél év között" = "more than three months",
    "fél évnél hosszabb ideig" = "more than half a year"
  )),
  company_type_SME = revalue(company_type_SME, c("KKV" = "SME")),
  company_type_local = revalue(company_type_local, c("magyar nagyvállalat" = "local corporation")),
  company_type_multi = revalue(company_type_multi, c("multinacionális vállalat" = "multinational corporation")),
  company_type_public = revalue(company_type_public, c("közszféra" = "public sector")),
  company_type_other = revalue(company_type_other, c("egyéb" = "other")),
  value_added = revalue(value_added, c(
    "Hozzáadott érték" = "Value added",
    "Munkaidő kitöltése" = "Filling in working time"
  )),
  up_or_out = revalue(up_or_out, c(
    "Up or out (elsősorban a vállalati ranglétrán való elhelyezkedés jelenti a sikert)" = "Up or out",
    "Multiple career path (a sikeres szakmai karrier nem függ a vállalati hierarchiában betöltött pozíciótól)" = "Multiple career path"
  )),
  ppl_room = revalue(ppl_room, c(
    "1 fő" = "1 person",
    "2-3 fő" = "2-3 people",
    "4-5 fő" = "4-5 people",
    "6-8 fő" = "6-8 people",
    "még többen" = "more people"
  )),
  meeting = revalue(meeting, c(
    "napi" = "Daily",
    "hetente többször" = "More times a week",
    "hetente egyszer" = "Once a week",
    "havonta kétszer" = "Twice a month",
    "havonta" = "Monthly",
    "még ritkábban" = "Less often"
  )),
  online_contact = revalue(online_contact, c(
    "nem" = "no",
    "igen" = "yes"
  )),
  atypical_platform = revalue(atypical_platform, c("platform alapú" = "platform based")),
  atypical_telework = revalue(atypical_telework, c("távmunka" = "telework")),
  atypical_part_time = revalue(atypical_part_time, c("részmunka" = "part-time job")),
  atypical_startup = revalue(atypical_startup, c("saját vállalkozás indítása (startup)" = "startup")),
  atypical_non_fixed = revalue(atypical_non_fixed, c("nem helyhez kötött munkavégzés" = "non-fixed employment")),
  fivey_salary = revalue(fivey_salary, c(
    "1 - 1,5" = "1-1.5",
    "1,5 - 2" = "1.5-2",
    "2 - 2,5" = "2-2.5",
    "nem lényeges szempont" = "This is not a relevant aspect"  
  )),
  fivey_position = revalue(fivey_position, c(
    "felsővezető" = "top management",
    "középvezető" = "middle management",
    "beosztott" = "subordinate",
    "független szakértő / saját cégen belüli munkavégzés" = "independent expert/in-house work"
  )),
  work_abroad = revalue(work_abroad, c(
    "igen" = "yes",
    "nem" = "no",
    "még nem tudom" = "I don’t know yet"
  )),
  work_abroad_c = revalue(work_abroad_c, c(
    "Ausztria" =  "Austria",
    "Dánia" = "Denmark", 
    "Egyesült Királyság" =  "United Kingdom",
    "Franciaország" = "France" ,
    "Hollandia" = "Netherlands" ,
    "Németország" =  "Germany",
    "Olaszország" = "Italy" ,
    "Skandináv államok" =  "Scandinavian countries",
    "Spanyolország" = "Spain" ,
    "Svájc" = "Switzerland" ,
    "Amerikai Egyesült Államok" =  "United States",
    "Kanada" = "Canada", 
    "egyéb európai ország" =  "other European country",
    "egyéb nem európai ország" =  "other non-European country"
  )),
  work_abroad_p = revalue(work_abroad_p, c(
    "igen" = "yes",
    "nem" = "no"
  )),
  country_stay = revalue(country_stay, c(
    "állandó lakóhelyének környezetében" = "In the area of your permanent address",
    "annak környezetében, ahol jelenleg életvitelszerűen él" = "In the surrounding region",
    "ezektől eltérő, más megyében / városban" = "In other counties/cities",
    "a munkahely holléte számomra nem lényeges szempont" = "The whereabouts of workplace is irrelevant to me"
  )),
  stay_enviroment = revalue(stay_enviroment, c("Szellemi és kulturális közeg" = "intellectual and cultural environment")),
  stay_material = revalue(stay_material, c("Magasabb anyagi megbecsülés" = "higher material appreciation")),
  stay_housing = revalue(stay_housing, c("Lakhatási és megélhetési kiadások" = "housing and living expenses")),
  stay_family = revalue(stay_family, c("Családi kapcsolatok" = "family relationships")),
  stay_friends = revalue(stay_friends, c("Baráti kapcsolatok" = "friends relationships")),
  stay_development = revalue(stay_development, c("Szakmai kihívás és fejlődés lehetősége" = "professional challanges and opportunitiy for development")),
  scholarship = revalue(scholarship, c(
    "állami ösztöndíjas" = "full scholarship",
    "részösztöndíjas" = "partial scholarship",
    "önköltséges" = "self-financing"
  )),
  mobility_restrictions = revalue(mobility_restrictions, c(
    "igen" = "yes",
    "nem" = "no"
  )),
  sabbatical_know = revalue(sabbatical_know, c(
    "igen" = "yes",
    "nem" = "no"
  )),
  ho = revalue(ho, c(
    "igen, hetente több napra" = "yes, several days a week",
    "igen, heti egy napra" = "yes, one day a week",
    "igen, heti pár órára" = "yes, several hours a week",
    "nem" = "no"
  )),
  work_kind = revalue(work_kind, c(
    "fix idejű" = "fixed time",
    "bizonyos keretek között rugalmas" = "flexible within certain frames",
    "teljesen rugalmas" = "completely flexible"
  )),
  change_job = revalue(change_job, c(
    "1-2 évente" = "every 1 to 2 years",
    "3-8 évente" = "every 3 to 8 years",
    "8-15 évente" = "every 8 to 15 years",
    "15-30 évente" = "every 15 to 30 years",
    "soha" = "never"
  )),
  flexibility = revalue(flexibility, c(
    "egyáltalán nem" = "Not at all",
    "a környező települések még szóba jöhetnek" = "Surrounding settlements may still considered",
    "távolabbi települések is szóba jöhetnek, de csak Magyarországon" = "Further away settlements may be considered, but only in my own country",
    "külföldön is vállalnék munkát" = "I would also work abroad"
  )),
  baby = revalue(baby, c(
    "igen" = "yes",
    "nem" = "no"
  )),
)
```

### Remove outliers

``` r
survey <- survey_W_outliers %>% mutate(
  comf_salary = survey_hun$comf_salary, # in df survey_hun outliers already managed
  real_salary = survey_hun$real_salary
)
```

``` r
data.frame(
  a = survey_W_outliers_hun$comf_salary,
  b = survey_hun$comf_salary,
  c = survey_W_outliers$comf_salary,
  d = survey$comf_salary
) %>% arrange(a) %>% head(50)
```

``` 
        a      b      c      d
1     150 150000    150 150000
2     160 160000    160 160000
3     200 200000    200 200000
4     200 200000    200 200000
5     230 230000    230 230000
6     250 250000    250 250000
7     250 250000    250 250000
8     250 250000    250 250000
9     250 250000    250 250000
10    250 250000    250 250000
11    300 300000    300 300000
12    300 300000    300 300000
13    300 300000    300 300000
14    300 300000    300 300000
15    300 300000    300 300000
16    300 300000    300 300000
17    300 300000    300 300000
18    300 300000    300 300000
19    300 300000    300 300000
20    350 350000    350 350000
21    350 350000    350 350000
22    350 350000    350 350000
23    350 350000    350 350000
24    380 380000    380 380000
25    400 400000    400 400000
26    400 400000    400 400000
27    400 400000    400 400000
28    400 400000    400 400000
29    400 400000    400 400000
30    400 400000    400 400000
31    450 450000    450 450000
32    460 460000    460 460000
33    500 500000    500 500000
34    700     NA    700     NA
35    700     NA    700     NA
36   2000     NA   2000     NA
37  80000  80000  80000  80000
38 100000 100000 100000 100000
39 120000 120000 120000 120000
40 140000 140000 140000 140000
41 150000 150000 150000 150000
42 150000 150000 150000 150000
43 150000 150000 150000 150000
44 160000 160000 160000 160000
45 170000 170000 170000 170000
46 180000 180000 180000 180000
47 180000 180000 180000 180000
48 180000 180000 180000 180000
49 180000 180000 180000 180000
50 180000 180000 180000 180000
```

``` r
survey %>% skimr::skim()
```

|                                                  |            |
| :----------------------------------------------- | :--------- |
| Name                                             | Piped data |
| Number of rows                                   | 417        |
| Number of columns                                | 103        |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |            |
| Column type frequency:                           |            |
| factor                                           | 101        |
| numeric                                          | 2          |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |            |
| Group variables                                  | None       |

Data summary

**Variable type: factor**

| skim\_variable            | n\_missing | complete\_rate | ordered | n\_unique | top\_counts                          |
| :------------------------ | ---------: | -------------: | :------ | --------: | :----------------------------------- |
| sex                       |          0 |           1.00 | FALSE   |         2 | Fem: 253, Mal: 164                   |
| adress                    |          3 |           0.99 | FALSE   |        21 | Bud: 156, Pes: 66, Haj: 23, Sza: 19  |
| parents\_edu              |          2 |           1.00 | TRUE    |         5 | MSc: 180, hig: 99, BSc: 94, voc: 41  |
| uni                       |          6 |           0.99 | FALSE   |        16 | Cor: 146, Bud: 58, Bud: 44, Uni: 31  |
| level\_edu                |          0 |           1.00 | FALSE   |         5 | BSc: 302, MSc: 75, und: 33, oth: 5   |
| area\_edu                 |          0 |           1.00 | FALSE   |        14 | eco: 215, IT: 54, soc: 36, tec: 21   |
| continue\_edu             |          0 |           1.00 | FALSE   |         3 | yes: 211, no: 136, yes: 70           |
| english\_knowledge        |          1 |           1.00 | TRUE    |         4 | med: 193, upp: 176, bas: 44, non: 3  |
| german\_knowledge         |          0 |           1.00 | TRUE    |         4 | non: 176, bas: 126, med: 84, upp: 31 |
| employed                  |          0 |           1.00 | FALSE   |         2 | yes: 313, no: 104                    |
| pro\_employed             |        101 |           0.76 | FALSE   |         2 | no: 181, yes: 135                    |
| time\_employed            |        107 |           0.74 | TRUE    |         3 | mor: 196, les: 68, mor: 46           |
| company\_type\_SME        |        377 |           0.10 | FALSE   |         1 | SME: 40                              |
| company\_type\_local      |        394 |           0.06 | FALSE   |         1 | loc: 23                              |
| company\_type\_multi      |        353 |           0.15 | FALSE   |         1 | mul: 64                              |
| company\_type\_public     |        406 |           0.03 | FALSE   |         1 | pub: 11                              |
| company\_type\_other      |        398 |           0.05 | FALSE   |         1 | oth: 19                              |
| wpi\_wage                 |          2 |           1.00 | TRUE    |         4 | 5: 208, 4: 167, 3: 36, 2: 4          |
| wpi\_career               |          3 |           0.99 | TRUE    |         5 | 5: 242, 4: 135, 3: 26, 2: 8          |
| wpi\_improvement          |          3 |           0.99 | TRUE    |         5 | 5: 301, 4: 91, 3: 19, 2: 2           |
| wpi\_challenge            |          2 |           1.00 | TRUE    |         5 | 5: 224, 4: 150, 3: 34, 2: 6          |
| wpi\_enviroment           |          2 |           1.00 | TRUE    |         5 | 4: 177, 5: 149, 3: 75, 2: 12         |
| wpi\_prestige             |          2 |           1.00 | TRUE    |         5 | 4: 143, 3: 134, 2: 61, 5: 56         |
| wpi\_corporate            |          2 |           1.00 | TRUE    |         5 | 3: 156, 4: 97, 2: 93, 1: 38          |
| wpi\_housing              |          2 |           1.00 | TRUE    |         5 | 3: 141, 2: 103, 1: 83, 4: 64         |
| wpi\_abroad               |          2 |           1.00 | TRUE    |         5 | 4: 114, 3: 111, 5: 90, 2: 63         |
| wpi\_team                 |          2 |           1.00 | TRUE    |         5 | 4: 136, 3: 116, 5: 113, 2: 35        |
| wpi\_friends              |          2 |           1.00 | TRUE    |         5 | 5: 152, 4: 117, 3: 96, 2: 39         |
| wpi\_values               |          3 |           0.99 | TRUE    |         5 | 4: 141, 5: 138, 3: 91, 2: 29         |
| wpi\_develop              |          4 |           0.99 | TRUE    |         5 | 4: 144, 5: 107, 3: 102, 2: 40        |
| wpi\_well\_being          |          2 |           1.00 | TRUE    |         5 | 5: 158, 4: 142, 3: 83, 2: 22         |
| wpi\_manageable\_wl       |          2 |           1.00 | TRUE    |         5 | 5: 257, 4: 119, 3: 31, 2: 6          |
| wpi\_realxation           |          3 |           0.99 | TRUE    |         5 | 5: 161, 4: 149, 3: 78, 2: 21         |
| wpi\_ho                   |          2 |           1.00 | TRUE    |         5 | 5: 163, 4: 148, 3: 70, 2: 22         |
| wpi\_free\_timeing        |          3 |           0.99 | TRUE    |         5 | 4: 151, 5: 135, 3: 103, 2: 18        |
| wpi\_platform             |          4 |           0.99 | TRUE    |         5 | 4: 133, 5: 127, 3: 117, 2: 27        |
| quit\_respect             |          1 |           1.00 | TRUE    |         5 | 4: 161, 5: 133, 3: 92, 2: 24         |
| quit\_wageincrease        |          1 |           1.00 | TRUE    |         5 | 4: 159, 3: 135, 5: 64, 2: 48         |
| quit\_promotion           |          1 |           1.00 | TRUE    |         5 | 5: 165, 4: 143, 3: 76, 2: 28         |
| quit\_workload            |          1 |           1.00 | TRUE    |         5 | 4: 142, 5: 127, 3: 104, 2: 32        |
| quit\_overtime            |          1 |           1.00 | TRUE    |         5 | 5: 267, 4: 90, 3: 40, 2: 14          |
| quit\_enviroment          |          2 |           1.00 | TRUE    |         5 | 4: 163, 5: 138, 3: 91, 2: 15         |
| quit\_monotnous           |          1 |           1.00 | TRUE    |         5 | 3: 121, 5: 118, 4: 112, 2: 51        |
| quit\_salaryoffer         |          1 |           1.00 | TRUE    |         5 | 4: 132, 5: 126, 3: 111, 2: 30        |
| value\_added              |          1 |           1.00 | FALSE   |         2 | Val: 385, Fil: 31                    |
| up\_or\_out               |          1 |           1.00 | FALSE   |         2 | Mul: 331, Up : 85                    |
| prioritize\_wl\_balance   |         13 |           0.97 | TRUE    |         4 | 1: 236, 2: 86, 3: 52, 4: 30          |
| prioritize\_salary        |         13 |           0.97 | TRUE    |         4 | 2: 181, 3: 95, 1: 72, 4: 56          |
| prioritize\_pro\_progress |         13 |           0.97 | TRUE    |         4 | 3: 158, 4: 101, 1: 77, 2: 68         |
| prioritize\_realtions     |         13 |           0.97 | TRUE    |         4 | 4: 217, 3: 99, 2: 69, 1: 19          |
| ppl\_room                 |          1 |           1.00 | FALSE   |         5 | 2-3: 179, 4-5: 155, 6-8: 42, 1 p: 23 |
| meeting                   |          1 |           1.00 | TRUE    |         6 | Onc: 197, Mor: 133, Twi: 39, Dai: 25 |
| online\_contact           |          2 |           1.00 | FALSE   |         2 | yes: 238, no: 177                    |
| atypical\_platform        |        305 |           0.27 | FALSE   |         1 | pla: 112                             |
| atypical\_telework        |        227 |           0.46 | FALSE   |         1 | tel: 190                             |
| atypical\_part\_time      |        278 |           0.33 | FALSE   |         1 | par: 139                             |
| atypical\_startup         |        194 |           0.53 | FALSE   |         1 | sta: 223                             |
| atypical\_non\_fixed      |        186 |           0.55 | FALSE   |         1 | non: 231                             |
| fivey\_salary             |          1 |           1.00 | FALSE   |         4 | 1.5: 219, 2-2: 137, 1-1: 38, Thi: 22 |
| fivey\_position           |          3 |           0.99 | TRUE    |         4 | mid: 246, ind: 95, sub: 43, top: 30  |
| fiveyy\_development       |          1 |           1.00 | TRUE    |         4 | 5: 230, 4: 143, 3: 36, 2: 7          |
| work\_abroad              |          1 |           1.00 | FALSE   |         3 | I d: 180, yes: 132, no: 104          |
| work\_abroad\_c           |        289 |           0.31 | FALSE   |        14 | Uni: 21, Uni: 14, Aus: 13, oth: 12   |
| work\_abroad\_p           |        286 |           0.31 | FALSE   |         2 | yes: 120, no: 11                     |
| country\_stay             |          4 |           0.99 | FALSE   |         4 | In : 188, In : 156, The: 37, In : 32 |
| stay\_enviroment          |        188 |           0.55 | FALSE   |         1 | int: 229                             |
| stay\_material            |        177 |           0.58 | FALSE   |         1 | hig: 240                             |
| stay\_housing             |        199 |           0.52 | FALSE   |         1 | hou: 218                             |
| stay\_family              |        160 |           0.62 | FALSE   |         1 | fam: 257                             |
| stay\_friends             |        153 |           0.63 | FALSE   |         1 | fri: 264                             |
| stay\_development         |        154 |           0.63 | FALSE   |         1 | pro: 263                             |
| scholarship               |          2 |           1.00 | FALSE   |         3 | ful: 318, sel: 91, par: 6            |
| mobility\_restrictions    |         93 |           0.78 | FALSE   |         2 | no: 259, yes: 65                     |
| sabbatical\_know          |          2 |           1.00 | FALSE   |         2 | no: 344, yes: 71                     |
| ho                        |          1 |           1.00 | TRUE    |         4 | yes: 165, yes: 115, no: 95, yes: 41  |
| work\_kind                |          2 |           1.00 | TRUE    |         3 | fle: 315, com: 59, fix: 41           |
| change\_job               |          2 |           1.00 | FALSE   |         4 | eve: 273, eve: 66, eve: 45, nev: 31  |
| flexibility               |          2 |           1.00 | TRUE    |         4 | I w: 177, Sur: 120, Not: 64, Fur: 54 |
| baby                      |          2 |           1.00 | FALSE   |         2 | yes: 369, no: 46                     |
| goal\_confidence          |          3 |           0.99 | TRUE    |        11 | 8: 89, 10: 86, 7: 79, 6: 37          |
| ws\_high\_payment         |          5 |           0.99 | TRUE    |        11 | 0: 58, 2: 56, 1: 54, 3: 51           |
| ws\_stable                |          5 |           0.99 | TRUE    |        11 | 7: 84, 8: 61, 6: 55, 5: 50           |
| ws\_flexible              |          6 |           0.99 | TRUE    |        11 | 8: 80, 7: 73, 6: 52, 5: 46           |
| ws\_inspiring             |          5 |           0.99 | TRUE    |        11 | 7: 76, 6: 63, 8: 63, 5: 48           |
| ws\_management            |          6 |           0.99 | TRUE    |        11 | 10: 169, 9: 90, 8: 53, 7: 23         |
| ws\_large\_company        |          6 |           0.99 | TRUE    |        11 | 0: 87, 3: 60, 2: 57, 1: 52           |
| wb\_life                  |          4 |           0.99 | TRUE    |        11 | 8: 124, 7: 75, 9: 60, 6: 45          |
| wb\_finance               |          4 |           0.99 | TRUE    |        11 | 8: 75, 6: 68, 7: 63, 9: 42           |
| wb\_study                 |          4 |           0.99 | TRUE    |        11 | 8: 83, 7: 71, 9: 69, 10: 59          |
| wb\_conditions            |          3 |           0.99 | TRUE    |        11 | 9: 77, 8: 74, 10: 66, 7: 51          |
| wb\_time\_spent           |          3 |           0.99 | TRUE    |        11 | 8: 63, 7: 60, 6: 49, 9: 46           |
| wb\_meaningful            |          3 |           0.99 | TRUE    |        11 | 8: 84, 7: 80, 9: 64, 6: 45           |
| wb\_hopefull              |          3 |           0.99 | TRUE    |        11 | 10: 107, 8: 92, 7: 62, 9: 48         |
| feel\_happy               |          4 |           0.99 | TRUE    |         5 | töb: 216, idő: 92, min: 67, rit: 36  |
| feel\_stress              |          4 |           0.99 | TRUE    |         5 | töb: 216, idő: 92, min: 67, rit: 36  |
| feel\_lonely              |          4 |           0.99 | TRUE    |         5 | töb: 216, idő: 92, min: 67, rit: 36  |
| trust\_ppl                |          3 |           0.99 | TRUE    |        11 | 7: 101, 6: 80, 5: 60, 8: 46          |
| trust\_politicals         |          3 |           0.99 | TRUE    |        11 | 0: 101, 2: 65, 3: 49, 1: 48          |
| trust\_legals             |          3 |           0.99 | TRUE    |        11 | 5: 60, 6: 59, 4: 49, 3: 48           |
| trust\_police             |          3 |           0.99 | TRUE    |        11 | 5: 62, 6: 51, 7: 51, 3: 46           |
| trust\_military           |          3 |           0.99 | TRUE    |        11 | 5: 62, 7: 54, 8: 53, 4: 48           |

**Variable type: numeric**

| skim\_variable | n\_missing | complete\_rate |     mean |        sd |    p0 |    p25 |    p50 |    p75 |  p100 | hist  |
| :------------- | ---------: | -------------: | -------: | --------: | ----: | -----: | -----: | -----: | ----: | :---- |
| comf\_salary   |         45 |           0.89 | 326365.6 | 107238.56 | 80000 | 250000 | 300000 | 400000 | 6e+05 | ▁▇▇▃▃ |
| real\_salary   |         19 |           0.95 | 231213.6 |  63918.26 | 80000 | 180000 | 230000 | 270000 | 4e+05 | ▂▇▇▃▂ |

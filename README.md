# InfoVizion tools

Set of developement tools used in InfoVizion for Qlik projects

## Qlik Load Script support

`F5`: run default command, can be check and reload, just reload, or just check script

`Control Shift L`: opens windows on corresponding log file for last execution.

`//#!QV_SUPPRESS_ERROR` comment suppress parsing errors for one QVS statement
`//#!QV_SKIP_PARSING` comment suppress parsing errors for all remaining lines in file

## QVD Support:

Just click on QVD in left panel to see QVD metadata in preview window

## Expression file support

Extension works for files with extension `qlikview-vars`.

For each save corresponding json file should be created. Only most severe errors would be checked on that stage:
Some examples:

- `--` instead of `---` tag as delimiter between expressions
- Circular references in formulas (in dollar sign expansion)

`F5` run more detailed check, including check for undefinded variables, circular references, Qlik Expression syntax check and so on.

To introduce undefined variables one can use pseudo excression `LoadScriptVariables`
To suppress parse errors for one expression one can add tag `pragma` with value `skipError`

See example:

```
---
set: LoadScriptVariables
definition: vU.CurrentDate=15234,vU.StartDate='01.03.2012', vG.MaxValue=20
description: Перечисление переменных, устанавливаемых в скриптах загрузки. Со значениями которые будут подствавляться во время проверки правильности 
   формул, которые эти переменные используют. Формат: ИмяПеременной1=14234,ИмяПеременной2='ПримерСтроковогоЗначения'
---
set: СуммаПродажи
definition: (-Sum({<ТипДокумента={'Sales'},_Current={1},_ФлагДействующаяДата={1}>} Сумма)) ( ABRAKADABRA )
pragma: skipError
label:Продажи, руб
---
set: СуммаПродажиДоляНакопительно
definition: RangeSum(-Sum({<ТипДокумента={'Sales'},_Current={1},_ФлагДействующаяДата={1}>} Сумма)
                    /-Sum({<ТипДокумента={'Sales'},_Current={1},_ФлагДействующаяДата={1}>} TOTAL Сумма),0,RowNo())
label:% накопительно по продажам

```

Standard `Ctrl + /` combination comment / uncomment block with `# ` style comments
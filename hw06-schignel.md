---
title: "Homework 06: Data wrangling wrap up"
author: "Stephen Chignell"
date: "November 8, 2018"
output:
  html_document:
    toc: true
    keep_md: true
    theme: sandstone
---

## Goals

Complete two of six possible tasks on data wrangling.


## Task 1: Character data

Load `stringr` package for working with character strings

```r
library(stringr)
library(stringi)
```


### Exercise 14.2.5

**[1]** In code that doesn’t use stringr, you’ll often see paste() and paste0(). What’s the difference between the two functions? What stringr function are they equivalent to? How do the functions differ in their handling of NA?

*Answer:*

- `paste()` converts vectors to characters and concatenates them based on a rule (in this case, a specific separator). 
- `paste0()` accomplishes the same task, but assumes that sep = "", so it can be quicker option if the data is in the correct format. 

We see this in the following examples:

```r
# use sep for individual character strings
paste("a", "b", sep = "")
```

```
## [1] "ab"
```

```r
# use collapse for one string
x <- c("a", "b")
paste(x, collapse = "")
```

```
## [1] "ab"
```

```r
# use paste0() to assume "" as separator
paste0("a", "b")
```

```
## [1] "ab"
```

```r
# here we see that this is equivalent to `str_c` from stringr
str_c("a", "b")
```

```
## [1] "ab"
```

- With `str_c`, when a missing value is combined with another string, the result will be missing (NA). You can then use str_replace_na() to convert NA to "NA"

- `paste()`, on the otherhand, coerces the missing value (NA) to "NA" automatically. 



**[2]** In your own words, describe the difference between the sep and collapse arguments to str_c().

*Answer:*

- "sep"" is used to control how individual character strings are to be separated
- "collapse" is used to change a vector of strings into a single string.


**[3]** Use str_length() and str_sub() to extract the middle character from a string. What will you do if the string has an even number of characters?

*Answer:*

```r
odd <- "Steve"
str_length(odd)
```

```
## [1] 5
```

```r
# Middle character of 5 is 3
str_sub(odd, 3,3)
```

```
## [1] "e"
```

```r
even <- "Steve!"
str_length(even)
```

```
## [1] 6
```

```r
# Middle two characters of 6 is 3 and 4
str_sub(odd, 3,4)
```

```
## [1] "ev"
```


**[4]** What does str_wrap() do? When might you want to use it?

*Answer:*

`str_wrap()` is a wrapper around stringi::stri_wrap() which implements the Knuth-Plass paragraph wrapping algorithm. It reformats a character vector of strings based on a specificie dwidth and indentation and/or exdentation. 

This would be useful for customizing the display of large blocks of texts, to ensure they are printed as nice, legible paragraphs. 


**[5]** What does str_trim() do? What’s the opposite of str_trim()?

*Answer:*

- `str_trim()` removes whitespace from start and end of string. The "side" argument specifics the side on which to remove whitespace (left, right, or both).

- `str_pad()` does the opposite, adding whitespace based on specified width and side.



**[6]** Write a function that turns (e.g.) a vector c("a", "b", "c") into the string a, b, and c. Think carefully about what it should do if given a vector of length 0, 1, or 2.

*Answer:*

```r
a <- c("a", "b", "c")
a
```

```
## [1] "a" "b" "c"
```

```r
vec2str <- function(x){
  if (length(x) <=1)  {
    x 
  }else{ 
    str_c(x, collapse = ", ")
  }
}
vec2str(c("a", "b", "c"))
```

```
## [1] "a, b, c"
```


### Exercise 14.3.1.1 

**[1]** Explain why each of these strings don’t match a `\`: 

*Answer:*

- `"\"` This has a special behavior assigned to it already (i.e. escape)

- `"\\"` This escapes the escape behavior to create regular expression

- `"\\\"` This is a regular expression that requires a string, which must also be escaped in order to match a literal `\`.


**[2]** How would you match the sequence `"'\?`

*Answer:*

```r
z <- "\"\'\\?"
z
```

```
## [1] "\"'\\?"
```

```r
str_view("?", "\\?")
```

<!--html_preserve--><div id="htmlwidget-c84426b9b35ee1d2a21e" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-c84426b9b35ee1d2a21e">{"x":{"html":"<ul>\n  <li><span class='match'>?<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**[3]** What patterns will the regular expression \..\..\.. match? How would you represent it as a string?

*Answer:*

It will match the pattern: ".wildcard.wildcard.wildcard"

```r
string <- "\\..\\..\\.."
string
```

```
## [1] "\\..\\..\\.."
```

```r
str_view(".h.h.hurry!", "\\..\\..\\..")
```

<!--html_preserve--><div id="htmlwidget-64785d26a8027fe4c927" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-64785d26a8027fe4c927">{"x":{"html":"<ul>\n  <li><span class='match'>.h.h.h<\/span>urry!<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->



### Exercise 14.3.2.1

**[1]** How would you match the literal string "$^$"?

*Answer:*

```r
#example
str_view("F$^$@!", pattern = "\\$\\^\\$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-471f1c6dfe4dfe72daec" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-471f1c6dfe4dfe72daec">{"x":{"html":"<ul>\n  <li>F<span class='match'>$^$<\/span>@!<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


**[2]** Given the corpus of common words in stringr::words, create regular expressions that find all words that:

*Answer:*

Start with “y”.

```r
str_view(words, pattern = "^y", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-c6c72b052db08ac8ceac" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-c6c72b052db08ac8ceac">{"x":{"html":"<ul>\n  <li><span class='match'>y<\/span>ear<\/li>\n  <li><span class='match'>y<\/span>es<\/li>\n  <li><span class='match'>y<\/span>esterday<\/li>\n  <li><span class='match'>y<\/span>et<\/li>\n  <li><span class='match'>y<\/span>ou<\/li>\n  <li><span class='match'>y<\/span>oung<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

End with “x”

```r
str_view(words, pattern = "$x", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-5291252fdb942452cdc8" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-5291252fdb942452cdc8">{"x":{"html":"<ul>\n  <li><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

Are exactly three letters long. (Don’t cheat by using `str_length()`!)

```r
str_view(words, pattern = "^...$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-894a397f2c3ce3b3f56d" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-894a397f2c3ce3b3f56d">{"x":{"html":"<ul>\n  <li><span class='match'>act<\/span><\/li>\n  <li><span class='match'>add<\/span><\/li>\n  <li><span class='match'>age<\/span><\/li>\n  <li><span class='match'>ago<\/span><\/li>\n  <li><span class='match'>air<\/span><\/li>\n  <li><span class='match'>all<\/span><\/li>\n  <li><span class='match'>and<\/span><\/li>\n  <li><span class='match'>any<\/span><\/li>\n  <li><span class='match'>arm<\/span><\/li>\n  <li><span class='match'>art<\/span><\/li>\n  <li><span class='match'>ask<\/span><\/li>\n  <li><span class='match'>bad<\/span><\/li>\n  <li><span class='match'>bag<\/span><\/li>\n  <li><span class='match'>bar<\/span><\/li>\n  <li><span class='match'>bed<\/span><\/li>\n  <li><span class='match'>bet<\/span><\/li>\n  <li><span class='match'>big<\/span><\/li>\n  <li><span class='match'>bit<\/span><\/li>\n  <li><span class='match'>box<\/span><\/li>\n  <li><span class='match'>boy<\/span><\/li>\n  <li><span class='match'>bus<\/span><\/li>\n  <li><span class='match'>but<\/span><\/li>\n  <li><span class='match'>buy<\/span><\/li>\n  <li><span class='match'>can<\/span><\/li>\n  <li><span class='match'>car<\/span><\/li>\n  <li><span class='match'>cat<\/span><\/li>\n  <li><span class='match'>cup<\/span><\/li>\n  <li><span class='match'>cut<\/span><\/li>\n  <li><span class='match'>dad<\/span><\/li>\n  <li><span class='match'>day<\/span><\/li>\n  <li><span class='match'>die<\/span><\/li>\n  <li><span class='match'>dog<\/span><\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>due<\/span><\/li>\n  <li><span class='match'>eat<\/span><\/li>\n  <li><span class='match'>egg<\/span><\/li>\n  <li><span class='match'>end<\/span><\/li>\n  <li><span class='match'>eye<\/span><\/li>\n  <li><span class='match'>far<\/span><\/li>\n  <li><span class='match'>few<\/span><\/li>\n  <li><span class='match'>fit<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>for<\/span><\/li>\n  <li><span class='match'>fun<\/span><\/li>\n  <li><span class='match'>gas<\/span><\/li>\n  <li><span class='match'>get<\/span><\/li>\n  <li><span class='match'>god<\/span><\/li>\n  <li><span class='match'>guy<\/span><\/li>\n  <li><span class='match'>hit<\/span><\/li>\n  <li><span class='match'>hot<\/span><\/li>\n  <li><span class='match'>how<\/span><\/li>\n  <li><span class='match'>job<\/span><\/li>\n  <li><span class='match'>key<\/span><\/li>\n  <li><span class='match'>kid<\/span><\/li>\n  <li><span class='match'>lad<\/span><\/li>\n  <li><span class='match'>law<\/span><\/li>\n  <li><span class='match'>lay<\/span><\/li>\n  <li><span class='match'>leg<\/span><\/li>\n  <li><span class='match'>let<\/span><\/li>\n  <li><span class='match'>lie<\/span><\/li>\n  <li><span class='match'>lot<\/span><\/li>\n  <li><span class='match'>low<\/span><\/li>\n  <li><span class='match'>man<\/span><\/li>\n  <li><span class='match'>may<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>new<\/span><\/li>\n  <li><span class='match'>non<\/span><\/li>\n  <li><span class='match'>not<\/span><\/li>\n  <li><span class='match'>now<\/span><\/li>\n  <li><span class='match'>odd<\/span><\/li>\n  <li><span class='match'>off<\/span><\/li>\n  <li><span class='match'>old<\/span><\/li>\n  <li><span class='match'>one<\/span><\/li>\n  <li><span class='match'>out<\/span><\/li>\n  <li><span class='match'>own<\/span><\/li>\n  <li><span class='match'>pay<\/span><\/li>\n  <li><span class='match'>per<\/span><\/li>\n  <li><span class='match'>put<\/span><\/li>\n  <li><span class='match'>red<\/span><\/li>\n  <li><span class='match'>rid<\/span><\/li>\n  <li><span class='match'>run<\/span><\/li>\n  <li><span class='match'>say<\/span><\/li>\n  <li><span class='match'>see<\/span><\/li>\n  <li><span class='match'>set<\/span><\/li>\n  <li><span class='match'>sex<\/span><\/li>\n  <li><span class='match'>she<\/span><\/li>\n  <li><span class='match'>sir<\/span><\/li>\n  <li><span class='match'>sit<\/span><\/li>\n  <li><span class='match'>six<\/span><\/li>\n  <li><span class='match'>son<\/span><\/li>\n  <li><span class='match'>sun<\/span><\/li>\n  <li><span class='match'>tax<\/span><\/li>\n  <li><span class='match'>tea<\/span><\/li>\n  <li><span class='match'>ten<\/span><\/li>\n  <li><span class='match'>the<\/span><\/li>\n  <li><span class='match'>tie<\/span><\/li>\n  <li><span class='match'>too<\/span><\/li>\n  <li><span class='match'>top<\/span><\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>two<\/span><\/li>\n  <li><span class='match'>use<\/span><\/li>\n  <li><span class='match'>war<\/span><\/li>\n  <li><span class='match'>way<\/span><\/li>\n  <li><span class='match'>wee<\/span><\/li>\n  <li><span class='match'>who<\/span><\/li>\n  <li><span class='match'>why<\/span><\/li>\n  <li><span class='match'>win<\/span><\/li>\n  <li><span class='match'>yes<\/span><\/li>\n  <li><span class='match'>yet<\/span><\/li>\n  <li><span class='match'>you<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

Have seven letters or more.

```r
str_view(words, pattern = "^.......", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-689af88d75df9d4c5979" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-689af88d75df9d4c5979">{"x":{"html":"<ul>\n  <li><span class='match'>absolut<\/span>e<\/li>\n  <li><span class='match'>account<\/span><\/li>\n  <li><span class='match'>achieve<\/span><\/li>\n  <li><span class='match'>address<\/span><\/li>\n  <li><span class='match'>adverti<\/span>se<\/li>\n  <li><span class='match'>afterno<\/span>on<\/li>\n  <li><span class='match'>against<\/span><\/li>\n  <li><span class='match'>already<\/span><\/li>\n  <li><span class='match'>alright<\/span><\/li>\n  <li><span class='match'>althoug<\/span>h<\/li>\n  <li><span class='match'>america<\/span><\/li>\n  <li><span class='match'>another<\/span><\/li>\n  <li><span class='match'>apparen<\/span>t<\/li>\n  <li><span class='match'>appoint<\/span><\/li>\n  <li><span class='match'>approac<\/span>h<\/li>\n  <li><span class='match'>appropr<\/span>iate<\/li>\n  <li><span class='match'>arrange<\/span><\/li>\n  <li><span class='match'>associa<\/span>te<\/li>\n  <li><span class='match'>authori<\/span>ty<\/li>\n  <li><span class='match'>availab<\/span>le<\/li>\n  <li><span class='match'>balance<\/span><\/li>\n  <li><span class='match'>because<\/span><\/li>\n  <li><span class='match'>believe<\/span><\/li>\n  <li><span class='match'>benefit<\/span><\/li>\n  <li><span class='match'>between<\/span><\/li>\n  <li><span class='match'>brillia<\/span>nt<\/li>\n  <li><span class='match'>britain<\/span><\/li>\n  <li><span class='match'>brother<\/span><\/li>\n  <li><span class='match'>busines<\/span>s<\/li>\n  <li><span class='match'>certain<\/span><\/li>\n  <li><span class='match'>chairma<\/span>n<\/li>\n  <li><span class='match'>charact<\/span>er<\/li>\n  <li><span class='match'>Christm<\/span>as<\/li>\n  <li><span class='match'>colleag<\/span>ue<\/li>\n  <li><span class='match'>collect<\/span><\/li>\n  <li><span class='match'>college<\/span><\/li>\n  <li><span class='match'>comment<\/span><\/li>\n  <li><span class='match'>committ<\/span>ee<\/li>\n  <li><span class='match'>communi<\/span>ty<\/li>\n  <li><span class='match'>company<\/span><\/li>\n  <li><span class='match'>compare<\/span><\/li>\n  <li><span class='match'>complet<\/span>e<\/li>\n  <li><span class='match'>compute<\/span><\/li>\n  <li><span class='match'>concern<\/span><\/li>\n  <li><span class='match'>conditi<\/span>on<\/li>\n  <li><span class='match'>conside<\/span>r<\/li>\n  <li><span class='match'>consult<\/span><\/li>\n  <li><span class='match'>contact<\/span><\/li>\n  <li><span class='match'>continu<\/span>e<\/li>\n  <li><span class='match'>contrac<\/span>t<\/li>\n  <li><span class='match'>control<\/span><\/li>\n  <li><span class='match'>convers<\/span>e<\/li>\n  <li><span class='match'>correct<\/span><\/li>\n  <li><span class='match'>council<\/span><\/li>\n  <li><span class='match'>country<\/span><\/li>\n  <li><span class='match'>current<\/span><\/li>\n  <li><span class='match'>decisio<\/span>n<\/li>\n  <li><span class='match'>definit<\/span>e<\/li>\n  <li><span class='match'>departm<\/span>ent<\/li>\n  <li><span class='match'>describ<\/span>e<\/li>\n  <li><span class='match'>develop<\/span><\/li>\n  <li><span class='match'>differe<\/span>nce<\/li>\n  <li><span class='match'>difficu<\/span>lt<\/li>\n  <li><span class='match'>discuss<\/span><\/li>\n  <li><span class='match'>distric<\/span>t<\/li>\n  <li><span class='match'>documen<\/span>t<\/li>\n  <li><span class='match'>economy<\/span><\/li>\n  <li><span class='match'>educate<\/span><\/li>\n  <li><span class='match'>electri<\/span>c<\/li>\n  <li><span class='match'>encoura<\/span>ge<\/li>\n  <li><span class='match'>english<\/span><\/li>\n  <li><span class='match'>environ<\/span>ment<\/li>\n  <li><span class='match'>especia<\/span>l<\/li>\n  <li><span class='match'>evening<\/span><\/li>\n  <li><span class='match'>evidenc<\/span>e<\/li>\n  <li><span class='match'>example<\/span><\/li>\n  <li><span class='match'>exercis<\/span>e<\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experie<\/span>nce<\/li>\n  <li><span class='match'>explain<\/span><\/li>\n  <li><span class='match'>express<\/span><\/li>\n  <li><span class='match'>finance<\/span><\/li>\n  <li><span class='match'>fortune<\/span><\/li>\n  <li><span class='match'>forward<\/span><\/li>\n  <li><span class='match'>functio<\/span>n<\/li>\n  <li><span class='match'>further<\/span><\/li>\n  <li><span class='match'>general<\/span><\/li>\n  <li><span class='match'>germany<\/span><\/li>\n  <li><span class='match'>goodbye<\/span><\/li>\n  <li><span class='match'>history<\/span><\/li>\n  <li><span class='match'>holiday<\/span><\/li>\n  <li><span class='match'>hospita<\/span>l<\/li>\n  <li><span class='match'>however<\/span><\/li>\n  <li><span class='match'>hundred<\/span><\/li>\n  <li><span class='match'>husband<\/span><\/li>\n  <li><span class='match'>identif<\/span>y<\/li>\n  <li><span class='match'>imagine<\/span><\/li>\n  <li><span class='match'>importa<\/span>nt<\/li>\n  <li><span class='match'>improve<\/span><\/li>\n  <li><span class='match'>include<\/span><\/li>\n  <li><span class='match'>increas<\/span>e<\/li>\n  <li><span class='match'>individ<\/span>ual<\/li>\n  <li><span class='match'>industr<\/span>y<\/li>\n  <li><span class='match'>instead<\/span><\/li>\n  <li><span class='match'>interes<\/span>t<\/li>\n  <li><span class='match'>introdu<\/span>ce<\/li>\n  <li><span class='match'>involve<\/span><\/li>\n  <li><span class='match'>kitchen<\/span><\/li>\n  <li><span class='match'>languag<\/span>e<\/li>\n  <li><span class='match'>machine<\/span><\/li>\n  <li><span class='match'>meaning<\/span><\/li>\n  <li><span class='match'>measure<\/span><\/li>\n  <li><span class='match'>mention<\/span><\/li>\n  <li><span class='match'>million<\/span><\/li>\n  <li><span class='match'>ministe<\/span>r<\/li>\n  <li><span class='match'>morning<\/span><\/li>\n  <li><span class='match'>necessa<\/span>ry<\/li>\n  <li><span class='match'>obvious<\/span><\/li>\n  <li><span class='match'>occasio<\/span>n<\/li>\n  <li><span class='match'>operate<\/span><\/li>\n  <li><span class='match'>opportu<\/span>nity<\/li>\n  <li><span class='match'>organiz<\/span>e<\/li>\n  <li><span class='match'>origina<\/span>l<\/li>\n  <li><span class='match'>otherwi<\/span>se<\/li>\n  <li><span class='match'>paragra<\/span>ph<\/li>\n  <li><span class='match'>particu<\/span>lar<\/li>\n  <li><span class='match'>pension<\/span><\/li>\n  <li><span class='match'>percent<\/span><\/li>\n  <li><span class='match'>perfect<\/span><\/li>\n  <li><span class='match'>perhaps<\/span><\/li>\n  <li><span class='match'>photogr<\/span>aph<\/li>\n  <li><span class='match'>picture<\/span><\/li>\n  <li><span class='match'>politic<\/span><\/li>\n  <li><span class='match'>positio<\/span>n<\/li>\n  <li><span class='match'>positiv<\/span>e<\/li>\n  <li><span class='match'>possibl<\/span>e<\/li>\n  <li><span class='match'>practis<\/span>e<\/li>\n  <li><span class='match'>prepare<\/span><\/li>\n  <li><span class='match'>present<\/span><\/li>\n  <li><span class='match'>pressur<\/span>e<\/li>\n  <li><span class='match'>presume<\/span><\/li>\n  <li><span class='match'>previou<\/span>s<\/li>\n  <li><span class='match'>private<\/span><\/li>\n  <li><span class='match'>probabl<\/span>e<\/li>\n  <li><span class='match'>problem<\/span><\/li>\n  <li><span class='match'>proceed<\/span><\/li>\n  <li><span class='match'>process<\/span><\/li>\n  <li><span class='match'>produce<\/span><\/li>\n  <li><span class='match'>product<\/span><\/li>\n  <li><span class='match'>program<\/span>me<\/li>\n  <li><span class='match'>project<\/span><\/li>\n  <li><span class='match'>propose<\/span><\/li>\n  <li><span class='match'>protect<\/span><\/li>\n  <li><span class='match'>provide<\/span><\/li>\n  <li><span class='match'>purpose<\/span><\/li>\n  <li><span class='match'>quality<\/span><\/li>\n  <li><span class='match'>quarter<\/span><\/li>\n  <li><span class='match'>questio<\/span>n<\/li>\n  <li><span class='match'>realise<\/span><\/li>\n  <li><span class='match'>receive<\/span><\/li>\n  <li><span class='match'>recogni<\/span>ze<\/li>\n  <li><span class='match'>recomme<\/span>nd<\/li>\n  <li><span class='match'>relatio<\/span>n<\/li>\n  <li><span class='match'>remembe<\/span>r<\/li>\n  <li><span class='match'>represe<\/span>nt<\/li>\n  <li><span class='match'>require<\/span><\/li>\n  <li><span class='match'>researc<\/span>h<\/li>\n  <li><span class='match'>resourc<\/span>e<\/li>\n  <li><span class='match'>respect<\/span><\/li>\n  <li><span class='match'>respons<\/span>ible<\/li>\n  <li><span class='match'>saturda<\/span>y<\/li>\n  <li><span class='match'>science<\/span><\/li>\n  <li><span class='match'>scotlan<\/span>d<\/li>\n  <li><span class='match'>secreta<\/span>ry<\/li>\n  <li><span class='match'>section<\/span><\/li>\n  <li><span class='match'>separat<\/span>e<\/li>\n  <li><span class='match'>serious<\/span><\/li>\n  <li><span class='match'>service<\/span><\/li>\n  <li><span class='match'>similar<\/span><\/li>\n  <li><span class='match'>situate<\/span><\/li>\n  <li><span class='match'>society<\/span><\/li>\n  <li><span class='match'>special<\/span><\/li>\n  <li><span class='match'>specifi<\/span>c<\/li>\n  <li><span class='match'>standar<\/span>d<\/li>\n  <li><span class='match'>station<\/span><\/li>\n  <li><span class='match'>straigh<\/span>t<\/li>\n  <li><span class='match'>strateg<\/span>y<\/li>\n  <li><span class='match'>structu<\/span>re<\/li>\n  <li><span class='match'>student<\/span><\/li>\n  <li><span class='match'>subject<\/span><\/li>\n  <li><span class='match'>succeed<\/span><\/li>\n  <li><span class='match'>suggest<\/span><\/li>\n  <li><span class='match'>support<\/span><\/li>\n  <li><span class='match'>suppose<\/span><\/li>\n  <li><span class='match'>surpris<\/span>e<\/li>\n  <li><span class='match'>telepho<\/span>ne<\/li>\n  <li><span class='match'>televis<\/span>ion<\/li>\n  <li><span class='match'>terribl<\/span>e<\/li>\n  <li><span class='match'>therefo<\/span>re<\/li>\n  <li><span class='match'>thirtee<\/span>n<\/li>\n  <li><span class='match'>thousan<\/span>d<\/li>\n  <li><span class='match'>through<\/span><\/li>\n  <li><span class='match'>thursda<\/span>y<\/li>\n  <li><span class='match'>togethe<\/span>r<\/li>\n  <li><span class='match'>tomorro<\/span>w<\/li>\n  <li><span class='match'>tonight<\/span><\/li>\n  <li><span class='match'>traffic<\/span><\/li>\n  <li><span class='match'>transpo<\/span>rt<\/li>\n  <li><span class='match'>trouble<\/span><\/li>\n  <li><span class='match'>tuesday<\/span><\/li>\n  <li><span class='match'>underst<\/span>and<\/li>\n  <li><span class='match'>univers<\/span>ity<\/li>\n  <li><span class='match'>various<\/span><\/li>\n  <li><span class='match'>village<\/span><\/li>\n  <li><span class='match'>wednesd<\/span>ay<\/li>\n  <li><span class='match'>welcome<\/span><\/li>\n  <li><span class='match'>whether<\/span><\/li>\n  <li><span class='match'>without<\/span><\/li>\n  <li><span class='match'>yesterd<\/span>ay<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


### Exercise 14.3.3.1

**[1]** Create regular expressions to find all words that:

*Answer:*

```r
# Start with a vowel.
str_view(words, pattern = "^[aeiou]", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-20c297512a14686d1bbc" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-20c297512a14686d1bbc">{"x":{"html":"<ul>\n  <li><span class='match'>a<\/span><\/li>\n  <li><span class='match'>a<\/span>ble<\/li>\n  <li><span class='match'>a<\/span>bout<\/li>\n  <li><span class='match'>a<\/span>bsolute<\/li>\n  <li><span class='match'>a<\/span>ccept<\/li>\n  <li><span class='match'>a<\/span>ccount<\/li>\n  <li><span class='match'>a<\/span>chieve<\/li>\n  <li><span class='match'>a<\/span>cross<\/li>\n  <li><span class='match'>a<\/span>ct<\/li>\n  <li><span class='match'>a<\/span>ctive<\/li>\n  <li><span class='match'>a<\/span>ctual<\/li>\n  <li><span class='match'>a<\/span>dd<\/li>\n  <li><span class='match'>a<\/span>ddress<\/li>\n  <li><span class='match'>a<\/span>dmit<\/li>\n  <li><span class='match'>a<\/span>dvertise<\/li>\n  <li><span class='match'>a<\/span>ffect<\/li>\n  <li><span class='match'>a<\/span>fford<\/li>\n  <li><span class='match'>a<\/span>fter<\/li>\n  <li><span class='match'>a<\/span>fternoon<\/li>\n  <li><span class='match'>a<\/span>gain<\/li>\n  <li><span class='match'>a<\/span>gainst<\/li>\n  <li><span class='match'>a<\/span>ge<\/li>\n  <li><span class='match'>a<\/span>gent<\/li>\n  <li><span class='match'>a<\/span>go<\/li>\n  <li><span class='match'>a<\/span>gree<\/li>\n  <li><span class='match'>a<\/span>ir<\/li>\n  <li><span class='match'>a<\/span>ll<\/li>\n  <li><span class='match'>a<\/span>llow<\/li>\n  <li><span class='match'>a<\/span>lmost<\/li>\n  <li><span class='match'>a<\/span>long<\/li>\n  <li><span class='match'>a<\/span>lready<\/li>\n  <li><span class='match'>a<\/span>lright<\/li>\n  <li><span class='match'>a<\/span>lso<\/li>\n  <li><span class='match'>a<\/span>lthough<\/li>\n  <li><span class='match'>a<\/span>lways<\/li>\n  <li><span class='match'>a<\/span>merica<\/li>\n  <li><span class='match'>a<\/span>mount<\/li>\n  <li><span class='match'>a<\/span>nd<\/li>\n  <li><span class='match'>a<\/span>nother<\/li>\n  <li><span class='match'>a<\/span>nswer<\/li>\n  <li><span class='match'>a<\/span>ny<\/li>\n  <li><span class='match'>a<\/span>part<\/li>\n  <li><span class='match'>a<\/span>pparent<\/li>\n  <li><span class='match'>a<\/span>ppear<\/li>\n  <li><span class='match'>a<\/span>pply<\/li>\n  <li><span class='match'>a<\/span>ppoint<\/li>\n  <li><span class='match'>a<\/span>pproach<\/li>\n  <li><span class='match'>a<\/span>ppropriate<\/li>\n  <li><span class='match'>a<\/span>rea<\/li>\n  <li><span class='match'>a<\/span>rgue<\/li>\n  <li><span class='match'>a<\/span>rm<\/li>\n  <li><span class='match'>a<\/span>round<\/li>\n  <li><span class='match'>a<\/span>rrange<\/li>\n  <li><span class='match'>a<\/span>rt<\/li>\n  <li><span class='match'>a<\/span>s<\/li>\n  <li><span class='match'>a<\/span>sk<\/li>\n  <li><span class='match'>a<\/span>ssociate<\/li>\n  <li><span class='match'>a<\/span>ssume<\/li>\n  <li><span class='match'>a<\/span>t<\/li>\n  <li><span class='match'>a<\/span>ttend<\/li>\n  <li><span class='match'>a<\/span>uthority<\/li>\n  <li><span class='match'>a<\/span>vailable<\/li>\n  <li><span class='match'>a<\/span>ware<\/li>\n  <li><span class='match'>a<\/span>way<\/li>\n  <li><span class='match'>a<\/span>wful<\/li>\n  <li><span class='match'>e<\/span>ach<\/li>\n  <li><span class='match'>e<\/span>arly<\/li>\n  <li><span class='match'>e<\/span>ast<\/li>\n  <li><span class='match'>e<\/span>asy<\/li>\n  <li><span class='match'>e<\/span>at<\/li>\n  <li><span class='match'>e<\/span>conomy<\/li>\n  <li><span class='match'>e<\/span>ducate<\/li>\n  <li><span class='match'>e<\/span>ffect<\/li>\n  <li><span class='match'>e<\/span>gg<\/li>\n  <li><span class='match'>e<\/span>ight<\/li>\n  <li><span class='match'>e<\/span>ither<\/li>\n  <li><span class='match'>e<\/span>lect<\/li>\n  <li><span class='match'>e<\/span>lectric<\/li>\n  <li><span class='match'>e<\/span>leven<\/li>\n  <li><span class='match'>e<\/span>lse<\/li>\n  <li><span class='match'>e<\/span>mploy<\/li>\n  <li><span class='match'>e<\/span>ncourage<\/li>\n  <li><span class='match'>e<\/span>nd<\/li>\n  <li><span class='match'>e<\/span>ngine<\/li>\n  <li><span class='match'>e<\/span>nglish<\/li>\n  <li><span class='match'>e<\/span>njoy<\/li>\n  <li><span class='match'>e<\/span>nough<\/li>\n  <li><span class='match'>e<\/span>nter<\/li>\n  <li><span class='match'>e<\/span>nvironment<\/li>\n  <li><span class='match'>e<\/span>qual<\/li>\n  <li><span class='match'>e<\/span>special<\/li>\n  <li><span class='match'>e<\/span>urope<\/li>\n  <li><span class='match'>e<\/span>ven<\/li>\n  <li><span class='match'>e<\/span>vening<\/li>\n  <li><span class='match'>e<\/span>ver<\/li>\n  <li><span class='match'>e<\/span>very<\/li>\n  <li><span class='match'>e<\/span>vidence<\/li>\n  <li><span class='match'>e<\/span>xact<\/li>\n  <li><span class='match'>e<\/span>xample<\/li>\n  <li><span class='match'>e<\/span>xcept<\/li>\n  <li><span class='match'>e<\/span>xcuse<\/li>\n  <li><span class='match'>e<\/span>xercise<\/li>\n  <li><span class='match'>e<\/span>xist<\/li>\n  <li><span class='match'>e<\/span>xpect<\/li>\n  <li><span class='match'>e<\/span>xpense<\/li>\n  <li><span class='match'>e<\/span>xperience<\/li>\n  <li><span class='match'>e<\/span>xplain<\/li>\n  <li><span class='match'>e<\/span>xpress<\/li>\n  <li><span class='match'>e<\/span>xtra<\/li>\n  <li><span class='match'>e<\/span>ye<\/li>\n  <li><span class='match'>i<\/span>dea<\/li>\n  <li><span class='match'>i<\/span>dentify<\/li>\n  <li><span class='match'>i<\/span>f<\/li>\n  <li><span class='match'>i<\/span>magine<\/li>\n  <li><span class='match'>i<\/span>mportant<\/li>\n  <li><span class='match'>i<\/span>mprove<\/li>\n  <li><span class='match'>i<\/span>n<\/li>\n  <li><span class='match'>i<\/span>nclude<\/li>\n  <li><span class='match'>i<\/span>ncome<\/li>\n  <li><span class='match'>i<\/span>ncrease<\/li>\n  <li><span class='match'>i<\/span>ndeed<\/li>\n  <li><span class='match'>i<\/span>ndividual<\/li>\n  <li><span class='match'>i<\/span>ndustry<\/li>\n  <li><span class='match'>i<\/span>nform<\/li>\n  <li><span class='match'>i<\/span>nside<\/li>\n  <li><span class='match'>i<\/span>nstead<\/li>\n  <li><span class='match'>i<\/span>nsure<\/li>\n  <li><span class='match'>i<\/span>nterest<\/li>\n  <li><span class='match'>i<\/span>nto<\/li>\n  <li><span class='match'>i<\/span>ntroduce<\/li>\n  <li><span class='match'>i<\/span>nvest<\/li>\n  <li><span class='match'>i<\/span>nvolve<\/li>\n  <li><span class='match'>i<\/span>ssue<\/li>\n  <li><span class='match'>i<\/span>t<\/li>\n  <li><span class='match'>i<\/span>tem<\/li>\n  <li><span class='match'>o<\/span>bvious<\/li>\n  <li><span class='match'>o<\/span>ccasion<\/li>\n  <li><span class='match'>o<\/span>dd<\/li>\n  <li><span class='match'>o<\/span>f<\/li>\n  <li><span class='match'>o<\/span>ff<\/li>\n  <li><span class='match'>o<\/span>ffer<\/li>\n  <li><span class='match'>o<\/span>ffice<\/li>\n  <li><span class='match'>o<\/span>ften<\/li>\n  <li><span class='match'>o<\/span>kay<\/li>\n  <li><span class='match'>o<\/span>ld<\/li>\n  <li><span class='match'>o<\/span>n<\/li>\n  <li><span class='match'>o<\/span>nce<\/li>\n  <li><span class='match'>o<\/span>ne<\/li>\n  <li><span class='match'>o<\/span>nly<\/li>\n  <li><span class='match'>o<\/span>pen<\/li>\n  <li><span class='match'>o<\/span>perate<\/li>\n  <li><span class='match'>o<\/span>pportunity<\/li>\n  <li><span class='match'>o<\/span>ppose<\/li>\n  <li><span class='match'>o<\/span>r<\/li>\n  <li><span class='match'>o<\/span>rder<\/li>\n  <li><span class='match'>o<\/span>rganize<\/li>\n  <li><span class='match'>o<\/span>riginal<\/li>\n  <li><span class='match'>o<\/span>ther<\/li>\n  <li><span class='match'>o<\/span>therwise<\/li>\n  <li><span class='match'>o<\/span>ught<\/li>\n  <li><span class='match'>o<\/span>ut<\/li>\n  <li><span class='match'>o<\/span>ver<\/li>\n  <li><span class='match'>o<\/span>wn<\/li>\n  <li><span class='match'>u<\/span>nder<\/li>\n  <li><span class='match'>u<\/span>nderstand<\/li>\n  <li><span class='match'>u<\/span>nion<\/li>\n  <li><span class='match'>u<\/span>nit<\/li>\n  <li><span class='match'>u<\/span>nite<\/li>\n  <li><span class='match'>u<\/span>niversity<\/li>\n  <li><span class='match'>u<\/span>nless<\/li>\n  <li><span class='match'>u<\/span>ntil<\/li>\n  <li><span class='match'>u<\/span>p<\/li>\n  <li><span class='match'>u<\/span>pon<\/li>\n  <li><span class='match'>u<\/span>se<\/li>\n  <li><span class='match'>u<\/span>sual<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# That only contain consonants. (Hint: thinking about matching “not”-vowels.)
str_view(words, pattern = "^[^aeiou]*$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-e38c0fb0fe091a6535de" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-e38c0fb0fe091a6535de">{"x":{"html":"<ul>\n  <li><span class='match'>by<\/span><\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>why<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# End with ed, but not with eed.
str_view(words, pattern = "[^e]ed$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-38e8eeb275fb519a54d3" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-38e8eeb275fb519a54d3">{"x":{"html":"<ul>\n  <li><span class='match'>bed<\/span><\/li>\n  <li>hund<span class='match'>red<\/span><\/li>\n  <li><span class='match'>red<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# End with ing or ise.
str_view(words, pattern = "(ing|ise)$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-5e6c4c7c55cbb0a2463c" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-5e6c4c7c55cbb0a2463c">{"x":{"html":"<ul>\n  <li>advert<span class='match'>ise<\/span><\/li>\n  <li>br<span class='match'>ing<\/span><\/li>\n  <li>dur<span class='match'>ing<\/span><\/li>\n  <li>even<span class='match'>ing<\/span><\/li>\n  <li>exerc<span class='match'>ise<\/span><\/li>\n  <li>k<span class='match'>ing<\/span><\/li>\n  <li>mean<span class='match'>ing<\/span><\/li>\n  <li>morn<span class='match'>ing<\/span><\/li>\n  <li>otherw<span class='match'>ise<\/span><\/li>\n  <li>pract<span class='match'>ise<\/span><\/li>\n  <li>ra<span class='match'>ise<\/span><\/li>\n  <li>real<span class='match'>ise<\/span><\/li>\n  <li>r<span class='match'>ing<\/span><\/li>\n  <li>r<span class='match'>ise<\/span><\/li>\n  <li>s<span class='match'>ing<\/span><\/li>\n  <li>surpr<span class='match'>ise<\/span><\/li>\n  <li>th<span class='match'>ing<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


**[2]** Empirically verify the rule “i before e except after c”.

*Answer:*

```r
# NOT SURE
str_view(words, pattern = "cie", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-700e052d01254a8a2e1f" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-700e052d01254a8a2e1f">{"x":{"html":"<ul>\n  <li>s<span class='match'>cie<\/span>nce<\/li>\n  <li>so<span class='match'>cie<\/span>ty<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
str_view(words, pattern = "(c).*(i).*(e)*", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-1c6fa594e2bd3bdd11ab" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-1c6fa594e2bd3bdd11ab">{"x":{"html":"<ul>\n  <li>a<span class='match'>chieve<\/span><\/li>\n  <li>a<span class='match'>ctive<\/span><\/li>\n  <li>asso<span class='match'>ciate<\/span><\/li>\n  <li><span class='match'>certain<\/span><\/li>\n  <li><span class='match'>chair<\/span><\/li>\n  <li><span class='match'>chairman<\/span><\/li>\n  <li><span class='match'>child<\/span><\/li>\n  <li><span class='match'>choice<\/span><\/li>\n  <li><span class='match'>city<\/span><\/li>\n  <li><span class='match'>claim<\/span><\/li>\n  <li><span class='match'>client<\/span><\/li>\n  <li><span class='match'>commit<\/span><\/li>\n  <li><span class='match'>committee<\/span><\/li>\n  <li><span class='match'>community<\/span><\/li>\n  <li><span class='match'>condition<\/span><\/li>\n  <li><span class='match'>consider<\/span><\/li>\n  <li><span class='match'>continue<\/span><\/li>\n  <li><span class='match'>council<\/span><\/li>\n  <li>de<span class='match'>cide<\/span><\/li>\n  <li>de<span class='match'>cision<\/span><\/li>\n  <li>des<span class='match'>cribe<\/span><\/li>\n  <li>ele<span class='match'>ctric<\/span><\/li>\n  <li>espe<span class='match'>cial<\/span><\/li>\n  <li>exer<span class='match'>cise<\/span><\/li>\n  <li>fun<span class='match'>ction<\/span><\/li>\n  <li>ma<span class='match'>chine<\/span><\/li>\n  <li>o<span class='match'>ccasion<\/span><\/li>\n  <li>pra<span class='match'>ctise<\/span><\/li>\n  <li>re<span class='match'>ceive<\/span><\/li>\n  <li>re<span class='match'>cognize<\/span><\/li>\n  <li>s<span class='match'>cience<\/span><\/li>\n  <li>se<span class='match'>ction<\/span><\/li>\n  <li>so<span class='match'>cial<\/span><\/li>\n  <li>so<span class='match'>ciety<\/span><\/li>\n  <li>spe<span class='match'>cial<\/span><\/li>\n  <li>spe<span class='match'>cific<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


**[3]** Is “q” always followed by a “u”?

*Answer:*

```r
str_view(words, pattern = "q[^u]", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-7b381508077c715d2ebd" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-7b381508077c715d2ebd">{"x":{"html":"<ul>\n  <li><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
Yes, "u" always follows "q" in the *words* dataset.


**[4]** Write a regular expression that matches a word if it’s probably written in British English, not American English. Note: rules based on the [Oxford dictionaries British and American English spelling](https://en.oxforddictionaries.com/spelling/british-and-spelling)

*Answer:*

```r
str_view(words, pattern = "[^aeiou]re$|[^aeiou]our$|[^aeiou]ise$|[^aeiou]yse$|[aeiou]ll[aeiou]$|[aeiou]ence$|[aeiou]ogue$|ae|oe", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-aa668e390139cf4a5e26" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-aa668e390139cf4a5e26">{"x":{"html":"<ul>\n  <li>adver<span class='match'>tise<\/span><\/li>\n  <li>cen<span class='match'>tre<\/span><\/li>\n  <li>co<span class='match'>lour<\/span><\/li>\n  <li>exer<span class='match'>cise<\/span><\/li>\n  <li>exper<span class='match'>ience<\/span><\/li>\n  <li>fa<span class='match'>vour<\/span><\/li>\n  <li><span class='match'>four<\/span><\/li>\n  <li><span class='match'>hour<\/span><\/li>\n  <li>h<span class='match'>ullo<\/span><\/li>\n  <li>la<span class='match'>bour<\/span><\/li>\n  <li>other<span class='match'>wise<\/span><\/li>\n  <li>prac<span class='match'>tise<\/span><\/li>\n  <li>rea<span class='match'>lise<\/span><\/li>\n  <li><span class='match'>rise<\/span><\/li>\n  <li>sc<span class='match'>ience<\/span><\/li>\n  <li>sh<span class='match'>oe<\/span><\/li>\n  <li>surp<span class='match'>rise<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->



**[5]** Create a regular expression that will match telephone numbers as commonly written in your country.

*Answer:*
In the United States, phone numbers are written in the following way: 1-(XXX)-XXX-XXXX


```r
# example
str_view("1-(555)-541-2679", pattern = "1-\\([0-9][0-9][0-9]\\)-[0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-3c2a0b2e3cad163aeaca" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-3c2a0b2e3cad163aeaca">{"x":{"html":"<ul>\n  <li><span class='match'>1-(555)-541-2679<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


### Exercise 14.3.4.1

**[1]** Describe the equivalents of ?, +, * in {m,n} form.

*Answer:*
Let's use the roman numerals provided by the R for Data Science Strings chapter:

```r
w <- "1888 is the longest year in Roman numerals: MDCCCLXXXVIII"

# ?: 0 or 1
str_view(w, "C{1,2}?")
```

<!--html_preserve--><div id="htmlwidget-896b229300c5a1b342cc" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-896b229300c5a1b342cc">{"x":{"html":"<ul>\n  <li>1888 is the longest year in Roman numerals: MD<span class='match'>C<\/span>CCLXXXVIII<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# +: 1 or more
str_view(w, "C{1,}")
```

<!--html_preserve--><div id="htmlwidget-045f0ee13a9ec14c999b" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-045f0ee13a9ec14c999b">{"x":{"html":"<ul>\n  <li>1888 is the longest year in Roman numerals: MD<span class='match'>CCC<\/span>LXXXVIII<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# *: 0 or more
str_view(w, "C{0,}")
```

<!--html_preserve--><div id="htmlwidget-8f1d2c0ca85b4b501f3d" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-8f1d2c0ca85b4b501f3d">{"x":{"html":"<ul>\n  <li><span class='match'><\/span>1888 is the longest year in Roman numerals: MDCCCLXXXVIII<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


**[2]** Describe in words what these regular expressions match: (read carefully to see if I’m using a regular expression or a string that defines a regular expression.)

1. ^.*$

*Answer:*
Matches any string (i.e, starts with anything, repeated indefinitely before the end)

2. "\\{.+\\}"

*Answer:*
Matches any character (repeated 1 or more times) surrounded by squiggle brackets

3. \d{4}-\d{2}-\d{2}

*Answer:*
Matches any digit repeated four times, dash, any digit repeated two times, dash, any digit repeated two times.

4. "\\\\{4}"

*Answer:*
Matches a string of two backslashes.


**[3]** Create regular expressions to find all words that:

- Start with three consonants.

```r
str_view_all(words, pattern = "^[^aeiou][^aeiou][^aeiou]", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-55ac5e276cdfbfaa52c8" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-55ac5e276cdfbfaa52c8">{"x":{"html":"<ul>\n  <li><span class='match'>Chr<\/span>ist<\/li>\n  <li><span class='match'>Chr<\/span>istmas<\/li>\n  <li><span class='match'>dry<\/span><\/li>\n  <li><span class='match'>fly<\/span><\/li>\n  <li><span class='match'>mrs<\/span><\/li>\n  <li><span class='match'>sch<\/span>eme<\/li>\n  <li><span class='match'>sch<\/span>ool<\/li>\n  <li><span class='match'>str<\/span>aight<\/li>\n  <li><span class='match'>str<\/span>ategy<\/li>\n  <li><span class='match'>str<\/span>eet<\/li>\n  <li><span class='match'>str<\/span>ike<\/li>\n  <li><span class='match'>str<\/span>ong<\/li>\n  <li><span class='match'>str<\/span>ucture<\/li>\n  <li><span class='match'>sys<\/span>tem<\/li>\n  <li><span class='match'>thr<\/span>ee<\/li>\n  <li><span class='match'>thr<\/span>ough<\/li>\n  <li><span class='match'>thr<\/span>ow<\/li>\n  <li><span class='match'>try<\/span><\/li>\n  <li><span class='match'>typ<\/span>e<\/li>\n  <li><span class='match'>why<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

- Have three or more vowels in a row.

```r
str_view_all(words, pattern = "[aeiou][aeiou][aeiou]", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-c4bb3b7533b736d776f2" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-c4bb3b7533b736d776f2">{"x":{"html":"<ul>\n  <li>b<span class='match'>eau<\/span>ty<\/li>\n  <li>obv<span class='match'>iou<\/span>s<\/li>\n  <li>prev<span class='match'>iou<\/span>s<\/li>\n  <li>q<span class='match'>uie<\/span>t<\/li>\n  <li>ser<span class='match'>iou<\/span>s<\/li>\n  <li>var<span class='match'>iou<\/span>s<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

- Have two or more vowel-consonant pairs in a row.

```r
str_view_all(words, pattern = "([aeiou][^aeiou]){2,}", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-9dbbda07e909bd3b8337" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-9dbbda07e909bd3b8337">{"x":{"html":"<ul>\n  <li>abs<span class='match'>olut<\/span>e<\/li>\n  <li><span class='match'>agen<\/span>t<\/li>\n  <li><span class='match'>alon<\/span>g<\/li>\n  <li><span class='match'>americ<\/span>a<\/li>\n  <li><span class='match'>anot<\/span>her<\/li>\n  <li><span class='match'>apar<\/span>t<\/li>\n  <li>app<span class='match'>aren<\/span>t<\/li>\n  <li>auth<span class='match'>orit<\/span>y<\/li>\n  <li>ava<span class='match'>ilab<\/span>le<\/li>\n  <li><span class='match'>awar<\/span>e<\/li>\n  <li><span class='match'>away<\/span><\/li>\n  <li>b<span class='match'>alan<\/span>ce<\/li>\n  <li>b<span class='match'>asis<\/span><\/li>\n  <li>b<span class='match'>ecom<\/span>e<\/li>\n  <li>b<span class='match'>efor<\/span>e<\/li>\n  <li>b<span class='match'>egin<\/span><\/li>\n  <li>b<span class='match'>ehin<\/span>d<\/li>\n  <li>b<span class='match'>enefit<\/span><\/li>\n  <li>b<span class='match'>usines<\/span>s<\/li>\n  <li>ch<span class='match'>arac<\/span>ter<\/li>\n  <li>cl<span class='match'>oses<\/span><\/li>\n  <li>comm<span class='match'>unit<\/span>y<\/li>\n  <li>cons<span class='match'>ider<\/span><\/li>\n  <li>c<span class='match'>over<\/span><\/li>\n  <li>d<span class='match'>ebat<\/span>e<\/li>\n  <li>d<span class='match'>ecid<\/span>e<\/li>\n  <li>d<span class='match'>ecis<\/span>ion<\/li>\n  <li>d<span class='match'>efinit<\/span>e<\/li>\n  <li>d<span class='match'>epar<\/span>tment<\/li>\n  <li>d<span class='match'>epen<\/span>d<\/li>\n  <li>d<span class='match'>esig<\/span>n<\/li>\n  <li>d<span class='match'>evelop<\/span><\/li>\n  <li>diff<span class='match'>eren<\/span>ce<\/li>\n  <li>diff<span class='match'>icul<\/span>t<\/li>\n  <li>d<span class='match'>irec<\/span>t<\/li>\n  <li>d<span class='match'>ivid<\/span>e<\/li>\n  <li>d<span class='match'>ocumen<\/span>t<\/li>\n  <li>d<span class='match'>urin<\/span>g<\/li>\n  <li><span class='match'>econom<\/span>y<\/li>\n  <li><span class='match'>educat<\/span>e<\/li>\n  <li><span class='match'>elec<\/span>t<\/li>\n  <li><span class='match'>elec<\/span>tric<\/li>\n  <li><span class='match'>eleven<\/span><\/li>\n  <li>enco<span class='match'>urag<\/span>e<\/li>\n  <li>env<span class='match'>iron<\/span>ment<\/li>\n  <li>e<span class='match'>urop<\/span>e<\/li>\n  <li><span class='match'>even<\/span><\/li>\n  <li><span class='match'>evenin<\/span>g<\/li>\n  <li><span class='match'>ever<\/span><\/li>\n  <li><span class='match'>ever<\/span>y<\/li>\n  <li><span class='match'>eviden<\/span>ce<\/li>\n  <li><span class='match'>exac<\/span>t<\/li>\n  <li><span class='match'>exam<\/span>ple<\/li>\n  <li><span class='match'>exer<\/span>cise<\/li>\n  <li><span class='match'>exis<\/span>t<\/li>\n  <li>f<span class='match'>amil<\/span>y<\/li>\n  <li>f<span class='match'>igur<\/span>e<\/li>\n  <li>f<span class='match'>inal<\/span><\/li>\n  <li>f<span class='match'>inan<\/span>ce<\/li>\n  <li>f<span class='match'>inis<\/span>h<\/li>\n  <li>fr<span class='match'>iday<\/span><\/li>\n  <li>f<span class='match'>utur<\/span>e<\/li>\n  <li>g<span class='match'>eneral<\/span><\/li>\n  <li>g<span class='match'>over<\/span>n<\/li>\n  <li>h<span class='match'>oliday<\/span><\/li>\n  <li>h<span class='match'>ones<\/span>t<\/li>\n  <li>hosp<span class='match'>ital<\/span><\/li>\n  <li>h<span class='match'>owever<\/span><\/li>\n  <li><span class='match'>iden<\/span>tify<\/li>\n  <li><span class='match'>imagin<\/span>e<\/li>\n  <li>ind<span class='match'>ivid<\/span>ual<\/li>\n  <li>int<span class='match'>eres<\/span>t<\/li>\n  <li>intr<span class='match'>oduc<\/span>e<\/li>\n  <li><span class='match'>item<\/span><\/li>\n  <li>j<span class='match'>esus<\/span><\/li>\n  <li>l<span class='match'>evel<\/span><\/li>\n  <li>l<span class='match'>ikel<\/span>y<\/li>\n  <li>l<span class='match'>imit<\/span><\/li>\n  <li>l<span class='match'>ocal<\/span><\/li>\n  <li>m<span class='match'>ajor<\/span><\/li>\n  <li>m<span class='match'>anag<\/span>e<\/li>\n  <li>me<span class='match'>anin<\/span>g<\/li>\n  <li>me<span class='match'>asur<\/span>e<\/li>\n  <li>m<span class='match'>inis<\/span>ter<\/li>\n  <li>m<span class='match'>inus<\/span><\/li>\n  <li>m<span class='match'>inut<\/span>e<\/li>\n  <li>m<span class='match'>omen<\/span>t<\/li>\n  <li>m<span class='match'>oney<\/span><\/li>\n  <li>m<span class='match'>usic<\/span><\/li>\n  <li>n<span class='match'>atur<\/span>e<\/li>\n  <li>n<span class='match'>eces<\/span>sary<\/li>\n  <li>n<span class='match'>ever<\/span><\/li>\n  <li>n<span class='match'>otic<\/span>e<\/li>\n  <li><span class='match'>okay<\/span><\/li>\n  <li><span class='match'>open<\/span><\/li>\n  <li><span class='match'>operat<\/span>e<\/li>\n  <li>opport<span class='match'>unit<\/span>y<\/li>\n  <li>org<span class='match'>aniz<\/span>e<\/li>\n  <li><span class='match'>original<\/span><\/li>\n  <li><span class='match'>over<\/span><\/li>\n  <li>p<span class='match'>aper<\/span><\/li>\n  <li>p<span class='match'>arag<\/span>raph<\/li>\n  <li>p<span class='match'>aren<\/span>t<\/li>\n  <li>part<span class='match'>icular<\/span><\/li>\n  <li>ph<span class='match'>otog<\/span>raph<\/li>\n  <li>p<span class='match'>olic<\/span>e<\/li>\n  <li>p<span class='match'>olic<\/span>y<\/li>\n  <li>p<span class='match'>olitic<\/span><\/li>\n  <li>p<span class='match'>osit<\/span>ion<\/li>\n  <li>p<span class='match'>ositiv<\/span>e<\/li>\n  <li>p<span class='match'>ower<\/span><\/li>\n  <li>pr<span class='match'>epar<\/span>e<\/li>\n  <li>pr<span class='match'>esen<\/span>t<\/li>\n  <li>pr<span class='match'>esum<\/span>e<\/li>\n  <li>pr<span class='match'>ivat<\/span>e<\/li>\n  <li>pr<span class='match'>obab<\/span>le<\/li>\n  <li>pr<span class='match'>oces<\/span>s<\/li>\n  <li>pr<span class='match'>oduc<\/span>e<\/li>\n  <li>pr<span class='match'>oduc<\/span>t<\/li>\n  <li>pr<span class='match'>ojec<\/span>t<\/li>\n  <li>pr<span class='match'>oper<\/span><\/li>\n  <li>pr<span class='match'>opos<\/span>e<\/li>\n  <li>pr<span class='match'>otec<\/span>t<\/li>\n  <li>pr<span class='match'>ovid<\/span>e<\/li>\n  <li>qu<span class='match'>alit<\/span>y<\/li>\n  <li>re<span class='match'>alis<\/span>e<\/li>\n  <li>re<span class='match'>ason<\/span><\/li>\n  <li>r<span class='match'>ecen<\/span>t<\/li>\n  <li>r<span class='match'>ecog<\/span>nize<\/li>\n  <li>r<span class='match'>ecom<\/span>mend<\/li>\n  <li>r<span class='match'>ecor<\/span>d<\/li>\n  <li>r<span class='match'>educ<\/span>e<\/li>\n  <li>r<span class='match'>efer<\/span><\/li>\n  <li>r<span class='match'>egar<\/span>d<\/li>\n  <li>r<span class='match'>elat<\/span>ion<\/li>\n  <li>r<span class='match'>emem<\/span>ber<\/li>\n  <li>r<span class='match'>epor<\/span>t<\/li>\n  <li>repr<span class='match'>esen<\/span>t<\/li>\n  <li>r<span class='match'>esul<\/span>t<\/li>\n  <li>r<span class='match'>etur<\/span>n<\/li>\n  <li>s<span class='match'>atur<\/span>day<\/li>\n  <li>s<span class='match'>econ<\/span>d<\/li>\n  <li>secr<span class='match'>etar<\/span>y<\/li>\n  <li>s<span class='match'>ecur<\/span>e<\/li>\n  <li>s<span class='match'>eparat<\/span>e<\/li>\n  <li>s<span class='match'>even<\/span><\/li>\n  <li>s<span class='match'>imilar<\/span><\/li>\n  <li>sp<span class='match'>ecific<\/span><\/li>\n  <li>str<span class='match'>ateg<\/span>y<\/li>\n  <li>st<span class='match'>uden<\/span>t<\/li>\n  <li>st<span class='match'>upid<\/span><\/li>\n  <li>t<span class='match'>elep<\/span>hone<\/li>\n  <li>t<span class='match'>elevis<\/span>ion<\/li>\n  <li>th<span class='match'>erefor<\/span>e<\/li>\n  <li>tho<span class='match'>usan<\/span>d<\/li>\n  <li>t<span class='match'>oday<\/span><\/li>\n  <li>t<span class='match'>oget<\/span>her<\/li>\n  <li>t<span class='match'>omor<\/span>row<\/li>\n  <li>t<span class='match'>onig<\/span>ht<\/li>\n  <li>t<span class='match'>otal<\/span><\/li>\n  <li>t<span class='match'>owar<\/span>d<\/li>\n  <li>tr<span class='match'>avel<\/span><\/li>\n  <li><span class='match'>unit<\/span><\/li>\n  <li><span class='match'>unit<\/span>e<\/li>\n  <li><span class='match'>univer<\/span>sity<\/li>\n  <li><span class='match'>upon<\/span><\/li>\n  <li>v<span class='match'>isit<\/span><\/li>\n  <li>w<span class='match'>ater<\/span><\/li>\n  <li>w<span class='match'>oman<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->


**[4]** Solve the beginner regexp crosswords at https://regexcrossword.com/challenges/beginner.



### Exercise 14.3.5.1

**[1]** Describe, in words, what these expressions will match:

1. (.)\1\1 

*Answer:* Any character that repeates three times in a row

2. "(.)(.)\\2\\1" 

*Answer:* Any two characters that appear consecutively but switch positions (e.g., abba)

3. (..)\1

*Answer* Any two characters that appear consecutively in the same position.

4. "(.).\\1.\\1"

*Answer* Any character, followed by any character, followed, by the first character, followed by any character, followed by the first character (e.g., "abada")


5. "(.)(.)(.).*\\3\\2\\1"

*Answer* Any four or more characters, followed by the third, second, and first characters (e.g., "abcdefcba")


**[2]** Construct regular expressions to match words that:

1. Start and end with the same character.

```r
str_view(words, pattern = "^(.).*\\1$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-99eb89d4637efbe54455" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-99eb89d4637efbe54455">{"x":{"html":"<ul>\n  <li><span class='match'>america<\/span><\/li>\n  <li><span class='match'>area<\/span><\/li>\n  <li><span class='match'>dad<\/span><\/li>\n  <li><span class='match'>dead<\/span><\/li>\n  <li><span class='match'>depend<\/span><\/li>\n  <li><span class='match'>educate<\/span><\/li>\n  <li><span class='match'>else<\/span><\/li>\n  <li><span class='match'>encourage<\/span><\/li>\n  <li><span class='match'>engine<\/span><\/li>\n  <li><span class='match'>europe<\/span><\/li>\n  <li><span class='match'>evidence<\/span><\/li>\n  <li><span class='match'>example<\/span><\/li>\n  <li><span class='match'>excuse<\/span><\/li>\n  <li><span class='match'>exercise<\/span><\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experience<\/span><\/li>\n  <li><span class='match'>eye<\/span><\/li>\n  <li><span class='match'>health<\/span><\/li>\n  <li><span class='match'>high<\/span><\/li>\n  <li><span class='match'>knock<\/span><\/li>\n  <li><span class='match'>level<\/span><\/li>\n  <li><span class='match'>local<\/span><\/li>\n  <li><span class='match'>nation<\/span><\/li>\n  <li><span class='match'>non<\/span><\/li>\n  <li><span class='match'>rather<\/span><\/li>\n  <li><span class='match'>refer<\/span><\/li>\n  <li><span class='match'>remember<\/span><\/li>\n  <li><span class='match'>serious<\/span><\/li>\n  <li><span class='match'>stairs<\/span><\/li>\n  <li><span class='match'>test<\/span><\/li>\n  <li><span class='match'>tonight<\/span><\/li>\n  <li><span class='match'>transport<\/span><\/li>\n  <li><span class='match'>treat<\/span><\/li>\n  <li><span class='match'>trust<\/span><\/li>\n  <li><span class='match'>window<\/span><\/li>\n  <li><span class='match'>yesterday<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

2. Contain a repeated pair of letters (e.g. “church” contains “ch” repeated twice.)

```r
str_view(words, pattern = "(.)(.).*\\1\\2.*", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-8ecfe3eab5be86c6231e" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-8ecfe3eab5be86c6231e">{"x":{"html":"<ul>\n  <li>ap<span class='match'>propriate<\/span><\/li>\n  <li><span class='match'>church<\/span><\/li>\n  <li>c<span class='match'>ondition<\/span><\/li>\n  <li><span class='match'>decide<\/span><\/li>\n  <li><span class='match'>environment<\/span><\/li>\n  <li>l<span class='match'>ondon<\/span><\/li>\n  <li>pa<span class='match'>ragraph<\/span><\/li>\n  <li>p<span class='match'>articular<\/span><\/li>\n  <li><span class='match'>photograph<\/span><\/li>\n  <li>p<span class='match'>repare<\/span><\/li>\n  <li>p<span class='match'>ressure<\/span><\/li>\n  <li>r<span class='match'>emember<\/span><\/li>\n  <li><span class='match'>represent<\/span><\/li>\n  <li><span class='match'>require<\/span><\/li>\n  <li><span class='match'>sense<\/span><\/li>\n  <li>the<span class='match'>refore<\/span><\/li>\n  <li>u<span class='match'>nderstand<\/span><\/li>\n  <li>w<span class='match'>hether<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

3. Contain one letter repeated in at least three places (e.g. “eleven” contains three “e”s.)

```r
str_view(words, pattern = "(.).*\\1.*\\1.*", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-3a0d9e02db6a0901cb4b" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-3a0d9e02db6a0901cb4b">{"x":{"html":"<ul>\n  <li>a<span class='match'>ppropriate<\/span><\/li>\n  <li><span class='match'>available<\/span><\/li>\n  <li>b<span class='match'>elieve<\/span><\/li>\n  <li>b<span class='match'>etween<\/span><\/li>\n  <li>bu<span class='match'>siness<\/span><\/li>\n  <li>d<span class='match'>egree<\/span><\/li>\n  <li>diff<span class='match'>erence<\/span><\/li>\n  <li>di<span class='match'>scuss<\/span><\/li>\n  <li><span class='match'>eleven<\/span><\/li>\n  <li>e<span class='match'>nvironment<\/span><\/li>\n  <li><span class='match'>evidence<\/span><\/li>\n  <li><span class='match'>exercise<\/span><\/li>\n  <li><span class='match'>expense<\/span><\/li>\n  <li><span class='match'>experience<\/span><\/li>\n  <li><span class='match'>individual<\/span><\/li>\n  <li>p<span class='match'>aragraph<\/span><\/li>\n  <li>r<span class='match'>eceive<\/span><\/li>\n  <li>r<span class='match'>emember<\/span><\/li>\n  <li>r<span class='match'>epresent<\/span><\/li>\n  <li>t<span class='match'>elephone<\/span><\/li>\n  <li>th<span class='match'>erefore<\/span><\/li>\n  <li>t<span class='match'>omorrow<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

### Exercise 14.4.2 

**[1]** For each of the following challenges, try solving it by using both a single regular expression, and a combination of multiple str_detect() calls.

1. Find all words that start or end with x.


```r
# regex
str_view_all(words, "^x|x$", match = TRUE)
```

<!--html_preserve--><div id="htmlwidget-a171b91800f62b9c2a70" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-a171b91800f62b9c2a70">{"x":{"html":"<ul>\n  <li>bo<span class='match'>x<\/span><\/li>\n  <li>se<span class='match'>x<\/span><\/li>\n  <li>si<span class='match'>x<\/span><\/li>\n  <li>ta<span class='match'>x<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# multiple str_detect
str_detect(words, "^x")
```

```
##   [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [12] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [23] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [34] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [45] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [56] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [67] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [78] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [89] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [100] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [111] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [122] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [133] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [144] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [155] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [166] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [177] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [188] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [199] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [210] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [221] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [232] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [243] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [254] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [265] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [276] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [287] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [298] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [309] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [320] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [331] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [342] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [353] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [364] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [375] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [386] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [397] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [408] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [419] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [430] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [441] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [452] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [463] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [474] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [485] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [496] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [507] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [518] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [529] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [540] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [551] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [562] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [573] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [584] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [595] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [606] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [617] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [628] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [639] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [650] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [661] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [672] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [683] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [694] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [705] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [716] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [727] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [738] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [749] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [760] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [771] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [782] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [793] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [804] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [815] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [826] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [837] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [848] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [859] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [870] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [881] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [892] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [903] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [914] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [925] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [936] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [947] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [958] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [969] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [980] FALSE
```

```r
str_detect(words, "x$")
```

```
##   [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [12] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [23] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [34] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [45] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [56] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [67] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [78] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [89] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [100] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE
## [111] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [122] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [133] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [144] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [155] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [166] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [177] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [188] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [199] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [210] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [221] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [232] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [243] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [254] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [265] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [276] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [287] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [298] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [309] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [320] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [331] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [342] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [353] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [364] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [375] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [386] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [397] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [408] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [419] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [430] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [441] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [452] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [463] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [474] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [485] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [496] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [507] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [518] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [529] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [540] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [551] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [562] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [573] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [584] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [595] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [606] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [617] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [628] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [639] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [650] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [661] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [672] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [683] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [694] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [705] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [716] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [727] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [738] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE
## [749] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [760] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [771] FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [782] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [793] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [804] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [815] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [826] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [837] FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE
## [848] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [859] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [870] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [881] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [892] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [903] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [914] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [925] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [936] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [947] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [958] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [969] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [980] FALSE
```

2. Find all words that start with a vowel and end with a consonant.

```r
str_detect(words, "^[aeiou][^aeiou]$")
```

```
##   [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [12] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [23] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [34] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [45] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE
##  [56] FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [67] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [78] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
##  [89] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [100] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [111] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [122] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [133] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [144] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [155] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [166] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [177] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [188] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [199] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [210] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [221] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [232] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [243] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [254] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [265] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [276] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [287] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [298] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [309] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [320] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [331] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [342] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [353] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [364] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [375] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [386] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [397] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [408] FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE  TRUE
## [419] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [430] FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE
## [441] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [452] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [463] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [474] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [485] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [496] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [507] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [518] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [529] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [540] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [551] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [562] FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE
## [573] FALSE FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE
## [584] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [595] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [606] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [617] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [628] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [639] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [650] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [661] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [672] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [683] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [694] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [705] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [716] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [727] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [738] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [749] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [760] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [771] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [782] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [793] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [804] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [815] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [826] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [837] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [848] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [859] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [870] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [881] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [892] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [903] FALSE FALSE FALSE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE
## [914] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [925] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [936] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [947] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [958] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [969] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
## [980] FALSE
```

3. Are there any words that contain at least one of each different vowel?

```r
sum(str_detect(words, "a")) +
sum(str_detect(words, "e")) +
sum(str_detect(words, "i")) +
sum(str_detect(words, "o")) +
sum(str_detect(words, "u"))
```

```
## [1] 1705
```


**[2]** What word has the highest number of vowels? What word has the highest proportion of vowels? (Hint: what is the denominator?)


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
df <- tibble(
  word = words, 
  i = seq_along(word)
)

# find the count and proportion of vowels for each word
df.vowels <- df %>% 
  mutate(
    vowels = str_count(word, "[aeiou]"),
    proportion = str_count(word, "[aeiou]")/str_length(word) 
        ) %>% 
  arrange(desc(vowels)) %>% 
  mutate()
```
There are eight words tied for the most vowels (5). 

Let's arange by proportion to find the word has the highest proportion of vowels:

```r
arrange(df.vowels, desc(proportion))
```

```
## # A tibble: 980 x 4
##    word       i vowels proportion
##    <chr>  <int>  <int>      <dbl>
##  1 a          1      1      1    
##  2 area      49      3      0.75 
##  3 idea     412      3      0.75 
##  4 europe   278      4      0.667
##  5 age       22      2      0.667
##  6 ago       24      2      0.667
##  7 air       26      2      0.667
##  8 die      228      2      0.667
##  9 due      250      2      0.667
## 10 eat      256      2      0.667
## # ... with 970 more rows
```
We can see that the answer is the letter "a", which makes sense, considering it is comprised of one letter, and that letter is a vowel.


### Exercise 14.4.3.1 

**[1]** In the previous example, you might have noticed that the regular expression matched “flickered”, which is not a colour. Modify the regex to fix the problem.

*Answer:*

```r
# add a space before "red"
colours <- c(" red", "orange", "yellow", "green", "blue", "purple")
colour_match <- str_c(colours, collapse = "|")
colour_match
```

```
## [1] " red|orange|yellow|green|blue|purple"
```

```r
#check problem is fixed (i.e. "flickered is not matched")
more <- sentences[str_count(sentences, colour_match) > 1]
str_view_all(more, colour_match)
```

<!--html_preserve--><div id="htmlwidget-682d0a6b27c2a80d892c" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-682d0a6b27c2a80d892c">{"x":{"html":"<ul>\n  <li>It is hard to erase <span class='match'>blue<\/span> or<span class='match'> red<\/span> ink.<\/li>\n  <li>The sky in the west is tinged with <span class='match'>orange<\/span><span class='match'> red<\/span>.<\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

**[2]** From the Harvard sentences data, extract:

1. The first word from each sentence.

```r
# get a reminder of the sentences
head(sentences)
```

```
## [1] "The birch canoe slid on the smooth planks." 
## [2] "Glue the sheet to the dark blue background."
## [3] "It's easy to tell the depth of a well."     
## [4] "These days a chicken leg is a rare dish."   
## [5] "Rice is often served in round bowls."       
## [6] "The juice of lemons makes fine punch."
```

```r
# use the word() function!
head(word(sentences, 1))
```

```
## [1] "The"   "Glue"  "It's"  "These" "Rice"  "The"
```

2.All words ending in ing.

```r
#Couldn't quite get this one...
#str_view_all(words, ".*ing$", match = TRUE)
#has.ing.words <- str_subset(sentences, ".*ing$")
#ing.words <- str_extract(has.ing.words,".*ing$" )
#str_extract(ing.words, ".*ing$", simplify = TRUE)
```


3. All plurals.

```r
# This one also stymied me!
```


### Exercise 14.4.4.1

**[1]** Find all words that come after a “number” like “one”, “two”, “three” etc. Pull out both the number and the word.




**[2]** Find all contractions. Separate out the pieces before and after the apostrophe.

*Answer:*



### Exercise 14.4.5.1

**[1]** Replace all forward slashes in a string with backslashes.

*Answer:*

```r
str_replace_all("Files/Documents/Homework", "/", "\\\\")
```

```
## [1] "Files\\Documents\\Homework"
```

**[2]** Implement a simple version of str_to_lower() using replace_all().

*Answer:*

```r
str_replace_all("Uppercase Stinks!", c("U" = "u", "S" = "s"))
```

```
## [1] "uppercase stinks!"
```

**[3]** Switch the first and last letters in words. Which of those strings are still words?

*Answer:*

```r
#Couldn't quite get this one...
#str_replace_all(words, "([[^ ]]+) ([[ $]]+)", "\\2 \\1")
```


### Exercise 14.4.6.1

**[1]** Split up a string like "apples, pears, and bananas" into individual components.

*Answer:*

```r
str_split("apples, pears, and bananas", " ")
```

```
## [[1]]
## [1] "apples," "pears,"  "and"     "bananas"
```

**[2]** Why is it better to split up by boundary("word") than " "?

*Answer:*
Because boundary("word") will give you each of the words and drop the commas:

```r
str_split("apples, pears, and bananas", boundary("word"))
```

```
## [[1]]
## [1] "apples"  "pears"   "and"     "bananas"
```

**[3]** What does splitting with an empty string ("") do? Experiment, and then read the documentation.

*Answer:*

```r
# experiment
str_split("apples, pears, and bananas", "")
```

```
## [[1]]
##  [1] "a" "p" "p" "l" "e" "s" "," " " "p" "e" "a" "r" "s" "," " " "a" "n"
## [18] "d" " " "b" "a" "n" "a" "n" "a" "s"
```
We see that it has split by character. In other words, it's equivalent to using boundary("character").

### Exercise 14.5.1

**[1]** How would you find all strings containing \ with regex() vs. with fixed()?

*Answer:*

```r
#with regex
str_view_all("\\_\\", regex("\\\\"))
```

<!--html_preserve--><div id="htmlwidget-f72e8970e1acbb07c321" style="width:960px;height:100%;" class="str_view html-widget"></div>
<script type="application/json" data-for="htmlwidget-f72e8970e1acbb07c321">{"x":{"html":"<ul>\n  <li><span class='match'>\\<\/span>_<span class='match'>\\<\/span><\/li>\n<\/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
#with fixed
#str_detect("\\_\\", fixed("\"))
```

**[2]** What are the five most common words in sentences?

*Answer:*

```r
# Couldn't figure this one out...
```



### Exercise 14.7.1

**[1]** Find the stringi functions that:

Count the number of words.

*Answer:* stri_count_words()

Find duplicated strings.

*Answer:* stri_duplicated() or stri_duplicated_any()

Generate random text. 

*Answer:* stri_rand_lipsum()


**[2]** How do you control the language that stri_sort() uses for sorting?

*Answer:* sorts the vector according to a lexicographic order, and allows you to specify a custom ICU (International Components for Unicode) collator.

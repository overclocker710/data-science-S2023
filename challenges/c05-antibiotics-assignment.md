Antibiotics
================
Daniel Heitz
03/05/2023

*Purpose*: Creating effective data visualizations is an *iterative*
process; very rarely will the first graph you make be the most
effective. The most effective thing you can do to be successful in this
iterative process is to *try multiple graphs* of the same data.

Furthermore, judging the effectiveness of a visual is completely
dependent on *the question you are trying to answer*. A visual that is
totally ineffective for one question may be perfect for answering a
different question.

In this challenge, you will practice *iterating* on data visualization,
and will anchor the *assessment* of your visuals using two different
questions.

*Note*: Please complete your initial visual design **alone**. Work on
both of your graphs alone, and save a version to your repo *before*
coming together with your team. This way you can all bring a diversity
of ideas to the table!

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category    | Needs Improvement                                                                                                | Satisfactory                                                                                                               |
|-------------|------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| Effort      | Some task **q**’s left unattempted                                                                               | All task **q**’s attempted                                                                                                 |
| Observed    | Did not document observations, or observations incorrect                                                         | Documented correct observations based on analysis                                                                          |
| Supported   | Some observations not clearly supported by analysis                                                              | All observations clearly supported by analysis (table, graph, etc.)                                                        |
| Assessed    | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support      |
| Specified   | Uses the phrase “more data are necessary” without clarification                                                  | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability                                 | Code sufficiently close to the [style guide](https://style.tidyverse.org/)                                                 |

## Due Date

<!-- ------------------------- -->

All the deliverables stated in the rubrics above are due **at midnight**
before the day of the class discussion of the challenge. See the
[Syllabus](https://docs.google.com/document/d/1qeP6DUS8Djq_A0HMllMqsSqX3a9dbcx1/edit?usp=sharing&ouid=110386251748498665069&rtpof=true&sd=true)
for more information.

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.4.0      ✔ purrr   1.0.1 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
    ## ✔ readr   2.1.3      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(ggrepel)
```

*Background*: The data\[1\] we study in this challenge report the
[*minimum inhibitory
concentration*](https://en.wikipedia.org/wiki/Minimum_inhibitory_concentration)
(MIC) of three drugs for different bacteria. The smaller the MIC for a
given drug and bacteria pair, the more practical the drug is for
treating that particular bacteria. An MIC value of *at most* 0.1 is
considered necessary for treating human patients.

These data report MIC values for three antibiotics—penicillin,
streptomycin, and neomycin—on 16 bacteria. Bacteria are categorized into
a genus based on a number of features, including their resistance to
antibiotics.

``` r
## NOTE: If you extracted all challenges to the same location,
## you shouldn't have to change this filename
filename <- "./data/antibiotics.csv"

## Load the data
df_antibiotics <- read_csv(filename)
```

    ## Rows: 16 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): bacteria, gram
    ## dbl (3): penicillin, streptomycin, neomycin
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df_antibiotics %>% knitr::kable()
```

| bacteria                        | penicillin | streptomycin | neomycin | gram     |
|:--------------------------------|-----------:|-------------:|---------:|:---------|
| Aerobacter aerogenes            |    870.000 |         1.00 |    1.600 | negative |
| Brucella abortus                |      1.000 |         2.00 |    0.020 | negative |
| Bacillus anthracis              |      0.001 |         0.01 |    0.007 | positive |
| Diplococcus pneumonia           |      0.005 |        11.00 |   10.000 | positive |
| Escherichia coli                |    100.000 |         0.40 |    0.100 | negative |
| Klebsiella pneumoniae           |    850.000 |         1.20 |    1.000 | negative |
| Mycobacterium tuberculosis      |    800.000 |         5.00 |    2.000 | negative |
| Proteus vulgaris                |      3.000 |         0.10 |    0.100 | negative |
| Pseudomonas aeruginosa          |    850.000 |         2.00 |    0.400 | negative |
| Salmonella (Eberthella) typhosa |      1.000 |         0.40 |    0.008 | negative |
| Salmonella schottmuelleri       |     10.000 |         0.80 |    0.090 | negative |
| Staphylococcus albus            |      0.007 |         0.10 |    0.001 | positive |
| Staphylococcus aureus           |      0.030 |         0.03 |    0.001 | positive |
| Streptococcus fecalis           |      1.000 |         1.00 |    0.100 | positive |
| Streptococcus hemolyticus       |      0.001 |        14.00 |   10.000 | positive |
| Streptococcus viridans          |      0.005 |        10.00 |   40.000 | positive |

# Visualization

<!-- -------------------------------------------------- -->

### **q1** Prototype 5 visuals

To start, construct **5 qualitatively different visualizations of the
data** `df_antibiotics`. These **cannot** be simple variations on the
same graph; for instance, if two of your visuals could be made identical
by calling `coord_flip()`, then these are *not* qualitatively different.

For all five of the visuals, you must show information on *all 16
bacteria*. For the first two visuals, you must *show all variables*.

*Hint 1*: Try working quickly on this part; come up with a bunch of
ideas, and don’t fixate on any one idea for too long. You will have a
chance to refine later in this challenge.

*Hint 2*: The data `df_antibiotics` are in a *wide* format; it may be
helpful to `pivot_longer()` the data to make certain visuals easier to
construct.

#### Visual 1 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. You must also show whether or not each bacterium is Gram
positive or negative.

``` r
# WRITE YOUR CODE HERE
df_pivoted <-
  df_antibiotics %>% 
  pivot_longer(
    cols = c("penicillin", "streptomycin", "neomycin"),
    values_to = "MIC",
    names_to = "antibiotic"
  )


min_MIC <- min(df_pivoted$MIC)
df_pivoted %>%
  mutate(normalized_MIC = MIC / min_MIC) %>% 
  ggplot() + 
  geom_col(position = "dodge", aes(fill = antibiotic, x = bacteria, y = normalized_MIC)) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  scale_y_log10() +
  geom_hline(yintercept = 0.1/min_MIC)
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.1-1.png)<!-- -->

``` r
df_pivoted
```

    ## # A tibble: 48 × 4
    ##    bacteria              gram     antibiotic       MIC
    ##    <chr>                 <chr>    <chr>          <dbl>
    ##  1 Aerobacter aerogenes  negative penicillin   870    
    ##  2 Aerobacter aerogenes  negative streptomycin   1    
    ##  3 Aerobacter aerogenes  negative neomycin       1.6  
    ##  4 Brucella abortus      negative penicillin     1    
    ##  5 Brucella abortus      negative streptomycin   2    
    ##  6 Brucella abortus      negative neomycin       0.02 
    ##  7 Bacillus anthracis    positive penicillin     0.001
    ##  8 Bacillus anthracis    positive streptomycin   0.01 
    ##  9 Bacillus anthracis    positive neomycin       0.007
    ## 10 Diplococcus pneumonia positive penicillin     0.005
    ## # … with 38 more rows

#### Visual 2 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. You must also show whether or not each bacterium is Gram
positive or negative.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_pivoted %>%
  mutate(normalized_MIC = MIC / min(MIC)) %>% 
  ggplot() + 
  geom_col(position = "dodge", aes(fill = antibiotic, x = bacteria, y = normalized_MIC)) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  scale_y_log10() +
  facet_grid(cols = vars(gram), scales = "free_x") 
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.2-1.png)<!-- -->

#### Visual 3 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# Visual 3
df_pivoted %>%
  filter(antibiotic == "penicillin") %>% 
  mutate(normalized_MIC = MIC / min(MIC)) %>%
  arrange(+normalized_MIC) %>%
  ggplot() + 
  geom_col(position = "dodge", aes(fill = antibiotic, x = reorder(bacteria, normalized_MIC), y = normalized_MIC)) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  scale_y_log10()
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.3_modified-1.png)<!-- -->

#### Visual 4 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
df_antibiotics %>%
  rowwise() %>% 
  mutate(max_MIC = 100000 * min(penicillin, streptomycin, neomycin)) %>% 
  ggplot(aes(x = reorder(bacteria, max_MIC), y = max_MIC, fill = gram)) +
  geom_col(position = "dodge") +
  labs(title = "MIC values by Gram stain of bacteria",
       x = "Bacteria", y = " Max MIC value * 10^5",
       fill = "Gram Stain") +
  scale_fill_manual(values = c("purple", "yellow")) +
  scale_color_identity(guide = "none") +
  theme_minimal() +
  theme(legend.position = "bottom") + 
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  scale_y_continuous(trans = "log10", limits = c(0.01, NA), breaks = c(0.01, 0.1, 1, 10, 100)) +
  geom_hline(yintercept = 10000)
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.4_modified-1.png)<!-- -->

#### Visual 5 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
library(tidyverse)
library(ggrepel)

df_antibiotics %>%
  rowwise() %>% 
  mutate(max_MIC = max(penicillin, streptomycin, neomycin)) %>% 
  mutate(avg_MIC = mean(c(penicillin, streptomycin, neomycin)),
         sd_MIC = sd(c(penicillin, streptomycin, neomycin))) %>%
  ggplot(aes(x = avg_MIC, y = sd_MIC)) +
  geom_point(size = 3, color = "black") +
  geom_text_repel(aes(label = bacteria), size = 3, color = "black",
                  box.padding = 0.5, max.segment.length = 10, max.overlaps = 100) +
  labs(title = "MIC values by Gram stain of bacteria",
       x = "Average MIC value of penicillin, streptomycin, and neomycin",
       y = "Standard deviation of MIC values") +
  theme_minimal()
```

    ## Warning in geom_text_repel(aes(label = bacteria), size = 3, color = "black", :
    ## Ignoring unknown parameters: `max.segment.length`

![](c05-antibiotics-assignment_files/figure-gfm/q1.5-1.png)<!-- -->

### **q2** Assess your visuals

There are **two questions** below; use your five visuals to help answer
both Guiding Questions. Note that you must also identify which of your
five visuals were most helpful in answering the questions.

*Hint 1*: It’s possible that *none* of your visuals is effective in
answering the questions below. You may need to revise one or more of
your visuals to answer the questions below!

*Hint 2*: It’s **highly unlikely** that the same visual is the most
effective at helping answer both guiding questions. **Use this as an
opportunity to think about why this is.**

#### Guiding Question 1

> How do the three antibiotics vary in their effectiveness against
> bacteria of different genera and Gram stain?

*Observations* - What is your response to the question above? - (Write
your response here) - Which of your visuals above (1 through 5) is
**most effective** at helping to answer this question? - visual 2 is the
most effective, because it shows you the gram of the bacteria as well as
has all the three antibiotics together. Graphs 3, 4, and 5 together do a
better job making it clear with regards to MIC, but do not display the
gram and also are three separate visuals (they could be faceted together
though). - Why? - Visual 2 is the only visual that displays everything
we want. Visual 1 we can look up the gram based on the name, but that’s
an extra step that visual 2 lets us skip.

#### Guiding Question 2

In 1974 *Diplococcus pneumoniae* was renamed *Streptococcus pneumoniae*,
and in 1984 *Streptococcus fecalis* was renamed *Enterococcus fecalis*
\[2\].

> Why was *Diplococcus pneumoniae* was renamed *Streptococcus
> pneumoniae*?

*Observations* - What is your response to the question above? - (Write
your response here) - Which of your visuals above (1 through 5) is
**most effective** at helping to answer this question? - Visual 4 does
the best, it shows it grouped with two other streptococcus bacteria. -
Why? - Because its resistances are similar to theirs, it was grouped
with them as streptococcus according to the american scientist article.

# References

<!-- -------------------------------------------------- -->

\[1\] Neomycin in skin infections: A new topical antibiotic with wide
antibacterial range and rarely sensitizing. Scope. 1951;3(5):4-7.

\[2\] Wainer and Lysen, “That’s Funny…” *American Scientist* (2009)
[link](https://www.americanscientist.org/article/thats-funny)

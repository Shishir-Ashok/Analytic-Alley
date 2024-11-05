---
title: Diwali Sales Data
date: 2024-10-29
images: 
  - /portfolio/Diwali Sales/diwali_sales_files/figure-html/unnamed-chunk-4-1.png
tags:
  - Markdown
  - Data Analysis
  - R
---

## Exploring Diwali Sales Data

### Context

- The Diwali Sales dataset, featured in the [TidyTuesday project](https://github.com/rfordatascience/tidytuesday/blob/master/data/2023/2023-11-14/readme.md) for the week of November 14, 2023, provides a comprehensive look at retail sales during the Diwali festival in India. It offers a detailed snapshot of consumer behavior during one of the most significant festive periods in India.

### Key Business Questions

1. **Which age group spends the most during Diwali?**
2. **How does expenditure vary across genders within each age group?**
3. **What are the common occupations of individuals in the highest spending age group?**
4. **Which states have the highest spending, and how does this vary by gender?**
5. **How does marital status influence spending patterns across different states and genders?**

### Insights and Trends

1. **Age Group Spending:** 
    - The age group 26-35 spends the most during Diwali. This demographic is likely to have higher disposable income and a greater inclination towards festive shopping.
    
2. **Gender-Based Expenditure:**
    - Within each age group, spending patterns vary significantly between genders. In general, females tend to spend more than males across all age groups.
    
3. **Occupational Insights:**
    - For individuals aged 26-35, the predominant occupations are in IT, healthcare, aviation, and banking. This suggests that marketing strategies aimed at these professional groups could be advantageous.

4. **State-Wise Spending:**
    - States such as Uttar Pradesh, Maharashtra, Karnataka, and Delhi rank among the highest in spending.

5. **Marital Status Influence:**
    - Married individuals tend to spend more than unmarried ones. This trend is consistent across both genders.

### Recommendations

1. **Targeted Marketing Campaigns:**
    - Focus marketing efforts on the 26-35 age group, especially targeting professionals and business owners. Tailor campaigns to highlight products and offers that appeal to this demographic.

2. **Gender-Specific Strategies:**
    - Develop gender-specific marketing strategies. For example, create campaigns that resonate with female consumers in high-spending states.
    
3. **State-Specific Promotions:**
    - Implement state-specific promotions, particularly in states with high spending. Customize offers and discounts to cater to the preferences of consumers in these regions.
    
4. **Enhanced Customer Engagement:**
    - Use insights from occupational data to engage with customers through personalized communication. Offer exclusive deals and loyalty programs for professionals.
    
    

``` r
library(extrafont)
```

```
## Registering fonts with R
```

``` r
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

``` r
library(scales) 
```

```
## 
## Attaching package: 'scales'
## 
## The following object is masked from 'package:purrr':
## 
##     discard
## 
## The following object is masked from 'package:readr':
## 
##     col_factor
```

``` r
loadfonts(device = "win")
```

```
## Agency FB already registered with windowsFonts().
## Algerian already registered with windowsFonts().
## Arial Black already registered with windowsFonts().
## Arial already registered with windowsFonts().
## Arial Narrow already registered with windowsFonts().
## Arial Rounded MT Bold already registered with windowsFonts().
## Bahnschrift already registered with windowsFonts().
## Baskerville Old Face already registered with windowsFonts().
## Bauhaus 93 already registered with windowsFonts().
## Bell MT already registered with windowsFonts().
## Berlin Sans FB already registered with windowsFonts().
## Berlin Sans FB Demi already registered with windowsFonts().
## Bernard MT Condensed already registered with windowsFonts().
## Blackadder ITC already registered with windowsFonts().
## Bodoni MT already registered with windowsFonts().
## Bodoni MT Black already registered with windowsFonts().
## Bodoni MT Condensed already registered with windowsFonts().
## Bodoni MT Poster Compressed already registered with windowsFonts().
## Book Antiqua already registered with windowsFonts().
## Bookman Old Style already registered with windowsFonts().
## Bookshelf Symbol 7 already registered with windowsFonts().
## Bradley Hand ITC already registered with windowsFonts().
## Britannic Bold already registered with windowsFonts().
## Broadway already registered with windowsFonts().
## Brush Script MT already registered with windowsFonts().
## Calibri already registered with windowsFonts().
## Calibri Light already registered with windowsFonts().
## Californian FB already registered with windowsFonts().
## Calisto MT already registered with windowsFonts().
## Cambria already registered with windowsFonts().
## Candara already registered with windowsFonts().
## Candara Light already registered with windowsFonts().
## Castellar already registered with windowsFonts().
## Centaur already registered with windowsFonts().
## Century already registered with windowsFonts().
## Century Gothic already registered with windowsFonts().
## Century Schoolbook already registered with windowsFonts().
## Chiller already registered with windowsFonts().
## Colonna MT already registered with windowsFonts().
## Comic Sans MS already registered with windowsFonts().
## Consolas already registered with windowsFonts().
## Constantia already registered with windowsFonts().
## Cooper Black already registered with windowsFonts().
## Copperplate Gothic Bold already registered with windowsFonts().
## Copperplate Gothic Light already registered with windowsFonts().
## Corbel already registered with windowsFonts().
## Corbel Light already registered with windowsFonts().
## Courier New already registered with windowsFonts().
## Curlz MT already registered with windowsFonts().
## Dubai already registered with windowsFonts().
## Dubai Light already registered with windowsFonts().
## Dubai Medium already registered with windowsFonts().
## Ebrima already registered with windowsFonts().
## Edwardian Script ITC already registered with windowsFonts().
## Elephant already registered with windowsFonts().
## Engravers MT already registered with windowsFonts().
## Eras Bold ITC already registered with windowsFonts().
## Eras Demi ITC already registered with windowsFonts().
## Eras Light ITC already registered with windowsFonts().
## Eras Medium ITC already registered with windowsFonts().
## Felix Titling already registered with windowsFonts().
## Fira Code already registered with windowsFonts().
## Fira Code Light already registered with windowsFonts().
## Fira Code Medium already registered with windowsFonts().
## Fira Code SemiBold already registered with windowsFonts().
## Footlight MT Light already registered with windowsFonts().
## Forte already registered with windowsFonts().
## Franklin Gothic Book already registered with windowsFonts().
## Franklin Gothic Demi already registered with windowsFonts().
## Franklin Gothic Demi Cond already registered with windowsFonts().
## Franklin Gothic Heavy already registered with windowsFonts().
## Franklin Gothic Medium already registered with windowsFonts().
## Franklin Gothic Medium Cond already registered with windowsFonts().
## Freestyle Script already registered with windowsFonts().
## French Script MT already registered with windowsFonts().
## Gabriola already registered with windowsFonts().
## Gadugi already registered with windowsFonts().
## Garamond already registered with windowsFonts().
## Georgia already registered with windowsFonts().
## Gigi already registered with windowsFonts().
## Gill Sans Ultra Bold already registered with windowsFonts().
## Gill Sans Ultra Bold Condensed already registered with windowsFonts().
## Gill Sans MT already registered with windowsFonts().
## Gill Sans MT Condensed already registered with windowsFonts().
## Gill Sans MT Ext Condensed Bold already registered with windowsFonts().
## Gloucester MT Extra Condensed already registered with windowsFonts().
## Goudy Old Style already registered with windowsFonts().
## Goudy Stout already registered with windowsFonts().
## Haettenschweiler already registered with windowsFonts().
## Harlow Solid Italic already registered with windowsFonts().
## Harrington already registered with windowsFonts().
## High Tower Text already registered with windowsFonts().
## HoloLens MDL2 Assets already registered with windowsFonts().
## Impact already registered with windowsFonts().
## Imprint MT Shadow already registered with windowsFonts().
## Informal Roman already registered with windowsFonts().
## Ink Free already registered with windowsFonts().
## Javanese Text already registered with windowsFonts().
## Jokerman already registered with windowsFonts().
## Juice ITC already registered with windowsFonts().
## Kristen ITC already registered with windowsFonts().
## Kunstler Script already registered with windowsFonts().
## Wide Latin already registered with windowsFonts().
## Leelawadee already registered with windowsFonts().
## Leelawadee UI already registered with windowsFonts().
## Leelawadee UI Semilight already registered with windowsFonts().
## Lucida Bright already registered with windowsFonts().
## Lucida Calligraphy already registered with windowsFonts().
## Lucida Console already registered with windowsFonts().
## Lucida Fax already registered with windowsFonts().
## Lucida Handwriting already registered with windowsFonts().
## Lucida Sans already registered with windowsFonts().
## Lucida Sans Typewriter already registered with windowsFonts().
## Lucida Sans Unicode already registered with windowsFonts().
## Magneto already registered with windowsFonts().
## Maiandra GD already registered with windowsFonts().
## Malgun Gothic already registered with windowsFonts().
## Malgun Gothic Semilight already registered with windowsFonts().
## Marlett already registered with windowsFonts().
## Matura MT Script Capitals already registered with windowsFonts().
## Microsoft Himalaya already registered with windowsFonts().
## Microsoft Yi Baiti already registered with windowsFonts().
## Microsoft New Tai Lue already registered with windowsFonts().
## Microsoft PhagsPa already registered with windowsFonts().
## Microsoft Sans Serif already registered with windowsFonts().
## Microsoft Tai Le already registered with windowsFonts().
## Microsoft Uighur already registered with windowsFonts().
## Mistral already registered with windowsFonts().
## Modern No. 20 already registered with windowsFonts().
## Mongolian Baiti already registered with windowsFonts().
## Monotype Corsiva already registered with windowsFonts().
## MS Outlook already registered with windowsFonts().
## MS Reference Sans Serif already registered with windowsFonts().
## MS Reference Specialty already registered with windowsFonts().
## MT Extra already registered with windowsFonts().
## MV Boli already registered with windowsFonts().
## Myanmar Text already registered with windowsFonts().
## Niagara Engraved already registered with windowsFonts().
## Niagara Solid already registered with windowsFonts().
## Nirmala UI already registered with windowsFonts().
## Nirmala UI Semilight already registered with windowsFonts().
## OCR A Extended already registered with windowsFonts().
## Old English Text MT already registered with windowsFonts().
## Onyx already registered with windowsFonts().
## Palace Script MT already registered with windowsFonts().
## Palatino Linotype already registered with windowsFonts().
## Papyrus already registered with windowsFonts().
## Parchment already registered with windowsFonts().
## Perpetua already registered with windowsFonts().
## Perpetua Titling MT already registered with windowsFonts().
## Playbill already registered with windowsFonts().
## Poor Richard already registered with windowsFonts().
## Pristina already registered with windowsFonts().
## Rage Italic already registered with windowsFonts().
## Ravie already registered with windowsFonts().
## Rockwell already registered with windowsFonts().
## Rockwell Condensed already registered with windowsFonts().
## Rockwell Extra Bold already registered with windowsFonts().
## Sans Serif Collection already registered with windowsFonts().
## Script MT Bold already registered with windowsFonts().
## Segoe Fluent Icons already registered with windowsFonts().
## Segoe MDL2 Assets already registered with windowsFonts().
## Segoe Print already registered with windowsFonts().
## Segoe Script already registered with windowsFonts().
## Segoe UI already registered with windowsFonts().
## Segoe UI Light already registered with windowsFonts().
## Segoe UI Semibold already registered with windowsFonts().
## Segoe UI Semilight already registered with windowsFonts().
## Segoe UI Black already registered with windowsFonts().
## Segoe UI Emoji already registered with windowsFonts().
## Segoe UI Historic already registered with windowsFonts().
## Segoe UI Symbol already registered with windowsFonts().
## Segoe UI Variable already registered with windowsFonts().
## Showcard Gothic already registered with windowsFonts().
## SimSun-ExtB already registered with windowsFonts().
## SimSun-ExtG already registered with windowsFonts().
## Sitka Text already registered with windowsFonts().
## Snap ITC already registered with windowsFonts().
## Stencil already registered with windowsFonts().
## Sylfaen already registered with windowsFonts().
## Symbol already registered with windowsFonts().
## System Detect already registered with windowsFonts().
## Tahoma already registered with windowsFonts().
## Tempus Sans ITC already registered with windowsFonts().
## Times New Roman already registered with windowsFonts().
## Trebuchet MS already registered with windowsFonts().
## Tw Cen MT already registered with windowsFonts().
## Tw Cen MT Condensed already registered with windowsFonts().
## Tw Cen MT Condensed Extra Bold already registered with windowsFonts().
## Verdana already registered with windowsFonts().
## Viner Hand ITC already registered with windowsFonts().
## Vivaldi already registered with windowsFonts().
## Vladimir Script already registered with windowsFonts().
## Webdings already registered with windowsFonts().
## Wingdings already registered with windowsFonts().
## Wingdings 2 already registered with windowsFonts().
## Wingdings 3 already registered with windowsFonts().
## Work Sans already registered with windowsFonts().
## Open Sans already registered with windowsFonts().
## Open Sans ExtraBold already registered with windowsFonts().
## Open Sans Light already registered with windowsFonts().
## Open Sans SemiBold already registered with windowsFonts().
## Roboto Black already registered with windowsFonts().
## Roboto already registered with windowsFonts().
## Roboto Light already registered with windowsFonts().
## Roboto Medium already registered with windowsFonts().
## Roboto Thin already registered with windowsFonts().
```

``` r
theme_set(
    theme_minimal() +
    theme(text = element_text(family = "Roboto"))
    )

col_theme <- c("#FF3200", "#011A51", "#FBA600", "#F8C1A6", "#B3E0BF",
"#2A9D3D", "#EDF181", "#DB7003", "#A30000", "#D1AAC2",
"#A5506D", "#97D1D9", "#916C37", "#FF5733", "#FF0066", 
"#328C97", "#33FF57", "#3357FF", "#FF33A1", "#A133FF")
```


``` r
diwali <- read.csv("Diwali Sales Data.csv")
glimpse(diwali)
```

```
## Rows: 11,251
## Columns: 15
## $ User_ID          <int> 1002903, 1000732, 1001990, 1001425, 1000588, 1000588,…
## $ Cust_name        <chr> "Sanskriti", "Kartik", "Bindu", "Sudevi", "Joni", "Jo…
## $ Product_ID       <chr> "P00125942", "P00110942", "P00118542", "P00237842", "…
## $ Gender           <chr> "F", "F", "F", "M", "M", "M", "F", "F", "M", "F", "M"…
## $ Age.Group        <chr> "26-35", "26-35", "26-35", "0-17", "26-35", "26-35", …
## $ Age              <int> 28, 35, 35, 16, 28, 28, 25, 61, 35, 26, 34, 20, 20, 2…
## $ Marital_Status   <int> 0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 1, 0,…
## $ State            <chr> "Maharashtra", "Andhra\xa0Pradesh", "Uttar Pradesh", …
## $ Zone             <chr> "Western", "Southern", "Central", "Southern", "Wester…
## $ Occupation       <chr> "Healthcare", "Govt", "Automobile", "Construction", "…
## $ Product_Category <chr> "Auto", "Auto", "Auto", "Auto", "Auto", "Auto", "Auto…
## $ Orders           <int> 1, 3, 3, 2, 2, 1, 4, 1, 2, 4, 1, 2, 2, 4, 3, 2, 3, 1,…
## $ Amount           <dbl> 23952.00, 23934.00, 23924.00, 23912.00, 23877.00, 238…
## $ Status           <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ unnamed1         <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
```
### Data Cleaning
- Renaming Andhra\\xa0Pradesh to Andhra Pradesh.
- Converting Marital_Status column values to a factor.
    - This would help with clear color differentiation for 1 (as red) and 2 (as blue), instead of a color range of values between 0 and 1.
- Removing columns that contain only NA values.
- Getting rid of rows where there aren't any values recorded for the Amount column.


``` r
diwali <- diwali |> 
    mutate(State = recode(State, "Andhra\xa0Pradesh" = "Andhra Pradesh"), .keep = "all" ) |> 
    mutate(Marital_Status = as.factor(Marital_Status)) |> 
    select(-Status, -unnamed1) |> 
    filter(!is.na(Amount))

glimpse(diwali)
```

```
## Rows: 11,239
## Columns: 13
## $ User_ID          <int> 1002903, 1000732, 1001990, 1001425, 1000588, 1000588,…
## $ Cust_name        <chr> "Sanskriti", "Kartik", "Bindu", "Sudevi", "Joni", "Jo…
## $ Product_ID       <chr> "P00125942", "P00110942", "P00118542", "P00237842", "…
## $ Gender           <chr> "F", "F", "F", "M", "M", "M", "F", "M", "F", "M", "F"…
## $ Age.Group        <chr> "26-35", "26-35", "26-35", "0-17", "26-35", "26-35", …
## $ Age              <int> 28, 35, 35, 16, 28, 28, 25, 35, 26, 34, 20, 20, 26, 2…
## $ Marital_Status   <fct> 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 1,…
## $ State            <chr> "Maharashtra", "Andhra Pradesh", "Uttar Pradesh", "Ka…
## $ Zone             <chr> "Western", "Southern", "Central", "Southern", "Wester…
## $ Occupation       <chr> "Healthcare", "Govt", "Automobile", "Construction", "…
## $ Product_Category <chr> "Auto", "Auto", "Auto", "Auto", "Auto", "Auto", "Auto…
## $ Orders           <int> 1, 3, 3, 2, 2, 1, 4, 2, 4, 1, 2, 2, 4, 2, 1, 1, 1, 4,…
## $ Amount           <dbl> 23952.00, 23934.00, 23924.00, 23912.00, 23877.00, 238…
```

</br>
</br>

### What age group spent the most?

``` r
diwali |> 
    select(Age.Group, Amount) |> 
    arrange(Age.Group) |> 
    group_by(Age.Group) |> 
    summarise(Total_Amount = sum(Amount)) |> 
    ggplot(aes(Age.Group, Total_Amount, fill = Age.Group)) + 
    geom_col() +
    scale_fill_manual(values = col_theme) +
    labs(
        title = "Top-Spending Age Group",
        subtitle = "Identifying High-Value Demographics",
        x = NULL,
        y = "Expenditure"
            ) +
    theme(plot.title = element_text(size = 18, face = "bold")) +
    scale_y_continuous(labels = comma) +
    coord_flip()
```

<img src="/portfolio/Diwali Sales/diwali_sales_files/figure-html/unnamed-chunk-4-1.png" width="672" />
</br>
</br>

### What is the expenditure across genders for each age group

``` r
diwali |> 
    select(Age.Group, Gender, Amount) |> 
    arrange(Age.Group) |> 
    group_by(Age.Group, Gender) |> 
    summarise(Total_Amount = sum(Amount), .groups = "drop") |> 
    ggplot(aes(Age.Group, Total_Amount, fill = Gender)) + 
    geom_col() +
    scale_fill_manual(values = col_theme) +
    labs(
        title = "Spending Patterns by Age Group and Gender",
        subtitle = "A Comparative Analysis",
        x = NULL,
        y = "Expenditure"
            ) +
    theme(plot.title = element_text(size = 18, face = "bold")) +
    scale_y_continuous(labels = comma) +
    coord_flip()
```

<img src="/portfolio/Diwali Sales/diwali_sales_files/figure-html/unnamed-chunk-5-1.png" width="672" />
</br>
</br>

### What are the occupations of people between 26-35

``` r
diwali |> 
    select(Age.Group, Occupation) |> 
    filter(Age.Group == "26-35") |> 
    group_by(Occupation) |> 
    summarise(Number_Of_Individuals = n()) |> 
    mutate(Occupation = fct_reorder(Occupation, Number_Of_Individuals)) |> 
    ggplot(aes(Occupation, Number_Of_Individuals, fill = Occupation)) +
    geom_col() +
    scale_fill_manual(values = col_theme) +
    labs(
        title = "Key Occupations Among 26-35 Age Group",
        x = NULL,
        y = "Number of Individuals"
            ) +
    theme(plot.title = element_text(size = 18, face = "bold")) +
    scale_y_continuous(labels = comma) +
    coord_flip()
```

<img src="/portfolio/Diwali Sales/diwali_sales_files/figure-html/unnamed-chunk-6-1.png" width="672" />
</br>
</br>

### Which state spent the most (segregated by gender)?

``` r
diwali |> 
    select(State, Amount, Gender) |> 
    group_by(State, Gender) |> 
    summarise(Total_Amount = sum(Amount), .groups = "drop") |> 
    mutate(State = fct_reorder(State, Total_Amount)) |> 
    ggplot(aes(State, Total_Amount, fill = Gender)) +
    geom_col() +
    scale_fill_manual(values = col_theme) +
    labs(
        title = "Top-Spending States by Gender",
        subtitle = "Regional Breakdown of Consumer Spending",
        x = NULL,
        y = "Expenditure"
            ) +
    theme(plot.title = element_text(size = 18, face = "bold")) +
    scale_y_continuous(labels = comma) +
    coord_flip()
```

<img src="/portfolio/Diwali Sales/diwali_sales_files/figure-html/unnamed-chunk-7-1.png" width="672" />
</br>
</br>


### Finding expenditure of females and males along with thier marital status

``` r
diwali |> 
    select(State, Marital_Status, Gender, Amount) |>
    arrange(State, Gender) |> 
    group_by(State, Gender, Marital_Status) |> 
    summarise(Total_Amount = sum(Amount), .groups = "drop") |> 
    mutate(State = fct_reorder(State, Total_Amount)) |> 
    ggplot(aes(State, Total_Amount, fill = Marital_Status)) +
    geom_bar(stat = "identity", position = "dodge") +
    labs(title = "Comapring expenditures between married and unmarried individuals", 
         subtitle = "Segregated by gender") +
    facet_wrap(~ Gender) +
    coord_flip() +
    theme(axis.text.x = element_text(angle = 45, hjust = 1, size = 10)) +
    scale_fill_manual(values = col_theme) +
    labs(
        title = "Spending Trends by Gender and Marital Status",
        subtitle = "Understanding Consumer Profiles",
        x = NULL,
        y = "Expenditure"
            ) +
    theme(plot.title = element_text(size = 18, face = "bold")) +
    scale_y_continuous(labels = comma)
```

<img src="/portfolio/Diwali Sales/diwali_sales_files/figure-html/unnamed-chunk-8-1.png" width="672" />



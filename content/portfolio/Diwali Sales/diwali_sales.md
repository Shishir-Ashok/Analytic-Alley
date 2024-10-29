---
title: Diwali Sales Data
date: 2024-10-29
authors:
  - name: Shishir Ashok
    link: https://github.com/shishir-ashok
    image: https://github.com/shishir-ashok.png
tags:
  - Markdown
  - Data Analysis
  - R
---

## Exploring Diwali Sales Data

### What

- The Diwali Sales dataset, featured in the [TidyTuesday project](https://github.com/rfordatascience/tidytuesday/blob/master/data/2023/2023-11-14/readme.md) for the week of November 14, 2023, provides a comprehensive look at retail sales during the Diwali festival in India. It offers a detailed snapshot of consumer behavior during one of the most significant festive periods in India.

### Why

- Diwali, the Festival of Lights, is a major celebration in India, marked by the exchange of gifts, new purchases, and festive decorations. Retailers experience a significant surge in sales during this period, making it a critical time for businesses. Analyzing this dataset can provide valuable insights into:

    - **Consumer Behavior**: Understanding how different demographics engage in festive shopping.
    - **Market Trends**: Identifying spending patterns.

- For me, this dataset offers a practical example of real-world data analysis, enhancing skills in data wrangling, visualization, and interpretation.

### How

- To analyze this dataset effectively, I've followed these steps:

    1. **Data Preparation**: Load the dataset and clean it by handling missing values, correcting data types, and filtering out irrelevant information.
    2. **Exploratory Data Analysis (EDA)**: Use summary statistics and visualizations to understand the distribution of data, identify patterns, and detect anomalies.
    3. **Segmentation Analysis**: Segment the data by different attributes such as age group, gender, marital status, and geographic zone to uncover specific trends.
    4. **Visualization**: Create visualizations using tools like `ggplot2` in R to illustrate key findings. For example, bar plots to show spending by state, or pie charts to display the distribution of product categories.
    5. **Insights and Reporting**: Summarize the findings in a report or blog post, highlighting the most interesting insights and their implications for businesses and consumers.

```
library(tidyverse)
library(scales)
theme_set(theme_minimal())
```

```
diwali <- read.csv('Diwali Sales Data.csv')

glimpse(diwali)
```

```
OUTPUT

Rows: 11,251
Columns: 15
$ User_ID          <int> 1002903, 1000732, 1001990, 1001425, 1000588, 1000588, 1001132, 1002092, 1003…
$ Cust_name        <chr> "Sanskriti", "Kartik", "Bindu", "Sudevi", "Joni", "Joni", "Balk", "Shivangi"…
$ Product_ID       <chr> "P00125942", "P00110942", "P00118542", "P00237842", "P00057942", "P00057942"…
$ Gender           <chr> "F", "F", "F", "M", "M", "M", "F", "F", "M", "F", "M", "F", "F", "M", "M", "…
$ Age.Group        <chr> "26-35", "26-35", "26-35", "0-17", "26-35", "26-35", "18-25", "55+", "26-35"…
$ Age              <int> 28, 35, 35, 16, 28, 28, 25, 61, 35, 26, 34, 20, 20, 26, 46, 24, 48, 29, 54, …
$ Marital_Status   <int> 0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0…
$ State            <chr> "Maharashtra", "Andhra\xa0Pradesh", "Uttar Pradesh", "Karnataka", "Gujarat",…
$ Zone             <chr> "Western", "Southern", "Central", "Southern", "Western", "Northern", "Centra…
$ Occupation       <chr> "Healthcare", "Govt", "Automobile", "Construction", "Food Processing", "Food…
$ Product_Category <chr> "Auto", "Auto", "Auto", "Auto", "Auto", "Auto", "Auto", "Auto", "Auto", "Aut…
$ Orders           <int> 1, 3, 3, 2, 2, 1, 4, 1, 2, 4, 1, 2, 2, 4, 3, 2, 3, 1, 1, 1, 4, 2, 1, 2, 3, 3…
$ Amount           <dbl> 23952.00, 23934.00, 23924.00, 23912.00, 23877.00, 23877.00, 23841.00, NA, 23…
$ Status           <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
$ unnamed1         <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
```

</br>

### Data Cleaning
- Renaming Andhra\\xa0Pradesh to Andhra Pradesh.
- Converting Marital_Status column values to a factor.
    - This would help with clear color differentiation for 1 (as red) and 2 (as blue), instead of a color range of values between 0 and 1.
- Removing columns that contain only NA values.
- Getting rid of rows where there aren't any values recorded for the Amount column.
```
diwali <- diwali |> 
    mutate(State = recode(State, "Andhra\xa0Pradesh" = "Andhra Pradesh"), .keep = "all" ) |> 
    mutate(Marital_Status = as.factor(Marital_Status)) |> 
    select(-Status, -unnamed1) |> 
    filter(!is.na(Amount))

glimpse(diwali)
```
```
OUTPUT

Rows: 11,239
Columns: 13
$ User_ID          <int> 1002903, 1000732, 1001990, 1001425, 1000588, 1000588, 1001132, 1003224, 1003…
$ Cust_name        <chr> "Sanskriti", "Kartik", "Bindu", "Sudevi", "Joni", "Joni", "Balk", "Kushal", …
$ Product_ID       <chr> "P00125942", "P00110942", "P00118542", "P00237842", "P00057942", "P00057942"…
$ Gender           <chr> "F", "F", "F", "M", "M", "M", "F", "M", "F", "M", "F", "F", "M", "F", "F", "…
$ Age.Group        <chr> "26-35", "26-35", "26-35", "0-17", "26-35", "26-35", "18-25", "26-35", "26-3…
$ Age              <int> 28, 35, 35, 16, 28, 28, 25, 35, 26, 34, 20, 20, 26, 24, 29, 54, 54, 19, 46, …
$ Marital_Status   <fct> 0, 1, 1, 0, 1, 1, 1, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1…
$ State            <chr> "Maharashtra", "Andhra Pradesh", "Uttar Pradesh", "Karnataka", "Gujarat", "H…
$ Zone             <chr> "Western", "Southern", "Central", "Southern", "Western", "Northern", "Centra…
$ Occupation       <chr> "Healthcare", "Govt", "Automobile", "Construction", "Food Processing", "Food…
$ Product_Category <chr> "Auto", "Auto", "Auto", "Auto", "Auto", "Auto", "Auto", "Auto", "Auto", "Aut…
$ Orders           <int> 1, 3, 3, 2, 2, 1, 4, 2, 4, 1, 2, 2, 4, 2, 1, 1, 1, 4, 2, 1, 2, 3, 3, 3, 4, 2…
$ Amount           <dbl> 23952.00, 23934.00, 23924.00, 23912.00, 23877.00, 23877.00, 23841.00, 23809.…
```


</br>

---

### What age group spent the most?
```
diwali |> 
    select(Age.Group, Amount) |> 
    arrange(Age.Group) |> 
    group_by(Age.Group) |> 
    summarise(Total_Amount = sum(Amount)) |> 
    ggplot(aes(Age.Group, Total_Amount, fill = Age.Group)) + 
    geom_col() +
    scale_y_continuous(labels = comma)
```

![](../../Diwali%20Sales/ageGroup_Expenditure.png)

### What is the expenditure across genders for each age group
```
diwali |> 
    select(Age.Group, Gender, Amount) |> 
    arrange(Age.Group) |> 
    group_by(Age.Group, Gender) |> 
    summarise(Total_Amount = sum(Amount), .groups = "drop") |> 
    ggplot(aes(Age.Group, Total_Amount, fill = Gender)) + 
    geom_col() +
    scale_y_continuous(labels = comma)
```

![](../../Diwali%20Sales/ageGroup_gender.png)

### What are the occupations of people between 26-35
```
diwali |> 
    select(Age.Group, Occupation) |> 
    filter(Age.Group == "26-35") |> 
    group_by(Occupation) |> 
    summarise(Number_Of_Individuals = n()) |> 
    mutate(Occupation = fct_reorder(Occupation, Number_Of_Individuals)) |> 
    ggplot(aes(Occupation, Number_Of_Individuals, fill = Occupation)) +
    geom_col() +
    coord_flip() 
```

![](../../Diwali%20Sales/occupation.png)

### Which state spent the most (segregated by gender)?
```
diwali |> 
    select(State, Amount, Gender) |> 
    group_by(State, Gender) |> 
    summarise(Total_Amount = sum(Amount), .groups = "drop") |> 
    mutate(State = fct_reorder(State, Total_Amount)) |> 
    ggplot(aes(State, Total_Amount, fill = Gender)) +
    geom_col() +
    labs(title = "State-wise Spendings") +
    scale_y_continuous(labels = comma) +
    coord_flip()
```

![](../../Diwali%20Sales/state-wise.png)

### Finding expenditure of females and males along with thier marital status
```
diwali |> 
    select(State, Marital_Status, Gender, Amount) |>
    arrange(State, Gender) |> 
    group_by(State, Gender, Marital_Status) |> 
    summarise(Total_Amount = sum(Amount), .groups = "drop") |> 
    mutate(State = fct_reorder(State, Total_Amount)) |> 
    ggplot(aes(State, Total_Amount, fill = Marital_Status)) +
    geom_bar(stat = "identity", position = "dodge") +
    labs(title = "Comapring state-wise expenditure between married and unmarried individuals", 
         subtitle = "Segregated by gender") +
    facet_wrap(~ Gender) +
    scale_y_continuous(labels = comma) +
    coord_flip() +
    theme(axis.text.x = element_text(angle = 45, hjust = 1, size = 10))
```

![](../../Diwali%20Sales/everything.png)

# Data Frames

## I created two functions:

1. <b>calculate_age()</b>: this function calculates tree ages from the date_planted column. It subtracts date_planted from the current day (today)
2. <b>get_height_range_desc()</b>: this function is to convert height_range_id to actual height range (eg. height_range_id '2' to '20-30 ft')

```{tip}
Functions are useful!
```

```python
# calculate ages from date_planted
def calculate_age(date_planted):
    today = date.today()
    return today.year - date_planted.year - ((today.month, today.day) < (date_planted.month, date_planted.day))

# get height range description from height_range_id
def get_height_range_desc(height_range):
    if height_range == 0:
        return '0-10 ft'
    elif height_range == 1:
        return '10-20 ft'
    elif height_range == 2:
        return '20-30 ft'
    elif height_range == 3:
        return '30-40 ft'
    elif height_range == 4:
        return '40-50 ft'
    elif height_range == 5:
        return '50-60 ft'
    elif height_range == 6:
        return '60-70 ft'
    elif height_range == 7:
        return '70-80 ft'
    elif height_range == 8:
        return '80-90 ft'
    elif height_range == 9:
        return '90-100 ft'
    elif height_range == 10:
        return '100+ ft'
    else:
        return ''
```

## The following work was done before visualizing data:

- Remove columns that will not be used in visualization from the base DataFrame (df).
- Rename columns to be more descriptive (eg. 'on_street' to 'street', 'genus_name' to 'genus')
- Correct data types
- Add calculated fields
    - 'allergen' will be used for the charts in Q1 to separate trees whether they cause allergies or not
    - 'year' will be used for charts that show the number of trees planted each year
    - 'age' will be used in tooltips on maps to show the ages of trees
    - 'height_range' will be used to describe actual height ranges in 'feet'
    - 'st_block' will be used in tooltips on maps to show locations of the trees
- Created two DataFrames
    - year_df: this DataFrame will be used for the first two questions
    - cherry_df: this DataFrame will be used for the last question

```{tip}
Include required fields only to reduce execution time!
```

```python
# remove columns that will not be used for visualization
df = data.drop(columns=['Unnamed: 0','std_street','assigned','street_side_name','civic_number','plant_area','curb','tree_id','cultivar_name','root_barrier'], axis=1)

# rename columns
df.rename(columns={
    'on_street':'street',
    'species_name':'species',
    'neighbourhood_name':'neighbourhood',
    'genus_name':'genus',
    'height_range_id':'height_range'
}, inplace=True)

# convert data type
df['diameter'] = df['diameter'].astype(int)
df['on_street_block'] = df['on_street_block'].astype(str)

# add calculated fields from other columns
df['allergen'] = np.where(df['genus'].isin(['ALNUS','FAGUS','BETULA','TYPHA','CASTANEA','ULMUS','CORYLUS','TSUGA','LARIX','ACER','QUERCUS','POPULUS']), True, False)
df['year'] = pd.DatetimeIndex(df['date_planted']).year
df['year'].fillna(0, inplace=True)
df['year'] = df['year'].astype(int)
df['height_range_desc'] = df['height_range'].apply(get_height_range_desc)
df['st_block'] = df['on_street_block'] + ' ' + df['street']

year_df = df[df['year']!=0]
year_df['age'] = year_df['date_planted'].apply(calculate_age)
year_df['height_range_desc'] = year_df['height_range'].apply(get_height_range_desc)

cherry_df = df.drop(columns=['date_planted','height_range','allergen','year'])
cherry_df = cherry_df[cherry_df['genus']=='PRUNUS']
```

### Math Equations:

```{margin}
Using LaTeX math formulas
```
    
```{math}
:label: eq_label
M
=
\frac{1}{n} 
\sum_{i=1}^{n} m_{i}
=
\frac{m_{1} + m_{2} + \cdots + m_{n}}{n}
```

Equation {eq}`eq_label` is to get the average number of planted trees per year.

```{math}
:label: eq_h
H
=
\frac{1}{n} 
(h_{1} + h_{2} + \cdots + h_{n})
```

Equation {eq}`eq_h` is to get the average height of planted trees.

# Import Data
```{margin}
Data source: <a href="https://raw.githubusercontent.com/UBC-MDS/exploratory-data-viz/main/data/vancouver_trees.csv" target="_blank">vancouver_trees.csv</a> 
```
```{note}
I am using the small data set (5000 rows)
```

```{tip}
When importing the data, used parse_dates for the date_planted column
```

```python
import pandas as pd
import numpy as np
import altair as alt
from datetime import date
#alt.data_transformers.enable('data_server')
alt.data_transformers.disable_max_rows()
pd.options.mode.chained_assignment = None #suppress warning
```

```python
# use small dataset (5000 rows)
url = 'https://raw.githubusercontent.com/UBC-MDS/data_viz_wrangled/main/data/Trees_data_sets/small_vancouver_trees.csv'

# make sure date_planted has a data type "datetime"
data = pd.read_csv(url, parse_dates=[5])
```
---
{"dg-publish":true,"permalink":"/ressurser/snippets/pandas-cheatsheet/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# Pandas Cheatsheet
## importér
```python
import pandas as pd
```

## Les json fra flere filer og slå sammen
```python
dfs = []
for file in os.listdir("Data/Sleep/"):
    dfs.append(pd.read_json(f"Data/Sleep/{file}"))
df = pd.concat(dfs)
```

## Se 10 første entries
```python
df.head()
```

## Hent entry i index 0
```python
df.iloc[0]
```

## Map data gjennom funksjon
```python
def get_minutes(levels, sleep_phase):
	return levels['summary'][sleep_phase]['minutes']
df['deepSleep'] = df.levels.apply(get_minutes, args=('deep'))
```

## Fjern uønskede kolonner
```python
df.drop(columns=([
    "logId", 
    "startTime", 
    "endTime", 
    "duration"
]), inplace=True)
```

## Fjern rader der hvor noen kolonner ikke har verdi
```python
df.dropna(inplace=True)
```

## Lag et lineplot
```python
import matplotlib.pyplot as plt
import seaborn as sns
from pandas.plotting import register_matplotlib_converters

%matplotlib inline

register_matplotlib_converters()

sns.set()

fig, ax = plt.subplots(figsize=(18,10))
sns.lineplot(ax=ax, data=df)
```
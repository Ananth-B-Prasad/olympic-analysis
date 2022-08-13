---
hide:
  - navigation
  - toc
---

# Recognition of Trend

(code taken from reference)

```python
def highlight(nation):

    if nation['Team/NOC'] == 'Japan':
        return ['background-color: #f3f2f1']*6
    else:
        return ['background-color: white']*6


df_21_full[['Rank','Team/NOC','Bronze Medal','Silver Medal','Gold Medal','Total']].iloc[:15].style.set_caption('Medals by Country: Summer Olympic Games sorted by Gold Medals [Top 15]')\
.bar(subset=['Gold Medal'], color='#f0c05a')\
.bar(subset=['Silver Medal'], color='Lightgray')\
.bar(subset=['Bronze Medal'], color='#a97142')\
.hide_index().apply(highlight, axis=1)

```

So, after I ran this code, the following image is shown:
https://imgur.com/P8EZUwc

the same trend is seen in most of the other summer olympics as well.
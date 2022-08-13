---
hide:
  - navigation
  - toc
---

# Background and Related Work

Hosting olympics is an incredible honor for a country to have.

The previous three Olympics were hosted by: Japan, Brazil, and the United Kingdom.

Here's how they have performed.

(code taken from reference)

```python
# Function for lightening colors
# https://www.python-graph-gallery.com/
def adjust_lightness(color, amount=0.5):
    import matplotlib.colors as mc
    import colorsys
    try:
        c = mc.cnames[color]
    except:
        c = color
    c = colorsys.rgb_to_hls(*mc.to_rgb(c))
    return colorsys.hls_to_rgb(c[0], c[1] * amount, c[2])
# Function for smoothing
def gaussian_smooth(x, y, grid, sd):
    weights = np.transpose([stats.norm.pdf(grid, m, sd) for m in x])
    weights = weights / weights.sum(0)
    return (weights * y).sum(1)


#Overall figure
fig, ax = plt.subplots(3,1, figsize=(10, 6),dpi=250, facecolor=background_color)


colors = ['#a97142', 'lightgray', '#f0c05a']

x_ticks = [1940,1950, 1960, 1970, 1980, 1990, 2000, 2010, 2020]

for axes, country in enumerate(['Japan', 'Brazil', 'United Kingdom']):
        # Define & transform data
        stream = df_new.query(f"region = ='{country}'")[['Year','Gold', 'Silver','Bronze']]
        y = [stream['Bronze'].values, stream['Silver'].values, stream['Gold'].values]
        x = np.array(df_new.query(f"region == '{country}'")['Year'])

        grid = np.linspace(x.min(), x.max(), num=250)
        y_smoothed = [gaussian_smooth(x, y_, grid, 1) for y_ in y]
        
        # Build plot
        ax[axes].stackplot(grid, y_smoothed, baseline="sym", colors=colors)

        line = np.array(y_smoothed).sum(0)
        ax[axes].plot(grid, line / 2, lw=1.5, color="white")
        ax[axes].plot(grid, -line / 2, lw=1.5, color="white")
        
        # Label
        ax[axes].text(1938,0,f'{country}',ha='right',va='center',fontfamily='serif',fontweight='bold',color='#323232')
        
        # Axes tweaks & visual changes
        ax[axes].set_ylim(-25, 25)
        ax[axes].set_xlim(1940, 2021)
        ax[axes].set_facecolor(background_color)
        ax[axes].spines[:].set_visible(False)
        ax[axes].yaxis.set_visible(False)
        ax[axes].tick_params(axis=u'both', which=u'both',length=0)
        for x_tick in x_ticks:
                ax[axes].axvline(x_tick, color='#e0e0e0', ls=(0, (1, 3)),lw=1, zorder=10)

for axes in range(0,2):
    ax[axes].xaxis.set_visible(False)

# Hosting Olympics shaded areas

ax[2].tick_params(axis='x', labelsize=6, color='#4d4d4d')
ax[2].set_xticks(x_ticks)
#ax[0].xaxis.tick_top()

ax[0].axvspan(2018,2022, facecolor='#244747',alpha=0.05)
ax[0].axvspan(1962,1966, facecolor='#244747',alpha=0.05)

ax[1].axvspan(2014,2018, facecolor='#244747',alpha=0.05)

ax[2].axvspan(2010,2014, facecolor='#244747',alpha=0.05)
ax[2].axvspan(1946,1950, facecolor='#244747',alpha=0.05)


# Title & Subtitle

fig.text(0,1,"Previous Hosts and their Olympic performances", fontsize=15,fontweight='bold',fontfamily='serif',color='#323232')
fig.text(0,0.95, 'The previous three hosts have all had successful games', fontsize=10,fontweight='bold',fontfamily='sansserif',color='#B73832')
fig.text(0,0.9, 'Hosting', fontsize=10,fontweight='bold',fontfamily='sansserif',color='#244747',alpha=0.25)

##

# Additional Text annotations

TEXTS = [
    {
        "text": 'Tokyo 2021\nJapan had a successful\ngames, winning more gold\nmedals than the previous\ntwo games combined',
        "ax": 0,
        "x": 0.8,
        "y": 1,
        "color": adjust_lightness("#323232", 1.1)
    },

    {
        "text": 'Rio 2016\nNot only did Brazil win\nmore golds than ever before,\nbut these were the first\nSouth American Olympics',
        "ax": 1,
        "x": 0.7,
        "y": 1.02,
        "color": adjust_lightness("#323232", 1.1)
    },
    
        {
        "text": 'London 2012\nThe hosts performed brilliantly,\nwinning the most gold medals\nfor decades, though this was \nbeaten in the very next games',
        "ax": 2,
        "x": 0.6,
        "y": 1.02,
        "color": adjust_lightness("#323232", 1.1)
    }
]

for text_box in TEXTS:
    ax[text_box["ax"]].text(
        x = text_box["x"],
        y = text_box["y"],
        s = text_box["text"], 
        ha="center",
        va="center",
        ma="left",
        fontsize=7.5,fontfamily='serif',
        color=text_box["color"],
        bbox=dict(
            boxstyle="round", 
            facecolor='#244747',alpha=0.05,
            edgecolor=text_box["color"],
            pad=0.6
        ),
        # This transform means we pass (0, 1) coordinates to locate
        # the text block
        transform=ax[text_box["ax"]].transAxes,
        zorder=999
    )
    # This ensures the text is on top of everything
    fig.texts.append(ax[text_box["ax"]].texts.pop())
    
fig.text(0.5, 0.02, "• Visualization by Josh Swords • Inspired by Cédric Scherer •",color='#404040',fontsize=5,ha="center")

fig

##
plt.subplots_adjust(wspace=0, hspace=0)

plt.show()
```

Here's an image of the result to see the trend
https://imgur.com/qiEmjFF


# Hypothesis Testing

## For Z - test 

```python
import pandas as pd
iris = pd.read_csv("Iris.csv")
iris.sample(50)
```

```python
from statsmodels.stats.weightstats import ztest

z, pval = ztest(iris['PetalLengthCm'], value=4.2)
print(z,pval)
```

## For graphs

### Creating binomial distribution
```python
from scipy.stats import binom

lst = []
for i in range(11):
    lst.append(round(binom.pmf(k=i, n=10, p=0.5),5))
```

## Graph 1
```python
import plotly.graph_objects as go

fig = go.Figure(go.Bar(
    y=lst,
    x=[i for i in range(11)]))

fig.update_layout(
    title={
        'text': "Fair Distribution",
        'y':0.9,
        'x':0.5,
        'xanchor': 'center',
        'yanchor': 'top'        
         },
         
    xaxis=dict(
        title='Number of Heads',
        tickmode='linear'),
        
    yaxis=dict(
        title='Probability'))
        
fig.update_layout({
'plot_bgcolor': 'rgba(0, 0, 0, 0)' })

fig.update_traces(marker_color='rgb(158,202,225)', 
                  marker_line_color='rgb(8,48,107)',
                  marker_line_width=2.5, opacity=0.6)

fig.show()
```

## Graph 2
```python
import plotly.graph_objects as go

fig = go.Figure(go.Bar(
    y=lst,
    x=[i for i in range(11)]))

fig.update_layout(
    title={
        'text': "Fair Distribution",
        'y':0.9,
        'x':0.5,
        'xanchor': 'center',
        'yanchor': 'top'},
        
    xaxis=dict(
        title='Number of Heads',
        tickmode='linear'),
        
    yaxis=dict(
        title='Probability'))
        
fig.update_layout({
        'plot_bgcolor': 'rgba(0, 0, 0, 0)'})

fig.update_traces(marker_color='rgb(158,202,225)', 
                  marker_line_color='rgb(8,48,107)',
                  marker_line_width=2.5, opacity=0.6)

fig.add_annotation(
        x=8,
        y=-0.005,
        xref="x",
        yref="y",
        text="Heads",
        showarrow=True,
        font=dict(
            family="Open Sans",
            size=13,
            color="black"
            ),
        align="center",
        arrowhead=2,
        arrowsize=1,
        arrowwidth=2,
        arrowcolor="#636363",
        bordercolor="#c7c7c7",
        borderwidth=2,
        borderpad=4,
        ax=0,
        ay=50,
        opacity=0.8
        )

fig.show()
```

## Graph 3
```python
colors = ['rgb(158,202,225)'] * 11
colors[8],colors[10],colors[9] = 'darkblue', 'darkblue', 'darkblue'

import plotly.graph_objects as go

fig = go.Figure(go.Bar(
    y=lst,
    x=[i for i in range(11)],
    marker_color=colors))

fig.update_layout(
    title={
        'text': "Fair Distribution",
        'y':0.9,
        'x':0.5,
        'xanchor': 'center',
        'yanchor': 'top'},
        
    xaxis=dict(
        title='Number of Heads',
        tickmode='linear'),
        
    yaxis=dict(
        title='Probability')
)
fig.update_layout({
        'plot_bgcolor': 'rgba(0, 0, 0, 0)'})

fig.update_traces(marker_line_color='rgb(8,48,107)',
                  marker_line_width=2.5, 
                  opacity=0.6)

fig.show()
```

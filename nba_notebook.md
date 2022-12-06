```python
###### DO NOT RUN ALL #######
# all nba data excel file will be overwritten
```


```python
from google.colab import drive
drive.mount('/content/drive')
```

    Drive already mounted at /content/drive; to attempt to forcibly remount, call drive.mount("/content/drive", force_remount=True).
    


```python
%cd /content/drive/My Drive/CS 625/project/hw7
```

    /content/drive/My Drive/CS 625/project/hw7
    


```python
!pip install chart_studio
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import chart_studio.plotly as py
import cufflinks as cf
import seaborn as sns
import plotly.express as px
%matplotlib inline

#import plotly.offline
from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
init_notebook_mode(connected=True)
cf.go_offline()

pd.set_option('display.max_columns', None)
pd.set_option('display.max_rows', None)
```

    Looking in indexes: https://pypi.org/simple, https://us-python.pkg.dev/colab-wheels/public/simple/
    Requirement already satisfied: chart_studio in /usr/local/lib/python3.8/dist-packages (1.1.0)
    Requirement already satisfied: six in /usr/local/lib/python3.8/dist-packages (from chart_studio) (1.15.0)
    Requirement already satisfied: plotly in /usr/local/lib/python3.8/dist-packages (from chart_studio) (5.5.0)
    Requirement already satisfied: requests in /usr/local/lib/python3.8/dist-packages (from chart_studio) (2.23.0)
    Requirement already satisfied: retrying>=1.3.3 in /usr/local/lib/python3.8/dist-packages (from chart_studio) (1.3.4)
    Requirement already satisfied: tenacity>=6.2.0 in /usr/local/lib/python3.8/dist-packages (from plotly->chart_studio) (8.1.0)
    Requirement already satisfied: chardet<4,>=3.0.2 in /usr/local/lib/python3.8/dist-packages (from requests->chart_studio) (3.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.8/dist-packages (from requests->chart_studio) (2022.9.24)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /usr/local/lib/python3.8/dist-packages (from requests->chart_studio) (1.24.3)
    Requirement already satisfied: idna<3,>=2.5 in /usr/local/lib/python3.8/dist-packages (from requests->chart_studio) (2.10)
    


<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-2.8.3.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>




<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-2.8.3.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>




```python
nba_21_22 = pd.read_csv('21-22_nba_stats.csv')
nba_21_22 = nba_21_22.drop(labels='Rk',axis=1)
nba_21_22 = nba_21_22.rename(columns={"3P%?": "3P%"})
```


```python
nba_21_22.head()
```





  <div id="df-8dcd83ba-53b9-4d8f-9651-c2c93204cd5d">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Team</th>
      <th>Season</th>
      <th>G</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Miami Heat*</td>
      <td>2021-2022</td>
      <td>82</td>
      <td>242.1</td>
      <td>39.6</td>
      <td>84.8</td>
      <td>0.467</td>
      <td>13.6</td>
      <td>35.8</td>
      <td>0.379</td>
      <td>26.0</td>
      <td>49.0</td>
      <td>0.531</td>
      <td>17.3</td>
      <td>21.4</td>
      <td>0.808</td>
      <td>9.8</td>
      <td>33.9</td>
      <td>43.7</td>
      <td>25.5</td>
      <td>7.4</td>
      <td>3.2</td>
      <td>14.6</td>
      <td>20.5</td>
      <td>110.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Atlanta Hawks*</td>
      <td>2021-2022</td>
      <td>82</td>
      <td>240.3</td>
      <td>41.5</td>
      <td>88.3</td>
      <td>0.470</td>
      <td>12.9</td>
      <td>34.4</td>
      <td>0.374</td>
      <td>28.6</td>
      <td>53.9</td>
      <td>0.531</td>
      <td>18.1</td>
      <td>22.3</td>
      <td>0.812</td>
      <td>10.0</td>
      <td>33.9</td>
      <td>44.0</td>
      <td>24.6</td>
      <td>7.2</td>
      <td>4.2</td>
      <td>11.9</td>
      <td>18.7</td>
      <td>113.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Los Angeles Clippers</td>
      <td>2021-2022</td>
      <td>82</td>
      <td>241.2</td>
      <td>40.1</td>
      <td>87.4</td>
      <td>0.458</td>
      <td>12.8</td>
      <td>34.2</td>
      <td>0.374</td>
      <td>27.3</td>
      <td>53.3</td>
      <td>0.512</td>
      <td>15.5</td>
      <td>19.6</td>
      <td>0.793</td>
      <td>9.1</td>
      <td>34.9</td>
      <td>44.0</td>
      <td>24.0</td>
      <td>7.4</td>
      <td>5.0</td>
      <td>13.7</td>
      <td>18.6</td>
      <td>108.4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Chicago Bulls*</td>
      <td>2021-2022</td>
      <td>82</td>
      <td>240.6</td>
      <td>41.7</td>
      <td>86.9</td>
      <td>0.480</td>
      <td>10.6</td>
      <td>28.8</td>
      <td>0.369</td>
      <td>31.1</td>
      <td>58.1</td>
      <td>0.535</td>
      <td>17.5</td>
      <td>21.5</td>
      <td>0.813</td>
      <td>8.7</td>
      <td>33.7</td>
      <td>42.3</td>
      <td>23.9</td>
      <td>7.1</td>
      <td>4.1</td>
      <td>12.8</td>
      <td>18.8</td>
      <td>111.6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Milwaukee Bucks*</td>
      <td>2021-2022</td>
      <td>82</td>
      <td>240.9</td>
      <td>41.8</td>
      <td>89.4</td>
      <td>0.468</td>
      <td>14.1</td>
      <td>38.4</td>
      <td>0.366</td>
      <td>27.8</td>
      <td>51.0</td>
      <td>0.544</td>
      <td>17.8</td>
      <td>22.9</td>
      <td>0.776</td>
      <td>10.2</td>
      <td>36.5</td>
      <td>46.7</td>
      <td>23.9</td>
      <td>7.6</td>
      <td>4.0</td>
      <td>13.4</td>
      <td>18.2</td>
      <td>115.5</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-8dcd83ba-53b9-4d8f-9651-c2c93204cd5d')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-8dcd83ba-53b9-4d8f-9651-c2c93204cd5d button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-8dcd83ba-53b9-4d8f-9651-c2c93204cd5d');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>





```python
nba_12_13 = pd.read_csv('12-13_nba_stats.csv')
nba_13_14 = pd.read_csv('13-14_nba_stats.csv')
nba_14_15 = pd.read_csv('14-15_nba_stats.csv')
nba_15_16 = pd.read_csv('15-16_nba_stats.csv')
nba_16_17 = pd.read_csv('16-17_nba_stats.csv')
nba_17_18 = pd.read_csv('17-18_nba_stats.csv')
nba_18_19 = pd.read_csv('18-19_nba_stats.csv')
nba_19_20 = pd.read_csv('19-20_nba_stats.csv')
nba_20_21 = pd.read_csv('20-21_nba_stats.csv')
```


```python
last_10_seasons = [nba_12_13, nba_13_14, nba_14_15, nba_15_16, nba_16_17, nba_17_18, nba_18_19, nba_19_20, nba_20_21, nba_21_22]
```


```python
nba_data = pd.concat(last_10_seasons)
```


```python
nba_data = nba_data.drop(labels='Rk',axis=1)
```

### Print DataFrame to Excel


```python
""" prints DataFrame to Excel
this was used to convert merged individual seasons df to excel file
excel was necessary in order to add w/l data to file
# commented out to prevent overwriting data"""
# nba_data.to_excel('all_nba_data.xlsx', index=False)
```




    ' prints DataFrame to Excel\nthis was used to convert merged individual seasons df to excel file\nexcel was necessary in order to add w/l data to file\n# commented out to prevent overwriting data'




```python
nba_data.head()
```





  <div id="df-f36aa662-5e74-4c1e-abda-ac2c77aa4047">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Team</th>
      <th>Season</th>
      <th>G</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Denver Nuggets*</td>
      <td>2012-2013</td>
      <td>82</td>
      <td>242.7</td>
      <td>40.7</td>
      <td>85.2</td>
      <td>0.478</td>
      <td>6.4</td>
      <td>18.5</td>
      <td>0.343</td>
      <td>34.4</td>
      <td>66.6</td>
      <td>0.516</td>
      <td>18.4</td>
      <td>26.2</td>
      <td>0.701</td>
      <td>13.3</td>
      <td>31.7</td>
      <td>45.0</td>
      <td>24.4</td>
      <td>9.3</td>
      <td>6.5</td>
      <td>15.3</td>
      <td>20.5</td>
      <td>106.1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Houston Rockets*</td>
      <td>2012-2013</td>
      <td>82</td>
      <td>241.2</td>
      <td>38.1</td>
      <td>82.7</td>
      <td>0.461</td>
      <td>10.6</td>
      <td>28.9</td>
      <td>0.366</td>
      <td>27.5</td>
      <td>53.8</td>
      <td>0.511</td>
      <td>19.2</td>
      <td>25.5</td>
      <td>0.754</td>
      <td>11.1</td>
      <td>32.3</td>
      <td>43.4</td>
      <td>23.2</td>
      <td>8.3</td>
      <td>4.4</td>
      <td>16.4</td>
      <td>20.3</td>
      <td>106.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Oklahoma City Thunder*</td>
      <td>2012-2013</td>
      <td>82</td>
      <td>241.8</td>
      <td>38.1</td>
      <td>79.3</td>
      <td>0.481</td>
      <td>7.3</td>
      <td>19.4</td>
      <td>0.377</td>
      <td>30.8</td>
      <td>60.0</td>
      <td>0.514</td>
      <td>22.2</td>
      <td>26.8</td>
      <td>0.828</td>
      <td>10.4</td>
      <td>33.2</td>
      <td>43.6</td>
      <td>21.4</td>
      <td>8.3</td>
      <td>7.6</td>
      <td>15.3</td>
      <td>20.2</td>
      <td>105.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>San Antonio Spurs*</td>
      <td>2012-2013</td>
      <td>82</td>
      <td>242.4</td>
      <td>39.1</td>
      <td>81.4</td>
      <td>0.481</td>
      <td>8.1</td>
      <td>21.5</td>
      <td>0.376</td>
      <td>31.1</td>
      <td>59.9</td>
      <td>0.519</td>
      <td>16.6</td>
      <td>21.0</td>
      <td>0.791</td>
      <td>8.1</td>
      <td>33.2</td>
      <td>41.3</td>
      <td>25.1</td>
      <td>8.5</td>
      <td>5.4</td>
      <td>14.7</td>
      <td>17.4</td>
      <td>103.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Miami Heat*</td>
      <td>2012-2013</td>
      <td>82</td>
      <td>242.4</td>
      <td>38.4</td>
      <td>77.4</td>
      <td>0.496</td>
      <td>8.7</td>
      <td>22.1</td>
      <td>0.396</td>
      <td>29.6</td>
      <td>55.4</td>
      <td>0.536</td>
      <td>17.4</td>
      <td>23.0</td>
      <td>0.754</td>
      <td>8.2</td>
      <td>30.4</td>
      <td>38.6</td>
      <td>23.0</td>
      <td>8.7</td>
      <td>5.4</td>
      <td>13.9</td>
      <td>18.7</td>
      <td>102.9</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-f36aa662-5e74-4c1e-abda-ac2c77aa4047')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-f36aa662-5e74-4c1e-abda-ac2c77aa4047 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-f36aa662-5e74-4c1e-abda-ac2c77aa4047');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>





```python
# dataframe for league averages for last 10 seasons
league_avg = nba_data.loc[nba_data['Team'] == 'League Average']
```


```python
league_avg
```





  <div id="df-8e628115-cb7e-44b4-ae58-e518f17c0eb5">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Team</th>
      <th>Season</th>
      <th>G</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2012-2013</td>
      <td>82</td>
      <td>241.9</td>
      <td>37.1</td>
      <td>82.0</td>
      <td>0.453</td>
      <td>7.2</td>
      <td>20.0</td>
      <td>0.359</td>
      <td>30.0</td>
      <td>62.1</td>
      <td>0.483</td>
      <td>16.7</td>
      <td>22.2</td>
      <td>0.753</td>
      <td>11.2</td>
      <td>31.0</td>
      <td>42.1</td>
      <td>22.1</td>
      <td>7.8</td>
      <td>5.1</td>
      <td>14.6</td>
      <td>19.8</td>
      <td>98.1</td>
    </tr>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2013-2014</td>
      <td>82</td>
      <td>242.0</td>
      <td>37.7</td>
      <td>83.0</td>
      <td>0.454</td>
      <td>7.7</td>
      <td>21.5</td>
      <td>0.360</td>
      <td>30.0</td>
      <td>61.5</td>
      <td>0.488</td>
      <td>17.8</td>
      <td>23.6</td>
      <td>0.756</td>
      <td>10.9</td>
      <td>31.8</td>
      <td>42.7</td>
      <td>22.0</td>
      <td>7.7</td>
      <td>4.7</td>
      <td>14.6</td>
      <td>20.7</td>
      <td>101.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2014-2015</td>
      <td>82</td>
      <td>242.0</td>
      <td>37.5</td>
      <td>83.6</td>
      <td>0.449</td>
      <td>7.8</td>
      <td>22.4</td>
      <td>0.350</td>
      <td>29.7</td>
      <td>61.2</td>
      <td>0.485</td>
      <td>17.1</td>
      <td>22.8</td>
      <td>0.750</td>
      <td>10.9</td>
      <td>32.4</td>
      <td>43.3</td>
      <td>22.0</td>
      <td>7.7</td>
      <td>4.8</td>
      <td>14.4</td>
      <td>20.2</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2015-2016</td>
      <td>82</td>
      <td>241.8</td>
      <td>38.2</td>
      <td>84.6</td>
      <td>0.452</td>
      <td>8.5</td>
      <td>24.1</td>
      <td>0.354</td>
      <td>29.7</td>
      <td>60.5</td>
      <td>0.491</td>
      <td>17.7</td>
      <td>23.4</td>
      <td>0.757</td>
      <td>10.4</td>
      <td>33.3</td>
      <td>43.8</td>
      <td>22.3</td>
      <td>7.8</td>
      <td>5.0</td>
      <td>14.4</td>
      <td>20.3</td>
      <td>102.7</td>
    </tr>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2016-2017</td>
      <td>82</td>
      <td>241.6</td>
      <td>39.0</td>
      <td>85.4</td>
      <td>0.457</td>
      <td>9.7</td>
      <td>27.0</td>
      <td>0.358</td>
      <td>29.4</td>
      <td>58.4</td>
      <td>0.503</td>
      <td>17.8</td>
      <td>23.1</td>
      <td>0.772</td>
      <td>10.1</td>
      <td>33.4</td>
      <td>43.5</td>
      <td>22.6</td>
      <td>7.7</td>
      <td>4.7</td>
      <td>14.0</td>
      <td>19.9</td>
      <td>105.6</td>
    </tr>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2017-2018</td>
      <td>82</td>
      <td>241.4</td>
      <td>39.6</td>
      <td>86.1</td>
      <td>0.460</td>
      <td>10.5</td>
      <td>29.0</td>
      <td>0.362</td>
      <td>29.1</td>
      <td>57.1</td>
      <td>0.510</td>
      <td>16.6</td>
      <td>21.7</td>
      <td>0.767</td>
      <td>9.7</td>
      <td>33.8</td>
      <td>43.5</td>
      <td>23.2</td>
      <td>7.7</td>
      <td>4.8</td>
      <td>14.3</td>
      <td>19.9</td>
      <td>106.3</td>
    </tr>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2018-2019</td>
      <td>82</td>
      <td>241.6</td>
      <td>41.1</td>
      <td>89.2</td>
      <td>0.461</td>
      <td>11.4</td>
      <td>32.0</td>
      <td>0.355</td>
      <td>29.7</td>
      <td>57.2</td>
      <td>0.520</td>
      <td>17.7</td>
      <td>23.1</td>
      <td>0.766</td>
      <td>10.3</td>
      <td>34.8</td>
      <td>45.2</td>
      <td>24.6</td>
      <td>7.6</td>
      <td>5.0</td>
      <td>14.1</td>
      <td>20.9</td>
      <td>111.2</td>
    </tr>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2019-2020</td>
      <td>71</td>
      <td>241.8</td>
      <td>40.9</td>
      <td>88.8</td>
      <td>0.460</td>
      <td>12.2</td>
      <td>34.1</td>
      <td>0.358</td>
      <td>28.7</td>
      <td>54.7</td>
      <td>0.524</td>
      <td>17.9</td>
      <td>23.1</td>
      <td>0.773</td>
      <td>10.1</td>
      <td>34.8</td>
      <td>44.8</td>
      <td>24.4</td>
      <td>7.6</td>
      <td>4.9</td>
      <td>14.5</td>
      <td>20.8</td>
      <td>111.8</td>
    </tr>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2020-2021</td>
      <td>72</td>
      <td>241.4</td>
      <td>41.2</td>
      <td>88.4</td>
      <td>0.466</td>
      <td>12.7</td>
      <td>34.6</td>
      <td>0.367</td>
      <td>28.5</td>
      <td>53.8</td>
      <td>0.530</td>
      <td>17.0</td>
      <td>21.8</td>
      <td>0.778</td>
      <td>9.8</td>
      <td>34.5</td>
      <td>44.3</td>
      <td>24.8</td>
      <td>7.6</td>
      <td>4.9</td>
      <td>13.8</td>
      <td>19.3</td>
      <td>112.1</td>
    </tr>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2021-2022</td>
      <td>82</td>
      <td>241.4</td>
      <td>40.6</td>
      <td>88.1</td>
      <td>0.461</td>
      <td>12.4</td>
      <td>35.2</td>
      <td>0.354</td>
      <td>28.2</td>
      <td>52.9</td>
      <td>0.533</td>
      <td>16.9</td>
      <td>21.9</td>
      <td>0.775</td>
      <td>10.3</td>
      <td>34.1</td>
      <td>44.5</td>
      <td>24.6</td>
      <td>7.6</td>
      <td>4.7</td>
      <td>13.8</td>
      <td>19.6</td>
      <td>110.6</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-8e628115-cb7e-44b4-ae58-e518f17c0eb5')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-8e628115-cb7e-44b4-ae58-e518f17c0eb5 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-8e628115-cb7e-44b4-ae58-e518f17c0eb5');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>




### 3PA Over Last 10 Seasons Line chart


```python

```


```python
season = league_avg['Season']
three_percentage = league_avg['3P%']
three_attempts = league_avg['3PA']

plt.style.use('fivethirtyeight')

plt.plot(season, three_attempts, label='3 Point Attempts')

#plt.legend()

plt.title("3 Point Attempts Last 10 NBA Seasons")
plt.xlabel("Season")
plt.ylabel("3 Point Attempts")
plt.ylim(0, 1.05 * max(three_attempts))
# plt.tick_params(axis='x', pad= .5, labelsize='small', )
# plt.ticklabel_format(useOffset=True)

plt.show()
```


    
![png](output_17_0.png)
    



```python
plt.style.use('fivethirtyeight')

plt.plot(season, three_percentage, label='3 Point %')

#plt.legend()

plt.title("3 Point Percentage Last 10 NBA Seasons")
plt.xlabel("Season")
plt.ylabel("3 Point Percentage")
plt.ylim(0, 1.05 * max(three_percentage))

plt.show()
```


    
![png](output_18_0.png)
    



```python
season = league_avg['Season']
ft_percentage = league_avg['FT%']
ft_attempts = league_avg['FTA']

plt.style.use('fivethirtyeight')

plt.plot(season, ft_attempts, label='Free Throw Attempts')
#plt.plot(season, three_percentage, label='3 Point %')

#plt.legend()

plt.title("Free Throw Attempts Last 10 NBA Seasons")
plt.xlabel("Season")
plt.ylabel("Free Throw Attempts")
plt.ylim(0, 1.05 * max(ft_attempts))

plt.show()
```


    
![png](output_19_0.png)
    



```python
season = league_avg['Season']
ft_percentage = league_avg['FT%']
ft_attempts = league_avg['FTA']

plt.style.use('tableau-colorblind10')

#plt.plot(season, ft_attempts, label='Free Throw Attempts')
plt.plot(season, ft_percentage, label='FT %')

plt.title("Free Throw % Last 10 NBA Seasons")
plt.xlabel("Season")
plt.ylabel("Free Throw %")
plt.ylim(0, 1.05 * max(ft_percentage))
#plt.tick_params(axis='x', pad= .5, labelsize='small')

plt.tight_layout()

plt.show()
```


    
![png](output_20_0.png)
    



```python
season = league_avg['Season']
points = league_avg['PTS']

plt.style.use('ggplot')

plt.plot(season, points)


plt.title("Points per Game Last 10 NBA Seasons")
plt.xlabel("Season")
plt.ylabel("Points")
plt.ylim(0, 1.05 * max(points))
plt.tick_params(axis='x', pad= .5, labelsize='small')

plt.show()
```


    
![png](output_21_0.png)
    



```python
import plotly.graph_objects as go
px.line(league_avg, x='Season', y='PTS', labels={'x': 'Season', 'y': 'Points'})
fig = go.Figure()
# fig.add_trace(go.Scatter(x=league_avg.Season, y=league_avg.PTS,
#                          mode='lines', name='Points'))
fig.add_trace(go.Scatter(x=league_avg.Season, y=league_avg['FG%'],
                         mode='lines', name='FG%'))
fig.add_trace(go.Scatter(x=league_avg.Season, y=league_avg['3P%'],
                         mode='lines', name='3P%'))

fig.update_yaxes(rangemode="tozero")
fig.update_layout(title='NBA Averages per Game Last 10 Seasons',
                  xaxis_title='Season', yaxis_title='Points')
fig.show(renderer='colab')
#fig.write_html("lst_10_seasons.html")
```


<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script src="https://cdn.plot.ly/plotly-2.8.3.min.js"></script>                <div id="54e99aee-9780-4293-bd32-77f3c7a6a0f6" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("54e99aee-9780-4293-bd32-77f3c7a6a0f6")) {                    Plotly.newPlot(                        "54e99aee-9780-4293-bd32-77f3c7a6a0f6",                        [{"mode":"lines","name":"FG%","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[0.453,0.454,0.449,0.452,0.457,0.46,0.461,0.46,0.466,0.461],"type":"scatter"},{"mode":"lines","name":"3P%","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[0.359,0.36,0.35,0.354,0.358,0.362,0.355,0.358,0.367,0.354],"type":"scatter"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"yaxis":{"rangemode":"tozero","title":{"text":"Points"}},"title":{"text":"NBA Averages per Game Last 10 Seasons"},"xaxis":{"title":{"text":"Season"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('54e99aee-9780-4293-bd32-77f3c7a6a0f6');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>



```python
# nba_stand_21_22 = pd.read_excel('21-22_nba_standings.xlsx')
```


```python
# nba_stand_21_22.head()
```

### Win/Loss Data added to nba stats dataframe


```python
# final merged file containing all team stats with win/loss record
nba_data_wl = pd.read_excel('all_nba_data.xlsx')
```


```python
nba_data_wl.head(3)
```





  <div id="df-9e168c7a-2606-4d3a-a8d2-5f7f388dd222">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Team</th>
      <th>Season</th>
      <th>Lookup</th>
      <th>G</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>Wins</th>
      <th>Losses</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Denver Nuggets*</td>
      <td>2012-2013</td>
      <td>2012-2013-Denver Nuggets*</td>
      <td>82</td>
      <td>242.7</td>
      <td>40.7</td>
      <td>85.2</td>
      <td>0.478</td>
      <td>6.4</td>
      <td>18.5</td>
      <td>0.343</td>
      <td>34.4</td>
      <td>66.6</td>
      <td>0.516</td>
      <td>18.4</td>
      <td>26.2</td>
      <td>0.701</td>
      <td>13.3</td>
      <td>31.7</td>
      <td>45.0</td>
      <td>24.4</td>
      <td>9.3</td>
      <td>6.5</td>
      <td>15.3</td>
      <td>20.5</td>
      <td>106.1</td>
      <td>57.0</td>
      <td>25.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Houston Rockets*</td>
      <td>2012-2013</td>
      <td>2012-2013-Houston Rockets*</td>
      <td>82</td>
      <td>241.2</td>
      <td>38.1</td>
      <td>82.7</td>
      <td>0.461</td>
      <td>10.6</td>
      <td>28.9</td>
      <td>0.366</td>
      <td>27.5</td>
      <td>53.8</td>
      <td>0.511</td>
      <td>19.2</td>
      <td>25.5</td>
      <td>0.754</td>
      <td>11.1</td>
      <td>32.3</td>
      <td>43.4</td>
      <td>23.2</td>
      <td>8.3</td>
      <td>4.4</td>
      <td>16.4</td>
      <td>20.3</td>
      <td>106.0</td>
      <td>45.0</td>
      <td>37.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Oklahoma City Thunder*</td>
      <td>2012-2013</td>
      <td>2012-2013-Oklahoma City Thunder*</td>
      <td>82</td>
      <td>241.8</td>
      <td>38.1</td>
      <td>79.3</td>
      <td>0.481</td>
      <td>7.3</td>
      <td>19.4</td>
      <td>0.377</td>
      <td>30.8</td>
      <td>60.0</td>
      <td>0.514</td>
      <td>22.2</td>
      <td>26.8</td>
      <td>0.828</td>
      <td>10.4</td>
      <td>33.2</td>
      <td>43.6</td>
      <td>21.4</td>
      <td>8.3</td>
      <td>7.6</td>
      <td>15.3</td>
      <td>20.2</td>
      <td>105.7</td>
      <td>60.0</td>
      <td>22.0</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-9e168c7a-2606-4d3a-a8d2-5f7f388dd222')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-9e168c7a-2606-4d3a-a8d2-5f7f388dd222 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-9e168c7a-2606-4d3a-a8d2-5f7f388dd222');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>





```python
# selects all league average categories for every season
all_seasons = nba_data_wl.loc[nba_data_wl['Team'] == 'League Average']
```


```python
# selects 3pt and win categories for every season
all_seasons_wins = nba_data_wl[['Season','Team','FGA','FG%','FTA','FT%','3PA','3P%','TRB','AST','PTS','Wins']]
```


```python
# filters playoff teams and creates dataframe for 2013-2014 season
playoffs_2014 = all_seasons_wins[(all_seasons_wins['Team'].str.contains("\*")) & (all_seasons_wins['Season'] == '2013-2014')]
playoffs_2014.head(20)
```





  <div id="df-c4da4034-1d22-41ba-a74f-8293e5045622">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Season</th>
      <th>Team</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>TRB</th>
      <th>AST</th>
      <th>PTS</th>
      <th>Wins</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>31</th>
      <td>2013-2014</td>
      <td>Los Angeles Clippers*</td>
      <td>82.5</td>
      <td>0.474</td>
      <td>29.1</td>
      <td>0.730</td>
      <td>24.0</td>
      <td>0.352</td>
      <td>43.0</td>
      <td>24.6</td>
      <td>107.9</td>
      <td>57.0</td>
    </tr>
    <tr>
      <th>32</th>
      <td>2013-2014</td>
      <td>Houston Rockets*</td>
      <td>80.5</td>
      <td>0.472</td>
      <td>31.1</td>
      <td>0.712</td>
      <td>26.6</td>
      <td>0.358</td>
      <td>45.3</td>
      <td>21.4</td>
      <td>107.7</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>34</th>
      <td>2013-2014</td>
      <td>Portland Trail Blazers*</td>
      <td>87.0</td>
      <td>0.450</td>
      <td>23.5</td>
      <td>0.815</td>
      <td>25.3</td>
      <td>0.372</td>
      <td>46.4</td>
      <td>23.2</td>
      <td>106.7</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>2013-2014</td>
      <td>Oklahoma City Thunder*</td>
      <td>82.7</td>
      <td>0.471</td>
      <td>25.0</td>
      <td>0.806</td>
      <td>22.4</td>
      <td>0.361</td>
      <td>44.7</td>
      <td>21.9</td>
      <td>106.2</td>
      <td>59.0</td>
    </tr>
    <tr>
      <th>36</th>
      <td>2013-2014</td>
      <td>San Antonio Spurs*</td>
      <td>83.5</td>
      <td>0.486</td>
      <td>20.0</td>
      <td>0.785</td>
      <td>21.4</td>
      <td>0.397</td>
      <td>43.3</td>
      <td>25.2</td>
      <td>105.4</td>
      <td>62.0</td>
    </tr>
    <tr>
      <th>38</th>
      <td>2013-2014</td>
      <td>Dallas Mavericks*</td>
      <td>83.6</td>
      <td>0.474</td>
      <td>21.1</td>
      <td>0.795</td>
      <td>22.9</td>
      <td>0.384</td>
      <td>40.9</td>
      <td>23.6</td>
      <td>104.8</td>
      <td>49.0</td>
    </tr>
    <tr>
      <th>40</th>
      <td>2013-2014</td>
      <td>Golden State Warriors*</td>
      <td>85.4</td>
      <td>0.462</td>
      <td>21.1</td>
      <td>0.753</td>
      <td>24.8</td>
      <td>0.380</td>
      <td>45.3</td>
      <td>23.3</td>
      <td>104.3</td>
      <td>51.0</td>
    </tr>
    <tr>
      <th>42</th>
      <td>2013-2014</td>
      <td>Miami Heat*</td>
      <td>76.5</td>
      <td>0.501</td>
      <td>23.0</td>
      <td>0.760</td>
      <td>22.3</td>
      <td>0.364</td>
      <td>36.9</td>
      <td>22.5</td>
      <td>102.2</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>43</th>
      <td>2013-2014</td>
      <td>Toronto Raptors*</td>
      <td>81.9</td>
      <td>0.445</td>
      <td>25.1</td>
      <td>0.782</td>
      <td>23.4</td>
      <td>0.372</td>
      <td>42.5</td>
      <td>21.2</td>
      <td>101.3</td>
      <td>48.0</td>
    </tr>
    <tr>
      <th>44</th>
      <td>2013-2014</td>
      <td>Atlanta Hawks*</td>
      <td>81.6</td>
      <td>0.458</td>
      <td>21.7</td>
      <td>0.781</td>
      <td>25.8</td>
      <td>0.363</td>
      <td>40.0</td>
      <td>24.9</td>
      <td>101.0</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>46</th>
      <td>2013-2014</td>
      <td>Washington Wizards*</td>
      <td>84.4</td>
      <td>0.459</td>
      <td>20.9</td>
      <td>0.731</td>
      <td>20.8</td>
      <td>0.380</td>
      <td>42.2</td>
      <td>23.3</td>
      <td>100.7</td>
      <td>44.0</td>
    </tr>
    <tr>
      <th>51</th>
      <td>2013-2014</td>
      <td>Brooklyn Nets*</td>
      <td>77.9</td>
      <td>0.459</td>
      <td>24.4</td>
      <td>0.753</td>
      <td>23.4</td>
      <td>0.369</td>
      <td>38.1</td>
      <td>20.9</td>
      <td>98.5</td>
      <td>44.0</td>
    </tr>
    <tr>
      <th>53</th>
      <td>2013-2014</td>
      <td>Charlotte Bobcats*</td>
      <td>82.1</td>
      <td>0.442</td>
      <td>24.4</td>
      <td>0.737</td>
      <td>17.9</td>
      <td>0.351</td>
      <td>42.7</td>
      <td>21.7</td>
      <td>96.9</td>
      <td>43.0</td>
    </tr>
    <tr>
      <th>54</th>
      <td>2013-2014</td>
      <td>Indiana Pacers*</td>
      <td>80.2</td>
      <td>0.449</td>
      <td>23.3</td>
      <td>0.779</td>
      <td>18.8</td>
      <td>0.357</td>
      <td>44.7</td>
      <td>20.1</td>
      <td>96.7</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>57</th>
      <td>2013-2014</td>
      <td>Memphis Grizzlies*</td>
      <td>82.0</td>
      <td>0.464</td>
      <td>20.3</td>
      <td>0.741</td>
      <td>14.0</td>
      <td>0.353</td>
      <td>42.4</td>
      <td>21.9</td>
      <td>96.1</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>60</th>
      <td>2013-2014</td>
      <td>Chicago Bulls*</td>
      <td>80.2</td>
      <td>0.432</td>
      <td>23.3</td>
      <td>0.779</td>
      <td>17.8</td>
      <td>0.348</td>
      <td>44.1</td>
      <td>22.7</td>
      <td>93.7</td>
      <td>48.0</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-c4da4034-1d22-41ba-a74f-8293e5045622')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-c4da4034-1d22-41ba-a74f-8293e5045622 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-c4da4034-1d22-41ba-a74f-8293e5045622');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>





```python
# dataframe for 2013-2014 league averages
league_avg_14 = league_avg.loc[league_avg['Season'] == '2013-2014']
```


```python
league_avg_14.head()
```





  <div id="df-3dbc7339-ec5b-4183-b3c4-7eb3cbae6fe4">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Team</th>
      <th>Season</th>
      <th>G</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>30</th>
      <td>League Average</td>
      <td>2013-2014</td>
      <td>82</td>
      <td>242.0</td>
      <td>37.7</td>
      <td>83.0</td>
      <td>0.454</td>
      <td>7.7</td>
      <td>21.5</td>
      <td>0.36</td>
      <td>30.0</td>
      <td>61.5</td>
      <td>0.488</td>
      <td>17.8</td>
      <td>23.6</td>
      <td>0.756</td>
      <td>10.9</td>
      <td>31.8</td>
      <td>42.7</td>
      <td>22.0</td>
      <td>7.7</td>
      <td>4.7</td>
      <td>14.6</td>
      <td>20.7</td>
      <td>101.0</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-3dbc7339-ec5b-4183-b3c4-7eb3cbae6fe4')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-3dbc7339-ec5b-4183-b3c4-7eb3cbae6fe4 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-3dbc7339-ec5b-4183-b3c4-7eb3cbae6fe4');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>




### Free Throw Attempts per Game


```python
# creates labels with team names for horizontal bar charts
labels = playoffs_2014['Team'].to_list()
labels
```




    ['Los Angeles Clippers*',
     'Houston Rockets*',
     'Portland Trail Blazers*',
     'Oklahoma City Thunder*',
     'San Antonio Spurs*',
     'Dallas Mavericks*',
     'Golden State Warriors*',
     'Miami Heat*',
     'Toronto Raptors*',
     'Atlanta Hawks*',
     'Washington Wizards*',
     'Brooklyn Nets*',
     'Charlotte Bobcats*',
     'Indiana Pacers*',
     'Memphis Grizzlies*',
     'Chicago Bulls*']




```python
# list of plot styles
print(plt.style.available)
```

    ['Solarize_Light2', '_classic_test_patch', 'bmh', 'classic', 'dark_background', 'fast', 'fivethirtyeight', 'ggplot', 'grayscale', 'seaborn', 'seaborn-bright', 'seaborn-colorblind', 'seaborn-dark', 'seaborn-dark-palette', 'seaborn-darkgrid', 'seaborn-deep', 'seaborn-muted', 'seaborn-notebook', 'seaborn-paper', 'seaborn-pastel', 'seaborn-poster', 'seaborn-talk', 'seaborn-ticks', 'seaborn-white', 'seaborn-whitegrid', 'tableau-colorblind10']
    

### Free Throw Attempts per Game


```python
wins_2014 = playoffs_2014['Wins']
season = playoffs_2014['Season']
team = playoffs_2014['Team']

nba_ft_percent = league_avg_14['FT%']
nba_fta = league_avg_14['FTA']
playoff_tm_fta = playoffs_2014['FTA']
playoff_tm_ft_percent = playoffs_2014['FT%']

# creates grouped barcharts
labels = labels
x_indexes = np.arange(len(labels))  # the label locations
width = 0.25  # the width of the bars

plt.style.use('tableau-colorblind10')

# horizontal grouped bar charts
plt.barh(x_indexes, playoff_tm_fta, width, label='Playoff Team')
plt.barh(x_indexes - width, nba_fta, width, label='League Avg')

plt.legend(loc='center left', bbox_to_anchor=(1, 0.5))
plt.yticks(ticks=x_indexes, labels=labels)

############ Remember to update title and labels when changing data ################
plt.title("Total Free Throw Attempts per Game (Playoff Teams 2014 Season)")
plt.ylabel("Team")
plt.xlabel("Free Throw Attempts")

plt.show()
```


    
![png](output_37_0.png)
    



```python
wins_2014 = playoffs_2014['Wins']
season = playoffs_2014['Season']
team = playoffs_2014['Team']

nba_ft_percent = league_avg_14['FT%']
nba_fta = league_avg_14['FTA']
playoff_tm_fta = playoffs_2014['FTA']
playoff_tm_ft_percent = playoffs_2014['FT%']

# grouped bar chart settings
labels = labels
x_indexes = np.arange(len(labels))  # the label locations
width = 0.25  # the width of the bars

plt.style.use('tableau-colorblind10')

# horizontal grouped bar charts
plt.barh(x_indexes, playoff_tm_ft_percent, width, label='Playoff Team')
plt.barh(x_indexes - width, nba_ft_percent, width, label='League Avg')

plt.legend(loc='center left', bbox_to_anchor=(1, 0.5))
plt.yticks(ticks=x_indexes, labels=labels)

############ Remember to update title and labels when changing data ################
plt.title("Free Throw % per Game (Playoff Teams 2014 Season)")
plt.ylabel("Team")
plt.xlabel("Free Throw %")

plt.show()
```


    
![png](output_38_0.png)
    


### 3pt% per Game


```python
wins_2014 = playoffs_2014['Wins']
season = playoffs_2014['Season']
team = playoffs_2014['Team']

nba_ft_percent = league_avg_14['FT%']
nba_fta = league_avg_14['FTA']
playoff_tm_fta = playoffs_2014['FTA']
playoff_tm_ft_percent = playoffs_2014['FT%']

nba_3pta =league_avg_14['3PA']
nba_3pt_prcnt =league_avg_14['3P%']
playoff_tm_3pta = playoffs_2014['3PA']
playoff_tm_3pt_prcnt = playoffs_2014['3P%']

# grouped bar chart settings
labels = labels
x_indexes = np.arange(len(labels))  # the label locations
width = 0.20  # the width of the bars

x_tick = playoffs_2014['3P%'].to_list()
y = wins_2014

plt.style.use('seaborn')

plt.barh(x_indexes, playoff_tm_3pt_prcnt, width, label='Playoff Team')
plt.barh(x_indexes - width, nba_3pt_prcnt, width, label='League Avg')

plt.legend(loc='center left', bbox_to_anchor=(1, 0.5))
plt.yticks(ticks=x_indexes, labels=labels)

############ Remember to update title and labels when changing data ################
plt.title("3Pt % per Game (Playoff Teams 2014 Season)")
plt.ylabel("Team")
plt.xlabel("3Pt %")

plt.show()
```


    
![png](output_40_0.png)
    


### Playoff Team Points per Game


```python
wins_2014 = playoffs_2014['Wins']
season = playoffs_2014['Season']
team = playoffs_2014['Team']

nba_ft_percent = league_avg_14['FT%']
nba_fta = league_avg_14['FTA']
playoff_tm_fta = playoffs_2014['FTA']
playoff_tm_ft_percent = playoffs_2014['FT%']

nba_3pta =league_avg_14['3PA']
nba_3pt_prcnt =league_avg_14['3P%']
playoff_tm_3pta = playoffs_2014['3PA']
playoff_tm_3pt_prcnt = playoffs_2014['3P%']

nba_pts = league_avg_14['PTS']
playoff_tm_pts = playoffs_2014['PTS']

# grouped bar chart settings
labels = labels
x_indexes = np.arange(len(labels))  # the label locations
width = 0.20  # the width of the bars

plt.style.use('ggplot')

plt.barh(x_indexes, playoff_tm_pts, width, label='Playoff Team')
plt.barh(x_indexes - width, nba_pts, width, label='League Avg')

# places legend outside of plot area for visibility
plt.legend(loc='center left', bbox_to_anchor=(1, 0.5))
plt.yticks(ticks=x_indexes, labels=labels)

############ Remember to update title and labels when changing data ################
plt.title("Points per Game (Playoff Teams 2014 Season)")
plt.ylabel("Team")
plt.xlabel("Points")


plt.show()
```


    
![png](output_42_0.png)
    


## 3PA vs FTA for Playoff Teams


```python
wins_2014 = playoffs_2014['Wins']
season = playoffs_2014['Season']
team = playoffs_2014['Team']

nba_ft_percent = league_avg['FT%']
nba_fta = league_avg['FTA']
playoff_tm_fta = playoffs_2014['FTA']
playoff_tm_ft_percent = playoffs_2014['FT%']
playoff_tm_three_pta = playoffs_2014['3PA']
playoff_tm_three_percent = playoffs_2014['3P%']

x = playoffs_2014['3P%']
y = wins_2014

x_indexes = np.arange(len(playoffs_2014))
width = 0.25

plt.style.use('fivethirtyeight')

# plot 3PA vs FTA
plt.scatter(playoff_tm_three_pta,playoff_tm_fta)
plt.ylim(0)
plt.xlim(0)

############ Remember to update title and labels when changing data ################
plt.title("3 Pt Attempts vs Free Throw Attempts per Game (Playoff Teams 2014 Season)")
plt.ylabel("Free Throw Attempts")
plt.xlabel("3 Pt Attempts")

plt.tight_layout()
plt.show()
```


    
![png](output_44_0.png)
    


### 3PA vs Wins for Playoff Teams


```python
wins_2014 = playoffs_2014['Wins']
season = playoffs_2014['Season']
team = playoffs_2014['Team']

nba_ft_percent = league_avg['FT%']
nba_fta = league_avg['FTA']
playoff_tm_fta = playoffs_2014['FTA']
playoff_tm_ft_percent = playoffs_2014['FT%']
playoff_tm_three_pta = playoffs_2014['3PA']
playoff_tm_three_percent = playoffs_2014['3P%']

x_indexes = np.arange(len(playoffs_2014))
width = 0.25

plt.style.use('seaborn')


# plot 3PA vs FTA
plt.scatter(playoff_tm_three_percent,wins_2014)
plt.ylim(0)
plt.xlim(0)

############ Remember to update title and labels when changing data ################
plt.title("3Pt Percentage vs Wins (Playoff Teams 2014 Season)")
plt.ylabel("Wins")
plt.xlabel("3Pt %")

#plt.tight_layout()

plt.show()
```


    
![png](output_46_0.png)
    



```python
wins_2014 = playoffs_2014['Wins']
season = playoffs_2014['Season']
team = playoffs_2014['Team']
nba_ft_percent = league_avg['FT%']
nba_fta = league_avg['FTA']
playoff_tm_fta = playoffs_2014['FTA']
playoff_tm_ft_percent = playoffs_2014['FT%']
playoff_tm_three_pta = playoffs_2014['3PA']
playoff_tm_three_percent = playoffs_2014['3P%']

x_indexes = np.arange(len(playoffs_2014))
width = 0.25

plt.style.use('tableau-colorblind10')

# plot 3PA vs FTA
plt.scatter(playoff_tm_three_pta,wins_2014)
plt.ylim(0)
plt.xlim(0)

############ Remember to update title and labels when changing data ################
plt.title("3 Pt Attempts vs Wins (Playoff Teams 2014 Season)")
plt.ylabel("Wins")
plt.xlabel("3 Pt Attempts")

plt.show()
```


    
![png](output_47_0.png)
    



```python
# creates dataframe for ALL Playoff Teams
plyff_12_13 = pd.read_csv('12-13_plyff_stats.csv')
plyff_13_14 = pd.read_csv('13-14_plyff_stats.csv')
plyff_14_15 = pd.read_csv('14-15_plyff_stats.csv')
plyff_15_16 = pd.read_csv('15-16_plyff_stats.csv')
plyff_16_17 = pd.read_csv('16-17_plyff_stats.csv')
plyff_17_18 = pd.read_csv('17-18_plyff_stats.csv')
plyff_18_19 = pd.read_csv('18-19_plyff_stats.csv')
plyff_19_20 = pd.read_csv('19-20_plyff_stats.csv')
plyff_20_21 = pd.read_csv('20-21_plyff_stats.csv')
plyff_21_22 = pd.read_csv('21-22_plyff_stats.csv')
plyff_10_seasons = [plyff_12_13, plyff_13_14, plyff_14_15, plyff_15_16, plyff_16_17, plyff_17_18, plyff_18_19, plyff_19_20, plyff_20_21, plyff_21_22]
plyff_5_seasons = [plyff_17_18, plyff_18_19, plyff_19_20, plyff_20_21, plyff_21_22]

# playoffs last 5 years
plyff_df = pd.concat(plyff_5_seasons)
# playoffs last 10 years
plyff_df2 = pd.concat(plyff_10_seasons)
```


```python
# test last 5 years playoff team dataframe for expected results
plyff_df.tail()
```





  <div id="df-ceebdfb0-ff65-484b-9370-03d046078f00">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tm</th>
      <th>Season</th>
      <th>G</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12</th>
      <td>Milwaukee Bucks</td>
      <td>2021-2022</td>
      <td>12</td>
      <td>240.0</td>
      <td>38.5</td>
      <td>88.0</td>
      <td>0.438</td>
      <td>10.6</td>
      <td>32.3</td>
      <td>0.327</td>
      <td>27.9</td>
      <td>55.7</td>
      <td>0.501</td>
      <td>15.2</td>
      <td>20.8</td>
      <td>0.731</td>
      <td>9.8</td>
      <td>40.7</td>
      <td>50.4</td>
      <td>20.8</td>
      <td>6.3</td>
      <td>4.5</td>
      <td>13.8</td>
      <td>19.2</td>
      <td>102.8</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Utah Jazz</td>
      <td>2021-2022</td>
      <td>6</td>
      <td>240.0</td>
      <td>35.0</td>
      <td>79.0</td>
      <td>0.443</td>
      <td>8.2</td>
      <td>29.8</td>
      <td>0.274</td>
      <td>26.8</td>
      <td>49.2</td>
      <td>0.546</td>
      <td>20.8</td>
      <td>26.5</td>
      <td>0.786</td>
      <td>9.3</td>
      <td>35.5</td>
      <td>44.8</td>
      <td>17.2</td>
      <td>4.0</td>
      <td>3.7</td>
      <td>11.8</td>
      <td>22.0</td>
      <td>99.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Atlanta Hawks</td>
      <td>2021-2022</td>
      <td>5</td>
      <td>240.0</td>
      <td>34.4</td>
      <td>78.2</td>
      <td>0.440</td>
      <td>11.4</td>
      <td>35.0</td>
      <td>0.326</td>
      <td>23.0</td>
      <td>43.2</td>
      <td>0.532</td>
      <td>17.2</td>
      <td>22.0</td>
      <td>0.782</td>
      <td>8.6</td>
      <td>30.8</td>
      <td>39.4</td>
      <td>18.6</td>
      <td>5.8</td>
      <td>2.4</td>
      <td>16.4</td>
      <td>21.6</td>
      <td>97.4</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Chicago Bulls</td>
      <td>2021-2022</td>
      <td>5</td>
      <td>240.0</td>
      <td>36.4</td>
      <td>90.2</td>
      <td>0.404</td>
      <td>10.4</td>
      <td>36.8</td>
      <td>0.283</td>
      <td>26.0</td>
      <td>53.4</td>
      <td>0.487</td>
      <td>12.0</td>
      <td>14.4</td>
      <td>0.833</td>
      <td>8.2</td>
      <td>35.8</td>
      <td>44.0</td>
      <td>23.0</td>
      <td>7.8</td>
      <td>3.2</td>
      <td>13.0</td>
      <td>18.6</td>
      <td>95.2</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2021-2022</td>
      <td>11</td>
      <td>240.3</td>
      <td>38.2</td>
      <td>83.7</td>
      <td>0.456</td>
      <td>12.3</td>
      <td>34.6</td>
      <td>0.355</td>
      <td>25.9</td>
      <td>49.2</td>
      <td>0.527</td>
      <td>17.7</td>
      <td>22.6</td>
      <td>0.785</td>
      <td>9.4</td>
      <td>32.6</td>
      <td>42.0</td>
      <td>22.9</td>
      <td>7.1</td>
      <td>4.6</td>
      <td>13.7</td>
      <td>21.5</td>
      <td>106.3</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-ceebdfb0-ff65-484b-9370-03d046078f00')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-ceebdfb0-ff65-484b-9370-03d046078f00 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-ceebdfb0-ff65-484b-9370-03d046078f00');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>





```python
# selects last 5 years playoff averages from all playoff teams
plyff_avg_5 = plyff_df.loc[plyff_df['Tm'] == 'Playoff Team Average']
```


```python
# test last 5 years playoff average dataframe
plyff_avg_5
```





  <div id="df-b0502d0d-fdd5-4bc5-b0c7-0f77cafda9c1">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tm</th>
      <th>Season</th>
      <th>G</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2017-2018</td>
      <td>10</td>
      <td>241.2</td>
      <td>38.4</td>
      <td>84.2</td>
      <td>0.456</td>
      <td>10.5</td>
      <td>29.9</td>
      <td>0.351</td>
      <td>27.9</td>
      <td>54.3</td>
      <td>0.513</td>
      <td>17.2</td>
      <td>22.5</td>
      <td>0.765</td>
      <td>9.2</td>
      <td>33.7</td>
      <td>42.8</td>
      <td>21.8</td>
      <td>7.3</td>
      <td>4.8</td>
      <td>13.2</td>
      <td>20.7</td>
      <td>104.4</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2018-2019</td>
      <td>10</td>
      <td>242.4</td>
      <td>38.5</td>
      <td>87.0</td>
      <td>0.443</td>
      <td>11.4</td>
      <td>32.9</td>
      <td>0.345</td>
      <td>27.1</td>
      <td>54.0</td>
      <td>0.502</td>
      <td>19.4</td>
      <td>24.7</td>
      <td>0.784</td>
      <td>10.2</td>
      <td>34.8</td>
      <td>45.0</td>
      <td>23.0</td>
      <td>7.0</td>
      <td>4.9</td>
      <td>13.4</td>
      <td>22.2</td>
      <td>107.7</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2019-2020</td>
      <td>10</td>
      <td>242.1</td>
      <td>38.8</td>
      <td>85.0</td>
      <td>0.457</td>
      <td>13.1</td>
      <td>36.3</td>
      <td>0.360</td>
      <td>25.8</td>
      <td>48.7</td>
      <td>0.529</td>
      <td>18.8</td>
      <td>23.9</td>
      <td>0.788</td>
      <td>8.8</td>
      <td>34.3</td>
      <td>43.2</td>
      <td>22.9</td>
      <td>7.0</td>
      <td>4.3</td>
      <td>14.0</td>
      <td>21.8</td>
      <td>109.6</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2020-2021</td>
      <td>11</td>
      <td>241.2</td>
      <td>40.2</td>
      <td>86.9</td>
      <td>0.462</td>
      <td>12.5</td>
      <td>34.3</td>
      <td>0.364</td>
      <td>27.7</td>
      <td>52.6</td>
      <td>0.527</td>
      <td>17.5</td>
      <td>22.2</td>
      <td>0.786</td>
      <td>9.9</td>
      <td>33.5</td>
      <td>43.4</td>
      <td>22.0</td>
      <td>6.7</td>
      <td>4.4</td>
      <td>12.2</td>
      <td>20.5</td>
      <td>110.3</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2021-2022</td>
      <td>11</td>
      <td>240.3</td>
      <td>38.2</td>
      <td>83.7</td>
      <td>0.456</td>
      <td>12.3</td>
      <td>34.6</td>
      <td>0.355</td>
      <td>25.9</td>
      <td>49.2</td>
      <td>0.527</td>
      <td>17.7</td>
      <td>22.6</td>
      <td>0.785</td>
      <td>9.4</td>
      <td>32.6</td>
      <td>42.0</td>
      <td>22.9</td>
      <td>7.1</td>
      <td>4.6</td>
      <td>13.7</td>
      <td>21.5</td>
      <td>106.3</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-b0502d0d-fdd5-4bc5-b0c7-0f77cafda9c1')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-b0502d0d-fdd5-4bc5-b0c7-0f77cafda9c1 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-b0502d0d-fdd5-4bc5-b0c7-0f77cafda9c1');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>





```python
plyff_df2.head(5)
```





  <div id="df-5e5ec74b-e9bd-4760-b426-d1eb869a04e2">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tm</th>
      <th>Season</th>
      <th>G</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Denver Nuggets</td>
      <td>2012-2013</td>
      <td>6</td>
      <td>240.0</td>
      <td>37.2</td>
      <td>84.8</td>
      <td>0.438</td>
      <td>7.0</td>
      <td>22.5</td>
      <td>0.311</td>
      <td>30.2</td>
      <td>62.3</td>
      <td>0.484</td>
      <td>21.7</td>
      <td>29.7</td>
      <td>0.730</td>
      <td>11.8</td>
      <td>26.8</td>
      <td>38.7</td>
      <td>21.2</td>
      <td>9.0</td>
      <td>2.8</td>
      <td>14.5</td>
      <td>23.2</td>
      <td>103.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Golden State Warriors</td>
      <td>2012-2013</td>
      <td>12</td>
      <td>246.3</td>
      <td>39.0</td>
      <td>84.4</td>
      <td>0.462</td>
      <td>8.7</td>
      <td>22.3</td>
      <td>0.388</td>
      <td>30.3</td>
      <td>62.1</td>
      <td>0.489</td>
      <td>16.0</td>
      <td>21.8</td>
      <td>0.736</td>
      <td>11.5</td>
      <td>34.6</td>
      <td>46.1</td>
      <td>21.7</td>
      <td>6.4</td>
      <td>4.9</td>
      <td>16.5</td>
      <td>23.8</td>
      <td>102.7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>San Antonio Spurs</td>
      <td>2012-2013</td>
      <td>21</td>
      <td>247.1</td>
      <td>38.0</td>
      <td>82.2</td>
      <td>0.463</td>
      <td>7.9</td>
      <td>20.8</td>
      <td>0.378</td>
      <td>30.2</td>
      <td>61.4</td>
      <td>0.492</td>
      <td>16.4</td>
      <td>21.5</td>
      <td>0.763</td>
      <td>9.7</td>
      <td>32.9</td>
      <td>42.6</td>
      <td>21.9</td>
      <td>7.9</td>
      <td>5.0</td>
      <td>12.9</td>
      <td>19.1</td>
      <td>100.3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Houston Rockets</td>
      <td>2012-2013</td>
      <td>6</td>
      <td>240.0</td>
      <td>34.3</td>
      <td>81.0</td>
      <td>0.424</td>
      <td>11.7</td>
      <td>33.7</td>
      <td>0.347</td>
      <td>22.7</td>
      <td>47.3</td>
      <td>0.479</td>
      <td>19.7</td>
      <td>27.7</td>
      <td>0.711</td>
      <td>10.8</td>
      <td>32.7</td>
      <td>43.5</td>
      <td>18.0</td>
      <td>5.8</td>
      <td>5.5</td>
      <td>15.8</td>
      <td>23.2</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Brooklyn Nets</td>
      <td>2012-2013</td>
      <td>7</td>
      <td>250.7</td>
      <td>37.0</td>
      <td>83.7</td>
      <td>0.442</td>
      <td>6.1</td>
      <td>19.4</td>
      <td>0.316</td>
      <td>30.9</td>
      <td>64.3</td>
      <td>0.480</td>
      <td>19.3</td>
      <td>25.4</td>
      <td>0.758</td>
      <td>11.7</td>
      <td>30.4</td>
      <td>42.1</td>
      <td>19.9</td>
      <td>6.3</td>
      <td>5.6</td>
      <td>11.6</td>
      <td>19.9</td>
      <td>99.4</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-5e5ec74b-e9bd-4760-b426-d1eb869a04e2')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-5e5ec74b-e9bd-4760-b426-d1eb869a04e2 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-5e5ec74b-e9bd-4760-b426-d1eb869a04e2');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>





```python
# selects last 10 years playoff averages from all playoff teams
plyff_avg_10 = plyff_df2.loc[plyff_df2['Tm'] == 'Playoff Team Average']
```


```python
# test last 10 years playoff averages
plyff_avg_10.head()
```





  <div id="df-57d638aa-3116-4339-a290-0fe7ec6983ee">
    <div class="colab-df-container">
      <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tm</th>
      <th>Season</th>
      <th>G</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>FG%</th>
      <th>3P</th>
      <th>3PA</th>
      <th>3P%</th>
      <th>2P</th>
      <th>2PA</th>
      <th>2P%</th>
      <th>FT</th>
      <th>FTA</th>
      <th>FT%</th>
      <th>ORB</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2012-2013</td>
      <td>11</td>
      <td>243.5</td>
      <td>35.0</td>
      <td>79.4</td>
      <td>0.441</td>
      <td>7.1</td>
      <td>20.6</td>
      <td>0.344</td>
      <td>27.9</td>
      <td>58.8</td>
      <td>0.475</td>
      <td>18.1</td>
      <td>24.0</td>
      <td>0.751</td>
      <td>10.5</td>
      <td>30.8</td>
      <td>41.3</td>
      <td>19.3</td>
      <td>7.0</td>
      <td>4.8</td>
      <td>13.8</td>
      <td>22.2</td>
      <td>95.2</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2013-2014</td>
      <td>11</td>
      <td>242.5</td>
      <td>36.4</td>
      <td>80.0</td>
      <td>0.455</td>
      <td>8.0</td>
      <td>22.3</td>
      <td>0.358</td>
      <td>28.3</td>
      <td>57.6</td>
      <td>0.492</td>
      <td>18.8</td>
      <td>24.7</td>
      <td>0.763</td>
      <td>10.2</td>
      <td>30.9</td>
      <td>41.1</td>
      <td>19.8</td>
      <td>7.1</td>
      <td>4.4</td>
      <td>13.3</td>
      <td>22.0</td>
      <td>99.5</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2014-2015</td>
      <td>10</td>
      <td>242.8</td>
      <td>36.8</td>
      <td>84.5</td>
      <td>0.435</td>
      <td>8.8</td>
      <td>25.5</td>
      <td>0.344</td>
      <td>28.0</td>
      <td>59.0</td>
      <td>0.475</td>
      <td>18.1</td>
      <td>25.0</td>
      <td>0.725</td>
      <td>11.0</td>
      <td>34.0</td>
      <td>45.0</td>
      <td>21.8</td>
      <td>7.6</td>
      <td>5.4</td>
      <td>13.5</td>
      <td>21.9</td>
      <td>100.5</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2015-2016</td>
      <td>11</td>
      <td>241.5</td>
      <td>36.5</td>
      <td>83.1</td>
      <td>0.440</td>
      <td>9.1</td>
      <td>25.8</td>
      <td>0.354</td>
      <td>27.4</td>
      <td>57.3</td>
      <td>0.478</td>
      <td>17.6</td>
      <td>23.6</td>
      <td>0.747</td>
      <td>10.5</td>
      <td>32.5</td>
      <td>43.0</td>
      <td>19.9</td>
      <td>7.3</td>
      <td>5.0</td>
      <td>13.2</td>
      <td>21.1</td>
      <td>99.8</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Playoff Team Average</td>
      <td>2016-2017</td>
      <td>10</td>
      <td>240.9</td>
      <td>38.5</td>
      <td>83.7</td>
      <td>0.460</td>
      <td>10.5</td>
      <td>29.1</td>
      <td>0.361</td>
      <td>28.0</td>
      <td>54.6</td>
      <td>0.513</td>
      <td>18.5</td>
      <td>23.8</td>
      <td>0.776</td>
      <td>9.7</td>
      <td>32.1</td>
      <td>41.8</td>
      <td>22.2</td>
      <td>7.6</td>
      <td>4.7</td>
      <td>13.3</td>
      <td>20.8</td>
      <td>106.0</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-57d638aa-3116-4339-a290-0fe7ec6983ee')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-57d638aa-3116-4339-a290-0fe7ec6983ee button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-57d638aa-3116-4339-a290-0fe7ec6983ee');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>





```python
import plotly.graph_objects as go
# px.line(league_avg, x='Season', y='PTS', labels={'x': 'Season', 'y': 'Points'})
fig = go.Figure()
# fig.add_trace(go.Scatter(x=league_avg.Season, y=league_avg.PTS,
#                          mode='lines', name='Points'))
fig.add_trace(go.Scatter(x=league_avg.Season, y=league_avg['FG%'],
                         mode='lines+markers', name='Reg Season FG%'))
fig.add_trace(go.Scatter(x=plyff_avg_10.Season, y=plyff_avg_10['FG%'],
                         mode='lines+markers', name='Playoff FG%'))
fig.add_trace(go.Scatter(x=league_avg.Season, y=league_avg['3P%'],
                         mode='lines+markers', name='Reg Season 3P%'))
fig.add_trace(go.Scatter(x=plyff_avg_10.Season, y=plyff_avg_10['3P%'],
                         mode='lines+markers', name='Playoff 3P%'))
fig.add_trace(go.Scatter(x=league_avg.Season, y=league_avg['2P%'],
                         mode='lines+markers', name='Reg Season 2P%'))
fig.add_trace(go.Scatter(x=plyff_avg_10.Season, y=plyff_avg_10['2P%'],
                         mode='lines+markers', name='Playoff 2P%'))
# fig.add_trace(go.Scatter(x=league_avg.Season, y=league_avg['FT%'],
#                          mode='lines+markers', name='Reg Season FT%'))
# fig.add_trace(go.Scatter(x=plyff_avg_10.Season, y=plyff_avg_10['FT%'],
#                          mode='lines+markers', name='Playoff FT%'))

fig.update_yaxes(rangemode="tozero")
fig.update_layout(title='NBA Reg Season vs Playoffs Last 10 Seasons',
                  xaxis_title='Season', yaxis_title='Shooting %', title_x=0.5)
fig.show(renderer='colab')
#fig.write_html("lst_10_seasons.html")
```


<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script src="https://cdn.plot.ly/plotly-2.8.3.min.js"></script>                <div id="045fb941-b461-4301-a2f1-5af3d1656444" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("045fb941-b461-4301-a2f1-5af3d1656444")) {                    Plotly.newPlot(                        "045fb941-b461-4301-a2f1-5af3d1656444",                        [{"mode":"lines+markers","name":"Reg Season FG%","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[0.453,0.454,0.449,0.452,0.457,0.46,0.461,0.46,0.466,0.461],"type":"scatter"},{"mode":"lines+markers","name":"Playoff FG%","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[0.441,0.455,0.435,0.44,0.46,0.456,0.443,0.457,0.462,0.456],"type":"scatter"},{"mode":"lines+markers","name":"Reg Season 3P%","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[0.359,0.36,0.35,0.354,0.358,0.362,0.355,0.358,0.367,0.354],"type":"scatter"},{"mode":"lines+markers","name":"Playoff 3P%","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[0.344,0.358,0.344,0.354,0.361,0.351,0.345,0.36,0.364,0.355],"type":"scatter"},{"mode":"lines+markers","name":"Reg Season 2P%","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[0.483,0.488,0.485,0.491,0.503,0.51,0.52,0.524,0.53,0.533],"type":"scatter"},{"mode":"lines+markers","name":"Playoff 2P%","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[0.475,0.492,0.475,0.478,0.513,0.513,0.502,0.529,0.527,0.527],"type":"scatter"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"yaxis":{"rangemode":"tozero","title":{"text":"Shooting %"}},"title":{"text":"NBA Reg Season vs Playoffs Last 10 Seasons","x":0.5},"xaxis":{"title":{"text":"Season"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('045fb941-b461-4301-a2f1-5af3d1656444');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>


### Regular Season vs Playoffs Rebounds


```python
import plotly.graph_objects as go
fig = go.Figure()

fig.add_trace(go.Bar(x=league_avg.Season, y=league_avg['TRB'],
                         name='Reg Season'))
fig.add_trace(go.Bar(x=plyff_avg_10.Season, y=plyff_avg_10['TRB'],
                         name='Playoffs'))

fig.update_yaxes(rangemode="tozero")
fig.update_layout(title='NBA Reg Season vs Playoffs Last 10 Seasons',
                  xaxis_title='Season', yaxis_title='Per Game Average', title_x=0.5,
                  barmode='group')
fig.show(renderer='colab')
#fig.write_html("lst_10_seasons.html")
```


<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script src="https://cdn.plot.ly/plotly-2.8.3.min.js"></script>                <div id="2bbe105e-0eb4-4383-95b5-ea9e1791703d" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("2bbe105e-0eb4-4383-95b5-ea9e1791703d")) {                    Plotly.newPlot(                        "2bbe105e-0eb4-4383-95b5-ea9e1791703d",                        [{"name":"Reg Season","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[42.1,42.7,43.3,43.8,43.5,43.5,45.2,44.8,44.3,44.5],"type":"bar"},{"name":"Playoffs","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[41.3,41.1,45.0,43.0,41.8,42.8,45.0,43.2,43.4,42.0],"type":"bar"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"yaxis":{"rangemode":"tozero","title":{"text":"Per Game Average"}},"title":{"text":"NBA Reg Season vs Playoffs Last 10 Seasons","x":0.5},"xaxis":{"title":{"text":"Season"}},"barmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('2bbe105e-0eb4-4383-95b5-ea9e1791703d');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>


### Regular Season vs Playoffs Turnovers


```python
import plotly.graph_objects as go
# px.line(league_avg, x='Season', y='PTS', labels={'x': 'Season', 'y': 'Points'})
fig = go.Figure()

fig.add_trace(go.Bar(x=league_avg.Season, y=league_avg['TOV'],
                         name='Reg Season'))
fig.add_trace(go.Bar(x=plyff_avg_10.Season, y=plyff_avg_10['TOV'],
                         name='Playoffs'))

fig.update_yaxes(rangemode="tozero")
fig.update_layout(title='NBA Reg Season vs Playoffs Last 10 Seasons',
                  xaxis_title='Season', yaxis_title='Per Game Average', title_x=0.5,
                  barmode='group')
fig.show(renderer='colab')
#fig.write_html("lst_10_seasons.html")
```


<html>
<head><meta charset="utf-8" /></head>
<body>
    <div>            <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS-MML_SVG"></script><script type="text/javascript">if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}</script>                <script type="text/javascript">window.PlotlyConfig = {MathJaxConfig: 'local'};</script>
        <script src="https://cdn.plot.ly/plotly-2.8.3.min.js"></script>                <div id="58fb35d8-fece-4c97-99d4-12b195a8b805" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("58fb35d8-fece-4c97-99d4-12b195a8b805")) {                    Plotly.newPlot(                        "58fb35d8-fece-4c97-99d4-12b195a8b805",                        [{"name":"Reg Season","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[14.6,14.6,14.4,14.4,14.0,14.3,14.1,14.5,13.8,13.8],"type":"bar"},{"name":"Playoffs","x":["2012-2013","2013-2014","2014-2015","2015-2016","2016-2017","2017-2018","2018-2019","2019-2020","2020-2021","2021-2022"],"y":[13.8,13.3,13.5,13.2,13.3,13.2,13.4,14.0,12.2,13.7],"type":"bar"}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"yaxis":{"rangemode":"tozero","title":{"text":"Per Game Average"}},"title":{"text":"NBA Reg Season vs Playoffs Last 10 Seasons","x":0.5},"xaxis":{"title":{"text":"Season"}},"barmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('58fb35d8-fece-4c97-99d4-12b195a8b805');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                            </script>        </div>
</body>
</html>


# References

* https://stackoverflow.com/questions/57906797/scatter-graphs-y-axis-always-starting-at-0
* https://stackoverflow.com/questions/48376580/google-colab-how-to-read-data-from-my-google-drive
* https://stackoverflow.com/questions/47230817/plotly-notebook-mode-with-google-colaboratory
* https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.loc.html
* https://www.basketball-reference.com/playoffs/NBA_2022.html (going back 10 seasons 2012-2022)
* https://plotly.com/python/
* https://plotly.com/python/getting-started/
* https://stackoverflow.com/questions/52956228/how-to-plot-using-plotly-with-offline-mode-out-not-in-ipython-notebooks
* https://www.youtube.com/watch?v=GGL6U0k8WYA&t=805s
* https://stackoverflow.com/questions/61233041/module-not-found-error-no-module-named-chart-studio
* https://stackoverflow.com/questions/57906797/scatter-graphs-y-axis-always-starting-at-0
* https://community.plotly.com/t/title-alignment-python/30820
* https://plotly.com/python/reference/layout/
* https://plotly.com/python/bar-charts/#grouped-bar-chart



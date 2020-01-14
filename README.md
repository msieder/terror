# DOPP Project - Terrorism and Wealth

Datasets
* Terrorism dataset from https://www.kaggle.com/START-UMD/gtd
* GDP dataset from https://data.worldbank.org/indicator/NY.GDP.MKTP.CD
* GDP per capita from https://data.worldbank.org/indicator/NY.GDP.PCAP.CD

Please note, that the names of the datasets may have been changed before reading in. Terrorism dataset is of format .csv while worldbank data is excel .xls. This is due to easier reading in. For the excel file the first 3 columns have to be deleted manually in MS Excel. If you need, i can provide both xls datasets.



```python
import pandas as pd
import numpy as np
#pip install pycountry
import pycountry
```


```python
terror = pd.read_csv("./globalterrorismdb_0718dist.csv",encoding="ISO-8859-1")
terror.rename(columns={'iyear':'Year','imonth':'Month','iday':'Day','country_txt':'Country',
                       'region_txt':'Region','attacktype1_txt':'AttackType','target1':'Target',
                       'nkill':'Killed','nwound':'Wounded','summary':'Summary','gname':'Group',
                       'targtype1_txt':'Target_type','weaptype1_txt':'Weapon_type','motive':'Motive'},
              inplace=True)
terror=terror[['Year','Month','Day','Country','Region','city','latitude','longitude','AttackType',
               'Killed','Wounded','Target','Summary','Group','Target_type','Weapon_type','Motive']]
terror['Casualities']=terror['Killed']+terror['Wounded']
terror = terror[terror.Year>=1991]
terror.head(5)
```

    D:\Programs\Anaconda3\lib\site-packages\IPython\core\interactiveshell.py:3058: DtypeWarning: Columns (4,6,31,33,61,62,63,76,79,90,92,94,96,114,115,121) have mixed types. Specify dtype option on import or set low_memory=False.
      interactivity=interactivity, compiler=compiler, result=result)
    




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
      <th>Year</th>
      <th>Month</th>
      <th>Day</th>
      <th>Country</th>
      <th>Region</th>
      <th>city</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>AttackType</th>
      <th>Killed</th>
      <th>Wounded</th>
      <th>Target</th>
      <th>Summary</th>
      <th>Group</th>
      <th>Target_type</th>
      <th>Weapon_type</th>
      <th>Motive</th>
      <th>Casualities</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>41338</th>
      <td>1991</td>
      <td>1</td>
      <td>30</td>
      <td>Jordan</td>
      <td>Middle East &amp; North Africa</td>
      <td>Amman</td>
      <td>31.950001</td>
      <td>35.933331</td>
      <td>Facility/Infrastructure Attack</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>French Cultural Center</td>
      <td>NaN</td>
      <td>Unknown</td>
      <td>Government (Diplomatic)</td>
      <td>Incendiary</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>44962</th>
      <td>1991</td>
      <td>1</td>
      <td>0</td>
      <td>Philippines</td>
      <td>Southeast Asia</td>
      <td>Lopez</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Bombing/Explosion</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>30 meter cement bridge</td>
      <td>NaN</td>
      <td>New People's Army (NPA)</td>
      <td>Transportation</td>
      <td>Explosives</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>44963</th>
      <td>1991</td>
      <td>1</td>
      <td>0</td>
      <td>Guatemala</td>
      <td>Central America &amp; Caribbean</td>
      <td>El Subin</td>
      <td>16.633333</td>
      <td>-90.183333</td>
      <td>Assassination</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Helicopter, President Jorge Serrano Elias*</td>
      <td>NaN</td>
      <td>Guatemalan National Revolutionary Unity (URNG)</td>
      <td>Private Citizens &amp; Property</td>
      <td>Firearms</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>44964</th>
      <td>1991</td>
      <td>1</td>
      <td>0</td>
      <td>Philippines</td>
      <td>Southeast Asia</td>
      <td>Conner</td>
      <td>17.795745</td>
      <td>121.322798</td>
      <td>Armed Assault</td>
      <td>8.0</td>
      <td>0.0</td>
      <td>detachment</td>
      <td>NaN</td>
      <td>New People's Army (NPA)</td>
      <td>Military</td>
      <td>Firearms</td>
      <td>NaN</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>44965</th>
      <td>1991</td>
      <td>1</td>
      <td>1</td>
      <td>Colombia</td>
      <td>South America</td>
      <td>Buenaventura district</td>
      <td>3.881820</td>
      <td>-77.070420</td>
      <td>Bombing/Explosion</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>Pacific Oil Pipeline</td>
      <td>NaN</td>
      <td>Simon Bolivar Guerrilla Coordinating Board (CGSB)</td>
      <td>Utilities</td>
      <td>Explosives</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
gdp = pd.read_excel("./gdp_total.xls",)
gdp.rename(columns={"Country Name":"CountryName","Country Code":"CountryCode"},inplace=True)
gdp.drop(gdp.columns[[range(2,35)]], axis=1,inplace=True)
gdp.drop("2019", axis=1, inplace=True)
gdp.head(5)
```

    D:\Programs\Anaconda3\lib\site-packages\pandas\core\indexes\base.py:4291: FutureWarning: Using a non-tuple sequence for multidimensional indexing is deprecated; use `arr[tuple(seq)]` instead of `arr[seq]`. In the future this will be interpreted as an array index, `arr[np.array(seq)]`, which will result either in an error or a different result.
      result = getitem(key)
    




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
      <th>CountryName</th>
      <th>CountryCode</th>
      <th>1991</th>
      <th>1992</th>
      <th>1993</th>
      <th>1994</th>
      <th>1995</th>
      <th>1996</th>
      <th>1997</th>
      <th>1998</th>
      <th>...</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aruba</td>
      <td>ABW</td>
      <td>8.721387e+08</td>
      <td>9.584632e+08</td>
      <td>1.082980e+09</td>
      <td>1.245688e+09</td>
      <td>1.320475e+09</td>
      <td>1.379961e+09</td>
      <td>1.531944e+09</td>
      <td>1.665101e+09</td>
      <td>...</td>
      <td>2.498883e+09</td>
      <td>2.390503e+09</td>
      <td>2.549721e+09</td>
      <td>2.534637e+09</td>
      <td>2.581564e+09</td>
      <td>2.649721e+09</td>
      <td>2.691620e+09</td>
      <td>2.646927e+09</td>
      <td>2.700559e+09</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>1.243909e+10</td>
      <td>1.585657e+10</td>
      <td>1.780428e+10</td>
      <td>2.000162e+10</td>
      <td>2.056105e+10</td>
      <td>2.048487e+10</td>
      <td>1.990711e+10</td>
      <td>1.936264e+10</td>
      <td>2.019176e+10</td>
      <td>1.936297e+10</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>1.060378e+10</td>
      <td>8.307811e+09</td>
      <td>5.768720e+09</td>
      <td>4.438321e+09</td>
      <td>5.538749e+09</td>
      <td>7.526447e+09</td>
      <td>7.648377e+09</td>
      <td>6.506230e+09</td>
      <td>...</td>
      <td>7.030716e+10</td>
      <td>8.379950e+10</td>
      <td>1.117897e+11</td>
      <td>1.280529e+11</td>
      <td>1.367099e+11</td>
      <td>1.457122e+11</td>
      <td>1.161936e+11</td>
      <td>1.011239e+11</td>
      <td>1.221238e+11</td>
      <td>1.057510e+11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>1.099559e+09</td>
      <td>6.521750e+08</td>
      <td>1.185315e+09</td>
      <td>1.880952e+09</td>
      <td>2.392765e+09</td>
      <td>3.199643e+09</td>
      <td>2.258516e+09</td>
      <td>2.545967e+09</td>
      <td>...</td>
      <td>1.204422e+10</td>
      <td>1.192696e+10</td>
      <td>1.289087e+10</td>
      <td>1.231978e+10</td>
      <td>1.277628e+10</td>
      <td>1.322824e+10</td>
      <td>1.138693e+10</td>
      <td>1.186135e+10</td>
      <td>1.302506e+10</td>
      <td>1.510250e+10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andorra</td>
      <td>AND</td>
      <td>1.106929e+09</td>
      <td>1.210014e+09</td>
      <td>1.007026e+09</td>
      <td>1.017549e+09</td>
      <td>1.178739e+09</td>
      <td>1.223945e+09</td>
      <td>1.180597e+09</td>
      <td>1.211932e+09</td>
      <td>...</td>
      <td>3.660531e+09</td>
      <td>3.355695e+09</td>
      <td>3.442063e+09</td>
      <td>3.164615e+09</td>
      <td>3.281585e+09</td>
      <td>3.350736e+09</td>
      <td>2.811489e+09</td>
      <td>2.877312e+09</td>
      <td>3.013387e+09</td>
      <td>3.236544e+09</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>




```python
capita = pd.read_excel("./gdp_capita.xls")
capita.rename(columns={"Country Name":"CountryName","Country Code":"CountryCode"},inplace=True)
capita.drop(capita.columns[[range(2,35)]], axis=1,inplace=True)
capita.drop("2019", axis=1, inplace=True)
capita.head(5)
```




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
      <th>CountryName</th>
      <th>CountryCode</th>
      <th>1991</th>
      <th>1992</th>
      <th>1993</th>
      <th>1994</th>
      <th>1995</th>
      <th>1996</th>
      <th>1997</th>
      <th>1998</th>
      <th>...</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
      <th>2012</th>
      <th>2013</th>
      <th>2014</th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aruba</td>
      <td>ABW</td>
      <td>13496.003385</td>
      <td>14046.503997</td>
      <td>14936.827039</td>
      <td>16241.046325</td>
      <td>16439.356361</td>
      <td>16586.068436</td>
      <td>17927.749635</td>
      <td>19078.343191</td>
      <td>...</td>
      <td>24630.453714</td>
      <td>23512.602596</td>
      <td>24985.993281</td>
      <td>24713.698045</td>
      <td>25025.099563</td>
      <td>25533.569780</td>
      <td>25796.380251</td>
      <td>25239.600411</td>
      <td>25630.266492</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>AFG</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>438.076034</td>
      <td>543.303042</td>
      <td>591.162346</td>
      <td>641.872034</td>
      <td>637.165044</td>
      <td>613.856333</td>
      <td>578.466353</td>
      <td>547.228110</td>
      <td>556.302139</td>
      <td>520.896603</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Angola</td>
      <td>AGO</td>
      <td>865.692730</td>
      <td>656.361756</td>
      <td>441.200673</td>
      <td>328.673295</td>
      <td>397.179451</td>
      <td>522.643807</td>
      <td>514.295223</td>
      <td>423.593660</td>
      <td>...</td>
      <td>3122.780766</td>
      <td>3587.883798</td>
      <td>4615.468028</td>
      <td>5100.095808</td>
      <td>5254.882338</td>
      <td>5408.410496</td>
      <td>4166.979684</td>
      <td>3506.072885</td>
      <td>4095.812942</td>
      <td>3432.385736</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Albania</td>
      <td>ALB</td>
      <td>336.586995</td>
      <td>200.852220</td>
      <td>367.279225</td>
      <td>586.416340</td>
      <td>750.604449</td>
      <td>1009.977668</td>
      <td>717.380567</td>
      <td>813.790264</td>
      <td>...</td>
      <td>4114.140150</td>
      <td>4094.362119</td>
      <td>4437.178067</td>
      <td>4247.614279</td>
      <td>4413.081743</td>
      <td>4578.666720</td>
      <td>3952.829458</td>
      <td>4124.108907</td>
      <td>4532.890162</td>
      <td>5268.848504</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andorra</td>
      <td>AND</td>
      <td>19532.540150</td>
      <td>20547.711790</td>
      <td>16516.471027</td>
      <td>16234.809010</td>
      <td>18461.064858</td>
      <td>19017.174590</td>
      <td>18353.059722</td>
      <td>18894.521496</td>
      <td>...</td>
      <td>43338.866758</td>
      <td>39736.354063</td>
      <td>41100.729938</td>
      <td>38392.943901</td>
      <td>40626.751632</td>
      <td>42300.334128</td>
      <td>36039.653496</td>
      <td>37224.108916</td>
      <td>39134.393371</td>
      <td>42029.762737</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>



# Data Cleaning / Merging / etc
**GDP Data**
* Calculating Mean of GDP and GDP capita
* Interpolating GDP and Capita Data


**Terrorism Data**
* Around 9.000 terror attacks cannot be assigned to a country with gdp (International)
* 154 Countries of the GDP dataset have had a terror attack between 1991 and 2018
* Mapping Country Names of Terrorism Database to World Bank Names
* Performing ISO-Lookup for the countries (ISO-3)
* Grouping terrorist attacks by country and by country+year
* No Data for Year 1993 (and 2018) -> interpolation for 1993




```python
gdp["Average"] = gdp[gdp.keys()[2:]].mean(axis=1)
capita["Average"] = capita[capita.keys()[2:]].mean(axis=1)
```


```python
gdp = round(gdp)
capita = round(capita,3)
```


```python
gdp[gdp.keys()[2:30]] = gdp[gdp.keys()[2:30]].interpolate(method="linear",limit_direction='both',axis=1)
capita[capita.keys()[2:30]] = capita[capita.keys()[2:30]].interpolate(method="linear",limit_direction='both',axis=1)
```


```python
terror = terror.replace({'Country': {"Russia":"Russian Federation",
                                     "Czech Republic":"Czechia",
                                     "Slovak Republic":"Slovakia",
                                     "Iran":"Iran, Islamic Republic of",
                                     "Syria":"Syrian Arab Republic",
                                     "Venezuela":"Venezuela, Bolivarian Republic of",
                                     "North Korea":"Korea, Democratic People's Republic of",
                                     "South Korea":"Korea, Republic of",
                                     "Democratic Republic of the Congo":"Congo, The Democratic Republic of the",
                                     "Zaire":"Congo, The Democratic Republic of the",
                                     "Republic of the Congo":"Congo",
                                     "Bolivia":"Bolivia, Plurinational State of",
                                     "Bosnia-Herzegovina":"Bosnia and Herzegovina",
                                     "Brunei":"Brunei Darussalam",
                                     "East Timor":"Timor-Leste",
                                     "Ivory Coast":"Côte d'Ivoire",
                                     "Tanzania":"Tanzania, United Republic of",
                                     "Vietnam":"Viet Nam",
                                     "Taiwan":"Taiwan, Province of China",
                                     "Macedonia":"North Macedonia",
                                     "Moldova":"Moldova, Republic of",
                                     "Laos":"Lao People's Democratic Republic",
                                     "Serbia-Montenegro":"Montenegro",
                                     "Swaziland":"Eswatini",
                                     "Macau":"Macao",
                                     "St. Kitts and Nevis":"Saint Kitts and Nevis",
                                     "St. Lucia":"Saint Lucia",
                                     "West Bank and Gaza Strip":"Palestine, State of"}})


terror = terror.replace("Kosovo","Serbia")
terror = terror.replace("Yugoslavia","Serbia")
terror = terror.replace("Czechoslovakia","Czechia")
terror = terror.replace("East Germany (GDR)","Germany")
terror = terror.replace("West Germany (FRG)","Germany")
terror = terror.replace("Soviet Union","Russian Federation")

print(len(terror)-sum(terror.Country.isin(gdp.CountryName).astype(int)))
# Around 9.000 Terror attacks cannot be assigned to a country with gdp
print(sum(gdp.CountryName.isin(terror.Country).astype(int)))
#154 Countries from gdp are also in the terrorism dataset.
#Numbers might change further depending on preprocessing
```

    9351
    154
    


```python
terrorgroup = pd.DataFrame()
terrorgroup["Attacks"] = terror.groupby("Country")["Country"].count()
```


```python
# ISO lookup
input_countries = terrorgroup.index.values

countries = {}
for country in pycountry.countries:
    countries[country.name] = country.alpha_3

codes = [countries.get(country, 'Unknown code') for country in input_countries]

# Adding to dataframe
terrorgroup["ISO"]=codes
terrorgroup["Country"]=terrorgroup.index.values
#terrorgroup.head()
```


```python
terroryear = pd.DataFrame()
terroryear["Attacks"] = terror.groupby(["Country","Year"])["Country"].count()
terroryear.reset_index(inplace=True)
terroryear["ISO"]=[countries.get(country, 'Unknown code') for country in terroryear.Country]

test = pd.DataFrame()
test["Country"]=gdp.CountryName.unique()
test["ISO"]=gdp.CountryCode.unique()

for i in range(1991,2018):
    test[str(i)]=0
    df=terroryear[terroryear.Year==i]
    for j in df.Country.unique():
        test.loc[test.Country==j, [str(i)]] = df.loc[df.Country==j,"Attacks"].unique()[0]

test["1993"]=np.nan
test[test.keys()[2:29]] = round(test[test.keys()[2:29]].interpolate(method="linear",limit_direction='backward',axis=1,limit_area="inside"))
test[test.keys()[2:29]] = test[test.keys()[2:29]].astype('int32')
terroryear = test
```


```python
# Drop Countries where no Terrorist attacks are available?
gdp_terror = gdp[terroryear.sum(axis=1)!=0]
capita_terror = capita[terroryear.sum(axis=1)!=0]
terroryear = terroryear[terroryear.sum(axis=1)!=0]
```

# Data Visualization
Contains the following plots:
* Terrorism world plot
* Terrorism EU Plot
* GDP per capita world plot



```python
#pip install plotly
import plotly as py 
import plotly.graph_objs as go 
import plotly.express as px
  
# some more libraries to plot graph 
from plotly.offline import download_plotlyjs, init_notebook_mode, iplot, plot 
  
```

# [Return Period Visualisation Tool](https://github.com/COMP0016-IFRC-Team5/data-visualiser) 

A data visualizer tool produces the return period – loss graph of low-impact 
hazardous events for 91 countries. This tool use publicly available data from 
national disaster loss databases, but also available for other resources.  

This tool is a part of a UCL IXN project: "Define return periods for low-impact 
hazardous events" with IFRC. 

 

## Project Introduction 

Define return periods for low-impact hazardous events  

IFRC and individual Red Cross/Red Crescent National Societies are increasingly 
focusing on dedicating resources to take anticipatory action to mitigate the 
impacts of relatively high-frequency, low-intensity natural hazards. In other 
words, instead of reserving funds for those events that are expected to take 
place once every five years, IFRC and the National Societies aim to address 
events that occur more frequently, such as once per two or three years.  

In order to assess when an event (whether forecasted or already occurred) has 
reached the new threshold, we need to have impact exceedance curves based on 
observational data for these specific types of hazardous events. Using publicly 
available data from national disaster loss databases (DesInventar), EM-DAT, IFRC 
and other sources, the goal of this project would be to create exceedance curves 
and tables for multiple impacts for as many National Societies as possible. 

 

## Get Started 

**Check out our [online demo](https://github.com/COMP0016-IFRC-Team5/data-visualiser).** 

### Installation

```bash
git clone https://github.com/COMP0016-IFRC-Team5/data-visualiser
cd data-visualiser
```

### Requirements

1. Install dependencies in any preferred way

- Using conda ([Anaconda](https://docs.anaconda.com/anaconda/install/index.html) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html))
```bash
conda env create -f conda_env.yml
conda activate data-processor
```

- Using pip ([Python 3.10+](https://www.python.org/downloads/))
```bash
pip install -r requirements.txt
```
2. Get data using data-downloader module
3. Process data using data-processor module


### Usage

#### To run the example
The example shows a typical case which produce the return period - deaths & affected people graphs for floods and earthquakes in Albania and Pakistan. Data used from past 15 years.

```bash
python example.py
```
A typical process could be done in 5 steps:
1. set data folder path
2. set countries/country
3. set event(s)
4. set data time range
5. plot graph(s) 

##### 1. Set input data

###### 1.1 To use default data
from DesInventar and EM-DAT, call [`visualiser.set_data_folder()`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/example.py#L4):

```python
visualiser.set_data_folder('./data')
``` 

###### 1.1.1 To choose sliced or orignial data 
Switch between [`Folders.unsliced`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/visualiser/_config.py#L7) and [`Folders.sliced`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/visualiser/_config.py#L7).

To choose sliced data:

```python
__SELECTED_FOLDER = Folders.sliced
```


To choose unsliced data:

```python
__SELECTED_FOLDER = Folders.unsliced
```
###### 1.2 To deploy the tool for other data resource
Call [`visualiser.set_data_folder()`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/example.py#L4) to set the path of target csv file. The data should be organized in format of 
`[Country Name]/[Hazardous Events.csv]`

The data should contians these colomns:

| deaths         | directly_affected       | indirectly_affected	| start_date	 | secondary_end	 |
|----------------|-------------------------|---------------------|-------------|----------------|
| 0              | 100                     | 200               	 | 1911-02-18  | 1911-02-21     |
| 5              | 60                      | 300               	 | 1912-02-18  | 1912-02-21     |
| 3              | 100                     | 100               	 | 1914-02-18  | 1914-02-21     |
| 10             | 220                     | 400               	 | 1916-02-18  | 1916-02-21     |



##### 2.1 See available countries 
Call [`visualiser.get_available_countries()`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/example.py#L5) after setting the path of csv data file. 

```python
print(visualiser.get_available_countries())
```


###### 2.2 Switch between single and multiple countries
This tool supports stimultaneous access to graphs of different disasters in single or multiple countries, by switch between setting contents in variable [`countries`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/example.py#L6) and [`country`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/example.py#L7). 

To access graphs of multi-countries:

```python
countries = ["<country name>", "<country name>", ..]
```


To access graphs of single country:

```python
country = "<country name>"
```


##### 3. Switch between single and multiple events

Similar to step 2:

```python
events = ["<hazardous event name>",
          "<hazardous event name>", ..]
```

or

```python
event = "<hazardous event name>"
```

##### 4. Choose time range
By adding the last parameter `years_required` in [`visualiser.plot_exceedance_curves()`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/example.py#L17), use data from past `<years_required>` years, or leave it blank to use all available data.

```python
visualiser.plot_exceedance_curves(
    countries,
    events,
    [visualiser.Loss.deaths, visualiser.Loss.affected_people],
    <years_required>
    )
```

##### 5. Plot and Table
Run the script and you get the graphs you want.
To be noticed, the tool also provide a method to extract key return peroid and organized as a table. The table can be easily accessed by calling [`visualiser.get_exceedance_table()`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/example.py#L22):

```python
tables = visualiser.get_exceedance_table(countries, events,15)
```

## License

[MIT](https://choosealicense.com/licenses/mit/)

## Authors

- Dekun Zhang    [@DekunZhang](https://www.github.com/DekunZhang)
- Hardik Agrawal [@Hardik2239](https://www.github.com/Hardik2239)
- Yuhang Zhou    [@1756413059](https://www.github.com/1756413059)
- Jucheng Hu     [@smgjch](https://www.github.com/smgjch)

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

#### Install dependencies in any preferred way

- Using conda ([Anaconda](https://docs.anaconda.com/anaconda/install/index.html) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html))
```bash
conda env create -f conda_env.yml
conda activate data-visualiser
```

- Using pip ([Python 3.10+](https://www.python.org/downloads/))
```bash
pip install -r requirements.txt
```
#### Optional steps (if you want to use the processed data in the repository):
1. Get data using data-downloader module 
2. Process data using data-processor module

### Usage

#### To run the example
The example shows a typical case which produce the return period - deaths & 
affected people graphs for floods and earthquakes in Albania and Pakistan. Data 
used from past 15 years.

```bash
python example.py
```
A typical process could be done in 3 steps:
1. set data folder path
2. plot graph(s)
3. get table(s)


##### 1. Set input data

To use default processed data:

```python
visualiser.set_data_folder('./data')
``` 
Then you can get the available countries for analysis by
calling [`visualiser.get_available_countries()`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/example.py#L5)
after setting the data folder.

```python
print(visualiser.get_available_countries())
```

##### 2. Plot graph(s)

API for plot exceedance curves:
```
visualiser.plot_exceedance_curves(
    countries,
    events,
    losses,
    years_required
)
```
Args:
- countries: A string or list of strings specifying the countries. 
- events: A string or list of strings specifying the events. 
- losses: A Loss enum or list of Loss enums specifying the losses. 
- years_required: An int specifying the maximum number of years of data 
  required. Default is -1.


##### 3. Get table(s)
The tool also provide a function to extract key return period for all metrics 
defined and organized as a table. The table can be easily accessed by calling 
[`visualiser.get_exceedance_table()`](https://github.com/COMP0016-IFRC-Team5/data-visualiser/blob/main/example.py#L22):

```
tables = visualiser.get_exceedance_table(
    countries,
    events,
    years_required
)
```

## Customise

### Add loss metrics
Currently, we only defined deaths and affected people (directly affected + 
indirectly affected). If you want to add more metrics, you can modify it at
`visualiser/_models/_loss.py`.

### Change folder to conduct analysis
In `visualiser/_config.py`, you can modify `__SELECTED_FOLDER` to the folder 
that you want to conduct analysis.

### Change labels and highlight points
You can find relevant code in `__add_label()` method and `__highlight()` method
for `Plotter` class

### Add new data source
If you want to use another data source, you need to put the data source under 
the `data` directory and ensure the folder structure is:
```text
data-visualiser/
├─ data/
│  ├─ new_data_source/
│  │  ├─ country_name/
│  │  │  ├─ EARTHQUAKES.csv
│  │  │  ├─ FLOODS.csv
│  │  │  ├─ STORMS.csv
```
For each csv file, the data should be parsed to contain these columns: `deaths`, 
`directly_affected`, `indirectly_affected`, `start_date`, and `secondary_end`.  
For example:

| deaths | directly_affected | indirectly_affected	 | start_date | secondary_end	 |
|--------|-------------------|----------------------|------------|----------------|
| 0      | 100               | 200               	  | 1911-02-18 | 1911-02-21     |
| 5      | 60                | 300               	  | 1912-02-18 | 1912-02-21     |
| 3      | 100               | 100               	  | 1914-02-18 | 1914-02-21     |
| 10     | 220               | 400               	  | 1916-02-18 | 1916-02-21     |

Next, you need to add a member in `visualiser/_adapters/_folders.py` with value
being the name of the data source folder.

Then, you need to modify `__SELECTED_FOLDER` in `_config.py`.

Note: you need to ignore or remove the labels after plot the curves if you are 
working with new data sources.

## License

[MIT](https://choosealicense.com/licenses/mit/)

## Authors

- Dekun Zhang    [@DekunZhang](https://www.github.com/DekunZhang)
- Hardik Agrawal [@Hardik2239](https://www.github.com/Hardik2239)
- Yuhang Zhou    [@1756413059](https://www.github.com/1756413059)
- Jucheng Hu     [@smgjch](https://www.github.com/smgjch)

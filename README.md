<h1>VAMPS API Interaction</h1>  

The VAMPS-API is a newly written set of functions that allow directly querying the VAMPS database for project data and images that go directly to your local drive. These functions still require logging in but the web interface GUI is not used or needed for this. Once logged in you can get metadata, dataset_ids, and search for projects by name or within a geographic range.
    
All functions (except get_image) return a JSON object so you can parse and display the data from your scripts. 

**Prerequisites**:
* Anaconda

**To Use:**  
* Clone this repository using git
* Create the conda environment and kernel using `conda env create -f environment.yml`
* Activate the environment, following Anaconda's instructions
* Run Jupyter notebook by typing `jupyter notebook` in terminal. If you encounter the error `IOPub data rate exceeded` when running the notebook, then exit out of jupyter in the terminal using Ctrl-C and then run `jupyter notebook --NotebookApp.iopub_data_rate_limit=10000000000` instead
* The folder/notebook will now be available on the Jupyter dashboard 
* Open the notebook `using_VAMPS_API_for_MVCO_datasets.ipynb`
* Make sure the right conda environment is selected as the kernel by selecting "Change kernel" on the "Kernel" menu, and select `conda env:vamps_api`
* Follow further instructions in the notebook

**API Functions**

* Get all metadata for a project.
* Search for metadata by name (returns a project list)
* Search for projects in a geographic location (returns a project list)
* Search for projects by name or substring (returns a project list)
* Get project information (owner, public_status, title, description)
* Get images (The same as on the visualization page from the GUI)
    Including: distance_heatmap, piecharts, barcharts, counts_table, metadata_table 
      alpha_diversity_table, dendrogram, frequency_heatmap

### Get Dataset IDs:
> You might want dataset_ids if you want an image or data comprising
> more or less that a complete project.
```python
data = {"project":"ICM_LCY_Bv6"}
r = s.post(conn['hosturl']+'/api/get_dids_from_project', timeout=15, data=data)  
result = json.loads(r.text)
```
#
### Get Project Metadata:
* Will return metadata in JSON format. 
> If you want a tabular csv file use 'image':'metadata_csv' with the create_image function.
> See notebook for more on this
```python
data = {"project":"ICM_LCY_Bv6"} 
r = s.post(conn['hosturl']+'/api/get_metadata_from_project', timeout=15, data=data)  
result = json.loads(r.text)
```
#
### Get Project Information:
```python
data = {"project":"KCK_LSM_TBS"}
r = s.post(conn['hosturl']+'/api/get_project_information', timeout=15, data=data)  
result = json.loads(r.text)
```
#
### Find Available Projects.
 ```python
 data = {
    'search_string':'',  # If not empty will search for projects with string in 
                         # project name, title or description (case insensitive)
    'include_info':''    # if present, data will include project information
 }
 r = s.post(conn['hosturl']+'/api/find_user_projects', timeout=15, data=data) 
 result = json.loads(r.text)
 ```
#
### Get Projects in Geographic Region
 * data: JSON Decimal Degrees; 
```python 
data = {'nw_lat':'42','nw_lon':'-75','se_lat':'40','se_lon':'-70'}
r = s.post(conn['hosturl']+'/api/find_projects_in_geo_area', timeout=15, data=data)  
result = json.loads(r.text)
```
#
### Find Projects by Metadata String
 * data: JSON substring to search all project metadata parameters (not values)
```python 
data = {'substring':'aux'}
r = s.post(conn['hosturl']+'/api/find_projects_by_metadata_str', timeout=15, data=data)  
result = json.loads(r.text)
```

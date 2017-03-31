# simview_notebooks
Semi-realtime processing of microscopy data.

# Contents
A Jupyter notebook featuring python code for quickly highlighting large regions of correlated pixels in volumetric imaging data.

# Requirements
* Python scientific computing stack.

    (e.g., [anaconda](https://www.continuum.io/downloads))

* [Thunder](http://thunder-project.org/)

    Install via pip:
    
    `pip install thunder-python`

* Pyspark & configured compute cluster

    Instructions for using the Janelia compute cluster can be found [here](http://wiki.int.janelia.org/wiki/display/ScientificComputing/Janelia+Compute+Cluster) on the Janelia intranet. 
   
    To log into the compute cluster on a Linux / OSX system: type this in the terminal: 
    
    `ssh login1.int.janelia.org`
        
    To log into the compute cluster on a Windows system : use [putty](http://www.putty.org/)  to connect to `login1.int.janelia.org` via `ssh`           
            
    Once you log in to the cluster, you will need scripts for launching spark jobs on the Janelia compute cluster. Instructions for getting the required scripts and environment variables set up can be found here: [spark-janelia](https://github.com/freeman-lab/spark-janelia)


# How to use this code 

log into the janelia compute cluster:

`ssh login1.int.janelia.org`

Schedule a spark job. To request 10 workers, use this command:

`spark-janelia launch-in -n10`

Once your job is ready, you will be logged in to a new node on the cluster. For simple debugging of your Spark settings, you can start up Python with Spark by running 

`spark-janelia start`

If Python and Spark are configured properly, this command will start a Python interpreter with a special variable `sc` used for parallelizing data across the cluster. To test the basic operation of the Spark cluster, run 

```python
sc.parallelize([0,1,2,3,4,5,6,7,8]).count()
```

If everything is working, you should see this (the total number of elements in the list we just parallelized): 
```python
9
```

Once it's clear that Python and Spark are working, it makes sense to start Spark with a Jupyter notebook. To do this, run 

`spark-janelia start -b`

This starts a server on the cluster which you can connect to via a browser on any computer connected to the secure Janelia network. To connect to that server, open a web browser and type 

`https://XXXX.int.janelia.org:9999`

where `XXXX` is the address of the node you are running the notebook from. If you have trouble finding the address of the node hosting the notebook server, open a new terminal window connect to `login1.int.janelia.org`, and type `qstat`. You will see a result like this: 

```zsh
[bennettd@h05u07 ~]$ qstat
job-ID     prior   name       user         state submit/start at     queue                          jclass                         slots ja-task-ID
------------------------------------------------------------------------------------------------------------------------------------------------
  17384173 2.16575 spark-clus bennettd     r     03/30/2017 17:02:51 hadoop2@h02u21.int.janelia.org spark.default                      3
  17384174 1.18068 QLOGIN     bennettd     r     03/30/2017 17:02:53 interactive.q@h05u07.int.janelia.org
  ```
  
Look for a node running job named `QLOGIN`, in this case `h05u07`. This is the node you use for hosting the Jupyter notebook.
    
Once the Jupyter Notebook server is running, use the browser to navigate to the notebook you want to run and click on it. This opens a new tab in your browser where code can be executed.

When everything is finished, release your cluster resources by running 

`spark-janelia destroy`

Alternatively, you can delete specific cluster jobs by running 

`qdel XXXXXXXX`

where `XXXXXXXX` is the task ID of your job (you can find the task IDs in the first column of the output from `qstat`)


  

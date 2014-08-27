wfdb-scaling
============

Improving the scalability of the WFDB Toolbox for Matlab and Octave


Abstract
--------
The PhysioNet WFDB Toolbox for MATLAB and Octave is a collection of functions for reading, writing, and processing physiologic signals and time series used by PhysioBank databases. Using the WFDB Toolbox, researchers have access to over 50 PhysioBank databases consisting of over 3TB of physiologic signals. These signals include ECG, EEG, EMG, fetal ECG, PLETH (PPG), ABP, respiration, and others. 

The WFDB Toolbox provides support for local concurrency; however, it does not currently support distributed computing environments. Consequently, researchers are unable to utilize the additional resources afforded by popular distributed frameworks. The present work improves the scalability of the WFDB Toolbox by adding support for distributed environments. Further, it aims to supply several best-practice examples illustrating the gains that can be realized by researchers leveraging this form of concurrency.


Overview
--------
This project aims to provide simple instruction and examples for researchers familiar with the WFDB Toolbox who are insterested in taking advantage of distributed computing environments. Specifically,

1. deploying an appropriate Amazon EC2 cluster, and
2. running their code on this cluster.


Cluster Deployment
------------------
Deploying and managing a cluster is often a non-trivial task. Therefore, we will be using [StarCluster](http://star.mit.edu/cluster) to quickly and simply create a suitable cluster on Amazon EC2.


### Installing StarCluster
Detailled steps can be found in [StarCluster's installation documentation](http://star.mit.edu/cluster/docs/latest/installation.html). 

However, many users can install the project using `pip`:

```sh
pip install StarCluster
```


### Configuring StarCluster
Detailled steps can be found in [StarCluster's quickstart guide](http://star.mit.edu/cluster/docs/latest/quickstart.html). 

However, for a WFDB-specific cluster the following steps should be followed. First, create a config file by running:
```sh
$ starcluster help
StarCluster - (http://star.mit.edu/cluster)
Software Tools for Academics and Researchers (STAR)
Please submit bug reports to starcluster@mit.edu

cli.py:87 - ERROR - config file /home/user/.starcluster/config does not exist

Options:
--------
[1] Show the StarCluster config template
[2] Write config template to /home/user/.starcluster/config
[q] Quit

Please enter your selection:
```
Enter 2 to create a new config file from the template. Second, update the template to correctly specify your `[aws info]`:

```
[aws info]
aws_access_key_id = #your aws access key id here
aws_secret_access_key = #your secret aws access key here
aws_user_id = #your 12-digit aws user id here
```

Third, create a WFDB keypair for accessing the instances that will be created:

```bash
starcluster createkey wfdbkey -o ~/.ssh/wfdbkey.rsa
```

Finaly, add WFDB-specific cluster templates from this project's [config](config) to yours:

```bash
curl https://raw.githubusercontent.com/tnaumann/wfdb-scaling/master/config >> ~/.starcluster/config
```


Quickstart VM
=============

The Mouse Light Acquistion Pipeline virtual machine is a self-contained instance of the complete pipeline system. It contains two example projects that demonstrate pipeline processing and additional ones may be added. A VMWare version is available for download.  For other virtual machine applications, please consult the application documentation regarding importing or converting VMWare VM instances.

Overview
--------

* Download and install VMWare Player, VirtualBox, or a similar application
* Download the virtual machine and unzip the archive (https://janelia.figshare.com/projects/MouseLight_Acquisition_Pipeline_VM/63212)
* Open the virtual machine in your host application and follow the instructions in the ReadMe included in the virtual machine archive or below

Using the Virtual Machine
-------------------------

**Please Note:** The project titled "Example" will operate direcly from this virtual machine image.  However the project titled "Brain 5x5x5 Section" requires the input data to be downloaded to your host machine. 

* Open and run the virtual machine in VMWare Player (free for individual or non-commercial user), Workstation, Fusion, or equivalent
* Log in to the virtual machine with user:pass mluser:pipeline
* View and control the activity pipeline system within the virtual machine by double-clicking the ViewPipeline desktop shortcut to open the web user interface
* View and control activity the pipeline system from your host machine or on your network by determining the virtual machine IP address (e.g., via `ifconfig`) and browsing to that `http://<vmip>:6101`.  For exampe, it may be something like `http://192.168.1.105:6101` on an internal network.

Operate the Example Project
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The example project is a small subsection of actual data where the source images have been truncated to allow for reasonably fast processing of tiles (seconds/minutes) for a small number of tiles to be able to view and explore the different stages of a functional pipeline project as well as to fit the required data into the virtual machine image without increasing the size greatly.

By default the Example project is activated, however the individual data processing stages are not,  Otherwise they may finish processing before you have a chance to explore the system.  When you are ready, select the `Stages` panel from the left, select the `Example` project from the dropdown in the upper right of the panel and start the `Line Fix` stage.  After a few moments tiles will being processing (generally only one or two at time depending on the stage, again due to keeping the requirements of the virtual machine - in this case memory and CPU - relatively low).  The downstream stages can be turned on as well, for the full project to run, or you can manage stages on and off individually if you would like to follow tiles through the system.

Operate the 5x5x5 Section Project
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The 5x5x5 project uses a subsection of actual data where the source images have *not* been altered in any way.  However to run this project you must download the data files to your host machine and place them in a shared folder to the virtual machine.

* Download the 5x5x5 project data
* Place the data in a directory named `pipeline` somewhere on the host machine
* In the virtual machine settings, add a shared folder by selecting that directory named `pipeline`

This will mount the data from the host machine in the virtual machine in a manner that is preconfigured in the 5x5x5 project.  Note that you can mount data in other ways (see below), however the example projects are preconfigured to work with this particular naming and mounting scheme. 

Processing Data from your Host Machine
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Any folders added as a shared folder for the virtual machine will appear under `/external/` to pipeline services in the virtual machine in and in their respective containers.  For example, the 5x5x5 project above is pre-configured to read data from `/external/pipeline/018-08-01_raw-5x5x5-tileid-13024` by sharing a directory named `pipeline` on the host with the 5x5x5 data (in a directory titled `018-08-01_raw-5x5x5-tileid-13024`).  If you were to also share a directory named `testdata` in the virtual machine settings, its contents would be accessible to the pipeline services as `/external/testdata`.

Frequently Asked Questions
^^^^^^^^^^^^^^^^^^^^^^^^^^

Where is the output?
""""""""""""""""""""

The output from the `Example` project is located in `/data/pipeline-output/sample`.  There is an auto-generated directory for each stage labeled with the stage depth and name.

The output from the `5x5x5` project is in a directory named `output` in the same `pipeline` directory that was mounted with the input data.

These settings can be changed by changing the output location for each of the stages in the projects.

Why is the Classifer stage named "Classifer (test)" and use the task "Axon Test"?
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

The `Example` project uses modified input data that only contains 81 tiles and whose input tile images are reduced in size.  This makes the project small enough to include in the virtual machine and provide a functional example.

As a result, the stage classifier also uses a modified ilastik project that does not do the actual classifier stage for real samples.  The overall behavior of the stage, and the project is identical to using the actual classifier stage.  For real sample processing, it is possible to duplicate the sample project and simply change the task used in the duplicated classifier stage to "Axon UInt16" (the real classifier task) rather than the "Axon Test" task.

How do I add more workers?
""""""""""""""""""""""""""

* Clone or copy additional instance of the pipeline virtual machine

    * Note that if you do a normal copy you may need to use the facilities in your virtual machine application (VMWare, or VirtualBox) to generate a new MAC address for the copy (the Clone command in VMWare does this automatically)

* In any worker-only copy, modify the system to not launch the core pipeline services at startup

    * `sudo systemctl disable pipeline`

* Update the worker-only copy to use your original instance with the full pipeline services running

    * In `/data/pipeline-systems/pipeline-worker` open `options.sh in` your editor of choice (e.g., nano is installed)
    * Change `PIPELINE_CORE_SERVICES_HOST` and `PIPELINE_API_HOST` from `172.17.0.1` to the IP address or hostname of the original virtual machine running the full pipeline services (use `ifconfig` in the virtual machine or similar)

For a third or more worker, duplicate the worker-only instance and only the first step is necessary.

Can I reset the sample project and run it again?
""""""""""""""""""""""""""""""""""""""""""""""""

Yes.  On the Stages panel, select the first stage in the project ("Line Fix" for the sample project) in the stage table.  With the stage selected, choose Completed from the drop down in the upper left the stage tile list below the stage table.

Choose "Resubmit All" from the upper right of the tile list.  If you have made changes and some tiles have failed, also select Failed from the drop down in the upper left and "Resubmit All" again.

It will take a few minutes for the first stage tiles to revert to "Queued" status and for that status to propogate down through the rest of the stages.

A second option is to duplicate the project, which will create a second project and a full second set of stages.  You will then need to adjust the root location for the project (and/or copy the input file to that location), and likely will want to change the stage output locations from their defaul ("copy" appended to the original).

How do view I logs for the services?
""""""""""""""""""""""""""""""""""""

In `/data/pipeline-systems/pipeline/pipeline-deploy-services` the script `./pipeline-logs.sh` will load a Docker container with access to the log storage.  `cd /var/log/pipeline` to review logs from the pipeline-api, pipeline-scheduler, and pipelient-client services.

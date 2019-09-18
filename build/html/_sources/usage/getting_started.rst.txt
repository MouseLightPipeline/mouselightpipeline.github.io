Getting Started
===============

Overview
--------

A Pipeline ``project`` consists of a root pipeline ``stage`` that is based on an input source, such as the output tiles during acquisition, and one or more downstream stages that perform data processing. The root stage itself does not perform any processing, but simply monitors the acquisition or other input source for existing or new data and then stores the required metadata for downstream stages.

Any stage, including the root stage, can have one or more downstream stages. These can be added or removed at any time, including while processing is active. Each stage defines its functionality, typically through a shell script that calls a processing function written in Python, MATLAB, or any other language or tool. That stage function is called for each tile or unit of work in the project once the parent stage has successfully completed its processing. Success or failure of the processing of each tile for each stage is tracked, logged, and reported.

The two primary elements of the system are the coordinator and the workers. The coordinator monitors workers, defines each project including creating and removing downstream stages, and is used to control which pipelines and stages are actively processing. Workers are distributed across multiple machines and execute the stage functions for individual each tile. They may do so either by perform the processing locally or by managing the submission and monitoring to some form of remote compute cluster. Workers may be added or removed from the system at any time as well as configured to take on more or less processing load as appropriate.
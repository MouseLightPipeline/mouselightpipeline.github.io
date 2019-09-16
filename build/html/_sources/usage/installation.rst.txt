Installation
============

This is a general deployment configuration for running the core services (e.g., databases), coordinator (api, scheduler, and client), along with specific deployments for workers on vega, sirius, and canopus (local and cluster proxy).

The system is built from:

* The default Compose file to launch core services
* A Compose file to add coordinator services
* A Compose file to add worker frontend clients (worker backend/api services must be local to the appropriate machine)
* There are a number of required environmental variables in each of the Compose files. For a new installation, copy and paste .env-template as .env and change the values as appropriate. The defaults in the template are configured for the standard vega, canopus, sirius systems.

In general it is preferable to run one of following systems for a given deployment directory and Compose project name. It is possible to change back and forth between the different deployment modes, however you must maintain the project name and associated between the scripts for each mode.

Full System Mode
^^^^^^^^^^^^^^^^^^^^^^

To manage the full system that includes all required services plus the worker frontend services for vega, canopus, and sirius use the up/stop/down/logs scripts. These scripts include all required Compose files for the full system.

By default the Compose project will be named "pipeline". If you plan to run multiple systems systems, or would like to override the project name, copy and paste options-template.sh as options.sh and change the PIPELINE_COMPOSE_PROJECT variable name.

Required Services Only
^^^^^^^^^^^^^^^^^^^^^^

To load the core services and required coordinator services, but not include any worker related services specific to vega, canopus, sirius, or others, use 'base.sh' in place of 'up.sh'. It also uses the options.sh file and default project name so can be used in conjunction with the stop/down/logs scripts.

Core Services Only (Development Mode)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The default Compose file will only load the core services, which are off-the-shelf containers and do not include any pipeline specific containers. This configuration is primarily useful for development where the pipeline-specific services will be operated manually outside of containers.

Because it is the default Compose file, it can be started without specific Compose flags, e.g., docker-compose up. To integrate with also running in the other modes, use dev.sh or the -p flag to specify a project name that matches the default or your override in options.sh.
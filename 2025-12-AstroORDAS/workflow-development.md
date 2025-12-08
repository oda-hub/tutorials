# Converting your analysis workflow to MMODA service

This tutorial demonstrates how to (almost) automatically convert your analysis/simulation in Jupyter notebook to the MMODA live service.

It covers the basics, more details are available in [the guide](https://odahub.io/docs/guide-development/) (but this tutorial is already adapted to using the current version of [Renkulab](http://renkulab.io)).

We will illustrate the process by making a simple combination of two MMODA data products. You are encouraged to try to use your own  workflow, if you have one in mind. 

## Project repository initialisation

- Create new project in the gitlab group 
https://gitlab.in2p3.fr/mmoda/workflows/
(intialize with README to simplify following steps).
You need to log-in into this gitlab instance. The simplest way is by using EduGain. Then ask for a repository access rights.

- You can use any editor for the notebook, e.g. you can start the local jupyterlab instance. But we suggest using [Renkulab](https://renkulab.io). 
- Add `requirements.txt` file (you can do it via gitlab web interface, so that your Renku environment will have needed dependencies installed). In the example we don't use much, so only 
  ```
  oda-api
  ```
is enough.

- Go to `renkulab.io`, login there with whatever account. Configure intergation of _Gitlab IN2P3_ in the top-right corner _Integrations_ menu.

- Create new Renku project, add gitlab code repo there and create a _session launcher_ using _Create from code_ option.

- Launch  Jupyter session.

## Querying MMODA product to be reused

- In the [main MMODA instance](https://www.astro.unige.ch/mmoda/) we enter **GRB170130A** into the object name field and click resolve. The _Start time_ and _End time_ fields will be updated.
- We select "INTEGRAL SPI/ACS" instrument and click "Submit" button.
- We select "Polar" instrument and "Submit" again.
- In both tabs, when data product is ready, we can get a python code to access the same data using `oda-api` python library by clicking "API code" button. These codes can be reused in the new workflow.

## Creating notebook
- Copy codes into the notebook, create an additional cell before and declare parameters there as variables.
- Add a _cell tag_ `parameters` to it using JupyterLab UI.
- We annotate input parameters using MMODA-specific [ontology](https://odahub.io/ontology). See [guide](https://odahub.io/docs/guide-development/#how-to-define-input-parameters-in-the-dedicated-parameters-cell) for more details. Basically, first 3 parameters are default parameters for the top panel, the annotations are fixed for them
```python
src_name = "GRB170130A" # oda:AstrophysicalObject
time_start = "2017-01-30T07:14:10.000" # oda:StartTimeISOT
time_stop = "2017-01-30T07:15:55.000" # oda:EndTimeISOT
time_bin_spi = 1 # oda:Float, oda:second
time_bin_polar = 1 # oda:Float, oda:second
```

- Now we do some manipulations over the obtained data and produce a combined product. In this case, for example, we will just plot two normalised lightcurves together. See [example notebook](./combine_lightcurves.ipynb).
- We declare and annotate the output product in a cell tagged as `outputs`.

## Triggering conversion

- To trigger a conversion, first commit the changes in an interactive session and push to the origin main branch.
- Go to gitlab - Settings->General and set "live-workflow-public". This will signalise that the repo is ready to be converted and you want it to appear as a public tab. The conversion bot will start working on it and building an MMODA backend. 
The service will appear in a staging instance https://staging.odahub.fr/mmoda

_NOTE: This conventions are subject to change_

**When ready, we can query the new service varying parameters and get a result. Let's try for another GRB170320A**

The Notebooker webapp
=====================

Notebooker's primary interface is a simple webapp written to allow users to view and
run Notebooker reports. It displays all results in a handy grid, and allows for rerunning
and parameter tweaking.
The entrypoint used to run Notebooks via the webapp is the
same as the external API, so as long as you are using the same environment (e.g. within
a docker image) you will get consistent results.


Report dashboard
----------------
The home page of the Notebooker webapp displays an overview of all reports which have recently run.
It is possible to view each full report by clicking "Result". It's also possible to rerun, delete, and
copy parameters of each report in the grid.

.. image:: /images/notebooker_homepage.png
   :width: 400
   :alt: Screenshot of Notebooker webapp homepage


Running a report
----------------
| First, you should click "Execute a Notebook" which is at the top of every page within the Notebooker webapp.
  This brings up a sidebar which shows the available results. Click on one to go to the "run report" interface.

.. image:: /images/sidebar.png
   :width: 400
   :alt: Screenshot of Notebooker webapp sidebar

| On the "Run a report" interface, you can see "Customise your report" on the
  left side and a preview of your notebook on the right. Here you can see where the parameters cell is,
  and therefore what can be overridden in the "Override parameters" box, highlighted in yellow.

.. image:: /images/nbkr_run_report.png
   :width: 400
   :alt: Screenshot of Notebooker "Run Report" interface

.. warning::
    In order to prevent users having to write JSON, the Override parameters box actually takes raw python statements
    and converts them into JSON. Therefore, it is strongly recommended that you run Notebooker in an environment
    where you either completely trust all of the user base,  or within a docker container
    where executing variable assignments will not have any negative side-effects.

Customisable elements:

* Report Title - the name of the report which will appear on the main screen and email subject upon completion. Can be left blank.
* Override parameters - the values which will override the parameters in the report (in python). Can be left blank.
* Email to - upon completion of the report, who should it be emailed to? Can be left blank.
* Generate PDF output - whether to generate PDFs or not. Requires xelatex to be installed - see :ref:`export to pdf`

Viewing results
---------------
| Once you have started running a report, a progress screen will show you the current status of the report
  including its status, a live-updating log of stdout/stderr, and additional metadata in the sidebar.

.. image:: /images/nbkr_running_report.png
   :width: 400
   :alt: Screenshot of Notebooker "Running Report" interface

If the job fails, the stack trace will be presented to allow for easier debugging.

.. image:: /images/error.png
   :width: 400
   :alt: Screenshot of an error


| If the job succeeds, the .ipynb will have been converted into HTML for viewing on this page.
| You can also get to this view by clicking the blue "Result" button on the homepage.
| If you are using a framework such as seaborn or matplotlib, the images will be available and served by the webapp.
| If you are using plotly, you can use offline mode to store the required javascript within the HTML render,
  or using online mode (recommended) so that the serialised notebook results are not too large.

.. image:: /images/nbkr_results.png
   :width: 400
   :alt: Screenshot of a successful report

It is also possible to either rerun a report from this view, or to clone its parameters. If it was saved as a PDF,
you can download using the button on the sidebar, or you can download as raw .ipynb.


Rerunning a report
------------------
There are three ways to rerun a report in the Notebooker webapp.

1. "Rerun" from the homepage
2. "Rerun" from the result page
3. "Clone parameters" from the result page

The first two options work the same - you rerun the report with exactly the same parameters again.
All reruns have the title "Rerun of <prior report title>".
The latter option, clone parameters, takes you to the "run a report" screen but with the parameters from that
report copied into the "override parameters" box.


Configuring the webapp
----------------------
The webapp comes with premade dummy settings, found in :py:class:`BaseConfig`.
Environments are determined by the NOTEBOOKER_ENVIRONMENT environment variable. When deploying the webapp
you may wish to override the values in these files with your own. To do this, you can either create your own patch
over the Notebooker settings file; or you can override each value with an environment variable of the same name.
The environment variables to be overridden are commented in detail in the settings file.

.. literalinclude:: ../../notebooker/web/config/settings.py
   :language: python
   :lines: 4-38
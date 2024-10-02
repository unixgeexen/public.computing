Splunk Fundamentals 1
=======================
* From org2020 free course

Five main functions of splunk
-----------------------------
* Index data - data tagged with source type, normalised
* Search and Investigate - search based on index - splunk search language
* Add knowledge
* Monitor and alert
* Report and analyze - single pane of glass

Three main building blocks
-----------------------------
* indexer - directories with data by age
* search head - splunk search language - distribute request to indexer, enhance result, dashboard, report, visualisation
* forwarder - forwards data - generally on source server

Scaling
-------
* single instance deployment - input, parsing, indexing, searching
* additional specific components - search head, indexers and forwarders
* clustering of components

Installing splunk
-------------------
* single instance deployment
* free splunk @ splunk.com
* splunk enterprise
* download now
* /opt/splunk/bin - splunk start/stop/restart/help
* go to displayed splunk web site
* splunk cloud - free cloud trial

apps - preconfigured environments - workspaces for specific use cases
roles - what a user is able to see, do and interact with - administrator, power, user

Getting data in
---------------
* types of data input - by admin user
* many ways to get data in
* add data in home app, settings drop down
* upload - unchanging data, monitor - files, directories, scripts, ports, forward - receive from external forwarder (main source in most production environments)
* source type - categorise type of data, pre trained source types
* app context - which app to apply a source type to (system - system wide)
* set index - directory where data is stored - generally split data into different index - limit data search, data retention, data access

monitor option
**************
* source to use

basic searching
---------------
search and reporting app
components
splunk bar
app bar
search bar
time range picker
how to
what to search - data summary - source type, host,

using the search bar - limit by time to limit search data
save as - to knowledge objects
patterns - see patterns in data
statistics - if produced
visualisations - if produced
transforming commands - create statistics or visualisations
search job remains active for 10 minutes by default, share search 7 days
export in various formats
fast, verbose or smart search modes

wild cards are an option in search AND, OR, NOT
fail* NOT password
can use parentheses
"exact phrases"
\ escapes "

from org2020 launchpad
* app - appids
* indexes - prd_syslog
* sourcetypes - netstat
* hosts - active hosts

field sidebar all fields discovered at indexing
selected fields - most important
interesing - at least 20% populated
fields preceded by field type
can add field to selected fields list
all fields link or more fields show all fields - get statistics
field names - case sensitive - values are not
various operators for fields = != > < or NOT host=mail*
status IN ("500","503","505")

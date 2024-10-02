Searching Splunk
=======================
* Pluralsight: Performing basic splunk searches - Thomas Henson

Course Introducion
-----------------------------
* Basic searches
* Field search basics
* Splunk Processing Language (SPL)
* Transformative Searches in Splunk
* Beyond splunk search Lookup 

Searching Machine data
----------------------
* Large amount of machine data - machine generated
* Index (bring in data), Search Head, Forwarder (pushes data) 
* Splunk search interface - Add Data, Splunk Apps, Splunk Docs
* index=prd_osnix appid=a00204
* splunk data options - semi-structured (RegEx), machine generated, often overlooked (dark data)
* Data source - servers via forwarders, cloud apps, workstations, regex (logs etc)
    * splunk generated data Buttercup (sample?)
* Index is a name specifying the type of data e.g. titanic

Splunk Search basics
--------------------
* splunk roles - which roles a user needs, role provides splunk functionalities
    * user - create and edit searches owned by the user
    * power user - edit shared object and alerts
    * admin - perform most splunk search functions
* data storage in splunk
    * data is indexed as it is ingested and becoming searchable environments
        * creates metadata - upload time for data
        * compress data
        * index data
* bucket management
    * bucket like a directory by age of data
    * division is based on data size
    * bucket allocation depends on performance and data size
    * performance requirements
    * storage requirements
    * splunk cluster size
    * lifecycle (defined when creating new index)
        * hot bucket (home path) e.g. 24hr
        * warm bucket (home path) (3 months) - what you need fast access to
        * cold bucket (cold path) (3+ months) - slower storage
        * frozen bucket (thawed path) (1 yr) - end state of our data - need to thaw to access it

Using Field Searches
--------------------
* Search Bar and Timeline
* Search Field Operators
* Navigate search sidebar
* Analyze search result field
* Best practices for search

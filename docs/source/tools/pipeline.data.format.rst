Pipeline Data Format
====================
Much of my work as a system administrator is reformatting the output of standard unix commands into something understandable by a program or script.  The pipeline concept is excellent and would be much more immediately useful if there was a standard data format used in the pipeline and that commands had an option to output that data format.  Within the code there will be an understanding of fields and meanings.  This can be so simply and efficiently output that it's shocking this hasn't occurred on a regular basis for standard unix tools.  Both humans and machines can be easily catered for by having a flag or environment variable to output in the appropriate format.  Having to reformat human readable format leaves much room for error.  Whilst I enjoy the challenge of the reformat why waste our time, repeatedly, doing this task.

There are multiple formats already for transferring data e.g. json, xml.  My aim is to document one for the environment in which I usually work - the unix command line.  Something which is easily used with standard unix utilities.  There are many different configurations on the unix systems I work on, so even though there are nice utilities in languages like perl and python, these are not always available.  I'm aiming for a small toolkit using base unix tools.

Much data is eventually for user consumption.  The various formats should be straight forward for humans to process.

Concepts to use for pipeline data format
----------------------------------------
* csv - very commonly used format for separating fields - which separator to use? - optional specification in file
* escaping - commonly used unix technique which is generally readable, aside the picket fence issue (could use ; - easy to type - not so often used in standard unix environment)
* standard ascii CO separators - FS (file - document), GS (group - page), RS (record), US (unit - field)
* multiple record types - many scripts/programs output multiple types of records - this is easily catered for with a record type field at the start of the record
* records are rows of (fairly) standard fields (columns) - some data is more complex and should probably use xml, json etc
* headers/data types/representation/width - for each record type these can be useful.  Headers are the most common information required but field data type, required representation e.g. data type conversions and field width - could be incorporated into some line numbering scheme header (0), others (-1,-2,-3)
   * headers should be sortable to easily handle removing duplicate headers which can be produce by running the same command over multiple systems
* timestamp/server - this can often be handy to identify the output
   * timestamp - yyyy-mm-dd hh:mm:ss.ttt tz
* server name - don't rely on sshm type wrappers to provide it
* conversion to other formats
* comment marker e.g. default #
* infile tags to specify comment marker, escape char and delimiter?

Ideas in common use
-------------------
* nmon data format
* csv

Use Cases
---------
* human written - use escaping to enter separators
* pipeline - convert separators to ascii CO separators - fields, subfields (not files, maybe records)

Tools
-----
.. code-block:: shell

   nl -w 10 -n rz -s"," # generate line numbering
   sed 's/\(.\)/\1/g'   # convert any escaped character to it's original
   sed 's/\.\,/US/g'   # convert special escaped character to ascii CO separators
   echo "$RecType,0,FieldName1,FieldName2,..."  # Write header line

PowerQuery
==========

Excel
-----
* Three steps
   * Get Data - existing data sources - excel/data/get data
   * Transform - clean and reorganise data - transform button
   * Load - load back into excel worksheet - close & load after transform OR load to skip transform
   * Configure relationships - helps with setting up queries
      * Select Table > Power Pivot > Add to Data Model
      * PowerPivot > Manage Data Model > Select Diagram View (bottom right icon OR View > Diagram View)
      * Build relationships
* Notes
   * Excel functionality doesn't work when using Power Query but you can view excel in a separate window
   * Excel/Data Option/Queries and Connections
   * close & load - saves back to workbook
   * double click on query in Data/Queries view to get back to PowerQuery Editor  

PowerBI
-------

* From Pluralsight - Getting Started with Power BI

* basic types of transforming data
   * slightly different for normal queries and excel queries
   * rename objects - edit queries/select query/update name property/close & apply OR RC
   * combine queries
      * edit queries/select starter query/Combine-Append queries/select tables - can remove applied steps
         * can create groups of queries e.g. not used in model
      * all types of joins - combine - merge - privacy/join type - do the join/expand columns from Table column
      * highlight a column/Remove Duplicates  
   * fixing metadata - edit queries/select table/select row one/use first row as headers
      * select column/update data type
   * filtering rows - standard text/numeric filters from row header
   * eliminating columns - select column headers/remove columns
   * adding columns


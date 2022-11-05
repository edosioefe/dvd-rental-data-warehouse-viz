# dvd-rental-data-warehouse-viz
I have created a datawarehouse for a postgresql dvd rental database. 

The first step for starting the process was deciding what I wanted the fact table to look like and what dimension tables will be in 
the data warehouse. 
From looking at the schema of the source database and the business process, i descided that I would join the rental and payments table together,
that way the rental for one film, its return and payment would make up the grain of the fact table. 
I also had to decide what my dimension tables were going to contain. This part was easy, I just picked tables that would be relevants to the analysis 
of the dvd rentals business process. The dimesion tables i used were:



film table: table was denomalised to contain both details on film and category.
store table: only store id and address id was given, so had to denormalise to add store location onto the table.
customer territory: denormailsed the city and country tables to create the table.
customer: customer table from source database lacked detail, so i created a new customer table using the mockaroo website.
date
hour



The denormalisation of the dimension tables was done so that the data warehouse model would be that of a star schema. This means less joins are required 
during analysis, increasing the datawarehouse's performance.



Once I had my planned the fact and dimension tables, I started creating the tables in Postgresql (pgadmin) for the data warehouse.



For the extraction process, I used Pentaho to connect to the source database. The transformation was also done in pentaho: postgresql queries where used to transform the data in pentaho e.g. complex joins to transform data for the fact table as well as data cleaning and formatting. 
These transformation were then put in a job so that the transformations could be loaded into the data warehoue and the whole ETL process could be automated. 



Problem faced (1):
Due to there being three data columns in my fact table, I knew I would have to have a date dimension and an hour dimension for all three of the columns.
This is problematics because when using Power BI, I would have to make date fields for all three date dimension tables.
Solution:
I created views for each date and hour column in the fact table. The views would contain the corresponding key, thus creating a many to many relationship. 
the views would then be connected to the date dimension and hour dimension table. This meant I only needed to create date fields for the one date dimension 
table. 



Problem faced (2):
There were several null date values, which meant that the surrage keys (being used as foreign keys) belonging to them would also be null.
solution:
I used the number "999" as a surragate key for the date null values. coalesce() was used during the data transformation to accomplish this.
the number would make it easy to have no null foreign keys while identifying which date values are null.



---Find sql scripts used for data transformation








# consumer-goods-processing

As part of this project, Databricks(free tier) is used for all the data loading, transformations, Delta table creation and updations.
Dashboard creation is also worked upon with Databricks Dashboard tool.
•	The data over here is a companies data where the main/parent data is the initial data dump, where as the child companies dimensions and facts are incremental.
•	Here the child company data is not in the same format or cleaned as it is wrt to parent.
•	We are cleaning, transforming and aggregating child’s data before loading to actual parent data.
Steps involved:
Volume created and data imported.
<img width="497" height="150" alt="image" src="https://github.com/user-attachments/assets/b49b083e-7239-48e7-85ac-39a118234e7a" />
 

Workspace created
 
<img width="386" height="211" alt="image" src="https://github.com/user-attachments/assets/2974ea96-9bb0-407a-bcdf-b3dc6762f4bc" />

 
Medallion Architecture followed and schemas created and loaded accordingly
 
Initial Parent company data directly loaded into gold schema
df_costumers = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/Volumes/consumer_goods/files/input_data_files/parent_data/dim_customers.csv")

df_costumers.write.mode("overwrite").saveAsTable("consumer_goods.gold.dim_customers")

df_gross_price = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/Volumes/consumer_goods/files/input_data_files/parent_data/dim_gross_price.csv")

and so on
Child data processing
Widgets created and used  
 
Multiple transformations done
•	Checking for duplicates
•	Trimming data
•	Correcting city names
•	Handling different date formats
•	Basic casting columns data types
•	Null value handling
•	Condition statements like when and otherwise
•	For loop statements
•	etc

city_mapping = {
    'Bengaluruu': 'Bengaluru',
    'Bengalore': 'Bengaluru',

    'Hyderabadd': 'Hyderabad',
    'Hyderbad': 'Hyderabad',

    'NewDelhi': 'New Delhi',
    'NewDheli': 'New Delhi',
    'NewDelhee': 'New Delhi'
}

allowed = ["Bengaluru", "Hyderabad", "New Delhi"]
df_no_duplicates=(
    df_no_duplicates
    .replace(city_mapping, subset=["city"])
    .withColumn("city", 
                F.when(F.col("city").isNull(),None)
                .when(F.col("city").isin(allowed), F.col("city"))
                .otherwise(None))
)
display(df_no_duplicates)
df_orders = df_orders.withColumn(
    "order_placement_date",
    F.regexp_replace(F.col("order_placement_date"), r"^[A-Za-z]+,\s*", "")
)

#Format date after

df_orders = df_orders.withColumn(
    "order_placement_date",
    F.coalesce(
        F.try_to_date("order_placement_date", "yyyy/MM/dd"),
        F.try_to_date("order_placement_date", "dd-MM-yyyy"),
        F.try_to_date("order_placement_date", "dd/MM/yyyy"),
        F.try_to_date("order_placement_date", "MMMM dd, yyyy"),
    )
)
df_orders.show(10)


Cleansed data is loaded into silver tables respectively
Applied aggregations and join on top of silver
Data loaded in final parent company data

Jobs/Pipelines created for dimension loading
 


Fact processing done at file trigger
Job triggered when file is available in location
 

Genie feature utilized 
 







Consolidated view created for Dashboard
Final Dashboard created
 

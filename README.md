# consumer-goods-processing

As part of this project, Databricks(free tier) is used for all the data loading, transformations, Delta table creation and updations.
Dashboard creation is also worked upon with Databricks Dashboard tool.
  •	The data over here is a companies data where the main/parent data is the initial data dump, where as the child companies dimensions         and facts are incremental.
  •	Here the child company data is not in the same format or cleaned as it is wrt to parent.
  •	We are cleaning, transforming and aggregating child’s data before loading to actual parent data.

**Step 1: Download dataset**
	
 	1 Dataset: https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms
 
	2 AdvantureWorksLT2022.bak

Setup SQL database for Microsoft:
**Step 2: Download SQL server**
	
 	1 https://www.microsoft.com/en-IN/sql-server/sql-server-downloads

 	2 Once download the express version
	
 	3 In express edition select basic
	
 	4 Accept term and condition
	
 	5 Then install

**Step 3: Download SQL server management studio**
	
 	1 https://learn.microsoft.com/en-us/ssms/download-sql-server-management-studio-ssms?redirectedfrom=MSDN
	
 	2 Restart for installation

**Step 4: Open SQL server management studio(SSMS)**
	
 	1. Search for SSMS in search bar
	
 	2. Select default server name DESKTOP-09RJQ41\SQLEXPRESS
	
 	3. click connect
	
 	4. Once connect under object explorer you can see below options
		a. Databases 
		b. security
		c. Server object
		d. Replication
		e. Management
	
 	5. Move downloaded AdventureWorksLT2022 backup to path "C:\Program Files\Microsoft SQL Server\MSSQL16.SQLEXPRESS\MSSQL\Backup"
	
 	6. Then we have to restore database that we Gonna connect to SSMS
	
 	7. Just right click on database then select restore database
	
 	8. Under Source click on device
	
 	9. click on three dots ...
	
 	10. Then click Add
	
 	11. Go to path "C:\Program Files\Microsoft SQL Server\MSSQL16.SQLEXPRESS\MSSQL\Backup"
	
 	12. Add Backup database file 
	
 	13. Click ok then let database restored.
	
 	14. Once load successfully you can see on left hand side
		under database you can see AdvantureWorksLT2022
		1. Database diagram 
		2. Table 
		3. Views 
		4. External Resources and many more
	
 	15. Try running table by selecting first 1000 rows

	Here we completed First step by installing Microsoft based SQL Server Management Studio

	B. Create resource Group and create ADF service 	

**Step 5:**
	###############first create user and password under ssms########################			
	#CREATE LOGIN luke WITH PASSWORD = '12345678'					#
	#											#
	#create user luke for login luke								#
	#											#
	#########################################################################
	
**Step 6: Create Resource group :**

	1. Let's start with create resource group. Search resource groups. 
	"https://portal.azure.com/#view/Microsoft_Azure_Marketplace/PlusNew.ReactView/package/hub/additionalConfig~/%7B%7D/selectedMenuItemId/undefined/createLanding/undefined"
	
	2. Click on create.
	
	3. In subscription select Azure subscription
	
	4. In resource group name : write "intech-rg". 
	
	5. Region Canada central
	
	6. Then click Next 
	
	7. Again click next no need insert name & Value
	
	8. Then click create 
	
	9. Under resource group you should see resource 

**Step 7: Now under resource group "intech-rg" create ADF service :**

##############Now let's start creating ADF Service####################

	1. Under resource group select your resource "intech-rg".
 
	2. Then click on create 

 	3. You will navigate to marketplace
	
 	4. Select checkbox azure services only 
  
	5. Search data factory select 
 
	6. Click create 
 
	7. Then navigate to subscription page 
 
	8. Select below details 
	
	A. Basic
	
	Instance details   
	Name:		intech-vishal
	Region:		Canada Central
	Version:	V2
	
	B. Under Git Configuration
	
	Enable: Configure git later
	
	C. No need to change Network | Advanced | Tags 
	
	D. Navigate to review and create 
	
	Then click on create 
	
	Wait until deployment of service 
	
	Once service deploy that means we can start with ADF

**Step 8: Create service azure key vault under resource group "intech-rg"**

	1. Search for Key-Vault
 
	2. create key vault
 
	3. Configuration
	
	A. Basic 
	
	Instance details
	
	Key Vault name: intech-keyvaultvg
	Region: Canada Central
	Pricing tier: Standard  
	 
	B. Dont change configuration Networking Tags
	
	C. Go to review + create 
	
	Click on create 
	
**Step 9: Authorize SSMS(On-Premises) User name & password under Key vault(IAM Service)**

	1. Under key vault Go to IAM 
	2. Search key vault admin
	3. click on that then add you guest as member
	4. click review and assign then click create 
	5. Now go to role assignment 
	6. check whether key vault administrator is assign or not 
	7. Now go to secret 
		under name : username 
		Secret value : luke
		
		under name : password 
		Secret value : 12345678
		
**Step 10: Create Storage account for Gen2 data lake**
	
	Data lake:
	
	Where we Gonna store our data.??
	
	Azure provide central location where we can store our data.
	
		1. Search for data lake 
		2. You will get "Azure Data lake, datalake, Data Lake Gen2" select that storage account
		3. Click on create 
		4. Configurations 
	
	A. Basics
		
		Write Storage account name: intechvg
		Dont chage other details
	
	B. Advanced 
		
		Enable hierarchical namespace
		
	C. Dont change Advanced | Networking | Data Protection | Encryption 
	
	D. Just select Review + create and select create under that
	
	E. Check deployent data lake gen2 is completed or not.
	
	Now navigate to resource group and check data lake gen2 is available or not.
	
	F Under container create Bronze , Silver and Gold container
		
**Step 11: Now connect On-premise SSMS to ADF Pipeline copy data under properties**

	1. Open ADF
	2. Launch ADF 
	3. Go to author under that click on + symbol and select pipeline under that again select pipeline.
	4. Now under activities select source and transform
	5. Under move and transform drag copy data 
	6 Configuration
	
		A. General 
			Name: Copy data
	
		B. Source
			
			1 Click + new search for SQL. Then select SQL server. Click continue
		
			2 Under SetProperties
				Name: SqlServer
			3 Click on Linked service. click new
				Under New Linked service 
					Name : SqlServerOnPremLinkedService
					
					Under Connect via integration runtime :
						select +new
							select self-hosted then --> click continue
								Under Integration runtime setup
									Name : SHIR(Self hosted integration runtime)
									Type: self-hosted
									click create
								Now "Integration runtime setup" will appear 
									Under that 
										Option 1: Express setup
											--> Click here to launch the express setup for this computer
										Let it download then install the file.
										Then click apply
				Now under that Connect via integration runtime select SHIR
			4 *************Server name
			Go to SSMS
				Write click on server name "DESKTOP-09RJQ41\SQLEXPRESS" select properties 
					Under Name : DESKTOP-09RJQ41\SQLEXPRESS copy this 
			Now navigate to new linked service 
				Under server name : DESKTOP-09RJQ41\SQLEXPRESS
			5 **************Database name
			Go to SSMS
				Under databases copy name "AdventureWorksLT2022"
				
			6 Now again navigate to new linked service
				*Database name paste new "AdventureWorksLT2022"
				username : luke 
				*below username Azure Key Vault
				*under AKV Linked service 
					select +new 
						under New linked service :
							Configuration :
								Azure subscription :
									select Azure subscription 
								Azure key vault name :
									intech-keyvaultvg
								Authentication method :
									System assigned managed identity
								Test connection : to Linked service
								Click create 
								
								Now to load secrete name :
									Go to IAM search "vault user" select Key Vault Secret User
									Configuration
										A. Go under members  :
											Under Assign access to :
												select managed identity
											Under members :
												+ select members 
													under that :
														Subscription : Azure subscription 1
														Managed identity : Data Factory (V2) (1)
															select members : intech-vishal
														Then click select
											Under name check intech-vishal
										B. Directly go to Review and assign 
											click on Review + assign 
										Then check whether role get added or not 
				Below AKV linked service 
					Select password 				
				Then click create 
				
			7 Now again navigate to SSMS right click on database "DESKTOP-09RJQ41\SQLEXPRESS"
				Under properties go to security
					under service authentication:
						select SQL server and wireless Authentication mode then click ok
						let sql server restarted 
				Navigate to Edit linked service "SqlServerOnPremLinkedService"
				Test connection 
				And check logs under SSMS under management under current session
				
				Then in SSMS Grant database and table for luke user
	################################################################################		
	#		use AdventureWorksLT2022;							#
	#		GRANT Select ON SalesT.Address TO luke;					#
	################################################################################		
			
			8 Then navigate to SetProperties and check table name is populated or not
				Click ok and preview data


**Step 12 Sink :**
	Steps 
		1. Under sink dataset click new
		2. Search for data lake gen2 click on 
		3. Select parque dataset click ok
		4. Give name to parquet parquetSink
		5. Linked Device click + 
				Name : AzureDataLakeStorageLinkedService
				Authentication type : AccountKey
				Account selection method : From Azure Subscription
				Azure subscription : Azure subscription 1 (f9d130e1-d311-4f9e-a6fd-8170f73eb503)
				Storage account name* : intechvg
				Test Connection : intechvg
		6. Click Create
		7. Set Properties
				Linked service* : AzureDataLakeStorageLinkedService
				Within File path : select file button on right 
				select bronze container of gen2 data storage
				click ok 
		8. Now click debug and see the data under bronze.
		
**Step 13 Drag lookup from Activities**
	SSMS 
	
	GRANT Select ON SCHEMA::SalesLT TO luke; 
	 
	Steps  : 
	
		1. Search for lookups. Drag the lookup
		2. Then click on setting
		3. Under source dataset click on open
		4. Inside table clear table 
		5. Now come to copy_all_pipeline
		6. unchecked first row only.
		7. Under Query select "Query" Radibutton
		Query:
		SELECT
		s.name AS SchemaName,
		t.name AS TableName
		from sys.tables t
		INNER JOIN sys.schemas s
		ON t.schema_id = s.schema_id
		WHERE s.name = 'SalesLT'; 
		8. Now drag foreach
		9. connect with copy data
		10. edit for each 
		11. under that drag copy data 
		12. Click on source
		13. Select radio button query
		14. Click on query box select expression builder 
		15. write Pipeline expression builder
		@{concat('SELECT * FROM', item().SchemaName, '.', item().TableName)}
		16. Then click ok
		
**Step 14 Set sink run time param So that using lookup we can fetch table schema and data**

	Now Sink
	
	Steps:
	
		1. Inside dataset select sink select parquetsink 
		2. Now click Open
		3. Navigate to param
		4. click new 
		Add two params first schemaname other tablename
		5. Now go to copy_all_tables 
		Under sink under dataset properties add value of schemaname : item().SchemaName
														 tablename	: item().TableName
		6. Now open ParquetSink navigate to connection
		7. Under file path click on directory open dynamic expression 
				@{concat(dataset().schemaname, '/', dataset().tablename)}
		8. Now click on filename box open dynamic expression 
				@{concat(dataset().tablename, '.parquet')}
		9. Now click on foreach go to setting click on items open expression builder
		@activity('Lookup1').output.value
		10. Now click on lookup and click on debug 
		
**Step 15 Create Azure databricks Service under resource group**
	Steps 
		1. Search azure databricks
		2. Click on create 
		3. Configuration
		
		A. Within Basics
			write workspace name : intech-databricks
		
		B. No need to change Networking | Encryption | Security and compliance | Tags
		
		C. Go to review and create 
			click on create 
		
		4. Let the deployment successfully and check in resource group for databricks
		
**Step 16 Create Databricks Cluster**

	Databricks
	
		Open resource group then open databricks from there
		
		##########################Create cluster##########################
		
		Configuration
		
		1. From left hand side open compute 
		2. click create compute 
		3. Under Policy select Single node
		4. Performance : Spark 3.5.0, Scala 2.1.2
		5. Node type : Standard_D4ds_v5 16GB Memory, 4 cores
		6. Termination after : 30 mins (To save the cost)  
		7. Click create cluster.
		
**Step 17 : Create bronze to silver notebook and write the code**

	1. In activity search for "notebook"
	2. Connect that with forEach
	3. now click on general give name "bronze_to_silver" 
	4. Go to Azure notebooks under Databricks linked service 
			click +
			Linked new service 
				Name : AzureDatabricksLinkedService
				Connect via integration runtime : AutoResolvedIntegrationRuntime
				Account selection method : From Azure Subscription
				Subscription : Azure Subscription
				Databricks workspace : intech databricks
				Select cluster : Existing interactive cluster
				Authentication type : azure key vault
					For Access token : 
						Go to databricks. Go under settings. Go to developer 
							Go to access token. Click on managed. Generate new token. 
								Write the comment ?	
									"in tech data eng proj".
									Token will generate copy that "dapi171efc80030c3774cc68cff786e29584"
									click done. 
									
									Now go to key vault.
									Under secret under name : databricks-key
									Secret value : dapi171efc80030c3774cc68cff786e29584
									click create
				Now AKV Linked service : AzureKeyVault1
				Secret name : databricks-key
				secret version : Latest version
				Choose from existing cluster : vishal girases cluster
	5. Go to settings 
		Under network path. Browse notebook --> users and bronze to silver notebook.
	6. Drag another notebook for silver to gold and do the same
		under general give name silver to gold 
		databricks linked service : use the same one
		setting : networking path browse notebook
	7. Run all

	
**Step 18 : Create silver to gold notebook and connect it To ADF pipelines**

**Step 19 : Publish and run the pipeline**

**Step 20 : Check the Parque file in gen2 storage**
	1 Check raw Parque files in bronze container
	2 Check Intermediate files in silver container 
	3 Check Target files in Gold container   
	
**Step 21 Linked GitHub repo to ADF and publish the changes**


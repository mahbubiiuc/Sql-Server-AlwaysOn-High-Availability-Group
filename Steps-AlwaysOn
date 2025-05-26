Step 1: Create a User [Name: LankaBDSrvAcc which is a ServiceAccount]
		-> With domain admin privilege [Domain, Schema Admin, Enterprize Admin]
		
Step 2: Create a VCO 
		-> Listener [Name: LankaBDAGListen]

Step 3: Install 
		-> Windows FailOver Cluster
		
Step 4:	Validate and Create 
		-> Windows Failover Cluster 
		-> Name: LankaBDHAG_Cluster
		
Step 5: Assign Cluster [Name: LankaBDHAG_Cluster] into Listener [Name: LankaBDAGListen] security tab property
		
Step 6: Install 
		-> Sql Server and SSMC
		
Step 7: Sql Server Configuration Manager
		-> Sql Server(MSSQLSERVER) Properties 
		-> Enable Always On Availability Group
		-> Enable FileStream All
		
Step 8: Create Login(s)
		On SQL01: CREATE LOGIN [DOMAIN\SQL02$] FROM WINDOWS;
		On SQL02: CREATE LOGIN [DOMAIN\SQL01$] FROM WINDOWS;
		
Step 9: Restore or Create DATABASE

Step 10: Enable Service Broker (if needed)
		-> 	USE master;
			GO
			ALTER DATABASE [DBName] SET ENABLE_BROKER WITH ROLLBACK IMMEDIATE;
			GO
			USE [DBName];
			GO
		-> On SQL01:
			CREATE ENDPOINT [SSBEndpoint] STATE = STARTED AS TCP  (LISTENER_PORT = 4022, LISTENER_IP = ALL ) FOR SERVICE_BROKER (AUTHENTICATION = WINDOWS);
			GRANT CONNECT ON ENDPOINT::[SSBEndpoint] TO [DOMAIN\SQL02$];

		   On SQL02:
			CREATE ENDPOINT [SSBEndpoint] STATE = STARTED AS TCP  (LISTENER_PORT = 4022, LISTENER_IP = ALL ) FOR SERVICE_BROKER (AUTHENTICATION = WINDOWS);
			GRANT CONNECT ON ENDPOINT::[SSBEndpoint] TO [DOMAIN\SQL01$];
		
Step 11:	Create Always On High Availabilty
		-> Name: LankaBDSQL_AG, ClusterType: Windows Failover Cluster
		-> Add Replica(s) [SQL01, SQL02], Set Backup Preferences [Any Replica Or Primary]

Step 12: Grant Permission
		-> 	On SQL01: GRANT CONNECT ON ENDPOINT::Hadr_endpoint TO [DOMAIN\SQL02$];
			On SQL02: GRANT CONNECT ON ENDPOINT::Hadr_endpoint TO [DOMAIN\SQL01$];

Step 13: Add Listener [IF Exception: The WSFC cluster could not bring the Network Name resource with DNS name 'LankaBDAGListen' online. --> check (Step 5)]
		-> Name: LankaBDAGListen [1433, add listener IP]
		

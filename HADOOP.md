# hadoop notes
http://hadoop.apache.org/docs/r1.2.1/hdfs_design.html#Replication+Pipelining


MASTER + SLAVE
---------------
	SAMPLE CONFIG:
		5 MACHINES  POOR MAN CONFIG
			1 MACHINE Resource Manager + NameNode (MASTER)
			4 MACHINE Data Manager + Data Node (SLAVE)

		100 MACHINES : RICH MAN EXAMPLE CONFIG
			1 Machine Name Node (MASTER)
			1 Machine Back up Name Node (MASTER)
			1 Machine Resource Manager (MASTER)
			98 Machine Data Manager + Data Node  (SLAVE)



	EXAMPLE CONFIG:
		EACH SLAVE MACHINE CONTAINS 
			-> NODE MANAGER >> AND << DATA NODE

		EACH MASTER NODE
			->NAME NODE + DATA NODE


	1. DATA WILL BE STORED AND PROCESS ONLY IN SLAVE MACHINE
	2. FILE WILL BE SPLIT INTO 128 MAX SIZE(CONIFGURABLE)
	3. EACH BLOCK WILL BE REPLCIATED 3 TIMES(CONFIG)
	4. EACH REPLICATION WILL BE STOREAD IN A SEPERATE SLAVE MACHINE -> FAULT TOLERANCE
	5. WHEN YOU PROCESS/READ DATA, ANY ONE OF THE REPLICATION WILL BE CONSIDERED


---------
FLUME 
---------
	STREAMING DATA TO HDFS, UNSTRUCTURED
	EXAMPLE TWITTER FEED


---------
SQOOP
---------
	PUSH BATCHES DATA TO HDFS, STRUCTURED DATA
	EXAMPLE RDBMS DATA


----------------
HDFS (STORAGE) READ/WRITE
----------------
(EACH DATA IS REPLICATED 3 TIMES, AND DIVIDED INTO 128 BLOCKS, EACH BLOCK FOR A PARTICULAR FILE IS STOREAD IN A DIFFERENT DATA NODE --> FAULT TOLERANT)

NAME NODE 	(MASTER)	-> METADATA LIST OF BLOCK/FILE THE ENTIRE METADATA IS STORED IN MAIN MEMORY,IF IT THE METADATA LIST IS FULL CANNOT ADD NEW FILES
DATA NODE	(SLAVE)	-> READ+WRITE

REMEMBER THAT THE FILE YOU CAN STORE IN HDFS IS DIVIDED BY THE REPLICATION FACTOR(DEFAULT 3)

SO IF THE QUESTION ASKS YOU HOW MANY FILE SIZE YOU CAN SAVE IN A 48 TB? --> 16TB (IF REPLICATION FACTOR 3)


NAMENODE CONTAIN NAMESPACE(DIRECTORY)
	Example:
		abc.txt
			block1
				slave1
			block2
				slave3

	HADOOP v.1
		-> CAN ONLY HAVE ONE ACTIVE NAME NODE SECONDARY NAME NODE IS USED FOR BACKUP AND CREATION OF NEW NAMENODE IF THE ORIGINAL ONE CRASHED/BURNED/EXPLODED
			-> IF THAT CRASHES
			-> EVERYTHING CRASHES
			-> SINGLE POINT FAILURE(SPOF)

		IF CRASHES
			-> USE SECONDARY NAME NODE TO CREATE/RECREATE NEW NAMENODE
			-> SECONDARY NAME NODE EVERYHOUR BACKUP DATA

	HADOOP v.2
			ACTIVE AND STANDBY NAMENODE --> 2 SERVERS
				IF CURRENT ACTIVE NAMENODE IS DOWN,
					THE STANDBY NAMENODE WILL TAKEOVER
				MAINTAINED BY ZOOKEEPER --> COORDINATION SERVICE, REPLICATING METADATAs

			SECONDARY NAMENODE IS OPTIONAL

	CLIENT --> HADOOP LIBRARY/SETS OF PROGRAMS
	CLIENT READ ANATOMY
		1. gets metadadata from namenode
		2. read data from datanodes

	CLIENT WRITE ANATOMY
		1. CLIENT TAKES DATA AND SPLITS INTO BLOCKS
		2. FOR EACH BLOCK IT TAKES A NUMBER(EX: 3) DATA NODES AND WRITES ALL THE BLOCKS IN PARALLEL, 
		3. EACH BLOCK REPLICATION WILL BE WRITTEN IN 	SERIAL(PIPELINE DATA NODE, WRITES IN THE 	FIRST DATA NODE AND TH EN SEND IT TO THE SECOND NODE TO WRITE IT)

	RACK?
		A RACK IS A SET OF COMPUTER SERVERS

		COMPUTER SERVER?
			YOUR DESKTOP WITHOUT THE MONITOR ONLY RAM + CPU

	IF YOU HAVE MULTIPLE RACKS
		1. REPLICATION ONE WILL BE WRITTEN IN ONE RACK, AND THE OTHER REPLICATIONS WILL BE WRITTEN IN OTHER RACKS

	---------------
	HEARTBEAT(SENT FROM DATANODE TO NAMENODE)
	---------------
		CONTAINS:
			TOTAL STORAGE CAPACITY
			FRACTION OF STORAGE IN USE
			NUMBER OF DATA CURRENCY TRANSFERS
		EVERY 3 SECONDS

		USED FOR NAMENODE BLOCK ALLOCATION/LOAD BALANCING



	!!!!!!!!
	READ HAPPENS UNTIL THE LAST BLOCK WRITTEN
	IF REPLICATION CRASHES, NEED ADMIN MANUALLY DELETE THE UNDERREPLICATE FILE AND RECREATE THE FILE
	DATA NODE CONTAINS CHECKSUMS FOR THE DATA IN THE BLOCK IN A SEPERATE FILE
	!!!!!!!!!!

----------------
YARN (PROCESSING)
----------------

RESOURCE MANAGER (MASTER)
NODE MANAGER	(SLAVE)









-----------------
HADOOP CMD
-----------------
	$ hadoop fs -ls  <<< list all file
	$ hadoop jar <<< to run



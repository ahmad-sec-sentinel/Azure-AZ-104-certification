## Hands on Lab Activities:
### 1. Storage Account basics and its type
### 2. Different types of redundancy

Key Learning: Inorder to use any storage service in Azure, we need to first create a storage account. This is unlike other cloud service providers where each storage service is individually managed. In Azure , all storage services are centrally managed using Azure Storage Account. 

### Azure Storage Services: Blob storage, File share/File storage, Table storage, Queue Storage

- Blob Storage : object storage
Use case: store unstructured data like images, backups, logs, and media; ideal for large binary objects, static website hosting, and data lakes. Best when you need scalable, cost‑tiered object storage with HTTP(S) access.  

- File Storage : managed SMB/NFS shares  
Use case: provide lift‑and‑shift file shares for legacy apps, home directories, or cross‑platform file sharing (Windows, Linux) using SMB/NFS; good for mounting as network drives and replacing on‑prem SMB shares.  

- Queue Storage :message queuing  
Use case: decouple components with reliable message queues for background processing, job scheduling, and asynchronous workflows; use when you need simple FIFO‑style messaging with high throughput and durability.  

- Table Storage :NoSQL key‑value store   
Use case: store semi‑structured, schema‑less data (telemetry, user metadata, catalogs) with fast lookups by partition/row key; choose Table for low‑cost, massively scalable key‑value workloads when relational features aren’t required  

### Types of Storage account   
### 1. Standard general purpose V2   
- This is the most commonly used storage account   
- Allows creation of  all the four storage services.  
- Supports all four types of redundancy (LRS,ZRS,GRS,GZRS)  
- Cheaper than premium accounts
  
### 2. Premium Block Blobs  
- Supports only blob storage  
- Only ZRS ,LRS replication ( Thus it cant have regional availability)  
- More expensive that standard blob storage  
- Used where high performance is needed with high data transaction rate. e.g. gaming application data

### 3. Premium File share  
- Supports only file storage service
- LRS, ZRS only
- supports both NFS and SMB protocol. (Note : A file storage under standard Gen Purpose V2 supports only SMB protocol)
- Use case: for high performamce , when NFS based protocol is a necessity
  
### 4. Premium page blobs  

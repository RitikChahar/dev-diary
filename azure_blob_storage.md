# Azure Blob Storage

## 1. **Docker Compose Setup (Azurite)**
   1.1. Add Azurite service to `docker-compose.yml`
   ```yaml
   services:
     azurite:
       image: mcr.microsoft.com/azure-storage/azurite
       container_name: azurite
       ports:
         - "10000:10000"  # Blob service
         - "10001:10001"  # Queue service
         - "10002:10002"  # Table service
       volumes:
         - azurite_data:/data
       command: "azurite --blobHost 0.0.0.0 --queueHost 0.0.0.0 --tableHost 0.0.0.0 --location /data --debug /data/debug.log"
   
   volumes:
     azurite_data:
   ```
   1.2. Start services
   ```bash
   docker-compose up -d azurite
   ```

## 2. **Python Connection with Correct API Version**
   2.1. **Install Required Package**
   ```bash
   pip install azure-storage-blob
   ```
   
   2.2. **Basic Connection Setup**
   ```python
   from azure.storage.blob import BlobServiceClient
   
   # Local Azurite connection
   connection_string = "DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;"
   
   client = BlobServiceClient.from_connection_string(
       connection_string,
       api_version="2023-11-03"  # Compatible with Azurite
   )
   ```
   
   2.3. **Production Azure Connection**
   ```python
   # Production connection string format
   connection_string = "DefaultEndpointsProtocol=https;AccountName=yourstorageaccount;AccountKey=your-account-key;EndpointSuffix=core.windows.net"
   
   client = BlobServiceClient.from_connection_string(
       connection_string,
       api_version="2023-11-03"
   )
   ```

## 3. **Upload Function Implementation**
   3.1. **Basic Upload Function**
   ```python
   async def upload_file_example():
       container_name = "documents"
       blob_name = "test-file.txt"
       data = b"Hello, Azure Blob Storage!"
       
       # Get container client
       container_client = client.get_container_client(container_name)
       
       # Create container if it doesn't exist
       try:
           container_client.create_container()
       except Exception:
           pass  # Container already exists
       
       # Upload blob
       blob_client = container_client.get_blob_client(blob_name)
       blob_client.upload_blob(
           data,
           content_type='text/plain',
           overwrite=True
       )
       
       print(f"Uploaded {blob_name} to {container_name}")
   ```
   
   3.2. **Upload with File Stream**
   ```python
   def upload_file_from_path(file_path: str, container: str, blob_name: str):
       with open(file_path, "rb") as data:
           container_client = client.get_container_client(container)
           blob_client = container_client.get_blob_client(blob_name)
           blob_client.upload_blob(data, overwrite=True)
   ```
   
   3.3. **Generate Upload URL (SAS Token)**
   ```python
   from datetime import datetime, timedelta, timezone
   from azure.storage.blob import generate_blob_sas, BlobSasPermissions
   
   def generate_upload_url(container: str, blob_name: str, expiry_minutes: int = 60):
       sas_token = generate_blob_sas(
           account_name=client.account_name,
           container_name=container,
           blob_name=blob_name,
           account_key=client.credential.account_key,
           permission=BlobSasPermissions(write=True),
           expiry=datetime.now(timezone.utc) + timedelta(minutes=expiry_minutes)
       )
       
       return f"https://{client.account_name}.blob.core.windows.net/{container}/{blob_name}?{sas_token}"
   ```

## 4. **Azure Storage Explorer Setup**
   4.1. **Installation**
   * Download and install [Azure Storage Explorer](https://azure.microsoft.com/en-us/products/storage/storage-explorer/)
   
   4.2. **Connect to Local Azurite**
   * Click "Connect to Azure Storage"
   * Select "Storage account or service"
   * Choose "Connection string"
   * Enter connection string:
   ```
   DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
   ```
   * Click "Next" → "Connect"
   
   4.3. **Connect to Production Azure**
   * Click "Connect to Azure Storage"
   * Select "Storage account or service"
   * Choose "Connection string"
   * Enter your production connection string
   * Click "Next" → "Connect"

## 5. **Common Operations**
   5.1. **List Containers**
   ```python
   def list_containers():
       containers = client.list_containers()
       for container in containers:
           print(f"Container: {container.name}")
   ```
   
   5.2. **List Blobs in Container**
   ```python
   def list_blobs(container_name: str):
       container_client = client.get_container_client(container_name)
       blobs = container_client.list_blobs()
       for blob in blobs:
           print(f"Blob: {blob.name}, Size: {blob.size}")
   ```
   
   5.3. **Download Blob**
   ```python
   def download_blob(container: str, blob_name: str) -> bytes:
       container_client = client.get_container_client(container)
       blob_client = container_client.get_blob_client(blob_name)
       blob_data = blob_client.download_blob()
       return blob_data.readall()
   ```
   
   5.4. **Delete Blob**
   ```python
   def delete_blob(container: str, blob_name: str):
       container_client = client.get_container_client(container)
       blob_client = container_client.get_blob_client(blob_name)
       blob_client.delete_blob()
   ```

## 6. **Environment Variables Setup**
   6.1. **Create `.env` file**
   ```env
   # Local Development
   AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
   
   # Production
   # AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=yourstorageaccount;AccountKey=your-key;EndpointSuffix=core.windows.net
   ```
   
   6.2. **Load in Python**
   ```python
   import os
   from dotenv import load_dotenv
   
   load_dotenv()
   connection_string = os.getenv("AZURE_STORAGE_CONNECTION_STRING")
   client = BlobServiceClient.from_connection_string(connection_string, api_version="2023-11-03")
   ```

## 7. **Troubleshooting Common Issues**
   7.1. **API Version Error**
   * Always specify `api_version="2023-11-03"` for Azurite compatibility
   * Alternative versions: `"2023-01-03"`, `"2022-11-02"`, `"2021-12-02"`
   
   7.2. **Connection Issues**
   * Ensure Azurite is running: `docker ps | grep azurite`
   * Check port availability: `netstat -an | grep 10000`
   * Verify connection string format (no extra spaces)
   
   7.3. **Container Access**
   * Create container before uploading blobs
   * Handle container creation exceptions gracefully
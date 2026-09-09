# Dataverse-docker

A skeleton Dataverse Docker installation as per:
https://guides.dataverse.org/en/latest/container/running/demo.html

# Before run
Update your 'secret-unblock-key' in demo/init.sh

# Run
```
docker compose -f compose.yml up -d 
```

# Log in
username: dataverseAdmin
password: admin1

# External tool - n8n webhook for XDI file
Update your 'secret-unblock-key'.

Add to Dataverse (via Docker exec).
Update your webhooks as required

```
curl -X POST -H 'Content-type: application/json' http://localhost:8080/api/admin/externalTools?unblock-key=secret-unblock-key -d \ "{ \"displayName\":\"n8n XDI\", \"description\":\"CDIF Metadata creation for XDI via n8n\", \"scope\":\"file\", \"type\":\"configure\", \"toolUrl\":\"http://localhost:5678/webhook-test/dataverse-xdi\", \"toolParameters\": { \"queryParameters\":[ {\"fileid\":\"{fileId}\"}, {\"filePid\":\"{filePid}\"}, {\"siteUrl\":\"{siteUrl}\"}, {\"key\":\"{apiToken}\"}, {\"datasetid\":\"{datasetPid}\"}, {\"datasetversion\":\"{datasetVersion}\"}, {\"locale\":\"{localeCode}\"} ]}, \"contentType\":\"application/octet-stream\"  }"

curl -X POST -H 'Content-type: application/json' http://localhost:8080/api/admin/externalTools?unblock-key=secret-unblock-key -d \ "{ \"displayName\":\"n8n Nexus\", \"description\":\"CDIF Metadata creation for XDI/Nexus via n8n\", \"scope\":\"file\", \"type\":\"configure\", \"toolUrl\":\"http://localhost:5678/webhook-test/dataverse-nexus\", \"toolParameters\": { \"queryParameters\":[ {\"fileid\":\"{fileId}\"}, {\"filePid\":\"{filePid}\"}, {\"siteUrl\":\"{siteUrl}\"}, {\"key\":\"{apiToken}\"}, {\"datasetid\":\"{datasetPid}\"}, {\"datasetversion\":\"{datasetVersion}\"}, {\"locale\":\"{localeCode}\"} ]}, \"contentType\":\"application/x-hdf5\"  }"
```

# CDIFNexMetadata requires permanent file ids:
```
curl -X PUT -d 'true' http://localhost:8080/api/admin/settings/:FilePIDsEnabled?unblock-key=secret-unblock-key
```
---
uid: api/files
---

# Files

The IIS Administration API provides a set of file APIs to interact directly with the file system. This means the API can be used locally or remotely to deploy content, make edits or check if files are up to date.

## General Files (/api/files)

The _/api/files_ endpoint exposes the metadata and content of files and directories on the machine. The files available through this api are limited to those specified in the _files_ configuration section of the [appsettings.json](../configuration/appsettings.json.md) file. Querying the _/api/files_ endpoint without specifying a directory will list the locations present in the configuration. These locations represent the root of all file system paths available through the API. Files can be created, deleted and manipulated with the standard HTTP post, delete, and patch verbs as long as the file path has the claims necessary to perform the operation.

### Examples

File
```
{
    "name": "iisstart.png",
    "id": "{id}",
    "type": "file",
    "physical_path": "C:\\inetpub\\wwwroot\\iisstart.png",
    "exists": "true",
    "size": "98757",
    "created": "2017-01-09T18:08:33.5130112Z",
    "last_modified": "2017-01-09T17:48:13.6180477Z",
    "last_access": "2017-01-09T17:48:13.6180477Z",
    "e_tag": "1d26aa08c67ed45",
    "parent": {
        "name": "wwwroot",
        "id": "{id}",
        "type": "directory",
        "physical_path": "C:\\inetpub\\wwwroot"
    },
    "claims": [
        "read",
        "write"
    ]
}
```

Directory
```
{
    "name": "wwwroot",
    "id": "{id}",
    "type": "directory",
    "physical_path": "C:\\inetpub\\wwwroot",
    "exists": "true",
    "created": "2017-01-09T18:08:33.1380257Z",
    "last_modified": "2017-01-30T17:01:15.2619212Z",
    "last_access": "2017-01-30T17:01:15.2619212Z",
    "total_files": "7",
    "parent": {
        "name": "inetpub",
        "id": "a-nA1LZCBZ9jIqxr6e2uWg",
        "type": "directory",
        "physical_path": "C:\\inetpub"
    },
    "claims": [
        "read",
        "write"
    ]
}
```

## Web Server Files (/api/webserver/files)

The _/api/webserver/files_ endpoint exposes the virtual file structure created by IIS. All file resources under this endpoint belong to some web site and the path of that file is relative to the website that it belongs to. This allows web sites to be treated as file system root, which is desirable in web server scenarios. Virtual directories are included among the normal directories for a complete view of the virtual file hierarchy of a website.

All web server files have a reference to a *file_info* which is the file metadata that can be obtained from the _/api/files_ endpoint. 

### Examples

Web File
```
{
    "name": "iisstart.png",
    "id": "{id}",
    "type": "file",
    "path": "/iisstart.png",
    "parent": {
        "name": "",
        "id": "{id}",
        "type": "application",
        "path": "/"
    },
    "website": {
        "name": "Default Web Site",
        "id": "{id}",
        "status": "started"
    },
    "file_info": {
        "name": "iisstart.png",
        "id": "{id}",
        "type": "file",
        "physical_path": "C:\\inetpub\\wwwroot\\iisstart.png"
    }
}
```

Web Directory
```
{
    "name": "imgs",
    "id": "{id}",
    "type": "directory",
    "path": "/imgs",
    "parent": {
        "name": "",
        "id": "{id}",
        "type": "application",
        "path": "/"
    },
    "website": {
        "name": "Default Web Site",
        "id": "{id}",
        "status": "started"
    },
    "file_info": {
        "name": "imgs",
        "id": "{id}",
        "type": "directory",
        "physical_path": "C:\\inetpub\\wwwroot\\imgs"
    }
}
```

## Copying/Moving Files (/api/files/copy | /api/files/move)

The API supports copying and moving files via the _/api/files/copy_ and _/api/files/move_ endpoints. The process of copying a file is performed by executing a POST request to the API that describes the desired copy operation.

POST
```
{
  "name":"{optional name of the destination file}",
  "file": {
    "id": "{id property of the file to copy}"
  },
  "parent": {
    "id": "{id property of the directory to copy to}"
  }
}
```

## Manipulating File Content (/api/files/content)

The resources under the _/api/files_ route contain only metadata for files. To manipulate the content of a file one must use the _/api/files/content/{id}_ route. This route uses the _application/octet-stream_ content type for transmitting data. Performing a GET request to the content URL of a file will retreive the raw bytes of the file. This operation supports HTTP range requests so the file can be downloaded in chunks. Performing a PUT request to the content URL of the file will replace the content of the file with the request body. This operation supports HTTP content-range requests to enable random access manipulation of files.

Download 2nd 500 bytes of a file
```    
    GET /api/files/content/{id}
    Access-Token: Bearer {Access-Token}
    Range: bytes=500-999
```

Edit 2nd 500 bytes of a 1000 byte file
```    
    PUT /api/files/content/{id}
    Access-Token: Bearer {Access-Token}
    Content-Range: bytes 500-999/1000
    Content-Length: 500
```

## Downloading Files (/api/files/download)

Files can be downloaded through the _/api/files/content_ endpoint of the API. The drawback of this endpoint is that since it is under the _api_ route, it requires an access token. This means that the file cannot be downloaded via a browser. The _/api/files/download_ endpoint enables the creation of temporary download links. These download links are unique, randomly generated, and do not require an access token. Generated download links take the form of _/downloads/{random_sequence}_. An optional time-to-live (ttl) parameter is available when generating a download link to specify how long the download link should be available. The default is five seconds.

When a download is generated, the link for the download is returned in the **Location** header of the HTTP response.

POST
```
{
    "file": {
        "id": "{id of the file to download}"
    },
    "ttl": "10000"
}
```

## Using File Shares

The files API can be used to manage files located on file shares. To enable this functionality, the file share must allow the necessary ACL permissions to the principal representing the machine that is running the API. As an example, suppose there is shared content located at _\\share\images_ and we want to allow an API running on a web server named _web-prod-1_ to enumerate these files. To allow this, administrator must place an Access Control Entry on the  _\\share\images_ directory that allows READ access to the _web-prod-1_ Active Directory object. Once this is done the service on the _web-prod-1_ machine will be able to access this directory, read the files, and then expose them through the API.
gdrive
======


## Overview
gdrive is a command line utility for uploading and downloading single files to your Google Drive.
This tool on its own does not do synchronization of any kind, if you want that you can use googles own tool.
It is meant for one-off uploads or downloads and integration with other unix tools. One use-case could be
daily uploads of a backup archive for off-site storage.

## Prerequisites
None, binaries are statically linked.
If you want to compile from source you need the go toolchain: http://golang.org/doc/install

## Installation
- Save the 'drive' binary to a location in your PATH (i.e. `/usr/local/bin/`)
- Or compile it yourself `go build drive.go`

### Downloads
<table>
    <tr>
      <td>drive-darwin-386</td>
      <td>https://drive.google.com/uc?id=0B3X9GlR6Embna3MzX2w1Vkkydmc</td>
    </tr>
    <tr>
      <td>drive-darwin-amd64</td>
      <td>https://drive.google.com/uc?id=0B3X9GlR6EmbndElsMVpaTm4yZ1k</td>
    </tr>
    <tr>
      <td>drive-freebsd-386</td>
      <td>https://drive.google.com/uc?id=0B3X9GlR6EmbnLVZDZ0dGYUVqRVk</td>
    </tr>
    <tr>
      <td>drive-freebsd-amd64</td>
      <td>https://drive.google.com/uc?id=0B3X9GlR6EmbnZlFxdGxaLWMtcjA</td>
    </tr>
    <tr>
      <td>drive-linux-386</td>
      <td>https://drive.google.com/uc?id=0B3X9GlR6EmbnemRmV25ubHdKemM</td>
    </tr>
    <tr>
      <td>drive-linux-amd64</td>
      <td>https://drive.google.com/uc?id=0B3X9GlR6EmbnSTY5RTZNT2djQmM</td>
    </tr>
    <tr>
      <td>drive-linux-arm</td>
      <td>https://drive.google.com/uc?id=0B3X9GlR6Embna1hSdWw1MFFvU00</td>
    </tr>
    <tr>
      <td>drive-windows-386.exe</td>
      <td>https://drive.google.com/uc?id=0B3X9GlR6EmbnZVVJeHoyeTNvcGs</td>
    </tr>
    <tr>
      <td>drive-windows-amd64.exe</td>
      <td>https://drive.google.com/uc?id=0B3X9GlR6EmbnVkJBOHJIUEpJWUk</td>
    </tr>
</table>

## Usage
    drive [global options] <verb> [verb options]

#### Options
    Global options:
        -a, --advanced Advanced Mode -- lets you specify your own oauth client id and secret on setup
        -c, --config   Set application path where config and token is stored. Defaults to ~/.gdrive
        -v, --version  Print version
        -h, --help     Show this help

    Verbs:
        delete:
            -i, --id File Id (*)
        download:
            -i, --id     File Id (*)
            -s, --stdout Write file content to stdout
                --pop    Download latest file, and remove it from google drive
        info:
            -i, --id File Id (*)
        list:
            -m, --max    Max results
            -t, --title  Title filter
            -q, --query  Query (see https://developers.google.com/drive/search-parameters)
            -s, --shared Show shared status (Note: this will generate 1 http req per file)
        share:
            -i, --id File Id (*)
        unshare:
            -i, --id File Id (*)
        upload:
            -f, --file  File to upload (*)
            -s, --stdin Use stdin as file content (*)
            -t, --title Title to give uploaded file. Defaults to filename
                --share Share uploaded file
        url:
            -i, --id       File Id (*)
            -p, --preview  Generate preview url (default)
            -d, --download Generate download url

## Examples
###### List files
    $ drive list
    Id                             Title                     Size     Created
    0B3X9GlR6EmbnenBYSFI4MzN0d2M   drive-freebsd-amd64       5 MB     2013-01-01 21:57:01
    0B3X9GlR6EmbnOVRQN0t6RkxVQk0   drive-windows-amd64.exe   5 MB     2013-01-01 21:56:41
    0B3X9GlR6Embnc1BtVVU1ZHp2UjQ   drive-linux-arm           4 MB     2013-01-01 21:57:23
    0B3X9GlR6EmbnU0ZnbGV4dlk1T00   drive-linux-amd64         5 MB     2013-01-01 21:55:06
    0B3X9GlR6EmbncTk1TXlMdjd1ODQ   drive-darwin-amd64        5 MB     2013-01-01 21:53:34

###### Upload file
    $ drive upload --file drive-linux-amd64
    Id: 0B3X9GlR6EmbnU0ZnbGV4dlk1T00
    Title: drive-linux-amd64
    Size: 5 MB
    Created: 2013-01-01 21:55:06
    Modified: 2013-01-01 21:55:06
    Owner: Petter Rasmussen
    Md5sum: 334ad48f6e64646071f302275ce19a94
    Shared: False
    Uploaded 'drive-linux-amd64' at 510 KB/s, total 5 MB

###### Download file
    $ drive download --id 0B3X9GlR6EmbnenBYSFI4MzN0d2M
    Downloaded 'drive-freebsd-amd64' at 2 MB/s, total 5 MB

###### Share a file
    $ drive share --id 0B3X9GlR6EmbnOVRQN0t6RkxVQk0
    File 'drive-windows-amd64.exe' is now readable by everyone @ https://drive.google.com/uc?id=0B3X9GlR6EmbnOVRQN0t6RkxVQk0

###### Pipe content directly to your drive
    $ echo "Hello World" | drive upload --stdin --title hello.txt
    Id: 0B3X9GlR6EmbnVHlHZWZCZVJ4eGs
    Title: hello.txt
    Size: 12 B
    Created: 2013-01-01 22:05:44
    Modified: 2013-01-01 22:05:43
    Owner: Petter Rasmussen
    Md5sum: e59ff97941044f85df5297e1c302d260
    Shared: False
    Uploaded 'hello.txt' at 6 B/s, total 12 B

###### Print file to stdout
    $ drive download --stdout --id 0B3X9GlR6EmbnVHlHZWZCZVJ4eGs
    Hello World

###### Get file info
    $ drive info --id 0B3X9GlR6EmbnVHlHZWZCZVJ4eGs
    Id: 0B3X9GlR6EmbnVHlHZWZCZVJ4eGs
    Title: hello.txt
    Size: 12 B
    Created: 2013-01-01 22:05:44
    Modified: 2013-01-01 22:05:43
    Owner: Petter Rasmussen
    Md5sum: e59ff97941044f85df5297e1c302d260
    Shared: False

###### Get a url to the file
    $ drive url --id 0B3X9GlR6EmbnVHlHZWZCZVJ4eGs
    https://drive.google.com/uc?id=0B3X9GlR6EmbnVHlHZWZCZVJ4eGs


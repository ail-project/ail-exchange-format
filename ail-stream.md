# AIL Stream Format version 1

## General description

## Field description and meaning

|Field|Description|Values|Required|
|:----|:----------|:-----|:-----|
|`format`|Format of the stream|`ail`|Yes|
|`version`|Version of the stream format|`1`|Yes|
|`type`|Type of the AIL object|`AIL object type`|Yes|
|`meta`|Dictionary of meta values|dict|No|
|`payload`|Dictionary with the actual payload or a reference|dict|No|

## AIL object types

|Name|Description|
|:---|:----------|
|item|Non-binary content|
|screenshot|Screenshot from AIL crawler|
|username|Username extracted|
|cryptocurrency|Cryptocurrency address extracted|
|pgp-dump|PGP metadata from a PGP key block|
|binary|Binary content|

# Example data
~~~
{
    "format": "ail",
    "version": 1,
    "type": "item",
    "meta": {
       "ail:uuid": "03c51929-eeab-4d47-9dc0-c667f94c7d2c",
       "ail:uuid_org": "28bc3db3-16da-461c-b20b-b944f4058708",
       "ail:id": "object_id",
       "ail:mime-type": "text/plain",
       "ail:subtype": "foobar",
       "ail:tags": ["mails", "custom_tag"],
       "ail:decoded": "True"
    },
    "payload": {
        "raw" : "MjhiYzNkYjMtMTZkYS00NjFjLWIyMGItYjk0NGY0MDU4NzA4Cg=="
        "compress": "gzip",
        "encoding": "base64"
    }
}
~~~

# JSON Schema

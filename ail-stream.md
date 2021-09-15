# AIL Stream Format version 1

|Field|Description|Values|Required|
|:----|:----------|:-----|:-----|
|`format`|Format of the stream|`ail`|Yes|
|`version`|Version of the stream format|`1`|Yes|
|`type`|Type of the AIL object|`AIL object type`|Yes|
|`meta`|Dictionary of meta values|dict|No|
|`payload`|Dictionary with the actual payload or a reference|dict|No| 



~~~
{
    "format": "ail",
    "version": 1,
    "type": "item",
    "meta": {
       "ail:uuid": "03c51929-eeab-4d47-9dc0-c667f94c7d2c",
       "ail:uuid_org: "28bc3db3-16da-461c-b20b-b944f4058708",
       "ail:mime-type": "text/plain",
       "ail:subtype": "foobar",
       "ail:decoded": "True",
       "link": { "type": "parent-child", "target": "88b11dea-165a-11ec-b7a4-c32ff06a8d2b" }
    },
    "payload": {
        "raw" : "MjhiYzNkYjMtMTZkYS00NjFjLWIyMGItYjk0NGY0MDU4NzA4Cg=="
        "compress": "gzip",
        "encoding": "base64"
    }
}
~~~

## Allowed values for "meta"

Meta is a dict with the following allowed values:
  * `ail:uuid`: a UUID. *MANDATORY*
  * `ail:uuid_org`: the UUID of the emitting org. *MANDATORY*
  * `ail:mime-type`: the MIME type of the message. ??? mandatory??
  * `ail:subtype`: the object type (see below). *MANDATORY*
  * `ail:decoded`: boolean. True if the message is decoded ??? mandatory??
  * `link`: a JSON dict describing an OPTIONAL link. *OPTIONAL*
    * "type": a string. The type of the relationship
    * "target": a UUID to a message . The target of the link.
    * Note: the source of the link is the field `meta.ail:uuid`

# AIL object types

|Name|Description|
|:---|:----------|
|item|Non-binary content|
|screenshot|Screenshot from AIL crawler|
|username|Username extracted|
|cryptocurrency|Cryptocurrency address extracted|
|pgp-dump|PGP metadata from a PGP key block|
|binary|Binary content|

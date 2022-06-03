# AIL Stream Format version 1

## Overview and use-cases
This document describes the AIL Stream format version 1. 
The AIL Stream format describes *messages*. For AIL the main purpose of having a well defined AIL Stream formatted message, is to enable the AIL2AIL sync mechanism. Next to AIL, the format is also used by [IntelMQ](https://intelmq.org) as a streaming format for IntelMQ-to-IntelMQ instance connections. For IntelMQ, the purpose is similar: enable connecting of IntelMQ instances to each other. 

In both cases, we want to know if a particular item was already received (possibly via another sync path) at the receiving end. For this, UUIDs are added. See the format description below.
Also the format supports meta: fields which describe the payload. 
The "format" field inside of "meta" specifies the particular payload. 

## When to use the AIL Stream format

Both IntelMQ and AIL will add the "meta" header and use the AIL Stream format for intelmq-to-intelmq or AIL-to-AIL exchanges. Inside of the respective tool, the header is not needed and MAY be striped off. Care needs to be taken to keep relevant information (such as the uuid) when passing . 

## ND-JSON
Any producer or consumer of the format MUST assume that multiple entries in the AIL stream format can be produced or consume in a single stream. Therefore, the parser or producer MUST support [ND-JSON](http://ndjson.org/).


# Field description and semantics


|Field|Description|Values|Required|
|:----|:----------|:-----|:-----|
|`format`|Format of the stream|`ail` or `intelmq`|Yes|
|`version`|Version of the stream format|`1`|Yes|
|`type`|Type of the object in the "payload" field| For AIL, see the `AIL object type` table. For IntelMQ, currently the only defined value is `event`|Yes|
|`meta`|(JSON) Dictionary of meta values. See below for a definition of fields|dict|No|
|`payload`|(JSON) Dictionary with the actual payload|dict|No|

Note that the payload field MAY be empty. In this case, the meaning of the AIL JSON message is that tags may update an existing previous message with the same "uuid".

## The `meta` header
|Field|Description|Values|Mandatory field?|
|:----|:----------|:-----|:-----|
|`uuid`| A UUIDv4 uuid of the AIL message. | _string_ | Yes |
|`uuid_org`| A UUIDv4 uuid of the emitting organisation. If the emitting org has a MISP UUID, it SHOULD re-use that MISP uuid. This will help in correlating AIL Stream messages to MISP org UUIDs.| _string_ | Yes |
|`tags` | A JSON array (list) of tags (strings) | list of arbitrary strings | No | 
|`encoding` | The encoding of the fields of the payload | "base64", _string_ | No |
|`compress` | The compression used (before `encoding`) | "gzip", ... _string_ | No |
|_prefix_:_key_ | any user of the AIL Streaming format MAY add extension key-value pairs to the `meta` header, as long as the key is prefixed. Example: `"ail:mime-type": "text/plain"` | _string_ | No |

    


## AIL object types

|Name|Description|
|:---|:----------|
|item|Non-binary content|
|screenshot|Screenshot from AIL crawler|
|username|Username extracted|
|cryptocurrency|Cryptocurrency address extracted|
|pgp-dump|PGP metadata from a PGP key block|
|binary|Binary content|


## IntelMQ object types
|Name|Description|
|:---|:----------|
|event|An [IntelMQ Data Format event](https://intelmq.readthedocs.io/en/maintenance/dev/data-format.html) (a JSON object)|

# Examples

~~~
{
    "format": "ail",
    "version": 1,
    "type": "item",
    "meta": {
       "uuid": "03c51929-eeab-4d47-9dc0-c667f94c7d2c",
       "uuid_org": "28bc3db3-16da-461c-b20b-b944f4058708",
       "tags": ["mails", "custom_tag"],
       "encoding": "base64",
       "compress": "gzip",
       "ail:id": "object_id",
       "ail:mime-type": "text/plain",
       "ail:subtype": "foobar"
    },
    "payload": {
       "raw" : "MjhiYzNkYjMtMTZkYS00NjFjLWIyMGItYjk0NGY0MDU4NzA4Cg=="
    }
}
~~~


~~~
{
    "format": "intelmq",
    "version": 2,
    "type": "event",
    "meta": {
       "uuid": "03c51929-eeab-4d47-9dc0-aaaaaaaaaaaa",
       "uuid_org": "28bc3db3-16da-461c-b20b-aaaaaaaaaaaa",
       "tags": []
    },
    "payload": {
       "raw" : "MjhiYzNkYjMtMTZkYS00NjFjLWIyMGItYjk0NGY0MDU4NzA4Cg==",
       "source.ip": "1.2.3.4",
       "destination.ip": "5.6.7.8",
       "source.asn": "35492",
       "classification.taxonomy": "Spam",
       "classification.type": "xyz",
       "...IntelMQ Data Format key...": "...IntelMQ data format value..."
    }
}
~~~



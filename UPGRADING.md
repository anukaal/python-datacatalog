# 3.0.0 Migration Guide

This document describes the breaking changes that have been made, and what you need to do to update your usage.

The most significant change was introduced at v2.0 release based on a [next-gen code generator](https://github.com/googleapis/gapic-generator-python), and includes substantial interface changes. Existing code written for eariler versions of this library will likely require updates to use this version.

If you experience issues or have questions, please file an [issue](https://github.com/googleapis/python-datacatalog/issues).

## Supported Python Versions

| Applicable previous versions |
|------------------------------|
| v1.0.0 or lower              |

> **WARNING**: Breaking change
> 
> The 2.0.0 release requires Python 3.6+.


## Method Calls

| Applicable previous versions |
|------------------------------|
| v1.0.0 or lower              |

> **WARNING**: Breaking change
>
> Methods expect request objects. We provide a script that will convert most common use cases.

* Install the library

```py
python3 -m pip install google-cloud-datacatalog
```

* The script `fixup_datacatalog_v1_keywords.py` is shipped with the library. It expects
an input directory (with the code to convert) and an empty destination directory.

```sh
$ fixup_datacatalog_v1_keywords.py --input-directory .samples/ --output-directory samples/
```

**Before:**
```py
from google.cloud import datacatalog_v1
datacatalog = datacatalog_v1.DataCatalogClient()
return datacatalog.lookup_entry(linked_resource=resource_name)
```


**After:**
```py
from google.cloud import datacatalog_v1
datacatalog = datacatalog_v1.DataCatalogClient()
return datacatalog.lookup_entry(request={'linked_resource': resource_name})
```

### More Details

In `google-cloud-datacatalog<=1.0.0`, parameters required by the API were positional parameters and optional parameters were keyword parameters.

**Before:**
```py
    def create_entry_group(
        self,
        parent,
        entry_group_id,
        entry_group=None,
        retry=google.api_core.gapic_v1.method.DEFAULT,
        timeout=google.api_core.gapic_v1.method.DEFAULT,
        metadata=None,
    ):
```

Since the 2.0.0 release, all methods have a single positional parameter `request`. Method docstrings indicate whether a parameter is required or optional.

Some methods have additional keyword only parameters. The available parameters depend on the `google.api.method_signature` annotation specified by the API producer.


**After:**
```py
    def create_entry_group(
        self,
        request: datacatalog.CreateEntryGroupRequest = None,
        *,
        parent: str = None,
        entry_group_id: str = None,
        entry_group: datacatalog.EntryGroup = None,
        retry: retries.Retry = gapic_v1.method.DEFAULT,
        timeout: float = None,
        metadata: Sequence[Tuple[str, str]] = (),
    ) -> datacatalog.EntryGroup:
```

> **NOTE:** The `request` parameter and flattened keyword parameters for the API are mutually exclusive.
> Passing both will result in an error.

Both of these calls are valid:

```py
response = client.create_entry_group(
    request={
        "parent": parent,
        "entry_group_id": entry_group_id,
        "entry_group": entry_group
    }
)
```

```py
response = client.create_entry_group(
    parent=parent, 
    entry_group_id=entry_group_id,
    entry_group=entry_group
)  # Make an API request.
```

This call is invalid because it mixes `request` with a keyword argument `entry_group`. Executing this code
will result in an error.

```py
response = client.create_entry_group(
    request={
        "parent": parent,
        "entry_group_id"=entry_group_id
    },
    entry_group=entry_group
)
```



## Enums and Types

| Applicable previous versions |
|:-----------------------------|
| v2.0.0 or lower              |

> **WARNING**: Breaking changes
> 
> The submodules `enums` and `types` have been removed; the `type` attributes were renamed to `type_` to avoid name collisions.

**Before:**
```py
from google.cloud import datacatalog_v1
entry = datacatalog_v1.types.Entry()
entry.type = datacatalog_v1.enums.EntryType.FILESET
```


**After:**
```py
from google.cloud import datacatalog_v1
entry = datacatalog_v1.Entry()
entry.type_ = datacatalog_v1.EntryType.FILESET
```

The renamed attributes are:

* `TagTemplateField.type` -> `TagTemplatedField.type_`
* `ColumnSchema.type` -> `ColumnSchema.type_`
* `Entry.type` -> `Entry.type_`

## Common Resource Path Helper Methods

| Applicable previous versions |
|:-----------------------------|
| v1.0.0 or lower              |

The `location_path` method existing in `google-cloud-datacatalog<=1.0.0` was renamed to `common_location_path` in v3.0.0.

If you are upgrading from v1.0.0 or lower, modify your code to use new method name.

If you are upgrading from v2.0.0, and constructing paths manually as described in [previous upgrade guide](https://github.com/googleapis/python-datacatalog/blob/v2.0.0/UPGRADING.md#project-path-helper-methods), now you can use `common_location_path` method.

There are also more resource path helper methods were added: `common_billing_account_path`, `common_folder_path`, `common_organization_path`, and `common_project_path`.

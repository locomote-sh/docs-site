---
title: Query API
layout: documentation
---
# Query API

The query API is a web API that allows file metadata to be queried over HTTP. The query API is provided via an endpoint URL, `/query.api`, mounted under each content repository’s public URL,  e.g. <https://locomote.sh/example/docs/query.api>.

_Locomote_ generates a database of files for every content repository. This file database is simply a list of all the publicly accessible files in a repository, together with the files' metadata. Within the browser, the file database is stored in an `IndexedDB` object store, and the query API works by querying this object store.

A number of indexes are defined on the file DB object store, and queries are defined as operations on these indexes. For example, the file path is used as the primary key for each file record and so queries on file paths can be defined using the `path` index name. A query operation is defined as a URL parameter in `{index}${op}` format, where `{index}` is an index name and `{op}` is the name of a query operation. Query operations are described in the following sections.

### $value

The `value` operation searches for records with an index value equal to the specified value. It is also the default query operation, so a query parameter specified in `{index}={value}` format is equivalent to `{index}$value={value}`.

When querying unique-value indexes, the query will return at most one record; however, when querying multi-value indexes, the query may return multiple records.

### $from

The `from` operation will match records where the index value is equal to or greater than the specified value. It may return multiple records.

### $to

The `to` operation wil match records where the index value is less than or equal to the specified value. It may return multiple records.

### $prefix

The `prefix` operation will match records where the index value has the specified prefix. It may return multiple records.

----

The query API also provides a number of control parameters which can be used to specify how query operations are combined, or to modify the contents and format of the query response.

### $join

Multiple query operations can be specified in a single request. By default these are joined using an _AND_ operation - i.e. every query term must be true for a record to be included in the result. The join parameter can be used to change this, so e.g. `$join=OR` means that a record is included in the result if any query term is matched.

### $orderBy

The `orderBy` parameter is used to control the sort order of matching records. It is used by specifying the name or path to a property on the file record which the results should be sorted by.

### $limit

The `limit` parameter is used to limit the number of records returned in the result.

### $from / $to

Similar to `limit`, the `from` and `to` parameters can be used to return only a subset of the matched records. The parameters specify the index of the first and last record to include in the result; if from isn’t specified then it defaults to 0; if to isn’t specified then it defaults to the last record.

### $format
The `format` parameter is used to specify the format the result is returned as. It takes the following values:

* `list`: This is the default value; results are returned as a list of records.
* `keys`: Return a list of just the primary keys (i.e. paths) of matched records.
* `lookup`: Return a map of primary keys (paths) to records.

## Examples

Return a list of records in a specific category:
<https://locomote.sh/example/docs/query.api?category=assets>

Return the first 10 file names under a specified path:
<https://locomote.sh/example/docs/query.api?path$prefix=assets/img&$limit=10&$format=keys>

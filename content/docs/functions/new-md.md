---
title: Postgres json_build_object function
subtitle: Builds a JSON object out of a variadic argument list.
enableTableOfContents: true
updatedOn: '2024-01-10T17:58:09.720Z'
---


`json_build_object` is used to construct a JSON object from a set of key-value pairs, creating a JSON representation of a row or set of rows. This has potential performance benefits compared to converting query results to JSON on the application side.

Function signature:
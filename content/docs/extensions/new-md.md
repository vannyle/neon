---
title: The neon extension
enableTableOfContents: true
updatedOn: '2023-11-28T18:39:00.765Z'
---

With each Neon project, Neon creates  a "neon" extension, which includes functions and views designed to gather Neon-specific metrics. The metrics are intended for use by the Neon team for the purpose of enhancing our service. The views are owned by a Neon system role (`cloud_admin`), but you are able to view them by connecting to the `postgres` database using `psql` and executing the command `\dv neon.*`, as shown below. At present, the extension includes two views for local file cache metrics. We may incorporate additional views in future releases.

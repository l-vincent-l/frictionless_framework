# Bigquery Format

```markdown remark type=danger
This functionality is currently disabled as being fixed in [#1217](https://github.com/frictionlessdata/frictionless-py/issues/1217)
```

Frictionless supports both reading tables from BigQuery source and treating a BigQuery dataset as a tabular data storage.

```bash tabs=CLI
pip install frictionless[bigquery]
pip install 'frictionless[bigquery]' # for zsh shell
```

## Reading Data

You can read from this source using `Package/Resource`, for example:

```python tabs=Python
import os
import json
from pprint import pprint
from apiclient.discovery import build
from oauth2client.client import GoogleCredentials
from frictionless import Resource
from frictionless.plugins.bigquery import BigqueryDialect

# Prepare BigQuery
# This file can be received from Google Console
os.environ["GOOGLE_APPLICATION_CREDENTIALS"] = ".google.json"
credentials = GoogleCredentials.get_application_default()
service = build("bigquery", "v2", credentials=credentials),
project = json.load(open(".google.json"))["project_id"],

# Read from BigQuery
dialect = BigqueryDialect(project=project, dataset='<dataset>', table='<table>')
resource = Resource(service, dialect=dialect)
pprint(resource.read_rows())
```

If you'd like to treat BigQuery dataset as a tabular storage:

```python tabs=Python
from pprint import pprint
from frictionless import Package

package = Package.from_bigquery(service=service, project=project, dataset='<dataset>')
pprint(package)
for resource in package.resources:
  pprint(resource.read_rows())
```

## Writing Data

> **[NOTE]** Timezone information is ignored for `datetime` and `time` types.

We can export a package to a BigQuery dataset:

```python
from pprint import pprint
from frictionless import Package

package = Package('path/to/datapackage.json')
package.to_bigquery(service, project=project, dataset='<dataset>')
```

Also, it's possible to save a resource as a Bigquery table using `resource.write()`.

## Configuration

There is the `BigqueryDialect` to configure how Frictionles works with BigQuery:

```python tabs=Python
from pprint import pprint
from frictionless import Resource
from frictionless.plugins.bigquery import BigqueryDialect

dialect = BigqueryDialect(project=project, dataset='<dataset>', table='<table>'
resource = Resource(service, dialect=dialect)
pprint(resource.read_rows())
```

## Reference

```yaml reference
references:
  - frictionless.formats.BigqueryControl
```

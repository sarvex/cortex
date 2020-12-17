# Python client

## Python client

* [cortex](python-client.md#cortex)
  * [client](python-client.md#client)
  * [local\_client](python-client.md#local_client)
  * [cluster\_client](python-client.md#cluster_client)
  * [env\_list](python-client.md#env_list)
  * [env\_delete](python-client.md#env_delete)
* [cortex.client.Client](python-client.md#cortex-client-client)
  * [create\_api](python-client.md#create_api)
  * [get\_api](python-client.md#get_api)
  * [list\_apis](python-client.md#list_apis)
  * [get\_job](python-client.md#get_job)
  * [refresh](python-client.md#refresh)
  * [patch](python-client.md#patch)
  * [delete\_api](python-client.md#delete_api)
  * [stop\_job](python-client.md#stop_job)
  * [stream\_api\_logs](python-client.md#stream_api_logs)
  * [stream\_job\_logs](python-client.md#stream_job_logs)

## cortex

### client

```python
client(env: str)
```

Initialize a client based on the specified environment.

**Arguments**:

* `env` - Name of the environment to use.

**Returns**:

Cortex client that can be used to deploy and manage APIs in the specified environment.

### local\_client

```python
local_client(aws_access_key_id: str, aws_secret_access_key: str, aws_region: str) -> Client
```

Initialize a client to deploy and manage APIs locally.

The specified AWS credentials will be used by the CLI to download models from S3 and authenticate to ECR, and will be set in your Predictor.

**Arguments**:

* `aws_access_key_id` - AWS access key ID.
* `aws_secret_access_key` - AWS secret access key.
* `aws_region` - AWS region.

**Returns**:

Cortex client that can be used to deploy and manage APIs locally.

### cluster\_client

```python
cluster_client(name: str, provider: str, operator_endpoint: str, aws_access_key_id: Optional[str] = None, aws_secret_access_key: Optional[str] = None) -> Client
```

Create a new environment to connect to an existing Cortex Cluster, and initialize a client to deploy and manage APIs on that cluster.

**Arguments**:

* `name` - Name of the environment to create.
* `provider` - The provider of your Cortex cluster. Can be "aws" or "gcp".
* `operator_endpoint` - The endpoint for the operator of your Cortex Cluster. You can get this endpoint by running the CLI command `cortex cluster info` for an AWS provider or `cortex cluster-gcp info` for a GCP provider.
* `aws_access_key_id` - AWS access key ID. Required when `provider` is set to "aws".
* `aws_secret_access_key` - AWS secret access key. Required when `provider` is set to "aws".

**Returns**:

Cortex client that can be used to deploy and manage APIs on a Cortex Cluster.

### env\_list

```python
env_list() -> list
```

List all environments configured on this machine.

### env\_delete

```python
env_delete(name: str)
```

Delete an environment configured on this machine.

**Arguments**:

* `name` - Name of the environment to delete.

## cortex.client.Client

### create\_api

```python
 | create_api(api_spec: dict, predictor=None, requirements=[], conda_packages=[], project_dir: Optional[str] = None, force: bool = True, wait: bool = False) -> list
```

Deploy an API.

**Arguments**:

* `api_spec` - A dictionary defining a single Cortex API. See [https://docs.cortex.dev/v/0.24](https://docs.cortex.dev/v/0.24) for schema.
* `predictor` - A Cortex Predictor class implementation. Not required when deploying a traffic splitter.
* `requirements` - A list of PyPI dependencies that will be installed before the predictor class implementation is invoked.
* `conda_packages` - A list of Conda dependencies that will be installed before the predictor class implementation is invoked.
* `project_dir` - Path to a python project.
* `force` - Override any in-progress api updates.
* `wait` - Streams logs until the APIs are ready.

**Returns**:

Deployment status, API specification, and endpoint for each API.

### get\_api

```python
 | get_api(api_name: str) -> dict
```

Get information about an API.

**Arguments**:

* `api_name` - Name of the API.

**Returns**:

Information about the API, including the API specification, endpoint, status, and metrics \(if applicable\).

### list\_apis

```python
 | list_apis() -> list
```

List all APIs in the environment.

**Returns**:

List of APIs, including information such as the API specification, endpoint, status, and metrics \(if applicable\).

### get\_job

```python
 | get_job(api_name: str, job_id: str) -> dict
```

Get information about a submitted job.

**Arguments**:

* `api_name` - Name of the Batch API.
* `job_id` - Job ID.

**Returns**:

Information about the job, including the job status, worker status, and job progress.

### refresh

```python
 | refresh(api_name: str, force: bool = False)
```

Restart all of the replicas for a Realtime API without downtime.

**Arguments**:

* `api_name` - Name of the API to refresh.
* `force` - Override an already in-progress API update.

### patch

```python
 | patch(api_spec: dict, force: bool = False) -> dict
```

Update the api specification for an API that has already been deployed.

**Arguments**:

* `api_spec` - The new api specification to apply
* `force` - Override an already in-progress API update.

### delete\_api

```python
 | delete_api(api_name: str, keep_cache: bool = False)
```

Delete an API.

**Arguments**:

* `api_name` - Name of the API to delete.
* `keep_cache` - Whether to retain the cached data for this API.

### stop\_job

```python
 | stop_job(api_name: str, job_id: str, keep_cache: bool = False)
```

Stop a running job.

**Arguments**:

* `api_name` - Name of the Batch API.
* `job_id` - ID of the Job to stop.

### stream\_api\_logs

```python
 | stream_api_logs(api_name: str)
```

Stream the logs of an API.

**Arguments**:

* `api_name` - Name of the API.

### stream\_job\_logs

```python
 | stream_job_logs(api_name: str, job_id: str)
```

Stream the logs of a Job.

**Arguments**:

* `api_name` - Name of the Batch API.
* `job_id` - Job ID.


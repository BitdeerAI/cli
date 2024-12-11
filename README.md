# Bitdeer AI Cloud CLI
Bitdeer AI Cloud CLI is a command-line interface (CLI) tool designed to interact with the Bitdeer AI Cloud. This tool allows users to manage and monitor their cloud resources with ease.

## Installation
To install `bitdeer-ai`, follow these steps:

1. Download the binary
  - For Windows: 
    - amd64 [link to Windows binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.3/windows_amd64.zip)
    - arm64 [link to Windows binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.3/windows_arm64.zip)
  - For macOS:
    - amd64 [link to macOS binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.3/darwin_amd64.zip)
    - arm64 [link to macOS binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.3/darwin_arm64.zip) 
  - For Linux:
    - amd64 [link to Linux binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.3/linux_amd64.zip)
    - arm64 [link to Linux binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.3/linux_arm64.zip)
2. Install the binary:
  - Move the downloaded binary to a directory included in your system's PATH.
3. Verify the installation:
```
bitdeer-ai --version
```

## Usage
`bitdeer-ai` provides various commands to manage your cloud resources. Below is the basic usage and the available commands.

### Basic Usage
```
BitdeerAI CLI is a CLI tool for BitdeerAI Cloud Platform.

Usage:
  bitdeer-ai [flags]
  bitdeer-ai [command]

Configuration
  login       Login to BitdeerAI Cloud
  logout      Logout from BitdeerAI Cloud

Managements
  container   Container cmd is to manage containers on BitdeerAI Cloud.
  training    Training cmd is to manage training jobs on BitdeerAI Cloud.

Additional Commands:
  help        Help about any command

Flags:
  -h, --help      help for bitdeer-ai
  -v, --version   version for bitdeer-ai

Use "bitdeer-ai [command] --help" for more information about a command.
```

### Configuration
Before you start using BitdeerAI CLI, you need to login to the BitdeerAI Cloud. You can login using the following command:
```
bitdeer-ai login --token "your-api-key"
```

### Container Commands
#### Create a container:
Usage:
```
bitdeer-ai container create [flags]
```

Examples:
```
bitdeer-ai container create -p <project-id> -n <container-name> -s <specification-type> -i <image> -w <workdir> --volume name1:/path1 --cmds <cmd> --args <arg> --envs <env> -r <region-id> -z <zone-id>
```

Flags:
- `-p, --project_id` **string**: Project ID (required)
- `-n, --name` **string**: Container Name (required)
- `-s, --spec` **string**: Machine Specification (required)
- `-w, --workdir` **string**: Working Directory
- `-v, --volume` **stringArray**: Container Volume Mounts, value is in the form of 'volumeName:/path/to/mount'
- `-i, --image` **string**: Container Image (required)
- `-a, --args` **stringArray**: Arguments
- `-c, --cmds` **stringArray**: Commands
- `-e, --envs` **stringArray**: Environment Variables
- `-r, --region_id` **string**: Region ID (default 'ap-southeast-1')
- `-z, --zone_id` **string**: Zone ID (default 'ap-southeast-1a')
- `-h, --help`: Help for create

### List containers
Note: page number starts from 1, maximum page size of 20 results

Usage:
```
bitdeer-ai container list -p <project_id> --page <page> --per_page <page_size>
```

Examples:
```
bitdeer-ai container list --per_page 5 --page 2
+---------------+-------+----------------------+--------------------------------------+---------+------------+------------------+------------+
| CONTAINER_ID  | NAME  | IMAGE                | MACHINE_SPECIFICATION                |  STATUS | CREATED_BY | ATTACHED_PROJECT | REGION     |
+---------------+-------+----------------------+--------------------------------------+---------+------------+------------------+------------+
| po-12345      | ctr1  | registry/pytorch:1.0 | 1x NVIDIA H100 GPU, 20 vCPUs, 194 GB | STOPPED | user@email | demo             | ap-southeast-1|ap-southeast-1a |
| po-23456      | ctr2  | registry/pytorch:1.0 | 1x NVIDIA H100 GPU, 20 vCPUs, 194 GB | STOPPED | user@email | demo             | ap-southeast-1|ap-southeast-1a |
| po-34567      | ctr3  | registry/pytorch:1.0 | 1x NVIDIA H100 GPU, 20 vCPUs, 194 GB | STOPPED | user@email | demo             | ap-southeast-1|ap-southeast-1a |
| po-45678      | ctr4  | registry/pytorch:1.0 | 1x NVIDIA H100 GPU, 20 vCPUs, 194 GB | STOPPED | user@email | demo             | ap-southeast-1|ap-southeast-1a |
| po-56789      | ctr5  | registry/pytorch:1.0 | 1x NVIDIA H100 GPU, 20 vCPUs, 194 GB | STOPPED | user@email | demo             | ap-southeast-1|ap-southeast-1a |
+---------------+-------+----------------------+--------------------------------------+---------+------------+------------------+------------+
|               |       |                      |                                      |         | PAGE: 2/2  | TOTAL: 10        |            |
+---------------+-------+----------------------+--------------------------------------+---------+------------+------------------+------------+
```

Flags:
- `--page` **int**: Page number, starts from 1 (default 1)
- `--per_page` **int**: Page size, range [1, 20] (default 20)
- `-p, --project_id` **string**: Project ID
- `-h, --help`: help for list

#### Display container details
Usage:
```
bitdeer-ai container get <container-id>
```

Examples:
```
bitdeer-ai container get po-12345
Container Overview:
+----------------------+----------------------------------------------+
| FIELD_NAME           | VALUE                                        |
+----------------------+----------------------------------------------+
| ContainerId          | po-12345                                     |
| Name                 | test                                         |
| ProjectID            | pj-12345                                     |
| ProjectName          | test                                         |
| Image                | registry/pytorch:latest                      |
| MachineSpecification | 1x NVIDIA H100 GPU (80GB), 20 vCPUs (194 GB) |
| Status               | RUNNING                                      |
| Region               | ap-southeast-1|ap-southeast-1a                                   |
| CreatedBy            | user@email.com                               |
| CreatedAt            | 2024-11-13T09:32:01Z                         |
+----------------------+----------------------------------------------+

Volumes:
+---+-------------+------------+---------+
| # | VOLUME_NAME | MOUNT_PATH | SIZE_GB |
+---+-------------+------------+---------+
| 1 | vol-1       | /mnt       |      50 |
| 2 | vol-2       | /home      |     100 |
+---+-------------+------------+---------+

Conditions:
+-------------------------------+--------+-------+
|            POD_CONDITION_TYPE | STATUS | ERROR |
+-------------------------------+--------+-------+
|                 POD_SCHEDULED | true   |       |
| POD_READY_TO_START_CONTAINERS | true   |       |
|               POD_INITIALIZED | true   |       |
|              CONTAINERS_READY | true   |       |
|                     POD_READY | true   |       |
+-------------------------------+--------+-------+
```

#### Execute interactive container shell
Usage:
```
  bitdeer-ai container exec <container-id>
```

Examples:
```
bitdeer-ai container exec po-12345
```

#### Get container logs
Usage:
```
  bitdeer-ai container logs [-f] <container-id>
```

Examples:
```
bitdeer-ai container logs po-12345
```

Flags:
- `-f, --follow`: Specify if the logs should be streamed
- `-h, --help`: help for logs

#### Delete container:
Usage:
```
bitdeer-ai container delete <container-id>
```

Examples:
```
bitdeer-ai container delete po-12345
```

#### Start container
Usage:
```
bitdeer-ai container start <container-id>
```

Examples:
```
bitdeer-ai container start po-12345
```

#### Stop container
Usage:
```
bitdeer-ai container stop <container-id>
```

Examples:
```
bitdeer-ai container stop po-12345
```

### Training Commands
#### Create a training job:
Usage
```
bitdeer-ai training create [flags]
```
Examples
```
bitdeer-ai training create -p <project-id> -n <job-name> -j <job-type> -w <worker-spec> -c <num-workers> -i <worker-image> -r <region-id> -z <zone-id>
```
Flags
- `-p, --project_id` **string**: Project ID (*required*)
- `-n, --job_name` **string**: Job Name (*required*)
- `-j, --job_type` **string**: Job Type (*required*)
- `-w, --worker_spec` **string**: Worker Spec (*required*)
- `-c, --num_workers` **int**: Number of Workers (*required*)
- `-i, --worker_image` **string**: Worker Image (*required*)
- `-r, --region_id` **string**: Region ID (required) (default "ap-southeast-1")
- `-z, --zone_id` **string**: Zone ID (required) (default "ap-southeast-1a")
- `--args` **stringArray**: Arguments
- `--cmds` **stringArray**: Commands
- `--envs` **stringArray**: Environment Variables
- `--working_dir` **string**: Working Directory
- `--volume_name` **string**: Volume Name
- `--volume_mount_path` **string**: Volume Mount Path
- `-h, --help`: Help for create

#### List all training jobs:
Usage
```
bitdeer-ai training list [flags]
```
Examples
```
bitdeer-ai training list

+--------+--------+-----------+-----------+---------+------------+----------------------+----------------+------------+
| JOB_ID |  NAME  |  STATUS   |   TYPE    | WORKERS | GPUS/WORKER|    CREATED_TIME      |ATTACHED_PROJECT|   REGION   |
+--------+--------+-----------+-----------+---------+------------+----------------------+----------------+------------+
| tj-03  | job-03 | COMPLETED | TORCH_JOB |       2 | 4x H100    | 2024-07-05T04:00:15Z | pj-01          | ap-southeast-1|ap-southeast-1a |
| tj-02  | job-02 | SUSPENDED | TORCH_JOB |       4 | 3x RTX3090 | 2024-07-05T03:30:45Z | pj-02          | ap-southeast-1|ap-southeast-1a |
| tj-01  | job-01 | FAILED    | TORCH_JOB |       6 | 2x RTX3090 | 2024-07-05T02:10:30Z | pj-01          | ap-southeast-1|ap-southeast-1a |
+--------+--------+-----------+-----------+---------+------------+----------------------+----------------+------------+
|                                                                  TOTAL:  3                                          |
+--------+--------+-----------+-----------+---------+------------+----------------------+----------------+------------+
```
#### Display a training job:
Usage
```
bitdeer-ai training get [flags] <job-id>
```
Examples
```
bitdeer-ai training get tj-03

+-----------------+--------------------------------+
| FIELD_NAME      | VALUE                          |
+-----------------+--------------------------------+
| TrainingJobID   | tj-03                          |
| JobName         | job-03                         |
| ProjectID       | pj-01                          |
| ProjectName     | LLM Project                    |
| Region          | ap-southeast-1|ap-southeast-1a                     |
| JobType         | TORCH_JOB                      |
| JobStatus       | COMPLETED                      |
| NumberOfWorkers | 2                              |
| SpecOfWorker    | 4x H100                        |
| WorkerImage     | btdr/pytorch-dist-mnist:latest |
| CreatedAt       | 2024-07-05T04:00:15Z           |
+-----------------+--------------------------------+
+----------------+-----------+
| WORKER_NAME    |   STATUS  |
+----------------+-----------+
| tj-03-master-0 | Succeeded |
| tj-03-worker-0 | Succeeded |
+----------------+-----------+
```
#### List workers for a training job:
Usage
```
bitdeer-ai training workers [flags] <job-id>
```

examples
```
bitdeer-ai training workers tj-03

+----------------+-----------+
| WORKER_NAME    |   STATUS  |
+----------------+-----------+
| tj-03-master-0 | Succeeded |
| tj-03-worker-0 | Succeeded |
+----------------+-----------+
```
#### Print logs for a worker in a training job:
Usage
```
bitdeer-ai training logs [flags] <job-id> <worker-name>
```

Examples
```
bitdeer-ai training logs -f tj-03 tj-03-master-0

2024-07-05T04:00:15Z: Starting worker tj-03-master-0
2024-07-05T04:00:15Z: Worker tj-03-master-0 is running
2024-07-05T04:00:15Z: Worker tj-03-master-0 is done
...
```
#### Delete a training job:
Usage
```
bitdeer-ai training delete [flags] <job-id>
```

Examples
```
bitdeer-ai training delete tj-03
```
#### Shutdown a training job:
Usage
```
bitdeer-ai training suspend [flags] <job-id>
```

Examples
```
bitdeer-ai training suspend tj-03
```
#### Restart a training job:
Usage
```
bitdeer-ai training resume [flags] <job-id>
```

Examples
```
bitdeer-ai training resume tj-03
```

## License
Bitdeer AI Cloud CLI is licensed under the MIT License.

## Support
If you encounter any issues or have questions, please contact our support team at [support email](mailto:support@bitdeer.com).

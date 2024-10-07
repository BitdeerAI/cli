# Bitdeer AI Cloud CLI
Bitdeer AI Cloud CLI is a command-line interface (CLI) tool designed to interact with the Bitdeer AI Cloud. This tool allows users to manage and monitor their cloud resources with ease.

## Installation
To install `bitdeer-ai`, follow these steps:

1. Download the binary
  - For Windows: 
    - amd64 [link to Windows binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.2/windows_amd64.zip)
    - arm64 [link to Windows binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.2/windows_arm64.zip)
  - For macOS:
    - amd64 [link to macOS binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.2/darwin_amd64.zip)
    - arm64 [link to macOS binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.2/darwin_arm64.zip) 
  - For Linux:
    - amd64 [link to Linux binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.2/linux_amd64.zip)
    - arm64 [link to Linux binary](https://github.com/BitdeerAI/cli/releases/download/v0.0.2/linux_arm64.zip)
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
- `-r, --region_id` **string**: Region ID (required) (default "sg")
- `-z, --zone_id` **string**: Zone ID (required) (default "sg-sg-1")
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
| tj-03  | job-03 | COMPLETED | TORCH_JOB |       2 | 4x H100    | 2024-07-05T04:00:15Z | pj-01          | sg|sg-sg-1 |
| tj-02  | job-02 | SUSPENDED | TORCH_JOB |       4 | 3x RTX3090 | 2024-07-05T03:30:45Z | pj-02          | sg|sg-sg-1 |
| tj-01  | job-01 | FAILED    | TORCH_JOB |       6 | 2x RTX3090 | 2024-07-05T02:10:30Z | pj-01          | sg|sg-sg-1 |
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
| Region          | sg|sg-sg-1                     |
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

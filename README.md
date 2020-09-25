# p2p-observatory

## Motivation & Vision
Public, user-run, permissionless networks’ dynamics are rather unpredictable due to the multiple types of applications and services that leverage them. Because they are not owned by a single user, analysing their behavior needs to be done through (non-invasive) proxies and probes, rather than through (invasive) controls (analogous to how one measures and observes patterns in a brain). Because of this hard challenge, most P2P networks put observability as a secondary goal.

In order to promote a healthy ecosystem of protocol developers where hypotheses are tested and verified through real world measurements, we envisioned the P2P Networks Observatory,  a project focused on measuring all kinds of Public P2P Networks. We believe that having the ability to get information from the real-time performance of the network can help tremendously in both real-time decision-making (e.g., request routing and forwarding) and in long-term protocol design (e.g., best ways to populate routing tables).

[HERE](RFMs.md) list of measurements that will be implemented as part of the P2P observatory.

## Deployment instructions

### Manifest file

To deploy a measurement plan you first need to create a `.toml` manifest file that defines the plan deployment parameters. 
The manifest file should include the following sections and fields

- `[plan]`: section that defines the parameters of the measurement plan
  - `builder`: the two options are `go` or `python3`
  - `name`: the name of the measurement plan
  - `version`: the version of the measurement plan
  - `app_directory`: the path to the root directory of the measurement plan files
  - `output_directory`: the path to the directory where the results of the measurement (metrics) are stored
  - `command`: the commmand that executes the measurement
- `[aws]`: section that defines the parameters of the AWS environment
  - `access_key`: The AWS access key of the account where the measurement will be deployed
  - `secret_key`: The AWS secret key of the account where the measurement will be deployed
  - `metrics_s3_bucket`: The S3 bucket where the metrics of the measurement will be stored
  - `[aws.servers]`: subsection that defines the parameters related to the deployment of the measurement servers
    - `regions`: array that lists the desired region names for the servers to be deployed
    - `availability_zones`: number of availability zones per region
    - `az_servers`: number of servers per availability zone
    - `instance_type`: the instance type name of the measurement servers
- `[elasticsearch]`: section that defines the parameters of the ElasticSearch cluster
  - `username`: The name of the master user
  - `password`: The password of the master user
  - `domain`: The domain of the elasticsearch cluster


### How to run

1. Install the following prerequisites:
  * Terraform: https://learn.hashicorp.com/tutorials/terraform/install-cli
  * Ansible: https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html
  * Docker: https://docs.docker.com/get-docker/

2. Populate the `toml` minefst file

3. Run the `start_observatory` script passing the manifest file as an argument:

```
./start_observatory --manifest config.toml
```


### Troubleshooting:

If you get the error:
> sudo: add-apt-repository: command not found

you need to install the the `software-properties-common` package:

`sudo apt install software-properties-common`

### TODO

- Improve Elastic Search authentication
- Provision Elastic Search cluster through Terraform
- Create Docker image for steps 1 and 2
- Compile ipfs-crawler in Dockerfile
- Get all variables from a config file instead of environment variables

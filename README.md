# Terraform AWS MWAA Quick Start

Quick start tutorial for Amazon Managed Workflows for Apache Airflow (MWAA) with Terraform. This is a word for word translation of the official [AWS quick start](https://docs.aws.amazon.com/mwaa/latest/userguide/quick-start.html) (with CloudFormation).

## Variables

Below is an example `terraform.tfvars` file that you can use in your deployments:

```ini
region   = "eu-central-1"
profile  = "airflow"
prefix   = "airflow"
vpc_cidr = "10.192.0.0/16"
public_subnet_cidrs = [
  "10.192.10.0/24",
  "10.192.11.0/24"
]
private_subnet_cidrs = [
  "10.192.20.0/24",
  "10.192.21.0/24"
]
mwaa_max_workers = 2
```

## DAGs

There's a test DAG file inside the local [`dags` directory](./dags), which was taken from the official tutorial for [Apache Airflow v1.10.12](https://airflow.apache.org/docs/apache-airflow/1.10.12/tutorial.html#example-pipeline-definition). You can place as many DAG files inside that directory as you want and Terraform will pick them up and upload them to S3. Alternatively, you can use the DAG sync via GitHub Actions as described [below](#dag-sync-via-gitbub-actions).

## Usage

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
```

Make sure to keep the terraform state files safe, as they contain your access keys to the S3 bucket.

## DAG sync via GitHub Actions

To use GitHub actions to sync the `dags` folder to S3, we can use the workflow in `.github/workflows/sync.yml`. To allow access, set up secrets via `Settings -> Secrets` and add the below variables, which you can read from the `terraform output`:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`
- `AWS_S3_BUCKET`

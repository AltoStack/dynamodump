# dynamodump: DynamoDB Backups and Restores

## Table of Contents

  * [Background](#background)
  * [What is it?](#what-is-it)
  * [Why creating this tool?](#why-creating-this-tool)
  * [How to use it?](#how-to-use-it)
  * [Todo](#todo)
  * [Contributing to the project](#contributing-to-the-project)
  
## Background

This is a fork of [dynamodbdump](https://github.com/VEVO/dynamodbdump) that aims to complete some of 
it's [TODOs](https://github.com/VEVO/dynamodbdump/blob/master/TODO.md).

## What is it?

This tool performs a backup of a given DynamoDB table and pushes it to a given folder in s3
in a format compatible with the AWS DataPipeline functionality.

It is also capable of restoring a backup from s3 to a given table both from
this tool or from a backup generated using the AWS DataPipeline functionality.

## Why create this tool?

Using the AWS DataPipelines to backup DynamoDB tables spawns EMR clusters which
can take some time, and for small tables it will cost you 20min of EMR runs for
just a few seconds of backup time, which makes no sense.

This tool can be run in a command-line, in a docker container and ending up on a
Kubernetes cron job very easily, allowing you to leverage your existing
architecture without additional costs.

## How to use it?

### With the command-line

To build `dynamodump`, you can use the `make build` command or manually get the dependencies (using `glide` or `go get`) and then use `go build` to build.

Then you can use the dynamodump binary you just built to start a backup or restore.

#### Backup

Usage:

```shell script
Usage:
  dynamodump backup [flags]

Flags:
  -s, --dynamo-table-batch-size int        Max number of records to read from the Dynamo table at once. Environment variable: DYN_DYNAMO_TABLE_BATCH_SIZE (default 1000)
  -w, --dynamo-table-batch-wait-time int   Number of milliseconds to wait between batches. Environment variable: DYN_WAIT_TIME (default 100)
  -t, --dynamo-table-name string           Name of the Dynamo table to actions. Environment variable: DYN_DYNAMO_TABLE_NAME (required)
  -o, --dynamo-table-region string         AWS region of the Dynamo table. Environment variable: DYN_DYNAMO_TABLE_REGION (required)
  -h, --help                               help for backup
  -f, --s3-bucket-folder-name string       Path inside the S3 bucket where to put actions. Environment variable: DYN_S3_BUCKET_FOLDER_NAME (required)
  -p, --s3-bucket-folder-name-suffix       Adds an autogenerated suffix folder named using the UTC date in the format YYYY-mm-dd-HH24-MI-SS to the provided S3 folder. Environment variable: DYN_S3_BUCKET_NAME_SUFFIX
  -b, --s3-bucket-name string              Name of the S3 bucket where to put the actions. Environment variable: DYN_S3_BUCKET_NAME (required)
  -d, --s3-bucket-region string            AWS region of the s3 Bucket. Environment variable: DYN_S3_BUCKET_REGION (required)
```

Example:

```shell script
./dynamodump backup \
  -t table-name \
  -o eu-west-1 \
  -b bucket-name \
  -f some/folder \
  -d us-east-1
```

## Todo

- [x] Cross Region Support (DynamoDB Table and S3 Bucket can be in different AWS regions)
- [ ] Cross Account Support (DynamoDB Table and S3 Bucket can be in different AWS accounts)
- [ ] Flag to force restore even if the `_SUCCESS` file is absent
- [ ] Ability to define S3 `StorageClass` of backed up files
- [ ] Ability to backup all DynamoDB Tables (Based on AWS Tags)
- [x] Integrate https://goreleaser.com/

## Contributing to the project

Anybody is more than welcome to create PR if you want to contribute to the project. A minimal testing and explanations about the problem will be asked but that's for sanity purposes.

We're friendly people, we won't bite if the code is not done the way we like! :)

If you don't have a lot of ideas but still want to contribute, we maintain a list of ideas we want to explore in the [Todo](#todo) section, you can start here!

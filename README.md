# Software Projects

## Media Database

For several years, I've been running a small, local SQL installation to keep track of films I've watched. As a fan of movies, this is a helpful way to categorize, track, and report on what I've watched in the past. Determining trends in the data becomes a simple exercise of writing a SQL query:

```sql
mysql> select * from movie where director like 'david lynch' order by year_made;
+----+-------------------------------+-------------+-----------+--------------+
| id | title                         | director    | year_made | date_watched |
+----+-------------------------------+-------------+-----------+--------------+
| 65 | Eraserhead                    | David Lynch |      1977 | NULL         |
| 72 | Blue Velvet                   | David Lynch |      1986 | NULL         |
| 74 | Twin Peaks: Fire Walk With Me | David Lynch |      1992 | NULL         |
| 89 | Lost Highway                  | David Lynch |      1997 | NULL         |
| 45 | Mulholland Drive              | David Lynch |      2001 | 2019-03-29   |
+----+-------------------------------+-------------+-----------+--------------+
```

As helpful as this database has been, I began to wonder whether I could build a cloud-native application that would provide higher availability and more durable storage for my movie database. Adding support to track other types of media, such as music, would also be a big plus.

As a result, I wrote my `media-db` project ([GitHub](https://github.com/alexpcook/media-db)). It consists of several Go packages that interface with AWS S3 as the database storage location:

* A `config` package that handles the connection information to AWS (used in concert with the AWS CLI connection configuration).
* A `schema` package that defines a common media interface and specific implementations (e.g. movies, music).
* A `service` package that interfaces with the AWS SDK for Go to call S3 APIs.
* A `cli` package that handles the user interface logic.

Right now, the CLI is capable of configuring connection information to AWS (profile, region, and bucket name), as well as creating, reading, updating, and deleting entries in the database. It so far supports two media types, music and movies.

```plain
❯ media-db
usage: media-db <command> [movie|music] [<flag>...]

where <command> is one of:
  setup         Configure the database connection to AWS
  create        Create an entry in the database
  read          Read entries from the database
  update        Update an entry in the database
  delete        Delete an entry from the database
```

In the future, I would like to enhance the project structure to enable easier addition of new types of media to the database. Restructuring the internals of the S3 object key storage could also save API calls to AWS and allow easier advanced searches and filtering of media by user-given criteria.

## Practice Music Scales API

I play piano and wanted to devise a way to make practicing scales more fun and challenging. One way to increase the level of difficulty is to play all the major and minor scales in a random order instead of following the circle of fifths. This forces you to better learn each scale in isolation rather than relying on the familiar pattern of adding an additional sharp or flat as you go around the circle of fifths.

To accomplish this goal, I wrote my `practice-music-scales` project ([GitHub](https://github.com/alexpcook/practice-music-scales)). This codebase contains:

* An `app` directory that contains a Go API to return major and minor scales in a random order, and a static website written in HTML, CSS, and JavaScript to display the result to the user.
* An `aws` directory that contains options to deploy the code to various AWS services.

Here's a screenshot of the deployed website showing the result of one particular API call to the Go service:

![practice-music-scales example](https://raw.githubusercontent.com/alexpcook/alexpcook.github.io/main/assets/images/scales.png)

Currently, the only supported AWS deployment option is a serverless stack comprised of AWS Lambda for the dynamic Go API, S3 for the static website, and an API Gateway to provide a common access point to these two services. In the future, I intend to finish an in-progress containerization option to deploy the app to an EKS cluster.

## YAML to EC2 Converter

This project ([GitHub](https://github.com/alexpcook/ec2yaml)) contains a simple Python CLI and boto3 integration to convert a YAML configuration file into an EC2 instance. In addition to controlling the usual EC2 instance settings, the CLI executes an extensive BASH user data script to add users (plus their ssh keys) and additional non-root EBS volumes.

## Infrastructure Templates

This project ([GitHub](https://github.com/alexpcook/infrastructure-templates)) is a general collection of useful infrastructure templates. Currently, it contains a Terraform module that creates an AWS VPC, Internet gateway, subnets, route tables, network access control lists (NACLs), and security groups. It then deploys a configurable number of EC2 instances to the VPC using a desired AMI. This is suitable for getting one or more EC2 instances running on AWS with the capability to SSH into them from a configurable public IP address.

In the future, it would be useful to add other common infrastructure deployment patterns, such as making an AWS EKS cluster and then configuring the local `kubectl` installation to connect to the cluster.

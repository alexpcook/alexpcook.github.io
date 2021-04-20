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
* A `cli` package that handles the user-interface logic.

Right now, the CLI is capable of configuring connection information to AWS (profile, region, and bucket name), as well as creating, reading, updating, and deleting entries in the database. It so far supports two media types, music and movies.

In the future, I would like to enhance the project structure to enable easier addition of new types of media to the database. Restructuring the internals of the S3 object key storage could also save API calls to AWS and allow easier advanced searches and filtering of media by user-given criteria.

## Practice Music Scales API

Add description

## YAML to EC2 Converter

Add description

## Infrastructure Templates

Add description

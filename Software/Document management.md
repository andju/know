---
publish on andju-know: true
---
# Document management
Even with more and more (electronic) paper coming in, I was initially skeptical whether I need a document management: I actually was happy with sorting pdfs into a folder structure. I had to try [paperless-ngx](https://docs.paperless-ngx.com/) to understand how much faster (especially searching and checking documents) and easier it is.
## Installation

> [!hint] PostgreSQL
> I strongly recommend using PostgreSQL as a database. With SQLite the performance starts to degrade when a few hundred documents have been added.

1. Install a [[Container engine]]
2. Create the folder `paperless-ngx` with the subfolders:
	1. `consume` (documents placed in this folder will be consumed by paperless-ngx)
	2. `data` (logs and the classification model)
	3. `export` (exports and database backups)
	4. `media`: (documents)
3. Download [these files](https://github.com/andju/docker-solutions/tree/main/paperless-ngx)  into the folder `paperless-ngx`
	1. Alternatively you can download the files from the [original source](https://github.com/paperless-ngx/paperless-ngx/tree/main/docker/compose)
4. Open a command line in the folder `paperless-ngx` and run `podman compose pull` and `podman compose up -d`
5. Open http://localhost:8000 in a browser

For first steps in paperless-ngx read this [article](https://itv4.de/paperless-ngx-mithilfe-von-docker-unter-windows-installieren/) 🇩🇪.

I recommend the following OCR Settings:

- Output Type: `pdfa`
- Language: `deu+eng`
- Deskew: Experiment with this setting - for some documents it improves the quality, other are skewed a bit more.

## Features

- This [article](https://piep.tech/posts/automatic-password-removal-in-paperless-ngx/) describes how to automatically remove passwords while consuming new documents.
## Maintenance
### Backup and restore
Include the entire paperless-ngx folder in your backup strategy. The documents (subfolder: media) and the classification model (subfolder: data) are automatically included.

The database however is stored on a volume and needs to be extracted (before the backup). The easiest way to do so, is to use the [document exporter](https://docs.paperless-ngx.com/administration/#exporter). The following command creates the backup `pgbackup.zip` in the folder `export`:
```
podman compose exec -T webserver document_exporter ../export --data-only -z -zn pgbackup
```
To restore the backup, extract the file `pgbackup.zip` in the folder `export` and run:
```
podman compose exec -T webserver document_importer ../export --data-only
```
### Database (DB) upgrade
New versions of PostgreSQL are not compatible with old data files. Therefore you need to backup, delete and restore the data when upgrading to a new version.

> [!caution] Simultaneous upgrade of paperless-ngx
> Do not upgrade paperless-ngx and the DB in the same step! A new version of paperless might change the DB structure - which could prevent restoring your data. It is recommended to run `podman compose pull` and `podman compose up -d` **before** changing the `docker-compose.yml` file.

1. Backup the database as described in [[#Backup and restore]]
2. Stop and remove the containers: `podman compose down`
3. Delete the volume containing the database (usually paperless_pgdata) in Podman
4. Update the `docker-compose.yml` file (number after `docker.io/library/postgres`)
5. Update the image: `podman compose pull`
6. Start paperless-ngx: `podman compose up -d`
7. Restore the database as described in [[#Backup and restore]]
### Direct database access
If you used the `docker-compose.yml` file from my repository, it includes `pgadmin`. This makes it possible to directly access the paperless-ngx database:
1. Open http://localhost:8888 in a browser
2. Enter username `admin@example.com` and password `admin`
3. Register the server (if not done yet):
	1. Right click on Servers: Register ➡️ Server...
	2. Enter a name of your choice and the following properties
		1. Name: `paperless-ngx`
		2. Host name/address: `db`
		3. Port: `5432`
		4. Maintenance database: `paperless`
		5. Username: `paperless`
		6. Password: `paperless`
4. Open the Server `paperless-ngx`
You need to have docker installed. (https://www.docker.com/products/docker-desktop)

1. To install and get the XLayer API running copy the following files to a directory on your machine:

	docker-compose.yml
	dockerCfg\my.cnf
	dockerCfg\init.sql

2. Check the docker-compose.yml file to make sure you are using the Windows paths under "volumes:" and that the paths are correct.  Comment out the *nix volumes.

3. Now from the directory you copied the files to run the following:

	docker-compose up -d

NOTE:
=====
I had to make my my.cnf READ-ONLY

The FIRST time you run this the database needs to be created.  For some reason this can take up to 7mins.  Be patient.  Look at the logs for that container using the following command:

	docker logs xlayer-api-db01
	
Run this a few times until you see the following:
	
	xlayer-api-db01 | xxxx-xx-xx xx:xx:xx 0 [Note] mysqld: ready for connections.
	xlayer-api-db01 | Version: '10.4.11-MariaDB-1:10.4.11+maria~bionic'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  mariadb.org binary distribution

You should then be able to connect to the MariaDB using an external tool like DBeaver. (localhost:3306)

---------------------------------------------------------------------------------------------------------
**** IF YOU SEE THIS: ****

	xlayer-api-db01 | xxxx-xx-xx xx:xx:xx [Note] [Entrypoint]: Temporary server started.
	
You need to wait for the database to be initialized completely.
---------------------------------------------------------------------------------------------------------

Useful URLS:
============
SWAGGER Documentation = https://localhost:9788/swagger-ui.html
Spring Management     = https://localhost:9789/
                      = https://localhost:9789/health
					  = https://localhost:9789/hawtio

TODO:
- Add XLayerAPI SpringBoot app to an Image that can be run from docker.
- Create Image with relevant mysql database installed and upload to XLayer docker repo.
- Check why the swagger documentation (ApiModelProperty for the TransactionResponse is not being displayed correctly.

ADDITIONAL NOTES:
=================
Adding a self-signed certificate was done by following this tutorial: https://www.thomasvitale.com/https-spring-boot-ssl-certificate/
This app has 2 Keystores.  They are both PKCS12 keystores and are kept in the "sxirestapi.p12" and |sxirestapi_2.p12" files (Keystore Password = sXi_Pass)
The "sxirestapi_2.p12" Keystore contains additional "Subject Alternative Name" records for localhost and 127.0.0.1 mainly for testing
NOTE: You will need to import the certificate (sxirestapiCert.crt) into any app wanting to communicate with xlayer-api (password = sXi_Pass)


Running as a windows service:
=============================
WinSW - https://github.com/kohsuke/winsw


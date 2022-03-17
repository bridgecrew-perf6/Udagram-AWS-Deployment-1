# Infrastructure Description
The following is the architecture diagram:


The user (web browser) interacts this application through static web hosting service provided by AWS S3 bucket. When the user requests a resource, the server requests SignedURL to AWS S3 and the server will send the resource back if the user has correct authentication.  The server is running on Elastic Beanstalk. After the code is deployed to Elastic Beanstalk, the server code is running on AWS EC2 instance and it interacts with RDS database and S3 bucket. An ORM, Sequelize is used to manage the connection to the RDS database. This database is used to store the relational data entries. S3 bucket is used to store object data.
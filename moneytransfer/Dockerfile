#Defining the Environment.
FROM java:8-jdk-alpine

#Copying the jar.
COPY ./target/moneytransfer-1.0-SNAPSHOT-fat.jar /usr/app/

#Setting the working directory.
WORKDIR /usr/app

#Running the jar
ENTRYPOINT ["java","-jar","moneytransfer-1.0-SNAPSHOT-fat.jar"]

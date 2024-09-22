## Simple python docker dev example for the official docker docs
https://docs.docker.com/language/python/containerize/
## PREVENTING PYC FILE CREATION:
.pyc files : python automatically generates compiled bytecode files (.pyc ) when a python script is executed fr the first time . 
these files are cached versions of the soruce code, which can impreve subsequesnt exectuion performance . 
. REASON FOR PREVENTION: in ceratin scenarios , such as development enviromentents or when working with sentitive code, it might be desirable to avoid creating these chached files 
this can heop prevent accidentail expose of source code or maintain a consistent state across diffrent enviromennts 
to do this in your docker file you simple do it this way 
```yaml 
ENV PYTHONDONTWRITEBYTECODE=1 # THAT IS IT 

```
## PREVENTING OUTPUT BUFFERING 
OUTPUT BUFFERING : pythons standard output and standard error are typeically buffered by default . this means that output is temporarily storred in memory until a certin amot of data is 
collected or the buffer is flushed . this can somethimes lead to unexpected behaviour , especially when applications crash without emitting any logs . 
REASON FOR PREVENTION: in scenarios where it crucial to see all output immediately , even in cases of crashes or uncexpected behaviour , preventing output buffering can essential for debugging 
and troubleshooting 

benefirs of using ENV PYTHONUNBUFFERED=1:
- immediate output: ensures that all output , including errors and warnings , is displayed immediately as its generated, providing real-time feedback . 
- debugging : faacilites debugging by allowing you to see the exact point at which an error eccurs and any relevant content . 
- trouble shooting : helps in troubleshooting issues by providing a clear view of the applications behavior and any petential problems 
- logging : ensures that logs are wirtten to teh appropriate destination without being delayed by buffering


## CREATING A NON PREVELEGED USER:


This ARG UID=10001 and RUN adduser block is part of a Dockerfile, used to create a non-privileged user in a Docker container to run applications more securely. Here's a breakdown:
1. ARG UID=10001

    This declares an argument called UID with a default value of 10001. This argument can be passed when building the Docker image to set a specific user ID (UID) for the user being created.

2. RUN adduser

    This executes the adduser command to create a new user in the Docker container.

Options used with adduser:

    --disabled-password: The user account is created without setting a password, meaning the user cannot log in interactively (a common security practice for service users).

    --gecos "": This typically provides information like the user's full name, but here it's set to an empty string to avoid prompting for additional user details.

    --home "/nonexistent": No home directory is created for the user. /nonexistent is used to signify that this user won't need a home directory.

    --shell "/sbin/nologin": This sets the login shell for the user to /sbin/nologin, which prevents the user from being able to log into a shell.

    --no-create-home: Ensures that no home directory is created for the user (since /nonexistent is specified as the home directory).

    --uid "${UID}": Assigns the user the UID provided by the ARG statement above, ensuring the user has a fixed, non-root UID (in this case, 10001, unless overridden).

    appuser: This is the name of the user being created.

Summary:

This snippet creates a non-privileged user named appuser with UID 10001 (or a user-supplied value), who cannot log in, does not have a password, and has no home directory or shell access. This is typically done for security purposes in Docker containers where you want an isolated user to run processes without risking root privileges.





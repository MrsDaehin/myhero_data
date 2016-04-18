# MyHero Data Service

This is the Data Service for a basic microservice demo application.
This provides a data storage layer for a voting system where users can vote for their favorite movie superhero.

Details on deploying the entire demo to a Mantl cluster can be found at
* MyHero Demo - [hpreston/myhero_demo](https://github.com/hpreston/myhero_demo)

The application was designed to provide a simple demo for Cisco Mantl.  It is written as a simple Python Flask application and deployed as a docker container.

Other services are:
* Data - [hpreston/myhero_data](https://github.com/hpreston/myhero_data)
* App - [hpreston/myhero_app](https://github.com/hpreston/myhero_app)
* Web - [hpreston/myhero_web](https://github.com/hpreston/myhero_web)

The docker containers are available at
* Data - [hpreston/myhero_data](https://hub.docker.com/r/hpreston/myhero_data)
* App - [hpreston/myhero_web](https://hub.docker.com/r/hpreston/myhero_app)
* Web - [hpreston/myhero_web](https://hub.docker.com/r/hpreston/myhero_web)

## Basic Application Details

Required

* flask
* ArgumentParser

# Installation

    pip install -r requirements.txt

# Usage

In order to run, the service needs 1 piece of information to be provided:
1. Data Server Authentication Key to Require

This detail can be provided in one of three ways.
1. As a command line argument
    - `python myhero_data/myhero_data.py --datasecret "DATA AUTH KEY" `
2. As environment variables
    - `export myhero_data_key="DATA AUTH KEY"`
    - `python myhero_data/myhero_data.py`
3. As raw input when the application is run
    - `python myhero_data/myhero_data.py`
    - `Data Server Key: DATA AUTH KEY`

A command line argument overrides an environment variable, and raw input is only used if neither of the other two options provide needed details.


# Accessing

Initial and Basic APIs
These are v1 APIs that require no authentication and will eventually be removed
* Basic List of Hero Choices
  * `curl http://localhost:5000/hero_list`
* Current results calculations
  * `curl http://localhost:5000/results`
* Place a vote for an option
  * `curl http://localhost:5000/vote/<HERO>`

New v2 APIs
These newer APIs require authentication as well as support more features
* Get the current list of options for voting
  * `curl -X GET -H "key: DATA AUTH KEY" http://localhost:5000/options`
* Add a new option to the list
  * `curl -X PUT -H "key: DATA AUTH KEY" http://localhost:5000/options -d '{"option":"Deadpool"}'`
* Replace the entire options list
  * `curl-X POST -H "key: DATA AUTH KEY" http://localhost:5000/options -d @sample_post.json`
  * Data should be of same format as a GET request
* Delete a single option from the list
  * `curl -X DELETE -H "key: DATA AUTH KEY" http://localhost:5000/options/Deadpool`
* Place a Vote for an option
  * `curl -X POST -H "key: DATA AUTH KEY" http://localhost:5000/vote/Deadpool`
* Get current results
  * `curl -X GET -H "key: DATA AUTH KEY" http://localhost:5000/results`
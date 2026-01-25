# DE-Docker-Workshop
Docker Workshop Codespaces

Module 1 Homework: Docker & SQL
## Question 1: Understanding Docker images

I ran the following command:

```bash
docker run -it --entrypoint=bash python:3.13
pip --version
```
The pip version in the python:3.13 image is 25.3.

## Question 2: Docker networking

pgAdmin connects to Postgres from inside the same Docker network.

The correct hostname and port are:

- **Hostname:** db
- **Port:** 5432

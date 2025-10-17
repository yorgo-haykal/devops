## TP part 01
### Question 1-1
It's better to use the -e flag because we don't want to put sensitive information in the Dockerfile which might be publicly accessible, and we do't want to hardcode our env variables so that it can be used by more than one user for with multiple instances of env vars.

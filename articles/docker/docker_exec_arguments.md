# Defining executables and arguments for docker images

There are two ways to specify what executable to run when a docker container starts: **ENTRYPOINT** and **CMD**.

Both are similar but have some differences described in this article:

1. [Alternative 1: CMD](#CMD)
2. [Alternative 2: ENTRYPOINT](#ENTRYPOINT)
3. [Example: Dynamic arguments at runtime](#Using-ENTRYPOINT-and-CMD-for-dynamic-arguments-at-runtime)
4. [Sources:](#Sources)

## CMD
- Use it over ENTRYPOINT if you easily want to override it with other executables
    - User specifying default arguments for **docker run** will override CMD in Dockerfile
- There can only be one CMD in a Dockerfile (if you define more CMD the last will take precedence)
- Accepted formats:
    ```docker
    CMD ["<executable>","<param1>","<param2>"] (preferred form)
    CMD ["<param1>","<param2>"] (as default parameters to ENTRYPOINT)
    CMD <command> <param1> <param2> (shell form)
    ```

Example of docker file using CMD to specify that nginx should be run when starting the container. CMD takes a list of strings.
```docker
FROM nginx:alpine
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## ENTRYPOINT
-  Will always be executed when running docker.
    - User specifying default arguments for **docker run** will be appended after all elements defined in ENTRYPOINT and replace any elements specified by **CMD**
- There can only be one ENTRYPOINT in a Dockerfile (if you define more the last will take precedence)
- Shell form prevents any CMD or command line arguments to be used.
- Accepted formats:
    ```docker
    ENTRYPOINT ["<executable>", "<param1>", "<param2>"] (preferred, exec form)
    ENTRYPOINT <command> <param1> <param2> (shell form)
    ```

Example of docker file using CMD to specify that nginx should be run when starting the container.
```docker
FROM nginx:alpine
EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```


## Using ENTRYPOINT and CMD for dynamic arguments at runtime
There are use cases where the docker should be limited to running one executable, but where different arguments can be supplied at runtime. Below is an example of a docker image that only will run nginx, with default arguments that can be overriden if required.

```docker
FROM nginx:alpine
EXPOSE 80
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
```

```sh
# Overriding above CMD arguments but not ENTRYPOINT
docker run nginxImage -g daemon on;
```

## Sources
- [Dockerfile cheatsheet by Kapeli](https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index)
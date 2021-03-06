# grpc-gateway-hello-world

## Pre-requisites
- Docker
- docker-compose
- Azure CLI

## Run
    
    $ docker compose up --build 
    
## Test
    
    $ curl -X POST http://localhost:8090/v1/example/echo -d '{"name": "hello world"}'

Note: When testing locally, ensure the correct port is exposed in the docker-compose.yml file.


## Run on azure

Login to azure portal

    $ az login

Create a resource group

    $ az group create --name <group_name> --location <azurelocation>

Create app service plan

    $ az appservice plan create --name <service_plan_name> --resource-group <group_name> --sku B1 --is-linux

Create webapp

    $ az webapp create --resource-group <group_name> --plan <service_plan_name> --name <webapp_name> --multicontainer-config-type compose --multicontainer-config-file docker-compose.yml

Change the container start time

    $ az webapp config appsettings set --resource-group <group_name> --name <webapp_name> --settings WEBSITES_CONTAINER_START_TIME_LIMIT=1800

## Test 

Once the app is up and running, 

    $ curl -X POST <webapp_name>.azurewebsites.net/v1/example/echo -d '{"name": "hello world"}'

Note: By default the app runs on port 80.


### Generate stubs
Follow [installation instructions](https://github.com/grpc-ecosystem/grpc-gateway#installation) before running the commands

    # Go stubs
    $ protoc -I ./proto --go_out ./proto \
            --go_opt paths=source_relative --go-grpc_out ./proto
            --go-grpc_opt paths=source_relative --grpc-gateway_out ./proto
            --grpc-gateway_opt paths=source_relative ./proto/helloworld/hello_world.proto

    # Python stubs
    $ python -m grpc_tools.protoc -I ./proto --python_out ./proto --grpc_python_out ./proto ./proto/helloworld/hello_world.proto
    # Fix import for hello_world_pb2_grpc.py
    $ sed -i 's|from helloworld|from proto.helloworld|g' proto/helloworld/hello_world_pb2_grpc.py

    # Swagger stub
    $ protoc -I ./proto --openapiv2_out ./proto --openapiv2_opt logtostderr=true ./proto/helloworld/hello_world.proto

Swagger docs with GitHub Pages: https://afrocircus.github.io/grpc-gateway-hello-world
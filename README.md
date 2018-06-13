Functions API Quickstart
========================
This is a quick tutorial for you to get things started.

Before we start
---------------
Make sure you got the following ready:
- Ubuntu Server 16.04 LTS
- Docker 1.1 or better (see sample guide on installing docker here)
- An account on docker hub

Install CLI tools
-----------------
Let's install the management tool first.

```bash
curl -L -o ./fn https://github.com/iron-io/functions/releases/download/0.2.72/fn_linux
chmod +x ./fn
# move it to an accessible path, in which case you can delete the local copy
sudo cp ./fn /usr/local/bin/
rm ./fn
```

Build our python example
------------------------
Move to our python example directory. We need to first customize your docker hub username. My username is `exampleguy`:

```bash
cd ~/funcapisdk/src/hellopython
sed 's/{{USERNAME}}/exampleguy/g' ./func.yaml.tmpl > ./func.yaml
```

Build and test it!

```bash
fn build -v
echo '{ "name": "john" }' | fn run
# output should be Hello john !!!
```

Everything fine? Time to publish our work!

```bash
docker login -u exampleguy
fn push -v
```


Using your docker image as web API
----------------------------------
Great work so far. Let's deploy our docker image using the web portal.

Point your browser to `apimanager.lizoc.com`. Create a new app using the `Create App` button on the main page. Give it a 
unique name and leave all other fields untouched. We'll call ours `examples`. Not very original isn't it?

Click on your app and add a route (click on `Add Route`):

- Path: /hellopython
- Image: exampleguy/hellopython:0.0.1

Leave all other fields as-is.

Let us test things out now:

```bash
curl -H "Content-Type: application/json" -X POST -d '{ "name": "john" }' https://api.lizoc.com/r/examples/hellopython
# output should be Hello john !!!
```


Console based deployment
------------------------
Here we will use curl to do everything in the command line. For more info, see [http://petstore.swagger.io/?url=https://raw.githubusercontent.com/iron-io/functions/master/docs/swagger.yml](our web documentation).

```bash
# get our server version
curl -H "accept: application/json" "https://api.lizoc.com/v1/version"
# get a list of all apps available
curl -H "accept: application/json" "https://api.lizoc.com/v1/apps"
```

See your app configuration:

```bash
curl -H "accepts: application/json" https://api.lizoc.com/v1/apps/examples
curl -H "accepts: application/json" https://api.lizoc.com/v1/apps/examples/routes
```

Deploy your app without UI:

```bash
curl -X POST -H "accept: application/json" -H "Content-Type: application/json" -d '{ "app": { "name": "examples2", config": { "additionalProp1": "string", "additionalProp2": "string", "additionalProp3": "string" } }, "error": {}}' "https://api.lizoc.com/v1/apps" 
```

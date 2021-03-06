[Dockerhub](https://hub.docker.com/r/heiss/port_cdstar)

If JWT will be generated by Sciebo RDS through certificates, then you need passive mode for FileTransferMode in server.py and can setup [here](https://github.com/Sciebo-RDS/port_cdstar/blob/main/src/api/project/files.py#L20) everything you need, because only this endpoint will be called from Sciebo RDS with a given metadata object in request body. So you generate the jwt in the same endpoint as you trigger all endpoints of cdstar. So you need take care about the file transfer, too. But sciebo RDS helps alot with this.
Look at [port-cdstar](https://github.com/Sciebo-RDS/port_cdstar/blob/master/src/api/project/files.py#L23) for example for jwt workflow and file transfer.

Otherwise you need all endpoints, because sciebo RDS takes care on calling them, but sciebo RDS takes care of authentication with oauth2 / passwords and getting the files.

Use "from RDS import ROParser" for parsing the metadata in request.json.get("metadata") and easier access for fields. An example ro-crate file from the software describo can be found in root folder `example-rocrate.json`.

Parse the metadata to your needs, example [here](https://github.com/Sciebo-RDS/Sciebo-RDS/blob/master/RDS/circle1_adapters_and_ports/port_zenodo/src/api/project/project.py#L59).

Use [request_jwt.py](https://github.com/Sciebo-RDS/port_cdstar/blob/master/src/lib/requests_jwt.py) for easier jwt support in requests `authorization` header, because the pypi version of it is unmaintained and has a bug for python 3.

API is defined [here](https://www.research-data-services.org/doc/impl/ports/port-invenio/).

# How to

Runs api service:

```bash
pipenv run python server.py
```

Creates requirements.txt

```bash
make update
```

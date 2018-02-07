# inception-function

This is a forked version of the work by [Magnus Erik Hvass Pedersen](https://github.com/Hvass-Labs/TensorFlow-Tutorials) - it has been re-packaged as an [OpenFaaS](https://www.openfaas.com) serverless function.

This means you can call a HTTP endpoint with a URL to an image and get back categorizations through machine learning - fast.

Changes:

* Added Dockerfile
* Download the "image" JPG to local storage instead of loading from local storage

Deploy on [OpenFaaS](https://www.openfaas.com):

```
$ faas-cli deploy --image alexellis/inception --name inception --fprocess "python3 index.py"
```

Invoke via `curl` or `faas-cli invoke`:

```
$ URL=https://upload.wikimedia.org/wikipedia/commons/6/61/Humpback_Whale_underwater_shot.jpg \
  echo $URL | faas-cli invoke inception

[{"name": "great white shark", "score": 0.5343291759490967}, {"name": "tiger shark", "score": 0.09276486188173294}, {"name": "grey whale", "score": 0.05899052694439888}, {"name": "sea lion", "score": 0.05105864629149437}, {"name": "hammerhead", "score": 0.019910583272576332}, {"name": "sturgeon", "score": 0.013177040033042431}, {"name": "stingray", "score": 0.00763126602396369}, {"name": "electric ray", "score": 0.006749240681529045}, {"name": "killer whale", "score": 0.005086909048259258}, {"name": "ice bear", "score": 0.003828041721135378}]
```

> Alternatively you can also pipe to `jq` or invoke via the built-in API Gateway and pick result type "JSON".

* Use `jq` tool to filter the top matches:

```
$ URL=https://upload.wikimedia.org/wikipedia/commons/6/61/Humpback_Whale_underwater_shot.jpg \
  echo $URL | faas-cli invoke inception \
  | jq '.[] | select(.score > 0.05)'

{
  "name": "great white shark",
  "score": 0.5343291759490967
}
{
  "name": "tiger shark",
  "score": 0.09276486188173294
}
{
  "name": "grey whale",
  "score": 0.05899052694439888
}
{
  "name": "sea lion",
  "score": 0.05105864629149437
}
```

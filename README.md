Transporter

Build
-----
`go build -a ./cmd/...`


Configure
---------
there is a sample config in test/config.yaml.  The config defines the endpoints, (either sources or sinks) that are available to the application.
```yaml
api:
  interval: 1 # number of milliseconds between metrics pings
  uri: "http://requestb.in/1a0zlf11"
nodes:
  localmongo:
    type: mongo
    uri: mongodb://localhost/boom
  supernick:
    type: elasticsearch
    uri: http://10.0.0.1,10.0.0.2:9200/indexname
  debug:
    type: file
    uri: stdout://
  crapfile:
    type: file
    uri: file:///tmp/crap
  stdout:
    type: file
    uri: stdout://
```

There is also a sample 'application.js' in test/application.js.  The application is responsible for building transporter pipelines.
Given the above config, this Transporter will copy from a file (in /tmp/crap) to stdout.
```js
t = Transporter()
t.add(Source({name:"crapfile"}).save({name:"stdout"}))

```

This will copy from the local mongo to a file on the local disk
```js
t = Transporter()
t.add(Source({name:"localmongo", namespace: "boom.foo"}).save({name:"tofile"}))
```

Transformers are also configured in the application.js as follows
```js
var t = Transporter()
var pipeline = Source({name:"mongodb-production", namespace: "compose.milestones2"})
pipeline = transporter.transform("transformers/transform1.js")
pipeline = transporter.transform("transformers/transform2.js")
pipeline = transporter.save({name:"supernick", namespace: "something/posts2"});
t.add(pipeline)
```
Run
---

- list `./transporter --config ./test/config.yaml list`
- run `./transporter --config ./test/config.yaml run ./test/application.js`


Contributing to Transporter
======================

[![Circle CI](https://circleci.com/gh/compose/transporter/tree/master.png?style=badge)](https://circleci.com/gh/compose/transporter/tree/master)

Want to help out with Transporter? Great! There are instructions to get you
started [here](CONTRIBUTING.md). If you'd like to contribute to the
documentation, please take a look at this [README.md](https://github.com/docker/docker/blob/master/docs/README.md).



Licensing
=========
Transporter is licensed under the New BSD. See LICENSE for full license text.


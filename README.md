# `wait-for` things in CircleCI

[CircleCI Orb](https://circleci.com/docs/2.0/orb-intro/) commands to wait for anything testable with a [Bourne shell](https://en.wikipedia.org/wiki/Bourne_shell) command

Usefull when you need to wait for background dependencies to be ready before next job steps. 


```yaml
version: 2.1
orbs:
wait-for: cobli/wait-for@x.x.x
jobs:
build:
    docker:
    - image: builder-image:tag
    steps:
    - checkout
    - run:
        name: Some stuff
        command: ./some_command
    - wait-for/sh-command:
        #checks if a file exists ...
        sh_command: "[ ! -f /tmp/foo.txt ]"
        #... each 10 seconds ...
        seconds_between_retries: 10
        #... for 60 seconds
        timeout: 60
    - run:
        name: Build
        command: ./build_command

```

Needs [timeout GNU command](https://www.gnu.org/software/coreutils/manual/html_node/timeout-invocation.html#timeout-invocation) in CircleCI machine or base container.

Beside `sh-command`, this Orb contains some higher level `commands` to be used to wait for others containers/backgroud services:
* wait-for/port: waits for a port to be open. Needs [netcat](http://nc110.sourceforge.net/) (present in all important linux distribution)
* wait-for/kafka: waits for a [Kafka](https://kafka.apache.org/) service to be ready. Needs [netcat](http://nc110.sourceforge.net/)
* wait-for/redis: waits for a [Redis](https://redis.io/) service to be ready. Needs [netcat](http://nc110.sourceforge.net/)
* wait-for/cassandra: waits for a [Cassandra](http://cassandra.apache.org/) database to be ready. Needs [netcat](http://nc110.sourceforge.net/)
* wait-for/localstack-s3: waits for a [Localstack](https://github.com/localstack/localstack) S3 service to be ready. Needs [curl](https://curl.haxx.se/dev/source.html)

See [examples](src/orb.yaml#L19)
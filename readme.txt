 Yandex.Tank is an extensible load testing utility for unix systems.
 It is written in Python and uses different load generator modules in different languages

Docker container


Install docker and use direvius/yandex-tank (or, if you need jmeter, try direvius/yandex-tank-jmeter) container. Default entrypoint is /usr/local/bin/yandex-tank so you may just run it to start test:


docker run \
    -v $(pwd):/var/loadtest \
    -v $SSH_AUTH_SOCK:/ssh-agent -e SSH_AUTH_SOCK=/ssh-agent \
    --net host \
    -it direvius/yandex-tank



$(pwd):/var/loadtest - current directory mounted to /var/loadtest in container to pass data for test (config file, monitoring config, ammo, etc)


tank will use load.yaml from current directory as default config, append -c custom-config-name.yaml to run with other config


you may pass other additional parameters for tank in run command, just append it after image name


$SSH_AUTH_SOCK:/ssh-agent - ssh agent socket mounted in order to provide use telegraf plugin (monitoring). It uses your ssh keys to remotely login to monitored hosts
If you want to do something in the container befor running tank, you will need to change entrypoint:


docker run \
    -v $(pwd):/var/loadtest \
    -v $SSH_AUTH_SOCK:/ssh-agent -e SSH_AUTH_SOCK=/ssh-agent \
    --net host \
    -it \
    --entrypoint /bin/bash \
    direvius/yandex-tank
Start test Within container with yandex-tank command:


yandex-tank -c config-name.yaml # default config is load.yaml


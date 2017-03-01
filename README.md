drone-auth-ecr is a docker image that lives in your private docker registry that can be pulled to auth against ECR from within drone, 
enabling drone to pull from ECR

**Author**: [Scott Lackey](https://github.com/scottlackey)

This image acts as a liazon between your [drone](http://readme.drone.io/0.5/) instance and [Amazon ECR](https://aws.amazon.com/ecr/). 
It has the AWS ECR client and the docker daemon installed so that it can pull an image from ECR. Since drone shares it's volumes the 
ECR image is available for all build steps.

To build
---------

- update the credentials file
- build the image with docker build, tag and push
- Only push to a private registry


example usage in Drone v.5 pipeline
-------------
```bash
pipeline:
  auth:
    image: drone-auth-ecr
    auth_config:
      username: ${DOCKER_REGISTRY_USERNAME}
      password: ${DOCKER_REGISTRY_PASSWORD}
    commands:
      - aws ecr get-login | bash 
      - docker pull 12345.dkr.ecr.us-west-2.amazonaws.com/my-image:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock

  build:
    image: 12345.dkr.ecr.us-west-2.amazonaws.com/my-image:latest
    commands:
     - npm intstall
     - npm test
```

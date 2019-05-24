# Docker AWS Secrets

Wrapper do AWS Secrets Manager. íra simplemente dar uma saida dos segredos. Útil para carregar
segredos em um arquivo .env ou exportar no ambiente, por exemplo, em estágios de CI.

Esse projeto foi inspirado em [https://github.com/Clevyio/docker-awssecrets](https://github.com/Clevyio/docker-awssecrets)

Exemplo: Gitlab CI

```yml
stages:
  - secrets
  - deploy

secrets:
  stage: secrets
  image: autodoc/awssecrets
  script:
    - awssecrets --region eu-west-1 --secret my-secrets > .ci-secrets
  artifacts:
    expire_in: 30 min 
    paths:
      # this will be available by default in all the next stages
      - .ci-secrets

release:
  stage: deploy
  image: node
  before_script:
    # you can import the content of the secrets file in your environment like so
    - export $(cat .ci-secrets | xargs)
    - echo "//registry.npmjs.org/:_authToken=${NPM_TOKEN}" >> ~/.npmrc
  script:
    - npm install -q
    - npm publish
```


Uso básico

```bash
$ docker run \
    --name awssecrets \
    --rm \
    -e AWS_ACCESS_KEY_ID=$SUA_ACCESS_KEY_ID \
    -e AWS_SECRET_ACCESS_KEY=$SUA_SECRET_ACCESS_KEY \
    autodoc/awssecrets awssecrets --region us-east-1 --secret nameOfSecret

SUPER_SECRET=toto
ULTRA_SECRET=tata
VERY_VERY_SECRET=poepoe
```

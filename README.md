# Docker AWS Secrets

Wrapper do AWS Secrets Manager. íra simplemente dar uma saida dos segredos. Útil para carregar
segredos em um arquivo .env ou exportar no ambiente, por exemplo, em estágios de CI.

Uso básico

```
$ docker run \
  -e AWS_SECRET_ACCESS_KEY \
  -e AWS_ACCESS_KEY_ID \
  autodoc/awssecrets --region eu-west-1 --secret nameOfSecret

SUPER_SECRET=toto
ULTRA_SECRET=tata
VERY_VERY_SECRET=poepoe
```

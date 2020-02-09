# quarkus-simple project

```bash
docker buildx ls
docker buildx create --name testbuilder
docker buildx use testbuilder
docker buildx inspect --bootstrap

docker buildx build --platform linux/arm64 -f src/main/docker/Dockerfile -t rmh78/quarkus-simple-native --push .
```
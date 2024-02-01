## Logseq
### Self-host Web App
https://github.com/logseq/logseq/blob/master/docs/docker-web-app-guide.md

```
docker pull ghcr.io/logseq/logseq-webapp:latest
docker run -d --rm -p 0.0.0.0:3001:80 ghcr.io/logseq/logseq-webapp:latest
```
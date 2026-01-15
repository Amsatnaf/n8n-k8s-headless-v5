# n8n + Postgres + Redis com Headless Services — v5

Correções deste release:
- **Workers corrigidos**: `enableServiceLinks: false` e `N8N_PORT=5678`.
- **n8n-main** também ajustado com as mesmas flags.

## Aplicar
```bash
kubectl apply -k .
```

## Reiniciar rollouts
```bash
kubectl -n n8n rollout restart sts/n8n-main
kubectl -n n8n rollout restart deploy/n8n-workers
kubectl -n n8n rollout status sts/n8n-main
kubectl -n n8n rollout status deploy/n8n-workers
```

## Verificações
```bash
kubectl -n n8n logs sts/n8n-main --tail=200
kubectl -n n8n logs deploy/n8n-workers --tail=200
kubectl -n n8n get endpoints n8n-postgres-hs n8n-redis-hs -o wide
kubectl -n n8n exec deploy/n8n-workers -- getent hosts n8n-postgres-0.n8n-postgres-hs || true
kubectl -n n8n exec deploy/n8n-workers -- getent hosts n8n-redis-0.n8n-redis-hs || true
```

## Observações
- Se for usar **homolog** no mesmo cluster, ajuste NodePort ou prefira Ingress.
- Você pode alterar `namespace:` no `kustomization.yaml` e ajustar `namespace.yaml` conforme necessário.
- Para reprodutibilidade, considere fixar versões de imagens (n8n/postgres/redis).
# n8n-k8s-headless-v5

# Ambientes

Este repositório usa `base/` e `overlays/{desenvolvimento,aceite,producao}`.

- `desenvolvimento`: `TempoMonolithic`, PVC pequeno, OTLP gRPC/HTTP e RBAC para Grafana no CRC.
- `aceite`: mantém o mesmo desenho lógico com PVC maior para homologação.
- `producao`: entrypoint declarativo com sizing maior; antes de usar em produção real, avalie migrar para `TempoStack`, object storage, HA e política formal de retenção.

Validação:

```bash
oc kustomize overlays/desenvolvimento >/tmp/tempo-dev.yaml
oc kustomize overlays/aceite >/tmp/tempo-aceite.yaml
oc kustomize overlays/producao >/tmp/tempo-prod.yaml
oc apply --dry-run=client -k overlays/desenvolvimento
```

Decisões:

- `storageClassName` não é fixado; o cluster escolhe a StorageClass padrão ou o overlay deve patchar uma classe específica.
- O health check do datasource Tempo no Grafana pode retornar `404` no gateway multi-tenant do Operator; valide traces reais e pods/Services.

Automação preservada:

- `.github/workflows/validate.yml`: renderiza todos os Kustomizations e executa `yamllint`.

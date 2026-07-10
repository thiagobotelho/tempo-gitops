# Ambientes

Este repositório usa `base/` e `overlays/{desenvolvimento,aceite,producao}`.

- `desenvolvimento`: `TempoMonolithic`, PVC pequeno, OTLP gRPC/HTTP e RBAC para Grafana no CRC.
- `aceite`: mantém o mesmo desenho lógico com PVC maior para homologação.
- `producao`: entrypoint declarativo com sizing maior; antes de usar em
  produção real, avalie migrar para `TempoStack`, object storage, HA,
  metrics-generator e política formal de retenção.

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
- `extraConfig` de metrics generator/local blocks/query frontend é mantido nos
  três overlays; não remova de aceite/producao sem validar TraceQL Metrics,
  exemplares e drilldown no Grafana.
- `TempoMonolithic` é mantido no CRC para evitar object storage obrigatório.
  `TempoStack` exige MinIO/S3/Azure/GCS/OpenShift Data Foundation e tenants
  configurados; use-o quando houver necessidade de HA, retenção formal e
  metrics-generator para Service Graph.

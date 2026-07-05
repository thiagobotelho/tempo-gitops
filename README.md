# Tempo GitOps

Red Hat OpenShift Distributed Tracing Platform em modo `TempoMonolithic`,
dimensionado para OpenShift Local. Recebe OTLP por gRPC/HTTP, persiste traces
em um PVC de 10 GiB e publica métricas operacionais para Prometheus.

## Fluxo

```text
aplicações -> OpenTelemetry Collector -> TempoMonolithic -> Grafana
```

O Collector do repositório `opentelemetry-gitops` é o ponto de entrada
recomendado: ele aplica limites/batching, exporta para Tempo e gera métricas RED
com exemplares. O Grafana correlaciona métricas, traces e logs.

## Deploy

```bash
oc apply -k overlays/cluster
oc -n openshift-tempo-operator get subscription,csv,pods
oc -n openshift-tempo-operator get tempomonolithic,pvc,service
```

Validação declarativa:

```bash
oc kustomize overlays/cluster >/tmp/tempo.yaml
oc apply --dry-run=server -f /tmp/tempo.yaml
```

## Drilldown

Esta implantação oferece busca e visualização de traces no Grafana. As métricas
RED que alimentam navegação e Drilldown vêm do connector `spanmetrics`.
Service Graph completo exige métricas de grafo geradas pelo Tempo
metrics-generator ou Grafana Alloy; isso não é presumido neste perfil para
evitar configurar campos não suportados pelo CRD Red Hat instalado.

TraceQL metrics e recursos recentes de Traces Drilldown variam com a versão do
backend entregue pelo Operator. Verifique primeiro o CSV e a imagem em execução,
em vez de assumir equivalência com o Tempo comunitário mais recente.

## Limites do perfil local

- uma única instância, sem alta disponibilidade;
- backend em PVC local;
- retenção/capacidade limitadas pelo volume;
- sem Route pública para ingestão;
- sem object storage, replicação ou disaster recovery.

Para produção, use `TempoStack`, object storage, autenticação multi-tenant,
criptografia, políticas de retenção e sizing baseados no volume de spans.

Referências: [Distributed Tracing no OpenShift](https://docs.redhat.com/en/documentation/openshift_container_platform/4.20/html/distributed_tracing/)
e [Grafana Tempo](https://grafana.com/docs/tempo/latest/).

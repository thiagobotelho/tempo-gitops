# Traces, tenants e armazenamento


O Collector autentica no gateway do Tempo e informa o tenant. Multi-tenancy separa acesso lógico, não cria alta disponibilidade. Monolithic com PV mantém dependência de armazenamento e instância. Teste tenant válido, tenant não autorizado, envio de spans e consulta pelo Grafana antes de considerar a correlação funcional.

## Distinguir as etapas

Verifique recepção OTLP, exportação autenticada do Collector e consulta pelo mesmo tenant. Um backend saudável não comprova que uma requisição específica foi amostrada. Kiali e Grafana consultam o gateway; a configuração e o diagnóstico do emissor mesh ficam no guia de observabilidade de service-mesh-gitops.

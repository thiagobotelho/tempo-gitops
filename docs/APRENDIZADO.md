# Traces, tenants e armazenamento

## O que observar

O Collector autentica no gateway do Tempo e informa o tenant. Multi-tenancy separa acesso lógico, não cria alta disponibilidade. Monolithic com PV mantém dependência de armazenamento e instância. Teste tenant válido, tenant não autorizado, envio de spans e consulta pelo Grafana antes de considerar a correlação funcional.

## Roteiro de leitura e validação

1. Leia o README e identifique o componente que recebe a configuração.
2. Confira namespaces, seletores, portas, identidades e dependências nos exemplos.
3. Ajuste apenas os campos necessários em um ambiente isolado; não use credenciais de produção.
4. Valide sintaxe e contratos locais antes de aplicar. Registre o resultado real separado deste guia.
5. Execute um caso permitido e um caso negado, conferindo a causa da resposta.
6. Defina reversão por recurso/arquivo e preserve dados persistentes antes de mudanças.

Os comentários dos manifests explicam escolhas; não comprovam disponibilidade do componente. Exemplos fora de entradas Kustomize não são implantados automaticamente.

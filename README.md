
# ⏱️ Tempo GitOps

Repositório GitOps para provisionamento e gerenciamento do **Red Hat Tempo Operator** no OpenShift.  
Esse repositório aplica práticas de **Infraestrutura como Código (IaC)** e é gerenciado via **Argo CD** para garantir consistência, versionamento e rastreabilidade.

---

## 📂 Estrutura de Pastas

```bash
tempo-gitops/
├── base/                     # Manifests genéricos do Operator
│   ├── kustomization.yaml
│   ├── namespace.yaml        # Namespace do Operator (tempo-system)
│   ├── operatorgroup.yaml    # OperatorGroup para o Operator Tempo
│   └── subscription.yaml     # Subscription para instalar o Operator Tempo
│
├── overlays/
│   ├── cluster/              # Overlay para instalação no cluster
│   │   ├── kustomization.yaml
│   └── └── tempo.yaml        # Definição do TempoStack               
│
└── README.md                 # Este arquivo
```

---

## 🚀 Instalação

### 1. Aplicar o Operator (base + overlay cluster)

```bash
oc apply -k overlays/cluster
```

Isso cria automaticamente os seguintes recursos:
- Namespace `openshift-tempo-operator`
- OperatorGroup para o Operator Tempo
- Subscription para instalar o Operator Tempo (canal `stable`)

### 2. Validar instalação

```bash
oc get pods -n openshift-tempo-operator
```

Você deve ver os pods do **tempo-operator** em execução.

---

## 🔍 Observações

- O canal do Operator utilizado: **stable**
- O namespace padrão utilizado: **tempo-system**
- Para produção, recomenda-se configurar `TLS`, `StorageClass` e `backend S3` no manifest `tempostack.yaml`.

---

## 📖 Referências

- [Red Hat Tempo Operator](https://access.redhat.com/documentation)
- [Grafana Tempo Docs](https://grafana.com/docs/tempo/latest/)
- [Argo CD](https://argo-cd.readthedocs.io/)

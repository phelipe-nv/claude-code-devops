## MODIFIED Requirements

### Requirement: Postgres roda sem volume persistente
O Deployment do Postgres em `k8s/kube-news.yml` SHALL rodar sem PersistentVolumeClaim, sem volumeMount e sem volumes — os dados ficam no filesystem efêmero do container e são perdidos em qualquer restart do pod (comportamento didático intencional).

#### Scenario: Pod do Postgres em estado Running sem PVC
- **WHEN** o manifest `k8s/kube-news.yml` é aplicado no cluster DOKS
- **THEN** o pod Postgres SHALL estar no estado `Running` sem nenhum PersistentVolumeClaim associado

#### Scenario: Dados perdidos ao restartar o pod
- **WHEN** o pod do Postgres é reiniciado ou recriado
- **THEN** os dados SHALL ser perdidos (banco vazio) por ausência de volume persistente

#### Scenario: Manifest não contém recursos de PVC
- **WHEN** o arquivo `k8s/kube-news.yml` é inspecionado
- **THEN** não SHALL existir nenhum objeto `kind: PersistentVolumeClaim` nem referências a `volumeMounts` ou `volumes` no Deployment do Postgres

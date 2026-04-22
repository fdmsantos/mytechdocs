# Cloud Security Posture Management (CSPM)

## Overview

CSPM is included in the **Cloud Posture Management** license.

## Containers

### Image Types

| Image Type | Description | Discovery Method | Key Properties |
|---|---|---|---|
| **Core Image** | Immutable content of the image; foundation for all other types. | — | Identified by SHA256 digest. Contains file-related findings (vulnerabilities, secrets, malware). Can reference another Core Image as its base. No scope — cannot belong to an asset group or policy directly. |
| **Build Image** | Image produced by a CI/CD pipeline or build process. | CLI scanning | Includes build metadata (build time, source repo, build environment). Contains build-specific findings and issues. Represents a Core Image. |
| **Registry Image** | Image stored in a container registry (AWS ECR, Azure ACR, Google GAR, JFrog Artifactory, Docker). | Cloud discovery or registry scanning | Includes registry metadata (FQDN, repository name, tags, manifest digests). Resides within an image repository. Represents a Core Image. |
| **Runtime Image** | Image stored, running, or defined in a workload (VMs, Kubernetes workloads). | Agentless Disk scan / XDR agent scan | Contains deployment and operational findings (config deviations, policy violations). File-related findings are derived from the connected Core Image. Represents a Core Image. |

All image types can be queried via XQL and are listed under **Inventory → All Assets → Compute → Container Images**.

## Automação

### Centro de Exclusão de Automação

Protege ativos críticos (utilizadores, IPs, domínios) de remediação automatizada por playbooks, Quick Actions ou CLI.

**Acesso:** `Settings → Configurations → Automation → Automation Exclusion Center`

- As políticas estão habilitadas por padrão, mas as listas ficam vazias até que ativos sejam adicionados.
- Quando uma política bloqueia uma ação, a exclusão aparece na **War Room** da issue.
- Políticas podem ser habilitadas/desabilitadas, mas não adicionadas ou removidas.
- Os comandos/scripts cobertos por cada política não são editáveis.
- Por padrão, apenas administradores têm acesso. Pode ser concedido a outras funções em `Investigation & Response → Automations`.

**Listas de ativos excluídos:**

- Cada política usa uma ou mais listas de ativos a proteger.
- As listas suportam filtros como `Equals`, `Ends with`, `Doesn't include`.
- As políticas **IAM User Hard/Soft Remediation** também suportam **grupos de ativos**, além de listas.
- Recomenda-se restringir permissões de edição das listas a funções específicas.

**Override Policy:**

Permite ignorar manualmente uma política e executar o comando no ativo protegido, usando o parâmetro `override-policy` na War Room. Requer que a opção de substituição esteja habilitada na política. Todas as execuções com este parâmetro ficam registadas nos **Logs de Auditoria de Gerenciamento**.
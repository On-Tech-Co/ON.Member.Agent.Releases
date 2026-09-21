# ON Gateway — Instalador

Este repositório distribui o instalador do **ON Gateway**, o aplicativo Windows usado pelos
atendentes da [Comunidade ON](https://comunidadeon.com) para acessar as plataformas dos parceiros
durante o atendimento.

Ele **não contém código-fonte**. Recebe apenas os artefatos publicados automaticamente pelo
pipeline de release.

## Instalação

Baixe o `.msi` mais recente em **[Releases](../../releases/latest)** e execute.

Antes de baixar, vale saber:

- **Requer privilégio de administrador.** A instalação é para toda a máquina, em
  `C:\Program Files\ON\On Gateway`.
- **O Windows vai avisar sobre "editor desconhecido".** O instalador ainda não é assinado
  digitalmente. É esperado, não é sinal de problema — em *Mais informações* → *Executar assim mesmo*.
- **Só Windows.**

Depois de instalar, o App aparece no menu Iniciar e na área de trabalho como **On Gateway**. Ele não
precisa ser aberto na mão: o Member WebApp o abre sozinho quando você acessa uma plataforma durante
o atendimento.

## Para automação e suporte

Cada release carrega um `manifest.json` descrevendo a versão publicada:

```
https://github.com/On-Tech-Co/ON.Member.Agent.Releases/releases/latest/download/manifest.json
```

```json
{
  "version": "1.2.87",
  "assemblyVersion": "1.2.87.0",
  "url": "https://github.com/On-Tech-Co/ON.Member.Agent.Releases/releases/download/v1.2.87/OnGateway-1.2.87-x86.msi",
  "sha256": "...",
  "sizeBytes": 92340230,
  "releasedAt": "2026-09-21T14:02:00Z",
  "signed": false
}
```

| Campo | Para que serve |
|---|---|
| `version` | O número que humano lê, e o que nomeia a tag e o arquivo |
| `assemblyVersion` | O que se compara com a versão instalada — o .NET sempre reporta quatro campos |
| `url` | Download imutável daquela versão específica |
| `sha256` | Conferir a integridade do arquivo baixado antes de executá-lo |
| `sizeBytes` | Tamanho exato, para progresso e detecção de download truncado |
| `releasedAt` | Quando aquela versão foi publicada (UTC) |
| `signed` | Se o binário está assinado digitalmente |

Para descobrir a versão instalada numa máquina, com o App aberto:

```
GET http://127.0.0.1:47831/health
```

```json
{"ok":true,"name":"ON.Member.Agent","version":"1.2.87.0","pid":18216,"utc":"..."}
```

O campo `version` dessa resposta corresponde ao `assemblyVersion` do manifest — e não ao `version`,
porque o .NET sempre reporta quatro campos.

## Verificando o download

```powershell
Get-FileHash .\OnGateway-1.2.87-x86.msi -Algorithm SHA256
```

O resultado deve bater com o `sha256` do `manifest.json` daquela release. Se não bater, o arquivo
chegou corrompido ou alterado — não instale.

## Suporte

Este repositório não aceita issues. Problemas com o App devem ir pelos canais internos de suporte da
Comunidade ON.

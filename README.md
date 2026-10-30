# sabia-toolkits

Índice público dos **toolkits** do [Sabiá](https://github.com/dxaraujo/sabia). O índice só aponta para os toolkits:
cada um vive no repositório dele, e aqui fica a entrada com a fonte, a versão e o commit exato.

## O que é um toolkit

O Sabiá é o fluxo e não sabe programar em nenhuma stack. Um toolkit traz esse conhecimento: um pacote de skills
(Java, Spring Batch, Angular…) descrito por um `toolkit.json` na raiz
([schema](https://raw.githubusercontent.com/dxaraujo/sabia/main/skills/sa-setup/assets/toolkit.schema.json)). Cada
skill responde a três perguntas:

- **Quem** a usa: `papeis` (codificador, revisor, especificador…).
- **Onde** ela se aplica: `stack` e `deteccao`; a setup só a pré-marca se o repositório casar.
- **Em que passo**: `passo`, opcional. Sem ele, é uma skill de apoio, que o agente carrega quando o artefato pede.
  Com ele, a skill assume um passo de um agente do Sabiá — escrever o PRD (`especificador`) ou codificar uma tarefa
  (`codificador`) — e pode delegar a um agente do toolkit (`agentes[]`). O Sabiá continua dono da revisão, do GATE,
  da verificação e do commit ([passos.md](https://github.com/dxaraujo/sabia/blob/main/skills/sabia/references/passos.md)).

## Usar

```sh
npx sabia toolkits listar            # este índice e os extras de ~/.sabia/indices.json
npx sabia toolkits instalar <nome>   # baixa para ~/.sabia/toolkits/<nome>/ e copia para as ferramentas
```

Depois, no projeto, peça ao agente "atualizar os agentes do Sabiá" para ligar o toolkit.

## Propor um toolkit

Abra um PR que acrescenta uma entrada em `plugins` do [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json):

```json
{
  "name": "spring-batch",
  "version": "1.0.0",
  "description": "Jobs Spring Batch 5: steps chunk e tasklet, readers e writers JDBC, testes de job",
  "source": { "source": "github", "repo": "<dono>/<repo>", "ref": "v1.0.0", "sha": "<commit de 40 caracteres>" }
}
```

- `name` igual ao `nome` do `toolkit.json`, em kebab-case, nunca começando com `sa-`.
- `source`: `github` (`repo`), `url` (qualquer git: GitHub, GitLab, self-hosted) ou `git-subdir` (`url` e `path`),
  sempre com `ref` e `sha`. O CLI clona a `ref` e recusa o toolkit se o commit não for o `sha`.
- O toolkit precisa passar na validação do CLI: campos obrigatórios, caminhos dentro da pasta e arquivos que
  existem. Confira antes com `npx sabia toolkits baixar <url-git>@<ref>`.
- Nada de nome de empresa, projeto ou dado de cliente: o índice é público.

**Estar no índice não é auditoria.** A entrada fixa a versão e o commit; quem instala decide se confia no conteúdo.
A cada `sabia toolkits atualizar`, o CLI mostra os arquivos novos, alterados e removidos antes de trocar, e nunca
executa nada do toolkit.

## Índice privado (GitLab da empresa)

O formato é o mesmo: um repositório com `.claude-plugin/marketplace.json`, num GitLab interno, com as entradas
apontando para os toolkits privados (`source` `url`, com `ref` e `sha`). O CLI clona com o git da pessoa (credencial,
SSH, CA), então privados e subgrupos funcionam sem configuração extra.

Para usar:

```sh
npx sabia toolkits listar --indice https://gitlab.<empresa>/<grupo>/sabia-toolkits.git
```

ou, para valer sempre, uma lista de URLs em `~/.sabia/indices.json`:

```json
["https://gitlab.<empresa>/<grupo>/sabia-toolkits.git"]
```

## Licença

[MIT](LICENSE). Cada toolkit tem a licença do repositório dele.

# Guia de Sincronização do SupoClip (Fork com Upstream)

Este repositório é um fork customizado mantido por `vmellos` a partir do projeto original `FujiwaraChoki/supoclip`.

## Configuração dos Remotes

```text
origin    git@github.com:vmellos/supoclip.git (fetch & push)  -> Seu Fork
upstream  git@github.com:FujiwaraChoki/supoclip.git (fetch & push)  -> Repositório Original
```

---

## Fluxo de Sincronização em 2 Etapas

Quando quiser sincronizar com as novidades e hotfixes do criador original:

### Etapa 1: Atualizar a `main`
Você vai para a `main` apenas para receber o código limpo do autor original e espelhar no seu GitHub:

```bash
git checkout main
git merge upstream/main
git push origin main
```

### Etapa 2: Selecionar a `custom` e incorporar as novidades
Agora sim você seleciona a sua branch `custom` e puxa as novidades da `main` para dentro dela:

```bash
git checkout custom
git merge main
git push origin custom
```

---

> [!TIP]
> **Você nem precisa decorar os comandos:**  
> Sempre que quiser atualizar no futuro, basta abrir o chat com o agente de IA e dizer:  
> *"Sincronize com o projeto original"* ou *"Puxe as novidades do upstream"*.  
> O agente executará o protocolo com segurança, validará se há conflitos e deixará você pronto na branch `custom`.

---

## Por que fazemos nessa ordem?

- **A branch `main` se mantém limpa:** Ela atua como um espelho idêntico do repositório oficial (`FujiwaraChoki/supoclip`), garantindo que o comando `git merge upstream/main` seja sempre um *fast-forward* instantâneo e livre de conflitos.
- **Suas customizações ficam protegidas:** A branch `custom` recebe as melhorias e correções da `main` mantendo intactos seus ajustes, regras de IA e arquivos de configuração.

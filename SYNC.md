# Guia de Sincronização do SupoClip (Fork com Upstream)

Este repositório é um fork customizado mantido por `vmellos` a partir do projeto original `FujiwaraChoki/supoclip`.

## Configuração dos Remotes

```text
origin    git@github.com:vmellos/supoclip.git (fetch & push)  -> Seu Fork
upstream  git@github.com:FujiwaraChoki/supoclip.git (fetch & push)  -> Repositório Original
```

---

## Fluxo do Dia a Dia

Siga os 3 passos abaixo para manter seu código sempre atualizado sem gerar conflitos na `main`:

```bash
# 1. No dia a dia, você trabalha e roda o projeto na branch custom:
git checkout custom

# 2. Quando quiser atualizar com novidades do criador original:
git checkout main
git merge upstream/main
git push origin main

# 3. Incorpora as novidades na sua branch de ajustes:
git checkout custom
git merge main
git push origin custom
```

---

## Dicas Adicionais

- **Nunca faça commits pessoais na branch `main`:** Deixe a `main` sempre idêntica à do `upstream`. Assim, o comando `git merge upstream/main` será sempre um *fast-forward* limpo e automático.
- **Resolução de Conflitos:** Se houver conflito no passo 3 (`git merge main`), resolva mantendo suas customizações e finalize com `git commit`.

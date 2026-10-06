# Wrapper — Agenda Comercial (GitHub Pages)

```
https://secretaria-de-vendas-alicerce.github.io/agenda-comercial/
https://secretaria-de-vendas-alicerce.github.io/agenda-comercial/?e=<e-mail>   (o link do convite)
```

## Por que este REDIRECIONA, e os outros embutem

Os wrappers do #9, #11, #13 e #21 carregam a `/exec` num `<iframe>`, e funcionam porque aqueles
web apps executam **como o dono**: a identidade vem do login próprio do app.

A Agenda executa como **quem acessa** — o Google Agenda que ela lê é o da pessoa. Dentro de um
iframe o login do Google não acontece (F0, D3), então iframe aqui dá tela branca. É por isso que
o card #22 do Hub está `via: 'exec'`.

O que este wrapper resolve é o outro problema, o mesmo que fez os demais existirem: a `/exec`
crua passa pelo roteador `/u/N/` do Google, e quem está com duas contas logadas (a pessoal e a da
equipe) abre na **conta errada** — o app responde "seu e-mail ainda não foi convidado". Com o `?e=`
do convite, o wrapper manda para a `/exec?authuser=<e-mail>`, que abre **naquela** conta; sem `?e=`,
vai pelo **seletor de conta** do Google.

**Não usar `AccountChooser?Email=…&continue=<exec>`** (era o desenho até 06/10): o chooser escolhe a
conta, mas o script.google.com não herda a escolha — cai na conta padrão (`/u/0`) ou numa conta
Workspace, vira `/a/<domínio>/…` e dá "Não foi possível abrir o arquivo".

## Publicar

```bash
gh repo create secretaria-de-vendas-alicerce/agenda-comercial --public --source=. --push
gh api -X POST repos/secretaria-de-vendas-alicerce/agenda-comercial/pages \
  -f "source[branch]=main" -f "source[path]=/"
```

Repo **público**, Pages em `/ (root)`, `.nojekyll` vazio na raiz.

## Quando mexer

- **A `/exec` mudou** (só acontece se alguém criar um deployment NOVO em vez de `-i <mesmo id>`,
  Regra 30): trocar em `index.html` (2 lugares: o `EXEC` do script e o link do `<noscript>`).
- **O endereço do Pages mudou**: trocar `LINK_PUBLICO` em `app/Config.gs`, dar push e implantar A.

`tests/wrapper.test.js` cobra os dois lados: a `/exec` do wrapper tem de ser uma só e bem formada,
e o `LINK_PUBLICO` do app tem de apontar para este endereço.

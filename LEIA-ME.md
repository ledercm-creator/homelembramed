# Landing page do LembraMed — publicar no Vercel

Pasta pronta para subir. Não precisa de build, npm nem framework: é HTML puro.

```
index.html      a página (o logo do cabeçalho está embutido dentro dela)
favicon.png     ícone da aba do navegador
icon-192.png    ícone para atalho na tela de início do celular
icon-512.png    ícone grande, da pré-visualização de links (WhatsApp, Facebook)
vercel.json     cabeçalhos (faz o .apk baixar em vez de abrir como texto)
```

---

## Publicar (jeito mais fácil, sem instalar nada)

1. Entre em <https://vercel.com> e crie a conta (pode entrar com o GitHub).
2. No painel, clique em **Add New… → Project**.
3. Escolha **Deploy a template or import a folder** e, na opção de upload, **arraste esta pasta inteira**.
   - Se o Vercel perguntar o *Framework Preset*, escolha **Other**.
   - Deixe *Build Command* e *Output Directory* em branco.
4. Clique em **Deploy**. Em menos de um minuto o site está no ar, em algo como
   `https://lembramed.vercel.app`.

### Pelo PowerShell (Windows 11), se preferir

```powershell
npm install -g vercel
cd "$HOME\Desktop\LembraMed\site-vercel"
vercel login
vercel --prod
```

### Domínio próprio

No projeto do Vercel: **Settings → Domains → Add**. Dá para usar um domínio que você
comprar (ex.: `lembramed.com.br`) ou ficar com o `.vercel.app` mesmo.

---

## Onde fica o arquivo do app (APK)

Hoje os botões de download apontam para o site que já existe no Cloudflare:

```
https://lembramed-dap.pages.dev/LembraMed.apk
```

Assim a página funciona **sem precisar mexer em mais nada**. Você tem duas opções:

### Opção A — só a landing page no Vercel (recomendado agora)

Não faça nada. O Vercel serve a página de venda e o Cloudflare continua entregando o APK
e o `update.json` que o app consulta para avisar de novas versões.

### Opção B — mover tudo para o Vercel

1. Copie o `LembraMed.apk` para dentro desta pasta.
2. No `index.html`, troque os **dois** links
   `https://lembramed-dap.pages.dev/LembraMed.apk` por `LembraMed.apk`.
   (Estão no botão do topo e no bloco final; o do topo tem um comentário indicando o lugar.)
3. Copie também o `site/update.json` do projeto para esta pasta e corrija a URL de dentro dele
   para o endereço novo.
4. **Importante:** no app, em `src/lib/license/licenseConfig.ts`, a constante
   `UPDATE_MANIFEST_URL` ainda aponta para o Cloudflare. Se o `update.json` mudar de endereço,
   é preciso alterar essa linha, recompilar o APK e republicar — senão quem já instalou
   nunca mais recebe aviso de atualização. Enquanto houver usuários na versão antiga,
   deixe o endereço do Cloudflare no ar.

Limite do Vercel para arquivos estáticos: 100 MB por arquivo. O APK (~20 MB) cabe tranquilo.

---

## A foto do topo (falta você colocar)

O espaço da foto já está montado no hero, com o véu verde desfocado por cima e o celular
sobreposto no canto. **Só falta o arquivo.** Enquanto ele não existir, aparece um degradê
verde no lugar — nada quebra.

1. Salve a imagem nesta pasta com o nome exato **`foto-hero.jpg`**.
2. Suba a pasta de novo no Vercel. Pronto.

**Como a foto deve ser**

| Item | Recomendado |
|---|---|
| Formato | retrato, proporção 4:5 (ex.: 1000 × 1250 px) |
| Peso | até ~300 KB (salve como JPG qualidade 80) |
| Enquadramento | o rosto deve ficar no **terço superior** — a parte de baixo fica escurecida pelo véu verde e coberta pelo celular |
| Cena | senhora idosa tomando o comprimido com um copo d'água, luz natural, expressão tranquila |

**Onde pegar uma foto livre para uso comercial** (grátis, sem precisar dar crédito):

- Pexels — <https://www.pexels.com/pt-br/pesquisar/idosa%20tomando%20rem%C3%A9dio/>
- Unsplash — <https://unsplash.com/s/photos/senior-woman-taking-medication>
- Freepik (confira a licença de cada arquivo) — <https://br.freepik.com/fotos/idosa-tomando-remedio>

> Evite salvar imagem achada no Google: a maioria tem dono e o uso em página de venda
> pode gerar cobrança. Uma foto real de uma cliente também funciona muito bem — nesse caso,
> peça autorização por escrito para usar a imagem.

**Ajustes finos**, se quiser, no `index.html` (procure por `.photo`):

- `center 28%` controla a altura do recorte. Aumente para 40% se o rosto estiver cortado.
- No bloco `.photo::before`, os valores `rgba(13,115,119,.10)` e `rgba(7,69,72,.30)` são a
  força do verde (quanto menor, mais transparente). Aumente para o verde ficar mais forte.
- `blur(.8px)` é a intensidade do desfoque (suave; use `blur(0)` para nenhum). Use `blur(0)` para a foto ficar nítida.

---

## Mexer no texto

Tudo está em um único `index.html`, em português e sem código complicado. Os pontos que
você provavelmente vai querer trocar:

| O quê | Onde procurar no arquivo |
|---|---|
| Preços dos planos | bloco `<section ... id="planos">`, nas três `div class="plan"` |
| WhatsApp | procure por `5549991522005` (aparece 5 vezes) |
| Versão do app no rodapé | procure por `versão 1.1.1` |
| Link do APK | procure por `LembraMed.apk` |

---

## Antes de divulgar

- [ ] Colocar a `foto-hero.jpg` na pasta (seção acima).
- [ ] Abrir a página no celular e testar o botão de download de ponta a ponta.
- [ ] Conferir se os links de WhatsApp abrem com a mensagem certa.
- [ ] Verificar se o preço e o texto dos planos batem com o que você pratica.
- [ ] Lembrar da verificação de desenvolvedor do Google: desde 30/09/2026, celulares Android
      certificados no Brasil exigem que o APK seja de um desenvolvedor verificado. Sem isso,
      o cliente enfrenta um fluxo avançado com espera de 24 h na primeira instalação.

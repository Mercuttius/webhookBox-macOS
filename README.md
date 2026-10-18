<p align="center">
  <img src="./assets/banner.png" alt="WebhookBox — caixa de entrada de webhooks para macOS" width="100%">
</p>

# WebhookBox para macOS

Receba, inspecione e reenvie webhooks em um aplicativo nativo para Mac. Organize eventos em caixas, consulte headers e body e mantenha o histórico no seu computador. Interface em SwiftUI, fonte Fira Code e tema escuro inspirado no GitHub.

## Download

**Requer macOS Sequoia 15 ou superior.** Escolha o arquivo correspondente ao processador do seu Mac:

| Seu Mac | Download |
| --- | --- |
| Apple Silicon — M1 ou posterior | [Baixar WebhookBox para Apple Silicon (.dmg)](https://github.com/Mercuttius/webhookBox-macOS/releases/download/v0.1.0/WebhookBox-macOS-arm64.dmg) |
| Intel | [Baixar WebhookBox para Intel (.dmg)](https://github.com/Mercuttius/webhookBox-macOS/releases/download/v0.1.0/WebhookBox-macOS-x86_64.dmg) |

Para identificar o processador, abra **menu Apple  → Sobre Este Mac**. Se aparecer “Chip Apple”, escolha Apple Silicon. Se aparecer “Processador Intel”, escolha Intel.

[Ver a versão e todos os arquivos](https://github.com/Mercuttius/webhookBox-macOS/releases/tag/v0.1.0) · [Verificar integridade dos downloads](https://github.com/Mercuttius/webhookBox-macOS/releases/download/v0.1.0/SHA256SUMS.txt)

> **Versão inicial de testes (v0.1.0).** Os aplicativos possuem assinatura local (*ad hoc*), mas ainda não têm assinatura Developer ID nem notarização da Apple. A versão Intel foi executada e testada localmente; a versão Apple Silicon foi compilada e teve sua arquitetura e integridade verificadas, mas ainda não foi executada em um Mac Apple Silicon. A instalação após download em outro Mac ainda precisa de validação.

## Instalação

1. Baixe o `.dmg` correspondente ao seu Mac.
2. Abra o arquivo baixado.
3. Arraste **WebhookBox** para a pasta **Applications / Aplicativos** mostrada na janela.
4. Ejete a imagem de disco.
5. Abra **Aplicativos → WebhookBox**.

**Não é necessário instalar Node.js, npm, Homebrew ou cloudflared.** Os componentes necessários já acompanham cada aplicativo na arquitetura correta.

### Se o macOS bloquear a primeira abertura

Como esta versão ainda não tem Developer ID/notarização, o macOS pode avisar que não consegue verificar o desenvolvedor ou a segurança do app.

Se você baixou o arquivo deste repositório e deseja autorizá-lo:

1. Tente abrir o WebhookBox em **Aplicativos** uma vez.
2. Abra **Ajustes do Sistema → Privacidade e Segurança**.
3. Localize o aviso referente ao **WebhookBox** e clique em **Abrir Mesmo Assim**.
4. Confirme a abertura e autentique-se, se solicitado.

Normalmente essa autorização é necessária apenas na primeira abertura dessa versão. Macs gerenciados por empresas podem impedir a autorização. Não é necessário desativar globalmente as proteções do macOS.

Veja as [instruções oficiais da Apple](https://support.apple.com/pt-br/102445). Se aparecer um aviso diferente, como arquivo danificado, baixe novamente o DMG e [relate o problema](https://github.com/Mercuttius/webhookBox-macOS/issues), informando a versão do macOS e o processador.

## Primeiro webhook local

1. Abra o WebhookBox.
2. Clique em **Nova caixa**, digite um nome, como `pagamentos`, e confirme.
3. Selecione a caixa na barra lateral.
4. Clique em **Iniciar servidor**. A porta padrão é `3000`.
5. Clique em **Copiar URL** na caixa selecionada.
6. Envie uma requisição HTTP para essa URL usando sua aplicação, Postman, Insomnia ou o Terminal.

Exemplo no Terminal: substitua `COLE_A_URL_DA_CAIXA_AQUI` pela URL copiada do app.

```sh
curl -X POST 'COLE_A_URL_DA_CAIXA_AQUI' \
  -H 'Content-Type: application/json' \
  -d '{"event":"payment.approved","amount":4990,"currency":"BRL"}'
```

O servidor retorna `202` quando o evento é salvo. A lista é atualizada aproximadamente a cada segundo. Selecione o evento para consultar **Body**, **Headers** e **Query**.

Se a porta estiver ocupada, encerre o outro servidor ou escolha outra porta em **Configurações** antes de iniciar. Copie novamente a URL depois de alterar a porta.

## Receber webhooks da internet

1. Crie uma caixa e inicie o servidor local.
2. Clique em **Túnel público → Abrir túnel público**.
3. Aguarde o estado **Túnel público conectado**.
4. Copie a **URL pública** da caixa desejada.
5. Configure essa URL no serviço que enviará os webhooks.

O app usa Cloudflare Quick Tunnels, sem exigir uma conta Cloudflare. A URL HTTPS é temporária e muda a cada nova conexão. Quem tiver a URL pode enviar eventos; o tráfego passa pela Cloudflare. O Mac precisa permanecer ligado, conectado e com o app aberto. O serviço é destinado a testes e não oferece garantia de disponibilidade. [Documentação da Cloudflare](https://developers.cloudflare.com/tunnel/get-started/quick-tunnels/).

Para interromper o acesso, use **Encerrar túnel**, pare o servidor ou encerre o aplicativo com **⌘Q**. Fechar apenas a janela não equivale a encerrar o app.

## Inspecionar, exportar e reenviar

- **Buscar e filtrar:** encontre eventos na lista por texto e método HTTP.
- **Copiar:** copie URLs e conteúdo das abas.
- **Exportar:** salve o evento completo em JSON; o body original fica em base64 para preservar os bytes, inclusive conteúdos binários.
- **Reenviar evento:** informe a URL de destino e confirme em **Enviar agora**. Authorization e cookies são removidos por padrão; a opção pode ser desativada. Reenvios podem repetir ações reais no serviço de destino.
- **Excluir evento ou caixa:** remova itens do histórico. Excluir uma caixa também remove os seus eventos.

## Histórico e limites

Os dados são armazenados localmente em:

```text
~/Library/Application Support/WebhookBoxDesktop/webhookbox.db
```

O histórico permanece após encerrar ou atualizar o app. O servidor não é iniciado automaticamente ao reabrir. Os dados são separados dos dados do WebhookBox de terminal.

| Item | Limite atual |
| --- | --- |
| Eventos exibidos | 100 mais recentes; filtros e contadores se referem a essa lista |
| Eventos armazenados | Até 10.000 |
| Tamanho do body | Até 1 MiB por requisição |
| Requisições | Até 120 por minuto |
| Retenção | 7 dias; limpeza ao iniciar e enquanto o servidor está ativo |

Bodies e headers podem conter informações sensíveis e são salvos sem criptografia própria do aplicativo. Para fazer um backup simples, encerre o app antes de copiar a pasta de dados inteira.

## Estado desta versão

- Intel: recebimento HTTP local, persistência, exportação, reenvio, fluxo Swift/Node e túnel HTTPS testados neste Mac de desenvolvimento.
- Apple Silicon: compilação ARM64, dependências ARM64, assinatura ad hoc e integridade do DMG verificadas. Teste de execução em hardware Apple Silicon pendente.
- Developer ID e notarização: ainda não disponíveis.
- Este repositório disponibiliza os instaladores; o motor original está em [Mercuttius/webhookBox](https://github.com/Mercuttius/webhookBox).

## Créditos e suporte

O app inclui Node.js, cloudflared e Fira Code, com suas licenças no pacote. O motor do WebhookBox utiliza licença MIT.

Encontrou um problema? [Abra uma issue](https://github.com/Mercuttius/webhookBox-macOS/issues) com a arquitetura (Intel/Apple Silicon), versão do macOS e passos para reproduzir. Remova tokens, credenciais e dados pessoais antes de anexar eventos ou capturas.

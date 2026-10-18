# WebhookBox macOS 0.2.0 — histórico completo e favoritos

Esta atualização inicia a evolução gratuita e local do app.

## Novidades

- Busca indexada no histórico inteiro por path, query, headers e body textual.
- Paginação de 100 eventos, favoritos persistentes e contadores completos.
- Retenção de 0 a 3650 dias e limite de 100 a 1.000.000 eventos; padrões de 7 dias e 10.000 eventos.
- Backup automático antes de migrar um histórico antigo não vazio.
- Menos recarregamentos da lista, mantendo interface nativa, Fira Code e identidade visual.

## Instalação e atualização

Requer macOS 15 ou superior. Escolha arm64 para Apple Silicon ou x86_64 para Intel. Encerre a versão anterior com ⌘Q, abra o DMG e arraste WebhookBox para Aplicativos, substituindo a versão anterior. O histórico é preservado e migrado na primeira abertura. Não reutilize o banco migrado em versões antigas.

As instruções completas de instalação, primeira abertura e uso estão no README do repositório. Os pacotes têm assinatura ad hoc, sem Developer ID/notarização da Apple.

## Validação e limites

Oito testes automatizados passaram. O diagnóstico nativo Intel verificou captura HTTP, pesquisa, favoritos, detalhes e renderização. Ambos os DMGs tiveram a integridade verificada. Apple Silicon foi compilado e inspecionado, mas ainda não executado em hardware ARM64.

Favoritos continuam sujeitos à retenção. Reduzir limites pode excluir eventos antigos. Backups de migração não são removidos automaticamente. Busca textual aceita palavras/frases e prefixo no último termo, não substrings arbitrárias nem conteúdo binário.

As novas funções funcionam localmente sem serviços pagos. Filas de entregas, mocks avançados e workflows permanecem planejados. Esta release continua marcada como versão de testes.

# MultiWA para macOS

Downloads e instruções de instalação do MultiWA. Este repositório só distribui o app: não tem código-fonte.

O MultiWA é um app nativo de macOS para usar vários WhatsApp ao mesmo tempo, numa janela só:

- contas do **WhatsApp Web**, cada uma isolada (cookies, armazenamento e sessão próprios), conectadas pelo QR code no celular;
- contas da **Whapi Cloud**, um número conectado num canal da [Whapi](https://whapi.cloud) e usado pelo token do canal, com tela nativa de conversas, mensagens, mídia e voz.

## Downloads

- Última versão: [https://github.com/lucas-canavarro/MultiWA-releases/releases/latest](https://github.com/lucas-canavarro/MultiWA-releases/releases/latest)
- Instalador direto: [https://github.com/lucas-canavarro/MultiWA-releases/releases/latest/download/instalar.sh](https://github.com/lucas-canavarro/MultiWA-releases/releases/latest/download/instalar.sh)

Cada versão traz `MultiWA.zip` (o app), `MultiWA.zip.sha256` (checksum), `instalar.sh`, `desinstalar.sh` e `MultiWA-para-instalar.zip` (pasta pronta para mandar a outra pessoa).

## Requisitos

- Mac com Apple Silicon (M1 ou mais novo).
- macOS 14 (Sonoma) ou mais novo.

## Instalar (recomendado)

Abra o Terminal (Aplicativos > Utilitários > Terminal), cole o comando abaixo (uma linha só) e aperte Enter:

```bash
bash -c 'pasta_do_download="$(mktemp -d "${TMPDIR:-/tmp}/multiwa-instalador.XXXXXX")" || exit 1; instalador_baixado="$pasta_do_download/instalar.sh"; apagar_o_instalador_baixado() { rm -f "$instalador_baixado"; rmdir "$pasta_do_download"; }; trap apagar_o_instalador_baixado EXIT; trap "exit 129" HUP; trap "exit 130" INT; trap "exit 143" TERM; { curl -fsSL https://github.com/lucas-canavarro/MultiWA-releases/releases/latest/download/instalar.sh -o "$instalador_baixado" || gh release download --repo lucas-canavarro/MultiWA-releases --pattern instalar.sh --output - > "$instalador_baixado"; } && [ "$(head -n 1 "$instalador_baixado")" = "#!/usr/bin/env bash" ] || { echo "Erro: não foi possível baixar o instalador do MultiWA (sem internet, GitHub fora do ar ou download vazio). Nada foi instalado; tente de novo mais tarde." >&2; exit 1; }; MULTIWA_INSTALADOR_EM_PASTA_TEMPORARIA=1 bash "$instalador_baixado" "$@"' instalar-multiwa
```

Não precisa de conta no GitHub nem de login. O comando baixa o instalador para uma pasta temporária, confere que é mesmo o instalador e só então o roda. O instalador baixa a última versão, confere o checksum SHA-256, instala em `/Applications` (ou em `~/Applications`, se o usuário não puder escrever em `/Applications`; nunca pede `sudo`) e abre o app. Se o download falhar (sem internet, GitHub fora do ar), ele para com uma mensagem de erro, sem instalar nada.

Primeira vez:

1. Clique em "Permitir" quando o macOS pedir para mostrar notificações.
2. Adicione cada conta (⌘N). WhatsApp Web: escaneie o QR code no celular em WhatsApp > Configurações > Dispositivos conectados. Whapi Cloud: escolha "Whapi Cloud (token)" e cole o token do canal.
3. O app passa a abrir sozinho quando o Mac liga (desligável nos Ajustes, ⌘,).
4. Microfone e câmera são pedidos na primeira mensagem de voz ou foto.

## Atualizar

Desde a versão 2.11.0, o MultiWA se atualiza sozinho. A cada 30 minutos (e quando você entra no Mac), ele procura a última versão publicada. Quando sai versão nova:

- com o MultiWA fechado, ele troca na hora e não abre o app;
- com o MultiWA aberto, ele espera você ficar 5 minutos sem usar o Mac (teclado, mouse e trackpad) e fora de ligação, fecha o app com calma, troca e abre de novo em segundo plano, sem tirar você do que está fazendo;
- se a versão nova não abrir neste Mac, ele volta sozinho para a anterior e não tenta de novo essa versão: espera a próxima.

Contas, sessões e ajustes continuam como estão. Antes da troca, guarda uma cópia da versão atual (para voltar, se precisar) e confere o checksum e a assinatura do pacote, como na instalação. Nunca abre o Terminal nem pergunta nada; o que fez fica registrado em `~/Library/Logs/MultiWA/atualizador.log`.

Para atualizar na hora, sem esperar, use um dos dois:

- no app: Ajustes (⌘,) > Atualizações > "Atualizar agora" (abre o Terminal já rodando o instalador);
- ou rode de novo o mesmo comando de instalar.

Quem está na versão 2.10.1 ou anterior atualiza uma vez por um desses dois caminhos; daí em diante, as versões novas entram sozinhas.

### Desligar ou ligar a atualização automática

Para desligar, rode o mesmo comando de instalar com `--sem-atualizacao-automatica` no fim (as próximas atualizações pelo Terminal ou pelo "Atualizar agora" não a ligam de novo):

```bash
bash -c 'pasta_do_download="$(mktemp -d "${TMPDIR:-/tmp}/multiwa-instalador.XXXXXX")" || exit 1; instalador_baixado="$pasta_do_download/instalar.sh"; apagar_o_instalador_baixado() { rm -f "$instalador_baixado"; rmdir "$pasta_do_download"; }; trap apagar_o_instalador_baixado EXIT; trap "exit 129" HUP; trap "exit 130" INT; trap "exit 143" TERM; { curl -fsSL https://github.com/lucas-canavarro/MultiWA-releases/releases/latest/download/instalar.sh -o "$instalador_baixado" || gh release download --repo lucas-canavarro/MultiWA-releases --pattern instalar.sh --output - > "$instalador_baixado"; } && [ "$(head -n 1 "$instalador_baixado")" = "#!/usr/bin/env bash" ] || { echo "Erro: não foi possível baixar o instalador do MultiWA (sem internet, GitHub fora do ar ou download vazio). Nada foi instalado; tente de novo mais tarde." >&2; exit 1; }; MULTIWA_INSTALADOR_EM_PASTA_TEMPORARIA=1 bash "$instalador_baixado" "$@"' instalar-multiwa --sem-atualizacao-automatica
```

Para ligar de novo (ou ligar num Mac que já tem o app), sem atualizar agora, use `--ativar-atualizacao-automatica` no fim:

```bash
bash -c 'pasta_do_download="$(mktemp -d "${TMPDIR:-/tmp}/multiwa-instalador.XXXXXX")" || exit 1; instalador_baixado="$pasta_do_download/instalar.sh"; apagar_o_instalador_baixado() { rm -f "$instalador_baixado"; rmdir "$pasta_do_download"; }; trap apagar_o_instalador_baixado EXIT; trap "exit 129" HUP; trap "exit 130" INT; trap "exit 143" TERM; { curl -fsSL https://github.com/lucas-canavarro/MultiWA-releases/releases/latest/download/instalar.sh -o "$instalador_baixado" || gh release download --repo lucas-canavarro/MultiWA-releases --pattern instalar.sh --output - > "$instalador_baixado"; } && [ "$(head -n 1 "$instalador_baixado")" = "#!/usr/bin/env bash" ] || { echo "Erro: não foi possível baixar o instalador do MultiWA (sem internet, GitHub fora do ar ou download vazio). Nada foi instalado; tente de novo mais tarde." >&2; exit 1; }; MULTIWA_INSTALADOR_EM_PASTA_TEMPORARIA=1 bash "$instalador_baixado" "$@"' instalar-multiwa --ativar-atualizacao-automatica
```

O desinstalador também remove a atualização automática.

Se você voltar de versão com `--voltar-versao-anterior`, a atualização automática não reinstala a versão de que você voltou; ela espera a próxima.

## Voltar à versão anterior

O mesmo comando, com `--voltar-versao-anterior` no fim:

```bash
bash -c 'pasta_do_download="$(mktemp -d "${TMPDIR:-/tmp}/multiwa-instalador.XXXXXX")" || exit 1; instalador_baixado="$pasta_do_download/instalar.sh"; apagar_o_instalador_baixado() { rm -f "$instalador_baixado"; rmdir "$pasta_do_download"; }; trap apagar_o_instalador_baixado EXIT; trap "exit 129" HUP; trap "exit 130" INT; trap "exit 143" TERM; { curl -fsSL https://github.com/lucas-canavarro/MultiWA-releases/releases/latest/download/instalar.sh -o "$instalador_baixado" || gh release download --repo lucas-canavarro/MultiWA-releases --pattern instalar.sh --output - > "$instalador_baixado"; } && [ "$(head -n 1 "$instalador_baixado")" = "#!/usr/bin/env bash" ] || { echo "Erro: não foi possível baixar o instalador do MultiWA (sem internet, GitHub fora do ar ou download vazio). Nada foi instalado; tente de novo mais tarde." >&2; exit 1; }; MULTIWA_INSTALADOR_EM_PASTA_TEMPORARIA=1 bash "$instalador_baixado" "$@"' instalar-multiwa --voltar-versao-anterior
```

## Desinstalar

```bash
curl -fsSL https://github.com/lucas-canavarro/MultiWA-releases/releases/latest/download/desinstalar.sh | bash
```

Contas e sessões ficam guardadas no Mac. Para apagar também os dados, baixe o [desinstalar.sh](https://github.com/lucas-canavarro/MultiWA-releases/releases/latest/download/desinstalar.sh) e rode `bash desinstalar.sh --apagar-dados`.

## Assinatura e aviso do macOS

O app é assinado com uma identidade própria (sempre a mesma, para as permissões do macOS continuarem valendo entre versões), mas não é notarizado pela Apple.

- Pelo comando de uma linha, o app abre direto: o `curl` não marca o download, e o instalador ainda remove a marca de quarentena por garantia.
- Quem baixar o `.zip` pelo navegador (ou receber por AirDrop, Mensagens ou e-mail) vai ver o macOS bloquear a abertura. Nesse caso, abra com clique direito no app > Abrir (e confirme em "Abrir"), ou use o comando de uma linha.

## Mandar para outra pessoa

Mande o `MultiWA-para-instalar.zip` da [última versão](https://github.com/lucas-canavarro/MultiWA-releases/releases/latest). Quem recebe descompacta, abre o Terminal, digita `bash ` (com espaço), arrasta o `instalar.sh` da pasta para a janela e aperta Enter: o instalador usa o `MultiWA.zip` ao lado dele, confere o checksum e a assinatura, tira a marca de quarentena e abre o app, sem precisar de internet.

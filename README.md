# Discord RPC

## Aviso de Suspensão

Esta biblioteca foi preterida em favor do GameSDK do Discord. [Saiba mais aqui](https://discordapp.com/developers/docs/game-sdk/sdk-starter-guide)

---

Esta é uma biblioteca para fazer a interface do seu jogo com um cliente de desktop Discord rodando localmente. É conhecido por funcionar no Windows, macOS e Linux. Você pode usar a lib diretamente se quiser, ou usá-la como um guia para escrever a sua própria se ela não se adequar ao seu jogo como está. PRs/feedback são bem-vindos se você tiver uma melhoria que todos possam querer, ou pode descrever como isso não atende às suas necessidades.

Estão incluídas aqui algumas demonstrações rápidas que implementam o subconjunto mínimo para mostrar o status atual e
tem callbacks para onde um jogo mais completo faria mais coisas (juntando-se, assistindo, etc).

## Documentação
 
A documentação mais atualizada do Rich Presence sempre pode ser encontrada em nosso [site do desenvolvedor](https://discordapp.com/developers/docs/rich-presence/how-to)! Se você estiver interessado em lançar sua própria implementação nativa do Rich Presence por meio de soquetes IPC em vez de usar nosso SDK—hey, você tem tempo livre, certo?—confira a [documentação do "Hard Mode"](https:// github.com/discordapp/discord-rpc/blob/master/documentation/hard-mode.md).

## Uso básico

Zeroith, você deveria estar preparado para construir coisas porque você é um desenvolvedor de jogos, certo?

Primeiro, acesse o [site de desenvolvedores do Discord](https://discordapp.com/developers/applications/me) e crie um aplicativo. Acompanhe o `Client ID` -- você precisará dele aqui para passar para a função init. 
 
### Configuração do Unreal Engine 4 ㅤㅤㅤㅤㅤㅤ

Para usar o plug-in Rich Presense com projetos do Unreal Engine:

1. Faça o download da [versão] mais recente (https://github.com/discordapp/discord-rpc/releases) para cada sistema operacional que você está direcionando e o código-fonte compactado
2. No zip do código-fonte, copie o plugin UE—`examples/unrealstatus/Plugins/discordrpc`—para o diretório de plugins do seu projeto
3. Em `[YOUR_UE_PROJECT]/Plugins/discordrpc/source/ThirdParty/DiscordRpcLibrary/`, crie uma pasta `Include` e copie `discord_rpc.h` e `discord_register.h` para ela do zip
4. Siga as etapas abaixo para cada SO
5. Crie seu projeto UE4
6. Inicie o editor e ative o plug-in Discord.

#### Janelas

- Em `[YOUR_UE_PROJECT]/Plugins/discordrpc/source/ThirdParty/DiscordRpcLibrary/`, crie uma pasta `Win64`
- Copie `lib/discord-rpc.lib` e `bin/discord-rpc.dll` de `[RELEASE_ZIP]/win64-dynamic` para a pasta `Win64`

#### Mac

- Em `[YOUR_UE_PROJECT]/Plugins/discordrpc/source/ThirdParty/DiscordRpcLibrary/`, crie uma pasta `Mac`
- Copie `libdiscord-rpc.dylib` de `[RELEASE_ZIP]/osx-dynamic/lib` para a pasta `Mac`

#### Linux

- Em `[YOUR_UE_PROJECT]/Plugins/discordrpc/source/ThirdParty/DiscordRpcLibrary/`, crie uma pasta `Linux`
- Dentro, crie outra pasta `x86_64-unknown-linux-gnu`
- Copie `libdiscord-rpc.so` de `[RELEASE_ZIP]/linux-dynamic/lib` para `Linux/x86_64-unknown-linux-gnu`

### Configuração da unidade

Se você é um desenvolvedor do Unity que deseja integrar o Rich Presence ao seu jogo, siga este guia simples para começar a ter sucesso:

1. Baixe as DLLs para qualquer plataforma que você precisar de [nossos lançamentos](https://github.com/discordapp/discord-rpc/releases)
2. Em seu projeto Unity, crie uma pasta `Plugins` dentro de sua pasta `Assets` se você ainda não tiver uma
3. Copie o arquivo `DiscordRpc.cs` de [aqui](https://github.com/discordapp/discord-rpc/blob/master/examples/button-clicker/Assets/DiscordRpc.cs) em seu `Assets` pasta. Este é basicamente seu arquivo de cabeçalho para o SDK

Nós temos nossa pasta `Plugins` pronta, então vamos ser específicos da plataforma!

#### Janelas

4. Crie as pastas `x86` e `x86_64` dentro de `Assets/Plugins/`
5. Copie `discord-rpc-win/win64-dynamic/bin/discord-rpc.dll` para `Assets/Plugins/x86_64/`
6. Copie `discord-rpc-win/win32-dynamic/bin/discord-rpc.dll` para `Assets/Plugins/x86/`
7. Clique em ambas as DLLs e certifique-se de que elas estejam direcionando as arquiteturas corretas no painel de propriedades do editor do Unity
8. Pronto!

#### Mac OS

4. Copie `discord-rpc-osx/osx-dynamic/lib/libdiscord-rpc.dylib` para `Assets/Plugins/`
5. Renomeie `libdiscord-rpc.dylib` para `discord-rpc.bundle`
6. Pronto!

#### Linux

4. Copie `discord-rpc-linux/linux-dynamic-lib/libdiscord-rpc.so` para `Assets/Plugins/`
5. Pronto!

Você está pronto para rolar! Para exemplos de código sobre como interagir com o SDK usando o arquivo de cabeçalho `DiscordRpc.cs`, confira [nosso exemplo](https://github.com/discordapp/discord-rpc/blob/master/examples/button-clicker /Assets/DiscordController.cs)

### Do pacote

Faça o download de um pacote de lançamento para sua(s) plataforma(s) -- eles têm subdiretórios com várias opções pré-construídas, selecione o que você precisa adicionar `/include` às suas inclusões de compilação, `/lib` aos seus caminhos de linker e vincule com `discord- rpc`. Para as compilações vinculadas dinamicamente, você precisará enviar o arquivo associado junto com o jogo.

### Do repositório

Primeiro, você vai querer `CMake`. Existem algumas maneiras diferentes de instalá-lo em seu sistema, e você deve consultar o [site deles](https://cmake.org/install/). Muitos gerenciadores de pacotes também fornecem maneiras de instalar o CMake.

Para ter certeza de que está instalado corretamente, digite `cmake --version` no seu tipo de terminal/cmd. Se você receber uma resposta com um número de versão, pronto!

Existe um arquivo [CMake](https://cmake.org/download/) que deve ser capaz de gerar a lib para você; Às vezes eu uso assim:

```sh
    cd <caminho para discord-rpc>
    compilação mkdir
    compilação de cd
    cmake .. -DCMAKE_INSTALL_PREFIX=<caminho para instalar discord-rpc para>
    cmake --build . --config Release --target install
```

Existe um script de construção wrapper `build.py` que executa `cmake` com algumas opções diferentes.

Normalmente, eu corro `build.py` para começar as coisas, então uso os arquivos de projeto gerados enquanto trabalho nas coisas. Ele depende da biblioteca `click`, então faça um rápido `pip install click` para ter certeza de que você o possui se quiser executar o `build.py`.

Existem algumas opções do CMake com as quais você pode se interessar:

| flag                                                                                     | default | does                                                                                                                                                  |
| ---------------------------------------------------------------------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ENABLE_IO_THREAD`                                                                       | `ON`    | When enabled, we start up a thread to do io processing, if disabled you should call `Discord_UpdateConnection` yourself.                              |
| `USE_STATIC_CRT`                                                                         | `OFF`   | (Windows) Enable to statically link the CRT, avoiding requiring users install the redistributable package. (The prebuilt binaries enable this option) |
| [`BUILD_SHARED_LIBS`](https://cmake.org/cmake/help/v3.7/variable/BUILD_SHARED_LIBS.html) | `OFF`   | Build library as a DLL                                                                                                                                |
| `WARNINGS_AS_ERRORS`                                                                     | `OFF`   | When enabled, compiles with `-Werror` (on \*nix platforms).                                                                                           |

## Construções contínuas

Por que temos três desses? Três vezes mais divertido!

| CI | emblema |
| -------------------- | -------------------------------------------------- -------------------------------------------------- -------------------------------------------- |
| TravisCI | [![Status da compilação](https://travis-ci.org/discordapp/discord-rpc.svg?branch=master)](https://travis-ci.org/discordapp/discord-rpc) |
| AppVeyor | [![Status da compilação](https://ci.appveyor.com/api/projects/status/qvkoc0w1c4f4b8tj?svg=true)](https://ci.appveyor.com/project/crmarsh/discord-rpc) |
| Buildkite (interno) | [![Status da compilação](https://badge.buildkite.com/e103d79d247f6776605a15246352a04b8fd83d69211b836111.svg)](https://buildkite.com/discord/discord-rpc) |

## Exemplo: presença de envio

Este é um "jogo" de aventura de texto que inicia/desinicia a conexão com o Discord e envia uma atualização de presença em cada comando.

## Exemplo: botão de clique

Este é um projeto de amostra [Unity](https://unity3d.com/) que envolve uma versão DLL da biblioteca e envia atualizações de presença quando você clica em um botão. Execute `python build.py unity` no diretório raiz para construir os arquivos de biblioteca corretos e coloque-os em suas respectivas pastas.

## Exemplo: unrealstatus

Este é um projeto de amostra [Unreal](https://www.unrealengine.com) que envolve a versão DLL da biblioteca com um plug-in Unreal, expõe uma classe blueprint para interagir com ela e usa isso para criar uma interface do usuário muito simples . Execute `python build.py unreal` no diretório raiz para construir os arquivos de biblioteca corretos e coloque-os em suas respectivas pastas.

## Wrappers e implementações

Abaixo está uma tabela de wrappers não oficiais desenvolvidos pela comunidade e implementações de Rich Presence em vários idiomas. Se você gostaria de ter o seu adicionado, por favor, faça um pull request adicionando seu repositório à tabela. O repositório deve incluir:

- O código
- Um breve ReadMe de como usá-lo
- Um exemplo de trabalho

###### Rich Presence Wrappers and Implementations

| Name                                                                      | Language                          |
| ------------------------------------------------------------------------- | --------------------------------- |
| [Discord RPC C++](https://https://github.com/MallowDiscord/discord-rpc/)  | C++                               |
| [Discord RPC D](https://github.com/voidblaster/discord-rpc-d)             | [D](https://dlang.org/)           |
| [discord-rpc.jar](https://github.com/Vatuu/discord-rpc 'Discord-RPC.jar') | Java                              |
| [java-discord-rpc](https://github.com/MinnDevelopment/java-discord-rpc)   | Java                              |
| [Discord-IPC](https://github.com/jagrosh/DiscordIPC)                      | Java                              |
| [Discord Rich Presence](https://npmjs.org/discord-rich-presence)          | JavaScript                        |
| [drpc4k](https://github.com/Bluexin/drpc4k)                               | [Kotlin](https://kotlinlang.org/) |
| [lua-discordRPC](https://github.com/pfirsich/lua-discordRPC)              | LuaJIT (FFI)                      |
| [pypresence](https://github.com/qwertyquerty/pypresence)                  | [Python](https://python.org/)     |
| [SwordRPC](https://github.com/Azoy/SwordRPC)                              | [Swift](https://swift.org)        |

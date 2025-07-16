# PhantomBox


-> Rodar software/games em um ambiente isolado.
-> Não depende de VMs, deixando leve e pratico.
-> Isolamento, controle granular, execução sem root.
-> User escolhe p nível de isolamento.
-> Multiplataforma com java e open-source.

[1] Interface do Usuário (JavaFX)
[2] Camada de Aplicação (Controladores)
[3] Camada de Domínio (Modelos, Configs, Perfil, Runner)
[4] Camada de Plataforma (Isoladores, sistema de arquivos, execução)


Config Json objetos:
~/.safeplay/games/minecraft/profile.json
(

{
  "name": "Wolfenstein",
  "exec": "./WolfensteinLauncher.sh",
  "platform": "linux",
  "wine": false,
  "network": false,
  "allowHome": false,
  "ramLimitMB": 4096,
  "cpuLimitPercent": 50,
  "gpuAccess": true,
  "network" {
    "networkAccess": true,
    "...": ...
  }
  "customMount": "/games/safe/Wolfenstein",
  "createdAt": "2025-07-15T13:47:00",
  "tags": ["crack", "Wolfenstein", "offline"],
  "notes": "Usar versão pirata apenas offline",
  "logs": []
}


).


Componentes:

Game -> Representa um jogo instalado (nome, ID, caminho, etc.).
GameProfile	-> Contém permissões e configurações daquele jogo.
SandboxRunner -> Roda o jogo com base no GameProfile usando o Isolator adequado.
Isolator (interface) -> Define como aplicar isolamento.
LinuxIsolator, WindowsIsolator -> Implementam os métodos de isolamento específicos do sistema.
JsonStorage -> Lê e escreve perfis no disco
LauncherController -> Coordena o lançamento, carrega configs, logs.
Logger -> Gera logs de execução do jogo.
MainUI -> Interface principal com botões e configurações.


Funcionalidades por módulos:

Game Manager(Adicionar jogo manualmente ou por instalador, Armazenar arquivos em local isolado (~/.safeplay/games/ ou C:\SafePlay\), Listar jogos instalados, Excluir ou reinstalar jogo).

Config Profile (Permitir exportar como JSON, Permitir importar um perfil salvo do objeto, Salvar alterações automaticamente ou sob comando).

Sandbox Executor (Rodar com firejail, bwrap, sandboxctl, ou ACLs (Windows), Capturar stdout/stderr, Monitorar tentativas de acesso a rede ou arquivos fora da sandbox, Logs armazenados no perfil do jogo).

Gerenciamento de sistema (Detectar sistema operacional, instala java, Ajustar comandos e permissões por SO, Rodar tudo como usuário comum, Detectar se firejail, wine, etc. estão instalados).

Isolamento (Instalação e execução ocorrem em um espaço separado).
Sem VM (Não usa máquina virtual; leve, direto e eficiente).
!! Expansão automática de espaço (O local de instalação se ajusta conforme a necessidade).
Bloqueio de internet (Permite bloquear ou liberar acesso à rede).
Controle de uso de recursos (Permitir limitação de RAM, CPU, GPU se necessário).
Permissões personalizáveis (Usuário escolhe permissões por objeto).
OS+Compatibilidade com Wine (Suporte opcional para rodar jogos Windows com Wine nos ambientes linux!!).
Configurações por jogo (Cada jogo pode ter suas próprias configurações).
!! (Nunca executa como administrador) !!
!! (Java com JavaFX e para ser básico e leve) !!

Criação e gestão de ambientes isolados (Cada jogo é instalado em uma pasta/volume dedicado, por exemplo:
~/.safeplay/games/<id_do_jogo>/, overlayfs e namespaces com firejail e bwrap na questão de isolar do sistema(acesso)).

Log e auditoria (Caminhos acessados, conexões tentadas via iptables ou bpftrace, arquivos modificados, etc...).

Atualizações e limpeza (Opção de reinstalar o ambiente (reseta a instalação do jogo) e sobrepõe o que foi utilizado, para não osbrar resquissos em disco. Botão para limpar cache, logs, arquivos temporários, ermitir backup e restauração de ambientes).

!! Depois -> Suporte a discos virtuais (Usar arquivos .img montáveis ou FUSE para diretórios virtuais)

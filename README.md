# CC-TP2 - Network Management System (NMS)

Sistema de monitorização de redes e gestão de agentes desenvolvido em Go no âmbito da unidade curricular de Comunicações por Computador, no Mestrado em Engenharia Informática da Universidade do Minho.

## Sobre o Projeto

Este projeto consiste numa infraestrutura de monitorização de rede (Network Management System) composta por um servidor centralizado e múltiplos nós agentes. O sistema permite a configuração de tarefas de monitorização via ficheiros JSON e a recolha contínua de métricas de rede e hardware, apresentando os dados através de uma Interface Gráfica de Terminal (TUI).

## Stack Tecnológica

* **Linguagem:** Go (Golang 1.23.2)
* **Interface Gráfica (TUI):** tview, tcell
* **Comunicação:** Sockets UDP e TCP (net)
* **Formatos de Dados:** JSON (Configurações), GOB (Serialização TCP)

## Arquitetura e Funcionalidades

* **Comunicação Híbrida (UDP/TCP):**
  * **UDP:** Utilizado para o registo de agentes (`serverNetTask`), atribuição de tarefas de monitorização e reporte contínuo de métricas para minimizar o overhead na rede.
  * **TCP:** Utilizado pelo módulo `serverAlertFlow` para a transmissão fiável de alertas críticos, garantindo que a informação de ultrapassagem de limiares (thresholds) não é perdida.
* **Monitorização Multimétrica:** Recolha configurável de métricas de Hardware (uso de CPU e RAM) e métricas de Ligação (Largura de banda, Jitter, Perda de Pacotes, Latência e Estatísticas de Interface).
* **Dashboard TUI:** Interface interativa baseada em texto que permite visualizar em tempo real os clientes conectados, métricas detalhadas e os logs gerais do servidor.
* **Persistência de Logs:** Módulo de base de dados (`server/db`) focado no uso de buffers em memória com rotinas ativas de persistência periódica para ficheiros físicos dentro da pasta `client_metrics`.

## Como Executar Localmente

### 1. Preparação do Ambiente
Configura as variáveis de ambiente necessárias (GOPATH, GOCACHE) executando o script de setup na raiz do projeto:

    source setup_env.sh

Caso ainda não tenhas as dependências instaladas, transfere-as através do módulo Go:

    go mod tidy

### 2. Iniciar o Servidor NMS
O servidor arranca consumindo um ficheiro JSON com a definição das tarefas de monitorização. Navega para a pasta do servidor e executa-o passando um ficheiro de input como argumento:

    cd server
    go run . inputs/test2.json

### 3. Simulação e Testes de Agentes
Podes simular o envio de pacotes de registo e reporte a partir da raiz do repositório para validar a comunicação UDP com o servidor:

    go run udp_packet_test2.go

## Estrutura de Diretórios

* `server/`: Código principal do Servidor NMS, contendo a lógica de UI (`view`), Base de Dados (`db`), parsing de tarefas (`config`) e processamento de rede.
* `server/inputs/`: Ficheiros JSON com as parametrizações de monitorização e thresholds.
* `tcp_handler/`: Lógica de rede e serialização (GOB) para a troca fiável de alertas TCP.
* `client_metrics/`: Diretório gerado automaticamente durante a execução, contendo os ficheiros de texto isolados por agente com o histórico de logs e métricas.

## Licença

Projeto desenvolvido para fins académicos.

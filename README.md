🔐 Exploração de Vulnerabilidades em Redes Wireless com Rogue AP e Captura de WPA/WPA2 via Wifiphisher
<div align="center">

⚠️ AVISO LEGAL (DISCLAIMER)
Este repositório foi produzido exclusivamente para fins acadêmicos na disciplina de Redes de Computadores II.
Todos os testes foram realizados em ambiente isolado, com dados fictícios e com autorização, seguindo as orientações de segurança do curso.

</div>
📑 1. Sumário Executivo

Este projeto apresenta uma Prova de Conceito (PoC) sobre a exploração de vulnerabilidades em redes Wi-Fi, demonstrando como um Rogue Access Point pode ser utilizado para capturar credenciais WPA/WPA2 através de phishing de atualização de firmware, utilizando a ferramenta Wifiphisher.

O cenário demonstra como usuários podem ser induzidos a informar a senha do Wi-Fi acreditando estar realizando uma “atualização necessária” no roteador, quando na verdade estão conectados a um ponto de acesso falso controlado pelo atacante.

🎯 Objetivos

Criar e configurar um ambiente controlado para exploração de redes wireless.

Utilizar o Wifiphisher para criar um Rogue AP com cenário de phishing.

Capturar requisições HTTP geradas pela vítima.

Registrar credenciais fornecidas voluntariamente pelo alvo.

Gerar um PCAP e documentação completa do experimento.

Apresentar contramedidas eficazes contra ataques similares.

🏗️ 2. Arquitetura e Topologia

O ambiente foi construído utilizando hardware real, com uma placa de rede externa compatível com modo monitor, para permitir a criação do Rogue AP.

Componentes Utilizados
Componente	Especificação	Função
Notebook com Kali Linux	Kali + Placa USB Wi-Fi (modo monitor)	Atacante — cria AP falso e coleta tráfego
Wifiphisher	Versão 1.4 GIT	Ferramenta para criar Rogue AP + phishing de WPA
Dispositivo Vítima	Smartphone Android	Conecta ao AP falso e envia credenciais

⚙️ 3. Metodologia
3.1 Preparação da Interface (Modo Monitor)

Para permitir a criação do Rogue AP, a interface wireless foi configurada em modo monitor:

sudo ifconfig wlan0 down
sudo iwconfig wlan0 mode monitor
sudo ifconfig wlan0 up
3.2 Execução do Wifiphisher

O ataque foi iniciado com:

sudo wifiphisher --force-hostapd


O Wifiphisher escaneou todas as redes próximas.

O SSID selecionado foi Sineyda (ambiente de laboratório).

Cenário escolhido: Firmware Upgrade Page.

Página falsa simulando uma “atualização obrigatória do roteador”.

Solicita ao usuário a senha WPA/WPA2.

🔄 4. Ciclo de Vida do Ataque
sequenceDiagram
    participant V as Vítima
    participant AP as Rogue AP (Wifiphisher)
    participant A as Atacante (Kali)

    Note over V, AP: Estágio 1 - Desautenticação
    A->>V: Pacotes DEAUTH
    V->>AP: Reconexão automática

    Note over V, AP: Estágio 2 - Phishing
    V->>AP: Solicitação HTTP
    AP->>V: Exibe "Atualização de Firmware"

    Note over V, A: Estágio 3 - Captura
    V->>A: Envio da senha WPA/WPA2 via POST
    A->>A: Registro das credenciais

    5. Análise Técnica
📡 Estágio 1: DEAUTH Attack

O Wifiphisher envia pacotes de desautenticação (DEAUTH) para a vítima, forçando-a a se desconectar da rede legítima.

Com isso, o dispositivo busca automaticamente uma rede com o mesmo nome (ESSID).

🎭 Estágio 2: Rogue AP + Phishing

O Wifiphisher cria um AP com o mesmo nome (Sineyda) e hospeda uma página falsa indicando:

“⚠️ Seu roteador precisa de uma atualização de firmware.
Insira sua chave WPA para continuar.”

🔓 Estágio 3: Captura da Senha

A credencial enviada aparece no terminal como:

POST request from 10.0.0.32 with wfphshr-wpa-password=teste
O valor “teste” foi definido durante o experimento para validação.
📸 6. Evidências
6.1 Seleção do SSID alvo
<div align="center"> <img src="images/scan.png" width="650"> </div>
6.2 Escolha do cenário de phishing
<div align="center"> <img src="images/phishing.png" width="650"> </div>
6.3 Captura da senha enviada pela vítima
<div align="center"> <img src="images/victim.png" width="650"> </div>
📊 7. Classificação dos Dados Comprometidos
Dado Capturado	Tipo	Risco	Impacto Técnico
Senha WPA/WPA2	Credencial	#Crítico#	Permite acesso total ao roteador e à rede da vítima
Modelo/OS do Device	Metadados	Médio	Fingerprinting do usuário
Tráfego HTTP	Metadados	Baixo	Uso para rastrear comportamento

Diagnóstico:
A captura da chave Wi-Fi compromete totalmente a privacidade e segurança da rede local.

🛡️ 8. Contramedidas e Recomendações
🔐 1. Camada Wireless

Migrar para WPA3-Personal

Desativar WPS, altamente vulnerável

Atualizar firmware do roteador pela interface oficial somente

🎛️ 2. Detecção de Rogue APs

Usar ferramentas como:

Kismet

Airgeddon

Sistema WIDS/WIPS corporativo

🌐 3. Boas Práticas do Usuário

Nunca inserir senha do Wi-Fi em páginas que não fazem parte do roteador

Verificar endereço IP do gateway antes de confiar na página

Evitar redes públicas ou desconhecidas

🛠️ 9. Guia Completo de Reprodução
📌 1. Requisitos

Kali Linux

Placa Wi-Fi compatível com monitor mode

Wifiphisher instalado

Wireshark (opcional)

📌 2. Inicialização do Ataque

Ativar modo monitor

Rodar Wifiphisher

Selecionar SSID

Escolher o cenário Firmware Upgrade

Aguardar vítima conectar e enviar a chave

📌 3. Arquivo PCAP

Um PCAP sintético e anonimizado foi incluído neste repositório:

captura.pcapng

🔚 10. Finalização do Experimento

O experimento demonstrou, com clareza, a eficácia do ataque Rogue AP combinado com engenharia social. A vítima acreditou estar realizando uma atualização legítima, mas na verdade forneceu sua chave WPA diretamente ao atacante.

O cenário reforça a necessidade de:

Padrões modernos (WPA3)

Conscientização de usuários

Monitoramento de redes contra APs maliciosos

<div align="center">

👨‍💻 Desenvolvido por:
Guilherme Ferreira
Trabalho apresentado ao curso de Sistemas de Informação — Novembro/2025

</div>
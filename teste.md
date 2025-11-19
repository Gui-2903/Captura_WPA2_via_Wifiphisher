🔐 Exploração de Vulnerabilidades em Redes Wireless com Rogue AP e Captura de WPA/WPA2 via Wifiphisher

<div align="center">

⚠️ **AVISO LEGAL (DISCLAIMER)**

Este repositório foi produzido exclusivamente para fins **acadêmicos** na disciplina de **Redes de Computadores II**.

Todos os testes foram realizados em **ambiente isolado**, com dados fictícios e com autorização, seguindo as orientações de segurança do curso.

</div>

---

## 📑 1. Sumário Executivo

Este projeto apresenta uma **Prova de Conceito (PoC)** sobre a exploração de vulnerabilidades em redes Wi-Fi, demonstrando como um **Rogue Access Point** pode ser utilizado para capturar credenciais **WPA/WPA2** através de **phishing de atualização de firmware**, utilizando a ferramenta **Wifiphisher**.

O cenário demonstra como usuários podem ser induzidos a informar a senha do Wi-Fi acreditando estar realizando uma “atualização necessária” no roteador, quando na verdade estão conectados a um ponto de acesso falso controlado pelo atacante.

### 🎯 Objetivos

* Criar e configurar um ambiente controlado para exploração de redes wireless.
* Utilizar o **Wifiphisher** para criar um **Rogue AP** com cenário de phishing.
* Capturar requisições HTTP geradas pela vítima.
* Registrar credenciais fornecidas voluntariamente pelo alvo.
* Gerar um PCAP e documentação completa do experimento.
* Apresentar contramedidas eficazes contra ataques similares.

---

## 🏗️ 2. Arquitetura e Topologia

O ambiente foi construído utilizando **hardware real**, com uma placa de rede externa compatível com **modo monitor**, para permitir a criação do Rogue AP.

### Componentes Utilizados

| Componente | Especificação | Função |
| :--- | :--- | :--- |
| **Notebook com Kali Linux** | Kali + Placa USB Wi-Fi (modo monitor) | Atacante — cria AP falso e coleta tráfego |
| **Wifiphisher** | Versão 1.4 GIT | Ferramenta para criar Rogue AP + phishing de WPA |
| **Dispositivo Vítima** | Smartphone Android | Conecta ao AP falso e envia credenciais |

---

## ⚙️ 3. Metodologia

### 3.1 Preparação da Interface (Modo Monitor)

Para permitir a criação do Rogue AP, a interface wireless foi configurada em modo monitor com os seguintes comandos:

```bash
sudo ifconfig wlan0 down
sudo iwconfig wlan0 mode monitor
sudo ifconfig wlan0 up
3.2 Execução do WifiphisherO ataque foi iniciado com o seguinte comando:Bashsudo wifiphisher --force-hostapd
O Wifiphisher escaneou todas as redes próximas.O SSID selecionado foi Sineyda (ambiente de laboratório).Cenário escolhido: Firmware Upgrade Page.Página falsa simulando uma “atualização obrigatória do roteador”.Solicita ao usuário a senha WPA/WPA2.🔄 4. Ciclo de Vida do AtaqueO ataque segue três estágios principais, conforme o diagrama de sequência:Snippet de códigosequenceDiagram
    participant V as Vítima
    participant AP as Rogue AP (Wifiphisher)
    participant A as Atacante (Kali)

    Note over V, AP: Estágio 1 - Desautenticação
    A->>V: Pacotes DEAUTH (Força desconexão da rede legítima)
    V->>AP: Reconexão automática ao AP falso (mesmo SSID)

    Note over V, AP: Estágio 2 - Phishing
    V->>AP: Solicitação HTTP
    AP->>V: Exibe "Atualização de Firmware" (Página falsa)

    Note over V, A: Estágio 3 - Captura
    V->>A: Envio da senha WPA/WPA2 via POST (Vítima insere a senha)
    A->>A: Registro das credenciais no terminal
📝 5. Análise Técnica📡 Estágio 1: DEAUTH AttackO Wifiphisher envia pacotes de desautenticação (DEAUTH) para a vítima, forçando-a a se desconectar da rede legítima. Com isso, o dispositivo busca automaticamente uma rede com o mesmo nome (ESSID), conectando-se ao Rogue AP.🎭 Estágio 2: Rogue AP + PhishingO Wifiphisher cria um AP com o mesmo nome (Sineyda) e hospeda uma página falsa indicando:“⚠️ Seu roteador precisa de uma atualização de firmware. Insira sua chave WPA para continuar.”🔓 Estágio 3: Captura da SenhaA credencial enviada pela vítima é capturada e aparece no terminal do atacante através de uma requisição POST:POST request from 10.0.0.32 with wfphshr-wpa-password=teste
O valor teste foi definido durante o experimento para validação.📸 6. EvidênciasAs imagens a seguir registram as etapas críticas do ataque:6.1 Seleção do SSID alvo6.2 Escolha do cenário de phishing6.3 Captura da senha enviada pela vítima📊 7. Classificação dos Dados ComprometidosDado CapturadoTipoRiscoImpacto TécnicoSenha WPA/WPA2CredencialCríticoPermite acesso total ao roteador e à rede da vítima.Modelo/OS do DeviceMetadadosMédioFingerprinting do usuário para futuros ataques.Tráfego HTTPMetadadosBaixoUso para rastrear comportamento da vítima.Diagnóstico:A captura da chave Wi-Fi compromete totalmente a privacidade e segurança da rede local.🛡️ 8. Contramedidas e Recomendações🔐 1. Camada WirelessMigrar para o padrão WPA3-Personal para proteção avançada.Desativar WPS, que é uma funcionalidade altamente vulnerável.Atualizar o firmware do roteador apenas pela interface oficial ou website do fabricante.🎛️ 2. Detecção de Rogue APsUtilizar ferramentas ou sistemas de monitoramento:KismetAirgeddonSistema WIDS/WIPS (Wireless Intrusion Detection/Prevention System) em ambientes corporativos.🌐 3. Boas Práticas do UsuárioNunca inserir a senha do Wi-Fi em páginas que não fazem parte da interface de administração do roteador.Verificar o endereço IP do gateway antes de confiar em páginas de login/atualização.Evitar redes públicas ou desconhecidas.🛠️ 9. Guia Completo de Reprodução📌 1. RequisitosKali LinuxPlaca Wi-Fi compatível com monitor modeWifiphisher instaladoWireshark (opcional para análise de tráfego)📌 2. Inicialização do Ataque (Passos)Ativar modo monitor na interface wireless (ver seção 3.1).Rodar o comando sudo wifiphisher --force-hostapd.Selecionar o SSID alvo.Escolher o cenário Firmware Upgrade Page.Aguardar vítima conectar e enviar a chave.📌 3. Arquivo PCAPUm PCAP sintético e anonimizado foi incluído neste repositório:captura.pcapng🔚 10. Finalização do ExperimentoO experimento demonstrou, com clareza, a eficácia do ataque Rogue AP combinado com engenharia social. A vítima acreditou estar realizando uma atualização legítima, mas na verdade forneceu sua chave WPA diretamente ao atacante.O cenário reforça a necessidade de:Adoção de padrões modernos (WPA3).Conscientização de usuários sobre táticas de phishing.Monitoramento de redes contra APs maliciosos.<div align="center">👨‍💻 Desenvolvido por:Guilherme Ferreira, joao pedro, danyel, gabrielTrabalho apresentado ao curso de Sistemas de Informação — Novembro/2025</div>
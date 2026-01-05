# Desenvolvimento de Firmware para Estação de Baixo Custo de Qualidade do Ar

Este repositório contém o firmware desenvolvido para uma estação de monitoramento em tempo real da qualidade do ar, focada no contexto de Cidades Inteligentes e Internet das Coisas (IoT). O projeto utiliza tecnologias acessíveis para fornecer dados cruciais para a gestão urbana.

## 📌 Visão Geral

A estação monitora gases como monóxido de carbono (CO), oximetria (OX) e dióxido de nitrogênio (NO2), além de temperatura e umidade. O objetivo é oferecer uma solução de baixo custo para complementar as estações oficiais, que possuem alto valor de implementação.

## 🏗️ Arquitetura do Sistema

### Hardware
O sistema é baseado no microcontrolador **Espressif ESP32-S3** e utiliza os seguintes componentes:
* **Sensores de Gás:** Alphasense (CO-B4, NO2-B43F, OX-B431) com interface analógica.
* **Sensor Ambiental:** SHT45 para temperatura e umidade (via I2C).
* **Conversor ADC:** Texas ADS1115 (16 bits) para garantir precisão nas leituras analógicas.
* **Multiplexador:** Texas HC4067 para gerenciar os múltiplos eletrodos dos sensores.
* **Armazenamento:** Módulo de cartão micro SD para registro local (log).
* **Relógio:** RTC DS1307 para carimbo de tempo (timestamp) das leituras.

### Conectividade
A transmissão sem fio é realizada através do protocolo **LoRaWAN** com um transceptor **Hoperf RFM95W**, permitindo comunicação de longa distância e baixa potência. O método de ativação utilizado é o **OTAA** (Over The Air Activation).

## 🚀 Funcionamento do Firmware

Desenvolvido em **C/C++** com o framework **Arduino** no ambiente **PlatformIO**, o firmware segue este fluxo:
1. **Configuração inicial:** Inicializa todos os sensores e estabelece a conexão LoRaWAN.
2. **Ciclo de Aquisição:** A cada minuto, o sistema percorre os pinos do multiplexador, realiza a conversão analógico-digital e armazena os dados em um buffer.
3. **Formatação do Frame:** Monta um pacote de dados contendo as leituras de gases, temperatura, umidade e data/hora.
4. **Armazenamento e Envio:** O frame é gravado no cartão SD (via comunicação **VSPI** para evitar conflitos de barramento) e enviado em formato hexadecimal para a rede.

## 📊 Estrutura de Rede

1. **Estação → Gateway:** Envio dos dados via LoRa RF.
2. **Gateway → The Things Stack:** Entrega dos dados ao servidor de rede (TTN).
3. **Servidor de Rede → Aplicação:** Decodificação do frame e visualização gráfica em painéis como **TagoIO** ou **Sentilo**.

## 📈 Resultados

* **Estabilidade:** Operação comprovada em testes laboratoriais estáticos por períodos de uma semana.
* **Confiabilidade:** Sucesso no registro local e na transmissão para a nuvem sem falhas de envio detectadas.
* **Resolução Técnica:** Superação de conflitos no barramento SPI compartilhado através da implementação de uma interface VSPI exclusiva para o cartão SD.

---
**Autores:** Jamerson Alves Muniz, Edson Tavares de Camargo, Márcio Seiji Oyamada e Leila Droprinchinski Martins.
**Evento:** SICITE 2024 - UTFPR.

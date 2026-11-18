# Sistema Preditivo de Irrigação com IoT

Projeto acadêmico da **Global Solution 2026.2**, desenvolvido para a disciplina de **Arquitetura e Organização de Computadores**, com o tema **Citizen Intelligence & Predictive Analytics**.

A proposta é acompanhar a umidade do solo de um jardim comunitário, identificar tendências de secagem e indicar quando uma nova irrigação poderá ser necessária. O cuidador consulta a sinalização no canteiro e o histórico remoto para decidir quando irrigar.

## Objetivo

Apresentar uma arquitetura que relacione sensores, conversão analógico-digital, processamento, memória, saídas digitais e comunicação IoT em uma solução para o cuidado de jardins comunitários.

## Escopo

O trabalho é **conceitual**, apresentado em relatório. Os componentes e o funcionamento descritos representam a arquitetura proposta, sem montagem física, simulação ou firmware executável nesta entrega. A irrigação permanece sob responsabilidade do cuidador.

## Componentes propostos

| Componente | Função |
| --- | --- |
| Raspberry Pi Pico W, com RP2040 | Processar as leituras, estimar a tendência e controlar a sinalização. |
| Sensor capacitivo DFRobot SEN0308 | Fornecer a leitura analógica de umidade do solo. |
| Sensor Adafruit SHT31-D | Medir temperatura e umidade relativa do ar pela interface I²C. |
| LEDs Kingbright WP7113 verde, amarelo e vermelho | Indicar o estado do solo junto ao canteiro. |
| Fonte micro-USB Raspberry Pi de 12,5 W | Alimentar a placa e os componentes conforme a distribuição descrita no relatório. |
| ThingSpeak | Registrar as leituras e os estados em um canal privado. |

## Funcionamento previsto

1. A cada **15 minutos**, o sistema realiza uma nova leitura dos sensores.
2. O ADC de **12 bits** converte o sinal analógico do sensor de solo. O SHT31-D fornece os dados do ar por I²C.
3. O RP2040 processa os dados e mantém até **oito amostras recentes na SRAM**. O firmware permanece na memória Flash da placa.
4. O histórico permite identificar alta, estabilidade ou queda da umidade e estimar se ela poderá atingir o limite de **30% nos próximos 60 minutos**.
5. Os LEDs indicam o estado do solo, e os dados são enviados por **Wi-Fi e HTTPS** ao ThingSpeak.

Com menos de três amostras, a tendência fica indefinida e a sinalização considera a leitura atual. Se a conexão cair, a análise local e os LEDs continuam funcionando na arquitetura proposta.

## Estados de sinalização

| LED | Estado | Condição |
| --- | --- | --- |
| Verde | Adequado | Umidade acima de 30%, sem previsão de atingir o limite nos próximos 60 minutos. Durante a coleta inicial, considera a leitura atual. |
| Amarelo | Atenção | Umidade acima de 30%, com previsão de atingir 30% ou menos em até 60 minutos. |
| Vermelho | Seco | Umidade atual de 30% ou menos. |

O índice de umidade depende de calibração para o substrato. O limite de 30% e o horizonte de 60 minutos são **referências didáticas ajustáveis ao canteiro**.

## Documentação

O [relatório do projeto](Relatorio_Sistema_Preditivo_de_Irrigacao_com_IoT.pdf) apresenta a escolha dos componentes, as alternativas comparadas, o diagrama de blocos, o fluxo de dados, o uso das memórias e a comunicação com o ThingSpeak. As referências técnicas estão ao final do documento.

Para consultar a entrega, abra o PDF em um leitor de documentos ou na visualização do GitHub.

## Integrantes

| Nome | RM |
| --- | --- |
| Guilherme Dias Nascimento | 576403 |
| Matheus Nunes | 574511 |

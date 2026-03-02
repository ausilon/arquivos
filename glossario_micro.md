# Glossário Mestre: Design de Hardware e VLSI (soc_top)

Este guia prático define os 50 termos fundamentais enfrentados durante o desenvolvimento físico do chip `soc_top`, desde a lógica RTL até o GDS final.

---

## 1. Arquitetura e Lógica (HDL)
* **HDL (Hardware Description Language):** Linguagem (Verilog/VHDL) que descreve o comportamento ou a estrutura do hardware.
* **RTL (Register Transfer Level):** Nível de abstração onde o código descreve como os dados fluem entre registradores.
* **Synthesis:** Processo de transformar código RTL em uma rede de portas lógicas (Netlist).
* **Netlist:** O "mapa" de conexões que lista todas as instâncias e como seus pinos estão interligados.
* **Testbench:** Código Verilog não sintetizável usado apenas para simular e testar o design.
* **Instanciação:** O ato de colocar um módulo dentro de outro (ex: `cpu_inst` dentro do `soc_top`).
* **Verilog Blackbox:** Um módulo cuja lógica interna é desconhecida pela síntese (apenas pinos e área são considerados).

## 2. Camada Física e Floorplan
* **Core Area:** A área interna do chip onde as células padrão e macros são efetivamente posicionadas.
* **Core Utilization:** Porcentagem da área total ocupada por lógica ativa (no `soc_top` usamos ~45%).
* **Aspect Ratio:** Relação entre a largura e a altura do chip (ex: 1.0 para um chip quadrado).
* **Site:** A menor unidade de grade no PDK que define onde uma célula pode ser encaixada.
* **Row:** Linhas horizontais onde as Standard Cells são alinhadas para compartilhar trilhas de alimentação.
* **Halo (ou Keepout):** Zona de exclusão ao redor de uma macro para evitar ruído e facilitar o roteamento.
* **Channel:** Espaço estratégico entre macros reservado para a passagem de grandes barramentos de fios.
* **Congestion:** Região onde há mais fios do que trilhas metálicas disponíveis para roteamento.
* **Die Area:** A área total de silício (incluindo as bordas/IOs) definida no arquivo de configuração.



## 3. Células e Componentes de Silício
* **Standard Cell:** Bloco lógico básico (AND, OR, Flip-flop) com altura padronizada para as Rows.
* **Drive Strength:** Capacidade de uma célula de carregar eletricamente o próximo estágio (ex: `inv_2` vs `inv_16`).
* **Filler Cells:** Células vazias que preenchem buracos nas Rows para manter a continuidade do silício.
* **Decap Cells (Decoupling Capacitors):** Células que estabilizam a voltagem local, funcionando como mini-baterias.
* **Tap Cells:** Conectam o substrato ao VDD/GND para evitar o efeito destrutivo de *latch-up*.
* **Tie Cells:** Conectam entradas lógicas ao '1' ou '0' de forma segura, sem ligar direto no barramento de força.
* **Antenna Diode:** Diodo que descarrega eletricidade estática acumulada em fios longos durante a fabricação.

## 4. Distribuição de Energia (PDN)
* **PDN (Power Distribution Network):** A malha de metais grossos que alimenta todo o chip.
* **Power Ring:** Anel metálico ao redor do core para distribuição primária de VDD e GND.
* **Power Straps:** Trilhas horizontais e verticais que cruzam o chip criando uma "grelha" de força.
* **IR Drop:** Queda de tensão causada pela resistência do metal (pode fazer a CPU "resetar" sozinha).
* **Electromigration:** Desgaste físico do metal causado por corrente excessiva ao longo de anos de uso.

## 5. Clock e Temporização (Timing)
* **Clock Skew:** A diferença no tempo de chegada do sinal de clock entre dois pontos diferentes do chip.
* **Clock Jitter:** Variação incerta no período do clock devido a ruídos elétricos.
* **CTS (Clock Tree Synthesis):** Processo de criar a árvore de buffers para balancear o sinal de clock.
* **Clock Latency:** O tempo total que o sinal leva desde a entrada (pino) até o último registrador.
* **Setup Time:** Tempo mínimo que um dado deve estar pronto antes da borda do clock chegar.
* **Hold Time:** Tempo mínimo que um dado deve permanecer estável após a borda do clock ter passado.



## 6. Roteamento e Interconexões
* **Via:** Pequena conexão vertical entre duas camadas de metal diferentes (ex: Metal 1 para Metal 2).
* **Metal Layers:** As diversas "camadas" de fiação (no Sky130, temos de Metal 1 a Metal 5).
* **Track:** O trilho pré-definido por onde o roteador automático desenha os fios.
* **Pitch:** Distância mínima entre o centro de dois fios paralelos.
* **Parasitics (RC):** Resistência e Capacitância indesejadas que surgem naturalmente nos fios metálicos.
* **SPEF (Standard Parasitic Exchange Format):** Arquivo que descreve todos os RCs para simulação precisa.
* **Slew Rate (Transition):** A velocidade com que o sinal sobe de 0 para 1 (se for lento, o chip falha).
* **Fanout:** O número de portas lógicas que uma única saída está tentando "empurrar".

## 7. Verificação e Fabricação
* **DRC (Design Rule Check):** Validação se o desenho respeita os limites físicos da fábrica (ex: espessura do metal).
* **LVS (Layout Vs Schematic):** Comparação entre o desenho físico (GDS) e a lógica original (Verilog).
* **GDSII:** O formato de arquivo final (o "blueprint") enviado para a fábrica (Foundry).
* **Foundry:** A fábrica de semicondutores (ex: SkyWater, TSMC).
* **Wafer:** Disco de silício onde centenas de chips são impressos simultaneamente.
* **Corner Analysis:** Teste do chip em cenários extremos (ex: Calor + Voltagem Baixa vs Frio + Voltagem Alta).
* **Signoff:** O selo final de qualidade que autoriza o envio do projeto para fabricação (Tape-out).

# ☄️ Chuva de Meteoros - Introdução à Arquitetura de Computadores (IAC)

Este repositório contém o projeto desenvolvido para a disciplina de **Introdução à Arquitetura de Computadores (2021/2022)**. O objetivo foi aplicar os fundamentos de arquitetura de computadores através do desenvolvimento de um jogo completo (Chuva de Meteoros) programado integralmente em **Linguagem Assembly** para o processador **PEPE-16**, utilizando o simulador **SEPE**.

## 👥 Autor
* **Nome:** Tiago Queiroz de Orduna Dores Louro
* **Número de Aluno:** 104101
* **Grupo:** 7

## 🏆 Classificação do Projeto
| Componente | Nota |
| :--- | :--- |
| **Versão Intermédia (0 a 5)** | 3.8 |
| **Versão Final (0 a 15)** | 12.0 |
| **Nota Final do Projeto (20)** | **15.8 / 20** |

## 🎮 O Jogo: Chuva de Meteoros

O jogador controla um *rover* na superfície do Planeta X com a missão de o defender de naves robô invasoras (meteoros vermelhos) e de recolher energia de meteoros bons (verdes).

### Mecânicas Principais:
* **Controlo do Rover:** Movimento lateral contínuo gerido por varrimento do teclado matricial.
* **Sistema de Combate:** Disparo de mísseis com alcance limitado para destruir naves inimigas.
* **Gestão de Energia:** 
  * Exibida em tempo real em Displays de 7-Segmentos.
  * Decresce autonomamente com o tempo (3 em 3 segundos) e ao disparar mísseis.
  * Aumenta ao absorver meteoros bons (+10%) e ao destruir naves inimigas (+5%).
* **Geração Pseudo-Aleatória:** O tipo de meteoro (25% bom, 75% inimigo) e a coluna de spawn são decididos através da leitura de bits "flutuantes" num periférico de entrada (`PIN`).

## 🛠️ Detalhes Técnicos e Arquitetura

O projeto foi construído para interagir diretamente com o hardware simulado (SEPE), demonstrando o domínio de conceitos de baixo nível:

* **Linguagem:** Assembly (PEPE-16).
* **Gestão de Interrupções (Hardware Interrupts):** 
  * Implementação de rotinas de interrupção ligadas a 3 relógios de tempo real (Real-Time Clocks) independentes.
  * Relógio 0 (400ms): Controla a velocidade de descida dos meteoros.
  * Relógio 1 (200ms): Controla a velocidade do míssil.
  * Relógio 2 (3000ms): Controla o decréscimo periódico da energia.
* **Sincronização de Processos:** Utilização de variáveis de *lock* partilhadas entre as rotinas de interrupção e o ciclo principal do programa para garantir a fluidez sem bloquear a CPU.
* **I/O Memory-Mapped (Periféricos):**
  * **MediaCenter:** Escrita direta na memória de vídeo para desenhar o cenário e renderizar píxeis no ecrã de 32x64 (cores ARGB).
  * **Teclado (Keypad):** Leitura via periféricos `POUT-2` (8 bits) e `PIN`.
  * **Displays:** Atualização dinâmica de energia, convertendo os valores internos do processador (complemento para 2) para formato decimal exibido nos três displays do periférico `POUT-1` (16 bits).

## 🚀 Como Executar

1. Abrir o simulador **SEPE**.
2. Carregar o circuito `projeto.cir` (que inclui a configuração do PEPE-16, MediaCenter, Relógios e Memória).
3. Garantir que os ficheiros multimédia (imagens e sons) estão na mesma pasta do circuito.
4. Carregar o ficheiro de código `.asm` e iniciar a simulação.

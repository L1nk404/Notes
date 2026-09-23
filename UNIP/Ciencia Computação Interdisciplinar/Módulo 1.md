# Aula 1
## Resistores
- São um dos componentes eletrônicos mais universalmente utilizados, onipresentes em praticamente qualquer circuito eletrônico.
- Podem ser encontrados em uma enorme variedade de valores, tamanhos e tipos, bem como destinados a aplicações das mais diversas, em circuitos de baixa e alta potência.
- De acordo com Capuano e Marino (2003), resistores "são componentes que têm por finalidade oferecer uma oposição à passagem da corrente elétrica por meio de seu material".
- A essa oposição, chamamos de resistência elétrica, cujo valor é medido em Ohms, e unidade de medida, cujo símbolo é a letra grega ômega - Ω. Podem ter os seus valores na casa de poucos Ohms, ou de seus múltiplos, e podem também ter valores fixos ou variáveis.
- Resistores de valores diversos.
## Grandezas Envolvidas

| Grandeza    | Símbolo | Unidade       |
| ----------- | ------- | ------------- |
| Tensão      | V       | Volt(V)       |
| Corrente    | I       | Ampère(A)     |
| Resistência | R       | Ohm($\Omega$) |
### Tensão
- A tensão, também denominada "diferença de potencial", refere-se ao potencial elétrico que existe entre os dois terminais de um gerador de eletricidade qualquer, que pode ser uma pilha, bateria, ou mesmo um dinamo de bicicleta.
- Ela é a **força eletromotri**, portanto, que **empurra** os elétrons através dos fios que ligam o dito gerador (pilha, bateria, ...) ao circuito alimentado, fazendo-o funcionar.
- Sua unidade é medida em Volts (V)
### Corrente Elétrica
- Quando nos referimos à corrente elétrica, estamos falando do fluxo dos elétrons que saem de um gerador, lfuem pelo circuito que estão alimentando e retornam à referida bateria, constituindo o que chamamos de **circuito fechado**.
- Quanto maior é a corrente que cicula por um circuito, mais gorros também são os fios.
### Resistência
- Damos o nome de resistência elétrica à propriedade que qualquer corpo possui de opor-se à passagem da corrente elétrica
- É medida em *Ohms ($\Omega$)*, e podemos entendê-la simplesmente fazendo uma analogia a um engarrafamento de trânsito: Se existe um estreitamento na estrada, os veículos tentem a movimentar-se mais devagar e com dificuldade, mutias vezes demorando muito tempo para passar por esse trecho, chegando **em menor quantidade** ao final do dito estreitamento
#### Efeito Joule
Todo circuito que apresenta uma resistência elétrica irá produzir calor.
![[Pasted image 20260923190522.png]]
## Lei de Ohm
A diferença de potencial entre os terminais de um circuito é igual ao produto da resistência desse circuito pela intensidade da corrente elétrica.
$$
V = R\times i
$$
# Aula 2
## Tipos de resistores
### 1. Resistores Fixos 📖
- Os **resistores de carvão**, também conhecidos como carbon comp, são os **mais antigos** e construídos com uma mistura de cerâmica e pó de carvão moldada em um cilindro.
- Os **resistores de filme de carbono** são mais estáveis, com um filme de carbono depositado quimicamente sobre um cilindro cerâmico e cortado em espiral para ajustar o valor resistivo.
- Os **resistores de filme metálico** utilizam um filme metálico, geralmente de níquel-cromo, depositado a vácuo sobre uma base cerâmica de alta pureza, oferecendo maior precisão e estabilidade.
- Os **resistores de fio** são construídos com um fio resistivo, normalmente de níquel-cromo ou cobre-níquel-manganês, enrolado em espiral sobre uma base isolante, permitindo a dissipação de altas potências.
### 2. Resistores Variáveis 🚀
- Os **potenciômetros são resistores ajustáveis**, com um cursor que se move sobre uma pista resistiva, **permitindo o controle de valores dentro de uma faixa específica,** comumente usados como controles de volume ou tonalidade.
- Os **fotorresistores ou LDRs são sensores de luz de baixo custo**, feitos de sulfeto de cádmio, **cuja resistência varia de acordo com a intensidade luminosa incidente**.
- Os **termistores NTC e PTC são resistores sensíveis à temperatura,** com o valor resistivo diminuindo (NTC) ou aumentando (PTC) conforme a temperatura varia, sendo amplamente utilizados como sensores térmicos em equipamentos domésticos e industriais.
## Leitura dos valores de Resistores
Cada algarismo do valor ôhmico passa a ser representa por uma cor específica, assim como o número de zeros (multiplicador) que o valor do resistor irá empregar e também seu valor de tolerância, conforme tabela:
![[Pasted image 20260923191731.png]]
![[Pasted image 20260923191646.png]]
### Resistores com quatro faixas coloridas
![[Pasted image 20260923191827.png]]
Para procedermos com a leitura, fazemos:
1. Primeiro algarismo: Marrom = 1
2. Segunda algarismo: Verde = 5
3. Multiplicador: Vermelho = x100
4. Tolerância: Prata = 10%
Combinamos os valores, teremos o número 15 multiplicado por 100, perfazendo, portando o valor de $1500\Omega$ para o resistor ( ou 1K5$\Omega$), com uma tolerância de 10%.


> [!NOTE] 
> As duas primeiras faixas sempre serão algarismo, a terceira o multiplicador e a última, resistência:
> 

| Posição  | Função            |
| -------- | ----------------- |
| 1ª faixa | **1º algarismo**  |
| 2ª faixa | **2º algarismo**  |
| 3ª faixa | **Multiplicador** |
| 4ª faixa | **Tolerância**    |
# Aula 3
Quando associamos
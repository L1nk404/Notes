## Index
- [[#Aula 1]]
- [[#Aula 2]]
## Aula 1
### Erro - Representação dos Números
#### Sistema Posicionais
Cada algarismo possui um **valor absoluto** e um **valor relativo**.
##### Valor relativo
- o valor relativo é relativo à sua posição;
- Contam-se as posições da direta para a esquerda;
- Inicia-se a contagem de posição do zero;

Vamos começar com um exemplo do sistema posicional na base 10:

| Posição      | 3                    | 2                  | 1                   | 0                  |
| ------------ | -------------------- | ------------------ | ------------------- | ------------------ |
| Número$_{10} | 2                    | 0                  | 2                   | 3                  |
| Relativo     | $2 \times 10³= 2000$ | $0 \times 10² = 0$ | $2 \times 10¹ = 20$ | $3 \times 10⁰ = 3$ |
> Assim: $2000+0+20+3=2023$

Agora na base 2 e convertendo para a base 10

| Posição       | 3             | 2                 | 1                  | 0                 |
| ------------- | ------------- | ----------------- | ------------------ | ----------------- |
| Número$_{2}$  | 1             | 0                 | 1                  | 1                 |
| Relativo      | $1\times2³=8$ | $0 \times 2² = 0$ | $1 \times 10¹ = 2$ | $1 \times 2⁰ = 1$ |
> Assim: $8+0+2+1=11$ ou ainda, $(1011)_{2}=(11)_{10}$
##### Convertendo de Decimal para Binário
Converter para binário é simples: Seja $X$ o número decimal que queremos converter. Seja $B$ a representação binária de $X$. Para cada iteração $k$ de $0$ até $n$ (onde $n$ é o número de bits necessários para representar $X$): 1. Calcule o resto $R_k$ da divisão de $X$ por $2$: $$R_k = X \bmod 2$$ 2. Atualize a representação binária $B$ adicionando o resto $R_k$ no início: $$B = R_k \cdot 2^k + B$$ 3. Atualize $X$ pela divisão inteira por $2$: $$X = \lfloor X / 2 \rfloor$$ O processo continua até que $X = 0$. Em resumo: $$B = \sum_{k=0}^{n} R_k \cdot 2^k$$onde $R_k$ é o resto da divisão de $X$ por $2$ na iteração $k$, e $n$ é o número de bits necessários para representar $X$. 
Dizemos que a **contribuição** do valor no **bit i** é $2^i$. 
Em resumo tudo o que estamos fazendo é: 
1. Dado um número decimal $X$, 
2. Dividir $X$ por 2 e adicionar o resto ao binário - O "resto" (0/1) será adicionado ao bit $k$ ($k$ = número da iteração) 
3. Repetir o processo até que $X = 0$. 
###### Exemplos 
Vamos ver um exemplo: 
Tome $X = 5$:
$5/2 = 2(1)$, onde (1) é $R_0$ (o resto). $\Rightarrow 5_d = 1..._b$ 
Agora $X = 2$, então: $2/2 = 1(0)$ $\Rightarrow 5_d = 01.._b$ 
Repetindo o processo: $1/2 = 0(1)$ ($R_3 = 1$ porque $0.2 + 1 = 1$)
Portanto: $5_d = 101_b$
### Erro - Aritmética de pontos flutuantes
- Aritmética de ponto flutuante é um sistema matemático utilizado em computação para representar números reais
- Um número real é representado por uma **mantissa** ou **fração significativa**, ou ainda, **coeficiente** e um **expoente** que indicia o valor de magnitude do número.
Obtém-se a mantissa posicionando a vírgula à direita do primeiro algarismo significativo.

> [!NOTE] Algorismo significativo
> Os **algarismos significativos** são os [números](https://brasilescola.uol.com.br/matematica/numeros.htm) relevantes para determinar a precisão de um número, sendo seu último algarismo conhecido como algarismo duvidoso

Veja o exemplo:

| Número         | Mantissa | Potência  |
| -------------- | -------- | --------- |
| $0,002$        | $2$      | $10^{-3}$ |
| $45.000.000$   | $4,5$    | $10⁷$     |
| $0,0000092851$ | $9,2851$ | $10^{-6}$ |

> [!NOTE] Definição: Ponto  Flutuante
> Sejam $\beta\in\mathbb{N}$,  $\beta\geq2$ a base, $t\in\mathbb{N}$ o número de dígitos da mantissa e $a,b\in \mathbb{Z}$, com $a\geq b$ os limites do exponete. Definimos o **sistema de ponto flutuante** $F(\beta , t,a,b)$ como o conjunto:
> $$
> F(\beta , t,a,b) = \{y \in \mathbb{R} | y=\pm (0,d_1d_2 ... d_t \times \beta^e)\} \cup \{0\}
> $$
> onde:
> - $d_i \in \{0,1,...., \beta - 1\}$, para $i =1,....,t$;
> - $d_1\neq 0$ (condição de normalização, evitando representações redundantes);
> - $e\in \mathbb{Z}$, onde $a\geq e \geq b$.
> 
> O conjunto $F(β,t,a,b)F(\beta,t,a,b)$ $F(β,t,a,b)$ é **finito** e seus elementos são chamados de **números de máquina** ou **números representáveis**.

Exemplo:
Suponhamos que uma máquina opere no sitema da seguinte forma:
- $\beta = 10$;
- $t=3$;
- $e \in [-5,5]$.

Nesse sistema, os números serão representados na seguinte forma:
$$
r=\pm{0d_1d_2d_3}\times 10^e
$$
- Seja $m$ o menor número **absoluto** representado pela máquina, então: $m=0,100\times 10⁵ = 10⁶$
- Seja $M$ o maior o número representado pela máquina, então: $M=0,999 \times 10^5 = 99900$

Portanto,
$$
r \in [-M, -m] \cup \ \{0\} \cup [m, M]
$$

> [!NOTE] $m$ é o ínfimo do módulo
>  Por definição, $m$ é dado por:
> $$
> m = \min_{y \in F^*} |y|
> $$
> 
>Logo, o elemento mínimo da reta real em $F$ é, na verdade, $-M$, e não $m$:
> $$\min_{y \in F} y = -M = -99900$$
> 
Formalmente, os dois limites têm papéis distintos:
>
> | Símbolo | Definição | Significado numérico |
> |---|---|---|
> | $m$ | $\min\limits_{y \in F^*} \lvert y \rvert$ | limiar de **underflow** |
> | $M$ | $\max\limits_{y \in F} \lvert y \rvert$ | limiar de **overflow** |
> | $-M$ | $\min\limits_{y \in F} y$ | mínimo global da reta em $F$ |
#### Truncamento e Arredondamento
- **Truncamento** - É o processo de deprezar a parte da mantissa que não nos interessa
- **Arredondamento** - É o processo de adicionar 1 ao último algarismo a não ser desprezado, se e somente se, o 1º algarismo a ser desprezado é maior ou igual a 5 e menor e igual a 9.

Exemplo:
Tome $x = 235,89 = 0,23589 \times 10³$.
Podemos arrendondar ou trucar o número:
- $x=0,235 \times 10³$ (truncamento)
- $x=0,246 \times 10³$ (arredondamento)

#### Exercício
Efetue a operação na base 10 com 3 algarismos significativos usando truncamento:
$x= (4,26+9,24)+5,06$ 
$x=13,50 + 5,06\implies 18,56$

Fazendo o truncamento:
> $18,5$
> 
## Aula 2
### Zeros de Funções

> [!NOTE] Teorema de Bolzano
> Se $f(x)$ é uma função contínua me um intervalo fechado $[a,b]$, e se $f(a)$ tem sinais opostos, então existe $c \in [a,b]$ tal que $f(c)=0$

> [!NOTE] Teorema da Raiz Racional
> Seja $p(x)$ um polinômio dado por 
> $$
> p(x)=a_nx^n+a_{n-1}x^{n-1}+...+a_1x+a_0
> $$
> onde $a_n,...,a_0 \in \mathbb{Z}$. Se $\frac{r}{s} \in \mathbb{Q}$, na forma irredutível, é uma raiz de $p(x)$, então $r$ divide $a_0$ e $s$ divide $a_n$
##### Exemplo
Seja $f(x)=4x³-5x²+16x-20$:
1. Possíveis valores para $r$ (divisores de $20$): ($|1|,|2|,|4|,|5|,|10|,|20|$)
2. Possíveis valores para $s$ (divisores de 4): ($|2|,|4|$)

> Lembre-se que $\frac{r}{s}$ é irredutível


> [!NOTE] Dispositivo de Briot-Ruffini
> Contents

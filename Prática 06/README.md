<p align="center">
  <img src="assets6/bannercefet.png" width="100%">
</p>

# PROSIN I — Processamento de Sinais I


- **Professor:** Rafael da Silva Chaves
- **Instituição:** Centro Federal de Educação Tecnológica Celso Suckow da Fonseca - CEFET/RJ
- **Dupla:** Lucas de Farias dos Santos e Luís Felipe Chaves de Oliveira
- **Semestre:** 2026.1

# Prática 6 — Projeto de Filtros IIR.
---

# Prática 6 — Questão 1(a)

# Projeto e Análise de um Filtro Passa-Baixas Butterworth de 2ª Ordem

Nesta etapa foi realizado:

- Projeto de um filtro digital passa-baixa de 2ª ordem;
- obtenção dos coeficientes da função de transferência;
- análise da resposta em frequência;
- construção do diagrama de polos e zeros;
- verificação do fator de normalização do filtro;
- estudo da influência da posição dos polos na resposta em frequência.

---

# Importação das Bibliotecas

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import signal
```

## Explicação

As bibliotecas utilizadas foram:

- `numpy` → cálculos numéricos;
- `matplotlib` → geração dos gráficos;
- `scipy.signal` → projeto e análise dos filtros digitais.

---

# Definição da Frequência de Corte

```python
fc_lp = 1000
```

## Explicação

Foi especificada uma frequência de corte de:

```text
fc = 1000 Hz
```

Esta frequência define a região onde o filtro começa a atenuar as componentes do sinal.

---

# Conversão para Frequência Normalizada

```python
wn_lp = fc_lp / (fs / 2)
```

## Explicação

A função:

```python
signal.butter()
```

utiliza frequência normalizada.

A normalização é feita em relação à frequência de Nyquist:

```math
f_N=\frac{f_s}{2}
```

Logo:

```math
W_n=\frac{f_c}{f_N}
```

---

# Projeto do Filtro Butterworth

```python
b_lp, a_lp = signal.butter(
    2,
    wn_lp,
    btype='low'
)
```

## Explicação

Foi projetado um filtro:

- Butterworth;
- passa-baixas;
- ordem 2.

A aproximação Butterworth possui resposta maximamente plana na banda passante.

---

# Coeficientes da Função de Transferência

```python
print(b_lp)
print(a_lp)
```

## Explicação

Os vetores retornados representam:

Numerador:

```math
B(z)=b_0+b_1z^{-1}+b_2z^{-2}
```

Denominador:

```math
A(z)=1+a_1z^{-1}+a_2z^{-2}
```

---

# Resposta em Frequência

```python
w_lp, h_lp = signal.freqz(
    b_lp,
    a_lp
)
```

## Explicação

A função:

```python
freqz()
```

calcula a resposta em frequência do filtro.

O resultado fornece:

```math
H(e^{j\omega})
```

para diversos valores de frequência.

---

# Resposta de Magnitude

```python
20*np.log10(abs(h_lp))
```

## Explicação

A magnitude é convertida para decibéis:

```math
|H(f)|_{dB}
=
20\log_{10}|H(f)|
```

permitindo visualizar claramente a atenuação produzida pelo filtro.

---

# Resposta de Fase

```python
np.unwrap(np.angle(h_lp))
```

## Explicação

A fase mostra o deslocamento introduzido pelo filtro para cada frequência.

A função:

```python
unwrap()
```

remove descontinuidades de ±π.

---

# Diagrama Polo-Zero

```python
plot_pole_zero(
    b_lp,
    a_lp,
    ...
)
```

## Explicação

O diagrama polo-zero permite analisar:

- estabilidade;
- seletividade;
- comportamento espectral.

Os zeros correspondem às raízes do numerador.

Os polos correspondem às raízes do denominador.

---

# Cálculo dos Coeficientes do Denominador

```python
m1 = a_lp[1]
m2 = a_lp[2]
```

## Explicação

Os coeficientes do denominador são utilizados para representar o filtro na forma:

```math
H(z)=
\frac{K(z+1)^2}
     {z^2+m_1z+m_2}
```

---

# Cálculo do Fator de Normalização

```python
K =
\frac{1+m_1+m_2}{4}
```

## Explicação

O fator de normalização garante:

```math
H(1)=1
```

ou seja, ganho unitário em frequência zero.

---

# Verificação da Normalização

```python
np.allclose(...)
```

## Explicação

Foi verificado se os coeficientes calculados pelo Butterworth já satisfazem:

```math
B(z)=K(z+1)^2
```

garantindo a normalização automática do filtro.

---

# Influência do Raio dos Polos

Foram analisados diferentes valores:

```python
r = [0.5, 0.7, 0.9, 0.99]
```

---

# Construção do Filtro com Raio Variável

```python
a1 = -2r\cos(\theta_0)
a2 = r^2
```

## Explicação

Os polos possuem a forma:

```math
p=r e^{\pm j\theta_0}
```

onde:

- `r` determina a distância ao centro;
- `θ₀` determina a frequência característica.

---

# Frequência Angular

```python
theta_0=
2\pi\frac{f_c}{f_s}
```

## Explicação

A frequência angular define a localização angular dos polos no círculo unitário.

---

# Efeito da Variação de r

À medida que:

```math
r \rightarrow 1
```

os polos aproximam-se do círculo unitário.

Consequentemente:

- a seletividade aumenta;
- a transição torna-se mais estreita;
- ocorre maior ressonância próxima da frequência de corte.

Quando:

```math
r \rightarrow 0
```

os polos aproximam-se da origem.

Nesse caso:

- a resposta torna-se mais suave;
- a seletividade diminui.

---

# Resultado dos Gráficos

## Resposta em Frequência do Filtro Butterworth

<p align="center">
  <img src="assets6/Q1AP6.png" width="900">
</p>

---

## Influência do Raio dos Polos

<p align="center">
  <img src="assets6/Q1A2P6.png" width="900">
</p>

---

# Interpretação dos Resultados

Observa-se que:

- o filtro Butterworth apresenta resposta plana na banda passante;
- a frequência de corte ocorre próxima de 1000 Hz;
- a fase varia continuamente ao longo da frequência;
- todos os polos permanecem dentro do círculo unitário, garantindo estabilidade;
- o aumento do raio dos polos torna o filtro mais seletivo.

---

# Conclusão

O filtro Butterworth de 2ª ordem foi projetado e analisado com sucesso.

A avaliação da resposta em frequência, da fase, do diagrama polo-zero e da influência do raio dos polos permitiu compreender como a localização dos polos afeta diretamente o comportamento espectral do filtro digital.


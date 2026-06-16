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



# Questão 1(a) Filtro Passa-Baixas  de 2ª Ordem

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

Questão 1(b)

# Projeto e Análise de um Filtro Passa-Altas Butterworth de 2ª Ordem

Nesta etapa foi realizado:

- projeto de um filtro digital passa-altas Butterworth de 2ª ordem;
- obtenção dos coeficientes da função de transferência;
- análise da resposta em frequência;
- análise da resposta de fase;
- construção do diagrama polo-zero;
- estudo da influência da posição dos polos na resposta espectral.

---

# Importação das Bibliotecas

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import signal
```

## Explicação

As bibliotecas utilizadas foram:

- `numpy` → operações matemáticas;
- `matplotlib` → geração dos gráficos;
- `scipy.signal` → projeto e análise dos filtros digitais.

---

# Definição da Frequência de Corte

```python
fc_hp = 2000
```

## Explicação

Foi especificada uma frequência de corte igual a:

```text
fc = 2000 Hz
```

O filtro deverá permitir a passagem das componentes acima dessa frequência e atenuar as componentes inferiores.

---

# Conversão para Frequência Normalizada

```python
wn_hp = fc_hp / (fs / 2)
```

## Explicação

A função Butterworth utiliza frequência normalizada em relação à frequência de Nyquist:

```math
f_N=\frac{f_s}{2}
```

Portanto:

```math
W_n=\frac{f_c}{f_N}
```

---

# Projeto do Filtro Passa-Altas

```python
b_hp, a_hp = signal.butter(
    2,
    wn_hp,
    btype='high'
)
```

## Explicação

Foi projetado um filtro:

- Butterworth;
- passa-altas;
- ordem 2.

A resposta Butterworth apresenta comportamento suave e sem ondulações na banda passante.

---

# Coeficientes da Função de Transferência

```python
print(b_hp)
print(a_hp)
```

## Explicação

Os coeficientes obtidos representam a função de transferência digital:

```math
H(z)=
\frac{b_0+b_1z^{-1}+b_2z^{-2}}
     {1+a_1z^{-1}+a_2z^{-2}}
```

---

# Cálculo da Resposta em Frequência

```python
w_hp, h_hp = signal.freqz(
    b_hp,
    a_hp
)
```

## Explicação

A função:

```python
freqz()
```

calcula a resposta em frequência do filtro:

```math
H(e^{j\omega})
```

---

# Resposta de Magnitude

```python
20*np.log10(abs(h_hp))
```

## Explicação

A magnitude é representada em decibéis:

```math
|H(f)|_{dB}
=
20\log_{10}|H(f)|
```

permitindo visualizar claramente a região de atenuação e a banda passante.

---

# Resposta de Fase

```python
np.unwrap(np.angle(h_hp))
```

## Explicação

A fase indica o deslocamento angular introduzido pelo filtro em cada componente de frequência.

A função:

```python
unwrap()
```

remove saltos artificiais de ±π.

---

# Diagrama Polo-Zero

```python
plot_pole_zero(
    b_hp,
    a_hp,
    ...
)
```

## Explicação

O diagrama polo-zero permite visualizar:

- localização dos polos;
- localização dos zeros;
- estabilidade do sistema;
- comportamento espectral do filtro.

---

# Estrutura dos Zeros do Filtro Passa-Altas

Em filtros passa-altas Butterworth digitais, os zeros ficam próximos de:

```math
z=1
```

---

## Significado

A frequência:

```math
\omega = 0
```

corresponde à componente DC.

Ao posicionar zeros próximos de:

```math
z=1
```

o filtro rejeita componentes de baixa frequência.

---

# Frequência Angular de Referência

```python
theta_0_hp =
2*np.pi*fc_hp/fs
```

## Explicação

A frequência angular determina a posição angular dos polos:

```math
p=re^{\pm j\theta_0}
```

---

# Variação do Raio dos Polos

Foram avaliados os valores:

```python
r = [0.5, 0.7, 0.9, 0.99]
```

---

# Coeficientes do Biquad Passa-Altas

```python
b0_hp = (1 + cos(theta_0))/2
b1_hp = -(1 + cos(theta_0))
b2_hp = (1 + cos(theta_0))/2
```

## Explicação

Esses coeficientes posicionam zeros em baixas frequências, produzindo comportamento passa-altas.

---

# Coeficientes dos Polos

```python
a1_hp = -2*r*cos(theta_0)
a2_hp = r²
```

## Explicação

Os polos permanecem em:

```math
p=re^{\pm j\theta_0}
```

onde:

- `r` controla a seletividade;
- `θ₀` controla a frequência característica.

---

# Influência do Raio dos Polos

Quando:

```math
r \rightarrow 1
```

os polos aproximam-se do círculo unitário.

Isso produz:

- maior seletividade;
- transição mais abrupta;
- resposta mais próxima do filtro ideal.

---

Quando:

```math
r \rightarrow 0
```

os polos aproximam-se da origem.

Nesse caso ocorre:

- menor seletividade;
- transição mais suave;
- resposta menos pronunciada.

---

# Resposta em Frequência para Diferentes Valores de r

```python
signal.freqz(...)
```

## Explicação

Foi calculada a resposta em frequência para cada valor de:

```math
r
```

permitindo comparar o efeito da posição dos polos.

---

# Resultado dos Gráficos

## Resposta em Frequência do Filtro Passa-Altas

<p align="center">
  <img src="assets6/Q1BP6.png" width="900">
</p>

---

## Influência do Raio dos Polos

<p align="center">
  <img src="assets6/Q1B2P6.png" width="900">
</p>

---

# Interpretação dos Resultados

Observa-se que:

- componentes abaixo de 2000 Hz são fortemente atenuadas;
- componentes acima da frequência de corte permanecem praticamente inalteradas;
- a fase varia com a frequência devido à natureza causal do filtro;
- os polos permanecem dentro do círculo unitário, garantindo estabilidade;
- o aumento do raio dos polos produz uma resposta mais seletiva e uma transição mais estreita.

---

# Conclusão

O filtro Butterworth passa-altas de 2ª ordem foi projetado e analisado com sucesso.

A análise da magnitude, da fase, do diagrama polo-zero e da influência do raio dos polos demonstrou como a localização dos polos afeta diretamente a seletividade e o comportamento espectral do filtro digital.

---

# Prática 6 — Questão 1(c)

# Projeto e Análise de um Filtro Rejeita-Faixa (Notch)

Nesta etapa foi realizado:

- projeto de um filtro rejeita-faixa digital de segunda ordem;
- definição da frequência central de rejeição;
- cálculo dos coeficientes da função de transferência;
- ajuste do ganho para normalização;
- análise da resposta em frequência;
- construção do diagrama de polos e zeros;
- estudo da influência do raio dos polos.

O objetivo é criar um filtro capaz de rejeitar uma frequência específica do sinal, mantendo as demais componentes praticamente inalteradas.

---

# Importação das Bibliotecas

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import signal
```

## Explicação

As bibliotecas utilizadas foram:

- `numpy` → operações matemáticas;
- `matplotlib` → geração dos gráficos;
- `scipy.signal` → projeto e análise de filtros digitais.

---

# Definição dos Parâmetros

```python
fs = 20000
fc = 7000
```

## Explicação

Onde:

- `fs` é a frequência de amostragem;
- `fc` é a frequência central da rejeição.

Neste caso:

```text
fs = 20 kHz
fc = 7 kHz
```

---

# Conversão para Frequência Digital

```python
theta_c = 2*np.pi*fc/fs
```

## Explicação

A frequência central deve ser convertida para radianos por amostra.

A relação utilizada é:



onde:

- \(f_c\) é a frequência central;
- \(f_s\) é a frequência de amostragem.

---

# Definição do Raio dos Polos

```python
r_default = 0.95
```

## Explicação

O parâmetro:

```python
r
```

controla a seletividade do filtro.

Valores próximos de:

```python
1
```

produzem rejeições mais estreitas e seletivas.

---

# Cálculo dos Coeficientes do Denominador

```python
m1 = -2*r_default*np.cos(theta_c)
m2 = r_default**2
```

## Explicação

Os polos são posicionados próximos ao círculo unitário.

O denominador assume a forma:



---

# Definição dos Zeros

```python
b = [1, 0, -1]
```

## Explicação

O numerador corresponde ao polinômio:

\[
z^2 - 1
\]

que cria zeros responsáveis pela rejeição de frequência.

---

# Função de Transferência

A estrutura geral do filtro é:

\[
H(z)=K\frac{z^2-1}{z^2+m_1z+m_2}
\]

onde:

- \(K\) é o ganho de normalização;
- \(m_1\) e \(m_2\) determinam a posição dos polos.

---

# Cálculo do Ganho de Normalização

```python
omega, h = signal.freqz(b, a, worN=[theta_c])
gain_at_fc = np.abs(h[0])
K = 1/gain_at_fc
```

## Explicação

O fator:

```python
K
```

é calculado para ajustar o ganho do filtro.

Após o cálculo:

```python
b_scaled = [K*val for val in b]
```

---

# Coeficientes Obtidos

```python
print(...)
```

## Explicação

São exibidos:

- valor de \(r\);
- coeficientes \(m_1\) e \(m_2\);
- fator de ganho \(K\);
- coeficientes finais do numerador;
- coeficientes finais do denominador.

---

# Resposta em Frequência

```python
w, H = signal.freqz(b_scaled, a, worN=8192)
```

## Explicação

A função:

```python
freqz()
```

calcula a resposta em frequência do filtro.

---

# Resposta de Magnitude

```python
20*np.log10(abs(H))
```

## Explicação

A magnitude é representada em decibéis.

O gráfico permite visualizar:

- banda rejeitada;
- profundidade da rejeição;
- comportamento fora da frequência central.

---

# Resposta de Fase

```python
np.angle(H)
```

## Explicação

Mostra a variação de fase introduzida pelo filtro ao longo da frequência.

---

# Frequência Central

```python
axs[0].axvline(fc)
```

## Explicação

Uma linha vertical é utilizada para destacar a frequência:

```text
7000 Hz
```

que corresponde ao centro da rejeição.

---

# Diagrama de Polos e Zeros

```python
zeros = np.roots(b_scaled)
poles = np.roots(a)
```

## Explicação

São calculadas as raízes do numerador e do denominador.

---

# Plotagem do Círculo Unitário

```python
unit_circle = plt.Circle(...)
```

## Explicação

O círculo unitário é utilizado para verificar:

- estabilidade;
- posição dos polos;
- posição dos zeros.

---

# Zeros

```python
ax.plot(...,'o')
```

## Explicação

Os zeros aparecem como círculos.

Eles são responsáveis pela rejeição da frequência desejada.

---

# Polos

```python
ax.plot(...,'x')
```

## Explicação

Os polos aparecem como cruzes.

Sua posição controla:

- seletividade;
- largura da banda rejeitada.

---

# Verificação da Estabilidade

```python
if np.all(np.abs(poles) < 1)
```

## Explicação

Um filtro digital é estável quando todos os polos estão dentro do círculo unitário.

Matematicamente:

\[
|p_i| < 1
\]

para todos os polos.

---

# Influência do Parâmetro r

Foram avaliados os valores:

```python
r_values = [0.5, 0.85, 0.95, 0.99]
```

---

# Recalculo dos Coeficientes

Para cada valor de:

```python
r
```

são recalculados:

```python
m1
m2
K
```

e posteriormente a resposta em frequência.

---

# Comparação das Respostas

```python
plt.plot(...)
```

## Explicação

As curvas obtidas permitem comparar o efeito da variação do raio dos polos.

---

# Efeito do Raio dos Polos

### r = 0.5

- rejeição larga;
- baixa seletividade.

---

### r = 0.85

- banda rejeitada mais estreita;
- seletividade intermediária.

---

### r = 0.95

- rejeição mais seletiva;
- comportamento mais próximo do ideal.

---

### r = 0.99

- rejeição extremamente estreita;
- alta seletividade;
- polos muito próximos do círculo unitário.

---

# Interpretação dos Resultados

O filtro rejeita-faixa remove componentes próximas de:

```text
7000 Hz
```

mantendo as demais frequências praticamente preservadas.

Observa-se que:

- a frequência central permanece fixa;
- o parâmetro \(r\) controla a largura da rejeição;
- quanto maior \(r\), mais estreita e seletiva é a banda rejeitada;
- todos os casos permanecem estáveis enquanto os polos estiverem dentro do círculo unitário.

---

# Resultado Final

Ao executar o código, obtém-se:

- projeto completo de um filtro rejeita-faixa digital;
- resposta de magnitude e fase;
- diagrama de polos e zeros;
- análise da estabilidade;
- estudo da influência do raio dos polos sobre a seletividade do filtro.

---

# Prática 6 — Questão 1(d)

# Projeto e Análise de um Filtro Notch Digital de Segunda Ordem

Nesta etapa foi realizado:

- projeto de um filtro Notch digital;
- definição da frequência de rejeição;
- cálculo dos coeficientes do filtro;
- análise da resposta em frequência;
- análise do diagrama de polos e zeros;
- estudo da influência do parâmetro \(r\).

O objetivo é rejeitar uma frequência específica do sinal preservando as demais componentes espectrais.

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
- `matplotlib` → geração de gráficos;
- `scipy.signal` → projeto e análise de filtros digitais.

---

# Definição dos Parâmetros

```python
fs_notch = 20000
fc_notch = 3000
```

## Explicação

Foram definidos:

- frequência de amostragem:

```python
fs = 20000 Hz
```

- frequência central do filtro Notch:

```python
fc = 3000 Hz
```

A frequência de 3000 Hz será atenuada pelo filtro.

---

# Conversão para Frequência Digital

```python
theta_c_notch = 2*np.pi*fc_notch/fs_notch
```

## Explicação

A frequência central deve ser convertida para radianos por amostra:

\[
\theta_c=\frac{2\pi f_c}{f_s}
\]

onde:

- \(f_c\) é a frequência de rejeição;
- \(f_s\) é a frequência de amostragem.

---

# Definição do Raio dos Polos

```python
r_notch = 0.95
```

## Explicação

O parâmetro:

```python
r
```

controla a largura da banda rejeitada.

Valores de \(r\):

- próximos de 1 → notch estreito;
- menores → notch mais largo.

---

# Cálculo dos Coeficientes do Numerador

```python
b_notch = [1, -2*np.cos(theta_c_notch), 1]
```

## Explicação

O numerador gera dois zeros sobre o círculo unitário:

\[
B(z)=z^2-2\cos(\theta_c)z+1
\]

Esses zeros anulam a frequência desejada.

---

# Cálculo dos Coeficientes do Denominador

```python
a_notch = [
    1,
    -2*r_notch*np.cos(theta_c_notch),
    r_notch**2
]
```

## Explicação

O denominador posiciona polos próximos aos zeros:

\[
A(z)=z^2-2r\cos(\theta_c)z+r^2
\]

Os polos aumentam a seletividade da rejeição.

---

# Fator de Normalização

```python
K_notch
```

## Explicação

Foi calculado um fator de ganho para garantir:

\[
H(1)=1
\]

ou seja, ganho unitário em DC.

O fator utilizado foi:

\[
K=
\frac{1-2r\cos(\theta_c)+r^2}
     {2-2\cos(\theta_c)}
\]

---

# Função de Transferência

Após a normalização, o filtro pode ser representado por:

\[
H(z)=
K\,
\frac
{z^2-2\cos(\theta_c)z+1}
{z^2-2r\cos(\theta_c)z+r^2}
\]

---

# Resposta em Frequência

```python
w_notch, H_notch = signal.freqz(
    b_notch_scaled,
    a_notch,
    worN=8192
)
```

## Explicação

A função:

```python
freqz()
```

calcula a resposta em frequência do filtro.

Foram obtidas:

- magnitude;
- fase.

---

# Gráfico da Magnitude

```python
20*np.log10(abs(H_notch))
```

## Explicação

A magnitude é apresentada em decibéis:

\[
|H(f)|_{dB}
=
20\log_{10}|H(f)|
\]

Observa-se uma forte atenuação em:

```python
3000 Hz
```

caracterizando o comportamento Notch.

---

# Gráfico da Fase

```python
np.angle(H_notch)
```

## Explicação

A fase mostra o deslocamento angular introduzido pelo filtro em cada frequência.

---

# Diagrama de Polos e Zeros

```python
zeros_notch = np.roots(b_notch_scaled)
poles_notch = np.roots(a_notch)
```

## Explicação

Foram calculadas as raízes do numerador e denominador.

Essas raízes representam:

- zeros;
- polos.

---

# Interpretação dos Zeros

Os zeros ficam localizados sobre o círculo unitário na frequência:

\[
\theta_c
\]

e são responsáveis pela rejeição total da componente de 3000 Hz.

---

# Interpretação dos Polos

Os polos ficam próximos dos zeros, porém dentro do círculo unitário.

Sua função é aumentar a seletividade da rejeição.

Quanto mais próximos do círculo unitário:

```python
r → 1
```

mais estreito será o notch.

---

# Verificação de Estabilidade

```python
np.all(np.abs(poles_notch) < 1)
```

## Explicação

Um filtro digital é estável quando:

\[
|p_i|<1
\]

para todos os polos.

Como:

```python
r = 0.95
```

todos os polos permanecem dentro do círculo unitário.

Portanto, o filtro é estável.

---

# Influência do Parâmetro r

Foram analisados os valores:

```python
r = [0.7, 0.85, 0.95, 0.99]
```

---

## r = 0.7

- notch largo;
- menor seletividade;
- maior faixa rejeitada.

---

## r = 0.85

- rejeição intermediária;
- melhor compromisso entre largura e seletividade.

---

## r = 0.95

- notch estreito;
- elevada seletividade.

---

## r = 0.99

- notch extremamente estreito;
- rejeição muito seletiva;
- polos muito próximos do círculo unitário.

---

# Interpretação dos Resultados

Observa-se que:

- a frequência de 3000 Hz é fortemente atenuada;
- as demais frequências permanecem praticamente inalteradas;
- o parâmetro \(r\) controla a largura da banda rejeitada;
- o filtro permanece estável para \(0<r<1\).

---

# Resultado dos Gráficos

## Resposta em Frequência do Filtro Notch

<p align="center">
  <img src="assets6/Q1D_1.png" width="900">
</p>

---

## Diagrama de Polos e Zeros

<p align="center">
  <img src="assets6/Q1D_2.png" width="700">
</p>

---

## Influência do Parâmetro r

<p align="center">
  <img src="assets6/Q1D_3.png" width="900">
</p>

---

# Resultado Final

Ao executar o código obtém-se:

- projeto completo de um filtro Notch digital;
- análise da magnitude e fase;
- visualização dos polos e zeros;
- verificação da estabilidade;
- estudo do efeito do parâmetro \(r\) sobre a largura da banda rejeitada.

---

# Prática 6 — Questão 2

# Projeto de Filtro Passa-Faixas em Cascata

Nesta etapa foi realizado:

- projeto de um filtro passa-altas de segunda ordem;
- projeto de um filtro passa-baixas de segunda ordem;
- associação em cascata dos dois filtros;
- análise da resposta em frequência;
- análise do diagrama de polos e zeros.

O objetivo é obter um filtro passa-faixas com banda útil compreendida entre:

- 6000 Hz;
- 8000 Hz.

---

# Importação das Bibliotecas

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import signal
```

## Explicação

As bibliotecas utilizadas foram:

- `numpy` → operações matemáticas;
- `matplotlib` → geração de gráficos;
- `scipy.signal` → projeto e análise de filtros digitais.

---

# Definição dos Parâmetros

```python
fs = 20000
fc1 = 6000
fc2 = 8000
```

## Explicação

Foram definidas:

- frequência de amostragem:

```python
fs = 20000 Hz
```

- frequência de corte inferior:

```python
fc1 = 6000 Hz
```

- frequência de corte superior:

```python
fc2 = 8000 Hz
```

---

# Estrutura do Filtro

O filtro passa-faixas é obtido pela cascata de:

```text
Passa-Altas → Passa-Baixas
```

onde:

```text
HPF(fc1 = 6000 Hz)
```

remove componentes abaixo de 6000 Hz

e

```text
LPF(fc2 = 8000 Hz)
```

remove componentes acima de 8000 Hz.

---

# Projeto do Filtro Passa-Altas

```python
b_hp, a_hp = signal.butter(
    2,
    fc1,
    btype='highpass',
    fs=fs,
    output='ba'
)
```

## Explicação

Foi utilizado um filtro:

```python
Butterworth de 2ª ordem
```

com frequência de corte:

```python
6000 Hz
```

---

# Características do Butterworth

O filtro Butterworth apresenta:

- resposta maximamente plana;
- ausência de ondulações na banda passante;
- transição suave entre bandas.

---

# Projeto do Filtro Passa-Baixas

```python
b_lp, a_lp = signal.butter(
    2,
    fc2,
    btype='lowpass',
    fs=fs,
    output='ba'
)
```

## Explicação

Foi projetado um segundo filtro Butterworth de:

```python
2ª ordem
```

com frequência de corte:

```python
8000 Hz
```

---

# Coeficientes dos Filtros

Os coeficientes retornados são:

```python
b_hp , a_hp
```

para o filtro passa-altas

e

```python
b_lp , a_lp
```

para o filtro passa-baixas.

Esses coeficientes representam a função de transferência digital:

```math
H(z)=\frac{B(z)}{A(z)}
```

---

# Formação da Cascata

```python
b_cascata = np.convolve(b_hp, b_lp)
a_cascata = np.convolve(a_hp, a_lp)
```

## Explicação

A cascata de filtros equivale à multiplicação das funções de transferência:

```math
H_{PF}(z)=H_{HP}(z)\cdot H_{LP}(z)
```

Como multiplicação no domínio Z corresponde à convolução dos coeficientes:

```python
np.convolve()
```

é utilizada para obter os coeficientes finais.

---

# Função de Transferência Resultante

O filtro resultante possui:

```python
ordem = 4
```

pois é formado por:

```text
2ª ordem + 2ª ordem
```

---

# Resposta em Frequência

```python
w_cascata, H_cascata = signal.freqz(
    b_cascata,
    a_cascata,
    worN=8192
)
```

## Explicação

A função:

```python
freqz()
```

calcula a resposta em frequência do filtro digital.

---

# Gráfico de Magnitude

```python
20*np.log10(abs(H_cascata))
```

## Explicação

A magnitude é convertida para:

```text
decibéis (dB)
```

permitindo observar:

- banda passante;
- banda de rejeição;
- frequência de corte.

---

# Frequências de Corte

As linhas verticais indicam:

```python
fc1 = 6000 Hz
```

e

```python
fc2 = 8000 Hz
```

delimitando a faixa de frequências transmitidas pelo filtro.

---

# Gráfico de Fase

```python
np.angle(H_cascata)
```

## Explicação

A fase mostra o deslocamento angular introduzido pelo filtro para cada frequência.

---

# Diagrama de Polos e Zeros

Os polos e zeros são obtidos por:

```python
zeros_cascata = np.roots(b_cascata)
poles_cascata = np.roots(a_cascata)
```

---

# Zeros

```python
np.roots(b_cascata)
```

## Explicação

Os zeros correspondem às frequências atenuadas pelo filtro.

Graficamente são representados por:

```text
○
```

---

# Polos

```python
np.roots(a_cascata)
```

## Explicação

Os polos determinam:

- estabilidade;
- seletividade;
- formato da resposta em frequência.

Graficamente são representados por:

```text
×
```

---

# Círculo Unitário

```python
plt.Circle((0,0),1)
```

## Explicação

O círculo unitário é utilizado para verificar a estabilidade.

Um filtro digital é estável quando:

```math
|p_i| < 1
```

para todos os polos.

---

# Interpretação da Resposta em Frequência

O filtro obtido:

- rejeita frequências abaixo de 6000 Hz;
- permite a passagem de frequências entre 6000 Hz e 8000 Hz;
- rejeita frequências acima de 8000 Hz.

Dessa forma comporta-se como um filtro passa-faixas.

---

# Interpretação do Diagrama de Polos e Zeros

Observa-se que:

- os polos permanecem dentro do círculo unitário;
- o sistema é estável;
- a distribuição dos polos determina a largura da banda passante;
- os zeros contribuem para a rejeição fora da faixa desejada.

---

# Comparação com os Filtros da Questão 1

Enquanto na Questão 1 foram analisados:

- passa-baixas;
- passa-altas;
- passa-faixas ressonante;
- notch;

nesta questão foi utilizado um método diferente para obter um passa-faixas.

Em vez de utilizar um único bloco de segunda ordem, o filtro foi construído pela associação em cascata de:

```text
Passa-Altas + Passa-Baixas
```

resultando em um filtro de ordem superior e maior seletividade.

---

# Resultado dos Gráficos

## Resposta em Frequência do Filtro Passa-Faixas em Cascata

<p align="center">
  <img src="assets6/Q2_MagnitudeFase.png" width="900">
</p>

---

## Diagrama de Polos e Zeros

<p align="center">
  <img src="assets6/Q2_PolosZeros.png" width="700">
</p>

---

# Resultado Final

Ao executar o código, obtém-se:

- projeto de um filtro passa-faixas digital;
- implementação por cascata de filtros Butterworth;
- resposta em magnitude e fase;
- diagrama de polos e zeros;
- verificação da estabilidade do sistema.

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

# Prática 6 — Questão 3

# Projeto de Filtro Rejeita-Faixas em Paralelo

Nesta etapa foi realizado:

- projeto de um filtro passa-baixas de segunda ordem;
- projeto de um filtro passa-altas de segunda ordem;
- combinação dos filtros em paralelo;
- análise da resposta em frequência;
- análise do diagrama de polos e zeros.

O objetivo é obter um filtro rejeita-faixas capaz de atenuar as frequências compreendidas entre:

- 1000 Hz;
- 4000 Hz.

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
fs_br = 20000
fc1_br = 1000
fc2_br = 4000
```

## Explicação

Foram definidas:

- frequência de amostragem:

```python
fs = 20000 Hz
```

- frequência limite inferior da banda rejeitada:

```python
fc1_br = 1000 Hz
```

- frequência limite superior da banda rejeitada:

```python
fc2_br = 4000 Hz
```

---

# Estrutura do Filtro

O filtro rejeita-faixas foi construído pela associação em paralelo de:

```text
Passa-Baixas + Passa-Altas
```

onde:

```text
LPF(fc = 1000 Hz)
```

permite a passagem das baixas frequências

e

```text
HPF(fc = 4000 Hz)
```

permite a passagem das altas frequências.

A região intermediária é rejeitada.

---

# Projeto do Filtro Passa-Baixas

```python
b_lpf_br, a_lpf_br = signal.butter(
    order_br,
    fc1_br,
    btype='lowpass',
    fs=fs_br,
    output='ba'
)
```

## Explicação

Foi utilizado um filtro Butterworth de segunda ordem com frequência de corte igual a:

```python
1000 Hz
```

---

# Projeto do Filtro Passa-Altas

```python
b_hpf_br, a_hpf_br = signal.butter(
    order_br,
    fc2_br,
    btype='highpass',
    fs=fs_br,
    output='ba'
)
```

## Explicação

Foi utilizado um filtro Butterworth de segunda ordem com frequência de corte igual a:

```python
4000 Hz
```

---

# Coeficientes dos Filtros

Os coeficientes obtidos são:

```python
b_lpf_br , a_lpf_br
```

para o passa-baixas

e

```python
b_hpf_br , a_hpf_br
```

para o passa-altas.

Esses coeficientes definem as funções de transferência digitais dos filtros.

---

# Associação em Paralelo

Diferentemente da cascata utilizada na Questão 2, nesta etapa os filtros são ligados em paralelo.

A estrutura pode ser representada por:

```text
             ┌─ LPF ─┐
Entrada ─────┤       ├──── Saída
             └─ HPF ─┘
```

A saída total é dada pela soma das saídas dos dois filtros.

---

# Denominador Equivalente

```python
a_parallel_br = np.convolve(
    a_lpf_br,
    a_hpf_br
)
```

## Explicação

O denominador equivalente é obtido pelo produto dos denominadores dos filtros individuais.

---

# Numerador Equivalente

```python
num1 = np.convolve(
    b_lpf_br,
    a_hpf_br
)

num2 = np.convolve(
    b_hpf_br,
    a_lpf_br
)
```

## Explicação

Como os filtros estão em paralelo, as funções de transferência são somadas:

```math
H(z)=H_{LPF}(z)+H_{HPF}(z)
```

Após colocar as frações sob um denominador comum, obtém-se:

```math
H(z)=
\frac{
B_{LPF}(z)A_{HPF}(z)
+
B_{HPF}(z)A_{LPF}(z)
}
{
A_{LPF}(z)A_{HPF}(z)
}
```

---

# Soma dos Polinômios

```python
b_parallel_br
```

## Explicação

O numerador final é obtido pela soma dos produtos cruzados dos coeficientes.

Essa operação produz a função de transferência equivalente do filtro rejeita-faixas.

---

# Resposta em Frequência

```python
w_parallel_br, H_parallel_br =
signal.freqz(
    b_parallel_br,
    a_parallel_br,
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
20*np.log10(abs(H_parallel_br))
```

## Explicação

A magnitude é convertida para decibéis para facilitar a visualização da atenuação.

A região entre:

```python
1000 Hz
```

e

```python
4000 Hz
```

apresenta forte redução de ganho.

---

# Gráfico de Fase

```python
np.angle(H_parallel_br)
```

## Explicação

O gráfico de fase mostra o deslocamento angular provocado pelo filtro em cada frequência.

---

# Frequências de Rejeição

As linhas verticais destacam:

```python
fc1_br = 1000 Hz
```

e

```python
fc2_br = 4000 Hz
```

delimitando a faixa rejeitada pelo sistema.

---

# Diagrama de Polos e Zeros

Os polos e zeros são obtidos por:

```python
zeros_parallel_br =
np.roots(b_parallel_br)

poles_parallel_br =
np.roots(a_parallel_br)
```

---

# Zeros

```python
np.roots(b_parallel_br)
```

## Explicação

Os zeros representam frequências nas quais ocorre forte atenuação da resposta.

No filtro rejeita-faixas, eles são responsáveis pela formação da banda rejeitada.

---

# Polos

```python
np.roots(a_parallel_br)
```

## Explicação

Os polos controlam:

- seletividade;
- estabilidade;
- inclinação das transições.

---

# Círculo Unitário

```python
plt.Circle((0,0),1)
```

## Explicação

O círculo unitário é utilizado para verificar a estabilidade do sistema.

O filtro é estável quando:

```math
|p_i|<1
```

para todos os polos.

---

# Interpretação da Resposta em Frequência

O filtro obtido:

- permite a passagem de frequências abaixo de 1000 Hz;
- rejeita frequências entre 1000 Hz e 4000 Hz;
- permite a passagem de frequências acima de 4000 Hz.

Portanto, apresenta comportamento típico de um filtro rejeita-faixas.

---

# Interpretação do Diagrama de Polos e Zeros

Observa-se que:

- os polos permanecem dentro do círculo unitário;
- o sistema é estável;
- os zeros produzem a região de rejeição;
- a posição dos polos controla a transição entre as bandas.

---

# Comparação com o Filtro Notch da Questão 1(d)

O filtro Notch desenvolvido anteriormente possui:

- rejeição extremamente localizada;
- faixa estreita de atenuação;
- alta seletividade em torno de uma única frequência.

Já o filtro rejeita-faixas desta questão apresenta:

- rejeição distribuída em uma faixa mais larga;
- atenuação entre 1000 Hz e 4000 Hz;
- comportamento adequado para eliminar uma banda inteira de frequências.

Assim, o filtro Notch pode ser considerado um caso particular de filtro rejeita-faixas com largura de banda muito pequena.

---

# Resultado dos Gráficos

## Resposta em Frequência do Filtro Rejeita-Faixas

<p align="center">
  <img src="assets6/Q3_MagnitudeFase.png" width="900">
</p>

---

## Diagrama de Polos e Zeros

<p align="center">
  <img src="assets6/Q3_PolosZeros.png" width="700">
</p>

---

# Resultado Final

Ao executar o código, obtém-se:

- projeto de um filtro rejeita-faixas digital;
- implementação por associação paralela de filtros Butterworth;
- resposta em magnitude e fase;
- diagrama de polos e zeros;
- verificação da estabilidade do sistema;
- comparação com o filtro Notch estudado anteriormente.

# Prática 6 — Questão 4(a)

# Quantização dos Coeficientes dos Filtros Digitais

Nesta etapa foi realizada:

- implementação de uma rotina de quantização de coeficientes;
- quantização dos coeficientes do filtro passa-faixas em cascata;
- quantização dos coeficientes do filtro rejeita-faixas em paralelo;
- análise da resposta em frequência para diferentes resoluções;
- análise da movimentação dos polos e zeros após a quantização.

O objetivo é verificar como a precisão numérica dos coeficientes afeta o comportamento dos filtros digitais.

---

# Importação das Bibliotecas

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import signal
```

## Explicação

As bibliotecas utilizadas foram:

- `numpy` → operações numéricas;
- `matplotlib` → geração de gráficos;
- `scipy.signal` → análise de filtros digitais.

---

# Função de Quantização

```python
def quantize_coefficients_simple(coefficients, bits):
```

## Explicação

Foi implementada uma função responsável por quantizar os coeficientes dos filtros utilizando uma representação de precisão limitada.

---

# Verificação de Vetor Vazio

```python
if not coefficients.size:
    return coefficients
```

## Explicação

Caso o vetor esteja vazio, ele é retornado sem alterações.

---

# Determinação da Faixa Dinâmica

```python
max_abs_coeff = np.max(np.abs(coefficients))
```

## Explicação

Calcula o maior valor absoluto presente entre os coeficientes.

Esse valor será utilizado para definir a faixa de quantização.

---

# Tratamento para Coeficientes Nulos

```python
if max_abs_coeff == 0:
    return np.zeros_like(coefficients)
```

## Explicação

Se todos os coeficientes forem iguais a zero, o vetor quantizado também será composto apenas por zeros.

---

# Número de Níveis de Quantização

```python
num_levels = 2**bits
```

## Explicação

O número de níveis disponíveis depende da quantidade de bits utilizada.

Exemplos:

| Bits | Níveis |
|--------|--------|
| 2 | 4 |
| 4 | 16 |
| 8 | 256 |
| 16 | 65536 |
| 32 | 4294967296 |

---

# Passo de Quantização

```python
quantization_step = (2 * max_abs_coeff) / num_levels
```

## Explicação

Calcula a distância entre dois níveis consecutivos de quantização.

Quanto maior o número de bits:

- menor o passo;
- menor o erro de quantização.

---

# Quantização dos Coeficientes

```python
quantized_coeffs =
np.round(coefficients / quantization_step)
* quantization_step
```

## Explicação

Cada coeficiente é:

1. dividido pelo passo;
2. arredondado para o nível mais próximo;
3. convertido novamente para o valor correspondente.

---

# Quantização do Filtro Passa-Faixas em Cascata

Os coeficientes utilizados foram obtidos na Questão 2:

```python
b_cascata
a_cascata
```

---

# Resoluções Testadas

```python
bits_to_test = [2, 4, 8, 16, 32]
```

## Explicação

Foram analisadas cinco precisões diferentes:

- 2 bits;
- 4 bits;
- 8 bits;
- 16 bits;
- 32 bits.

---

# Quantização dos Coeficientes

```python
b_cascata_q =
quantize_coefficients_simple(
    b_cascata,
    bits
)

a_cascata_q =
quantize_coefficients_simple(
    a_cascata,
    bits
)
```

## Explicação

Os coeficientes do numerador e do denominador são quantizados para cada quantidade de bits.

---

# Cálculo da Resposta em Frequência

```python
w_q, H_q =
signal.freqz(
    b_cascata_q,
    a_cascata_q,
    worN=8192
)
```

## Explicação

A função:

```python
freqz()
```

calcula a resposta em frequência do filtro quantizado.

---

# Comparação com o Filtro Original

```python
axs[0].plot(...)
axs[1].plot(...)
```

## Explicação

A resposta do filtro quantizado é comparada com a resposta do filtro original.

São avaliadas:

- magnitude;
- fase.

---

# Cálculo dos Polos e Zeros

```python
zeros_q = np.roots(b_cascata_q)
poles_q = np.roots(a_cascata_q)
```

## Explicação

Obtém-se a localização dos:

- polos;
- zeros;

após a quantização.

---

# Diagrama de Polos e Zeros

```python
ax.plot(...)
```

## Explicação

Os polos e zeros quantizados são comparados com aqueles obtidos no filtro original.

Isso permite visualizar diretamente o efeito da quantização.

---

# Interpretação dos Resultados do Filtro em Cascata

Quando poucos bits são utilizados:

- ocorre grande distorção dos coeficientes;
- polos e zeros se deslocam significativamente;
- a resposta em frequência é alterada.

À medida que o número de bits aumenta:

- os coeficientes aproximam-se dos valores originais;
- os polos retornam às posições desejadas;
- a resposta em frequência converge para a resposta ideal.

---

# Quantização do Filtro Rejeita-Faixas em Paralelo

Os coeficientes utilizados foram obtidos na Questão 3:

```python
b_parallel_br
a_parallel_br
```

---

# Quantização dos Coeficientes

```python
b_parallel_br_q =
quantize_coefficients_simple(
    b_parallel_br,
    bits
)

a_parallel_br_q =
quantize_coefficients_simple(
    a_parallel_br,
    bits
)
```

## Explicação

Os coeficientes do filtro rejeita-faixas também são quantizados para diferentes resoluções.

---

# Resposta em Frequência

```python
signal.freqz(
    b_parallel_br_q,
    a_parallel_br_q
)
```

## Explicação

A resposta do filtro quantizado é comparada com a resposta do filtro original.

São avaliadas:

- largura da faixa rejeitada;
- profundidade da rejeição;
- comportamento da fase.

---

# Polos e Zeros Quantizados

```python
np.roots(...)
```

## Explicação

São calculadas as novas posições dos polos e zeros após a quantização.

---

# Comparação Visual

Nos diagramas gerados:

- azul → filtro original;
- vermelho → filtro quantizado.

---

# Interpretação dos Resultados do Filtro Rejeita-Faixas

A quantização provoca:

- deslocamento dos polos;
- deslocamento dos zeros;
- alteração da frequência rejeitada;
- redução da profundidade do notch.

Os efeitos tornam-se mais severos para:

```python
2 bits
```

e

```python
4 bits
```

---

# Influência da Quantização

Quanto menor a quantidade de bits:

- maior o erro de arredondamento;
- maior a distorção espectral;
- maior o risco de degradação do filtro.

Quanto maior a quantidade de bits:

- menor o erro numérico;
- maior a fidelidade ao projeto original.

---

# Resultado Final

Ao executar o código, obtém-se:

- quantização dos coeficientes dos filtros;
- comparação entre filtros originais e quantizados;
- análise das respostas em frequência;
- análise dos diagramas de polos e zeros;
- avaliação do impacto da precisão numérica sobre o desempenho dos filtros digitais.

---

# Prática 6 — Questão 4(b)

# Quantização dos Coeficientes em Cada Bloco dos Filtros

Nesta etapa foi realizada:

- quantização individual dos blocos componentes dos filtros;
- quantização separada dos coeficientes dos filtros passa-baixas e passa-altas;
- reconstrução dos filtros após a quantização;
- análise das respostas em frequência;
- análise dos diagramas de polos e zeros.

O objetivo é verificar se a quantização realizada em cada bloco separadamente produz menor degradação quando comparada à quantização do filtro completo realizada na Questão 4(a).

---

# Estratégia Utilizada

Na Questão 4(a), a quantização foi aplicada diretamente sobre os coeficientes finais dos filtros:

```python
b_cascata
a_cascata
```

e

```python
b_parallel_br
a_parallel_br
```

Nesta etapa, a quantização é realizada antes da combinação dos blocos.

---

# Vantagem da Quantização por Blocos

Ao quantizar cada estágio separadamente:

- os coeficientes permanecem menores;
- a faixa dinâmica é reduzida;
- o erro de quantização tende a ser menor;
- a estabilidade pode ser melhor preservada.

---

# Quantização do Filtro Passa-Faixas em Cascata

O filtro passa-faixas foi construído a partir de:

```python
HPF + LPF
```

onde:

- HPF → filtro passa-altas;
- LPF → filtro passa-baixas.

---

# Resoluções Testadas

```python
bits_to_test = [2, 4, 8, 16, 32]
```

## Explicação

Foram analisadas cinco precisões diferentes:

- 2 bits;
- 4 bits;
- 8 bits;
- 16 bits;
- 32 bits.

---

# Quantização dos Blocos Individuais

```python
b_hp_q = quantize_coefficients_simple(b_hp, bits)
a_hp_q = quantize_coefficients_simple(a_hp, bits)

b_lp_q = quantize_coefficients_simple(b_lp, bits)
a_lp_q = quantize_coefficients_simple(a_lp, bits)
```

## Explicação

Cada bloco é quantizado separadamente antes da montagem do filtro completo.

---

# Reconstrução da Cascata

```python
b_cascata_blocks_q =
np.convolve(
    b_hp_q,
    b_lp_q
)

a_cascata_blocks_q =
np.convolve(
    a_hp_q,
    a_lp_q
)
```

## Explicação

Após a quantização dos blocos:

- os numeradores são convolvidos;
- os denominadores são convolvidos;

reconstruindo o filtro passa-faixas.

---

# Cálculo da Resposta em Frequência

```python
w_q_blocks, H_q_blocks =
signal.freqz(
    b_cascata_blocks_q,
    a_cascata_blocks_q,
    worN=8192
)
```

## Explicação

Calcula-se a resposta em frequência do filtro reconstruído após a quantização.

---

# Comparação com o Filtro Original

```python
axs[0].plot(...)
axs[1].plot(...)
```

## Explicação

São comparadas:

- magnitude;
- fase.

entre o filtro original e o filtro quantizado.

---

# Cálculo dos Polos e Zeros

```python
zeros_q_blocks =
np.roots(
    b_cascata_blocks_q
)

poles_q_blocks =
np.roots(
    a_cascata_blocks_q
)
```

## Explicação

Obtêm-se os polos e zeros do filtro reconstruído após a quantização dos blocos.

---

# Diagrama de Polos e Zeros

```python
ax.plot(...)
```

## Explicação

Os polos e zeros obtidos após a quantização são comparados com os polos e zeros do filtro original.

---

# Interpretação dos Resultados do Filtro em Cascata

A quantização individual dos blocos tende a:

- preservar melhor a estrutura do filtro;
- reduzir erros acumulados;
- minimizar deslocamentos dos polos;
- aproximar a resposta da versão ideal.

Para resoluções elevadas:

```python
16 bits
```

e

```python
32 bits
```

a diferença para o filtro original torna-se praticamente imperceptível.

---

# Quantização do Filtro Rejeita-Faixas em Paralelo

O filtro rejeita-faixas foi implementado utilizando:

```python
LPF + HPF
```

em paralelo.

---

# Quantização dos Blocos

```python
b_lpf_br_q =
quantize_coefficients_simple(
    b_lpf_br,
    bits
)

a_lpf_br_q =
quantize_coefficients_simple(
    a_lpf_br,
    bits
)

b_hpf_br_q =
quantize_coefficients_simple(
    b_hpf_br,
    bits
)

a_hpf_br_q =
quantize_coefficients_simple(
    a_hpf_br,
    bits
)
```

## Explicação

Cada bloco é quantizado separadamente antes da associação paralela.

---

# Formação do Filtro Paralelo

## Denominador

```python
a_parallel_blocks_q =
np.convolve(
    a_lpf_br_q,
    a_hpf_br_q
)
```

## Explicação

O denominador do sistema paralelo é obtido pelo produto dos denominadores individuais.

---

# Numerador

```python
num1_q =
np.convolve(
    b_lpf_br_q,
    a_hpf_br_q
)

num2_q =
np.convolve(
    b_hpf_br_q,
    a_lpf_br_q
)
```

## Explicação

Cada ramo é convertido para um denominador comum.

---

# Soma dos Ramos

```python
b_parallel_blocks_q
```

## Explicação

Os dois ramos são somados para formar o numerador final do filtro rejeita-faixas.

---

# Resposta em Frequência

```python
signal.freqz(
    b_parallel_blocks_q,
    a_parallel_blocks_q
)
```

## Explicação

A resposta em frequência do filtro quantizado é comparada com a resposta do filtro original.

---

# Polos e Zeros

```python
zeros_q_blocks
poles_q_blocks
```

## Explicação

São calculadas as novas posições dos polos e zeros após a quantização dos blocos.

---

# Comparação Visual

Nos diagramas:

- azul → filtro original;
- vermelho → filtro quantizado.

---

# Interpretação dos Resultados do Filtro Rejeita-Faixas

A quantização dos blocos geralmente apresenta:

- menor deslocamento dos polos;
- menor deslocamento dos zeros;
- melhor preservação da banda rejeitada;
- maior estabilidade numérica.

Os benefícios tornam-se mais evidentes para:

```python
2 bits
```

e

```python
4 bits
```

onde os erros de quantização são mais significativos.

---

# Comparação com a Questão 4(a)

Na quantização direta do filtro completo:

- os erros afetam coeficientes de ordem mais alta;
- os deslocamentos de polos podem ser maiores.

Na quantização por blocos:

- cada estágio preserva melhor suas características;
- os erros ficam distribuídos;
- a implementação costuma ser mais robusta.

---

# Influência do Número de Bits

Poucos bits:

- maior erro de arredondamento;
- maior distorção espectral;
- maior deslocamento dos polos e zeros.

Muitos bits:

- menor erro de quantização;
- resposta próxima da ideal;
- estabilidade preservada.

---

# Resultado Final

Ao executar o código, obtém-se:

- quantização individual dos blocos componentes;
- reconstrução dos filtros após a quantização;
- comparação entre filtros originais e quantizados;
- análise das respostas em frequência;
- análise dos diagramas de polos e zeros;
- comparação entre quantização por blocos e quantização do filtro completo.

---

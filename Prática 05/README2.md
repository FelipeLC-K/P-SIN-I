<p align="center">
  <img src="assets5/bannercefet.png" width="100%">
</p>

<h1 align="center">PROSIN</h1>

<p align="center">
  
  Prática 05
  
</p>

# PROSIN — Processamento de Sinais


- **Professor:** Rafael da Silva Chaves
- **Instituição:** Centro Federal de Educação Tecnológica Celso Suckow da Fonseca- CEFET/RJ
- **Dupla:** Lucas de Farias dos Santos e Luís Felipe Chaves de Oliveira
- **Semestre:** 2026.1

# Prática 5 — Transformadas de  e Wavelets  

---

# Questão 1

# Importação das Bibliotecas

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.linalg import hadamard
```

## Explicação

As bibliotecas utilizadas foram:

- `numpy` → operações matemáticas e vetoriais;
- `matplotlib.pyplot` → geração de gráficos;
- `scipy.linalg.hadamard` → geração da matriz de Hadamard.

---

# Definição dos Parâmetros

```python
N = 8
```

## Explicação

O parâmetro `N` define:

- comprimento das mensagens;
- tamanho da matriz de Hadamard;
- tamanho dos códigos ortogonais.

Neste caso:

```python
N = 8
```

---

# a) Inicialização da Semente Aleatória

```python
np.random.seed(42)
```

## Explicação

A função:

```python
np.random.seed()
```

garante reprodutibilidade dos resultados aleatórios.

Assim, as mesmas mensagens serão geradas sempre que o código for executado.

---

# Função para Cálculo do Espectro

```python
def get_spectrum(signal):

    spec = np.abs(np.fft.fft(signal))

    return np.fft.fftshift(spec)
```

## Explicação

A função:

```python
np.fft.fft()
```

calcula a Transformada Rápida de Fourier (FFT) do sinal.

A magnitude do espectro é obtida utilizando:

```python
np.abs()
```

Já:

```python
np.fft.fftshift()
```

centraliza o espectro em torno da frequência zero.

---

# Geração do Vetor de Frequências

```python
freqs = np.fft.fftshift(
    np.fft.fftfreq(N)
)
```

## Explicação

O vetor `freqs` representa as frequências normalizadas utilizadas na plotagem do espectro.

---

# Geração das Mensagens Originais

```python
m1 = np.random.choice([-1, 1], size=N)

m2 = np.random.choice([-1, 1], size=N)

m3 = np.random.choice([-1, 1], size=N)
```

## Explicação

As mensagens são compostas por valores binários bipolares:

:contentReference[oaicite:0]{index=0}

Cada vetor representa os dados transmitidos por um usuário.

---

# Exibição das Mensagens

```python
print("m1:", m1)

print("m2:", m2)

print("m3:", m3)
```

## Explicação

As mensagens geradas são exibidas no terminal.

---

# 2) Geração da Matriz de Hadamard

```python
H = hadamard(N)
```

## Explicação

A matriz de Hadamard é composta por valores:

:contentReference[oaicite:1]{index=1}

e possui a propriedade de ortogonalidade entre suas linhas.

---

# Seleção dos Códigos Ortogonais

```python
c1 = H[1, :]

c2 = H[2, :]

c3 = H[3, :]
```

## Explicação

Cada linha da matriz representa um código ortogonal diferente.

Os códigos:

- `c1`
- `c2`
- `c3`

serão utilizados por usuários distintos no sistema CDMA.

---

# Conceito de Ortogonalidade

Dois códigos são ortogonais quando:

:contentReference[oaicite:2]{index=2}

para:

```python
i ≠ j
```

Isso permite que múltiplos usuários compartilhem o mesmo canal sem interferência significativa.

---

# 3) Codificação dos Sinais

## Espalhamento Espectral

```python
s1 = m1 * c1

s2 = m2 * c2

s3 = m3 * c3
```

## Explicação

Cada mensagem é multiplicada pelo seu respectivo código de Hadamard.

Esse processo realiza o espalhamento espectral (*spread spectrum*).

---

# Conceito Matemático

O sinal codificado é dado por:

:contentReference[oaicite:3]{index=3}

Onde:

- \(m[n]\) → mensagem original;
- \(c[n]\) → código de Hadamard;
- \(s[n]\) → sinal espalhado.

---

# Organização dos Sinais

```python
signals_orig = [m1, m2, m3]

signals_coded = [s1, s2, s3]
```

## Explicação

As listas armazenam:

- sinais originais;
- sinais codificados.

---

# Plotagem dos Espectros

```python
fig, axes = plt.subplots(
    3,
    2,
    figsize=(12, 12)
)
```

## Explicação

São criados gráficos para comparar:

- espectros originais;
- espectros codificados.

---

# Plotagem dos Espectros Originais

```python
axes[i, 0].stem(
    freqs,
    get_spectrum(signals_orig[i]),
    linefmt='b-',
    markerfmt='bo'
)
```

## Explicação

O gráfico apresenta o espectro do sinal original.

---

# Plotagem dos Espectros Codificados

```python
axes[i, 1].stem(
    freqs,
    get_spectrum(signals_coded[i]),
    linefmt='g-',
    markerfmt='go'
)
```

## Explicação

O gráfico apresenta o espectro do sinal espalhado.

Após a codificação, a energia do sinal torna-se distribuída em uma faixa maior de frequências.

---

# Resultado Esperado do Espalhamento

O espalhamento espectral provoca:

- aumento da largura de banda;
- distribuição espectral da energia;
- maior robustez contra interferências;
- possibilidade de múltiplos acessos simultâneos.

---

# Plotagem dos Sinais

```python
fig, axes = plt.subplots(
    3,
    3,
    figsize=(15, 10)
)
```

## Explicação

Os gráficos exibem:

- mensagem original;
- código de Hadamard;
- sinal codificado.

para cada usuário.

---

# Visualização dos Sinais

## Usuário 1

```python
axes[0,0].stem(range(N), m1)
axes[0,1].stem(range(N), c1)
axes[0,2].stem(range(N), s1)
```

---

## Usuário 2

```python
axes[1,0].stem(range(N), m2)
axes[1,1].stem(range(N), c2)
axes[1,2].stem(range(N), s2)
```

---

## Usuário 3

```python
axes[2,0].stem(range(N), m3)
axes[2,1].stem(range(N), c3)
axes[2,2].stem(range(N), s3)
```

## Explicação

Cada linha representa um usuário diferente do sistema CDMA.

---

# Ajustes dos Gráficos

```python
for ax in axes.flatten():

    ax.set_ylim([-1.5, 1.5])

    ax.grid(True)
```

## Explicação

Os limites verticais são ajustados para melhorar a visualização dos sinais binários bipolares.

---

# Resultado dos Gráficos

## Espectros dos Sinais

<p align="center">
  <img src="assets5/Q1P5.png" width="900">
</p>

---

## Mensagens, Códigos e Sinais Codificados

<p align="center">
  <img src="assets5/doisQ1P5.png" width="900">
</p>

---

# b) Adição de Ruído AWGN ao Sistema CDMA

Nesta etapa, foi analisado o comportamento do sistema CDMA na presença de ruído branco gaussiano aditivo (*AWGN — Additive White Gaussian Noise*).

O objetivo é observar:

- degradação do sinal recebido;
- alteração do espectro;
- impacto da variância do ruído.

---

# Importação das Bibliotecas

```python
import numpy as np
import matplotlib.pyplot as plt
```

## Explicação

As bibliotecas utilizadas foram:

- `numpy` → operações matemáticas;
- `matplotlib.pyplot` → geração de gráficos.

---

# Definição das Variâncias do Ruído

```python
sigmas_sq = [1e-3, 1e-2, 1e-1, 1]
```

## Explicação

Foram utilizados diferentes níveis de potência de ruído:

- \(10^{-3}\)
- \(10^{-2}\)
- \(10^{-1}\)
- \(1\)

A variância controla a intensidade do ruído AWGN.

---

# Sinal Composto

```python
x_total = s1 + s2 + s3
```

## Explicação

O sinal transmitido total é formado pela soma dos sinais codificados dos usuários:

:contentReference[oaicite:0]{index=0}

Esse processo representa a multiplexação CDMA.

---

# Criação da Figura

```python
fig, axes = plt.subplots(
    len(sigmas_sq),
    2,
    figsize=(14, 16)
)
```

## Explicação

A figura contém:

- gráficos do sinal no tempo;
- gráficos do espectro;

para cada nível de ruído.

---

# Laço de Simulação

```python
for i, sigma2 in enumerate(sigmas_sq):
```

## Explicação

O código percorre cada valor de variância definido anteriormente.

---

# Geração do Ruído AWGN

```python
noise = np.random.normal(
    0,
    np.sqrt(sigma2),
    N
)
```

## Explicação

O ruído branco gaussiano é gerado com:

- média igual a zero;
- desvio padrão:

:contentReference[oaicite:1]{index=1}

---

# Conceito do Ruído AWGN

O sinal recebido pode ser representado por:

:contentReference[oaicite:2]{index=2}

Onde:

- \(x[n]\) → sinal transmitido;
- \(w[n]\) → ruído AWGN;
- \(y[n]\) → sinal recebido.

---

# Sinal Recebido

```python
y_n = x_total + noise
```

## Explicação

O ruído é somado ao sinal transmitido para simular um canal de comunicação real.

---

# Plotagem do Sinal no Tempo

```python
axes[i, 0].step(
    range(N),
    y_n,
    where='post',
    color='brown'
)
```

## Explicação

O gráfico apresenta o sinal recebido no domínio do tempo.

A função:

```python
step()
```

é utilizada para representar sinais discretos.

---

# Configuração do Gráfico Temporal

```python
axes[i, 0].set_title(
    f'Sinal Recebido y[n] (σ² = {sigma2})'
)

axes[i, 0].set_ylabel('Amplitude')

axes[i, 0].grid(True)
```

## Explicação

Cada gráfico apresenta:

- amplitude do sinal;
- valor da variância utilizada;
- grade auxiliar para visualização.

---

# Cálculo do Espectro

```python
y_spec = np.abs(np.fft.fft(y_n))
```

## Explicação

A FFT é aplicada ao sinal recebido para análise espectral.

A magnitude do espectro é obtida utilizando:

```python
np.abs()
```

---

# Centralização do Espectro

```python
y_spec_shifted = np.fft.fftshift(y_spec)
```

## Explicação

A função:

```python
fftshift()
```

reposiciona a frequência zero no centro do espectro.

---

# Plotagem do Espectro

```python
axes[i, 1].stem(
    freqs,
    y_spec_shifted,
    linefmt='r-',
    markerfmt='ro'
)
```

## Explicação

O gráfico apresenta o espectro do sinal recebido para cada nível de ruído.

---

# Configuração do Espectro

```python
axes[i, 1].set_title(
    f'Espectro de y[n] (σ² = {sigma2})'
)

axes[i, 1].grid(True)
```

## Explicação

O espectro permite visualizar o aumento da energia espalhada devido ao ruído.

---

# Configuração dos Eixos

```python
axes[-1, 0].set_xlabel('n (Amostras)')

axes[-1, 1].set_xlabel('Frequência Normalizada')
```

## Explicação

Os eixos horizontais representam:

- amostras discretas;
- frequência normalizada.

---

# Ajuste Final da Figura

```python
plt.tight_layout()

plt.show()
```

## Explicação

O comando:

```python
tight_layout()
```

organiza automaticamente os gráficos na janela.

---

# Efeito do Ruído no Sistema

À medida que a variância aumenta:

- o sinal torna-se mais distorcido;
- o espectro fica mais espalhado;
- aumenta a interferência no sistema;
- reduz-se a qualidade da recepção.

---

# Resultado dos Gráficos

## Sinais Recebidos e Espectros

<p align="center">
  <img src="assets5/Q2P5.png" width="950">
</p>

---

# d) Probabilidade de Erro de Bit (BER) em Sistema CDMA

Nesta etapa, foi analisado o desempenho do sistema CDMA na presença de ruído AWGN utilizando simulação de Monte Carlo.

O objetivo é calcular:

- probabilidade de erro de bit (BER);
- impacto da variância do ruído;
- desempenho dos usuários no sistema CDMA.

---

# Importação das Bibliotecas

```python
import numpy as np
import matplotlib.pyplot as plt
```

## Explicação

As bibliotecas utilizadas foram:

- `numpy` → operações matemáticas e vetoriais;
- `matplotlib.pyplot` → geração de gráficos.

---

# Reutilização dos Códigos de Hadamard

```python
codes = [c1, c2, c3]
```

## Explicação

Os códigos ortogonais gerados anteriormente são armazenados em uma lista para facilitar o processamento dos usuários.

---

# Número de Usuários

```python
num_users = len(codes)
```

## Explicação

O número total de usuários é definido automaticamente pela quantidade de códigos disponíveis.

---

# Variâncias do Ruído

```python
sigmas_sq = [1e-3, 1e-2, 1e-1, 1]
```

## Explicação

Foram utilizados diferentes níveis de ruído AWGN:

- \(10^{-3}\)
- \(10^{-2}\)
- \(10^{-1}\)
- \(1\)

A variância controla a potência do ruído.

---

# Número de Ensaios de Monte Carlo

```python
num_trials = 10000
```

## Explicação

A simulação utiliza:

```python
10000
```

ensaios independentes para estimar a BER com maior precisão estatística.

---

# Inicialização da Matriz de BER

```python
error_probabilities = np.zeros(
    (num_users, len(sigmas_sq))
)
```

## Explicação

A matriz armazena as probabilidades de erro de cada usuário para cada valor de variância.

---

# Laço das Variâncias

```python
for s_idx, sigma2 in enumerate(sigmas_sq):
```

## Explicação

O sistema é testado para cada nível de ruído definido anteriormente.

---

# Inicialização dos Erros

```python
total_errors_per_user = np.zeros(num_users)
```

## Explicação

O vetor contabiliza a quantidade total de erros de cada usuário.

---

# Simulação Monte Carlo

```python
for trial in range(num_trials):
```

## Explicação

Cada iteração representa uma transmissão independente do sistema CDMA.

---

# Geração dos Bits dos Usuários

```python
m_bits = np.random.choice(
    [-1, 1],
    size=num_users
)
```

## Explicação

Cada usuário transmite um único bit bipolar:

:contentReference[oaicite:0]{index=0}

---

# Espalhamento dos Bits

```python
s_spread_signals.append(
    m_bits[u_idx] * codes[u_idx]
)
```

## Explicação

Cada bit é multiplicado pelo código de Hadamard correspondente.

---

# Conceito Matemático do Espalhamento

O sinal transmitido é dado por:

:contentReference[oaicite:1]{index=1}

Onde:

- \(m[n]\) → bit transmitido;
- \(c[n]\) → código ortogonal;
- \(s[n]\) → sinal espalhado.

---

# Soma dos Usuários

```python
x_total = np.sum(
    s_spread_signals,
    axis=0
)
```

## Explicação

Os sinais espalhados de todos os usuários são somados no mesmo canal.

---

# Geração do Ruído AWGN

```python
noise = np.random.normal(
    0,
    np.sqrt(sigma2),
    N
)
```

## Explicação

O ruído branco gaussiano possui:

- média zero;
- variância:

:contentReference[oaicite:2]{index=2}

---

# Sinal Recebido

```python
y_n = x_total + noise
```

## Explicação

O sinal recebido é modelado por:

:contentReference[oaicite:3]{index=3}

Onde:

- \(x[n]\) → sinal transmitido;
- \(w[n]\) → ruído;
- \(y[n]\) → sinal recebido.

---

# Desespalhamento do Sinal

```python
recovered_bit_estimate =
(1/N) * np.dot(
    codes[u_idx],
    y_n
)
```

## Explicação

O receptor correlaciona o sinal recebido com o código do usuário desejado.

---

# Conceito do Correlator CDMA

A recuperação do bit é dada por:

:contentReference[oaicite:4]{index=4}

Graças à ortogonalidade dos códigos, os demais usuários são cancelados.

---

# Decisão do Bit

```python
recovered_message_bit =
np.sign(recovered_bit_estimate)
```

## Explicação

A decisão é feita utilizando limiar zero:

- positivo → bit +1;
- negativo → bit -1.

---

# Contagem de Erros

```python
if recovered_message_bit != m_bits[u_idx]:

    total_errors_per_user[u_idx] += 1
```

## Explicação

O erro é contabilizado quando o bit recuperado difere do bit transmitido.

---

# Cálculo da BER

```python
error_probabilities[u_idx, s_idx] =
total_errors_per_user[u_idx] / num_trials
```

## Explicação

A BER é calculada por:

:contentReference[oaicite:5]{index=5}

---

# Exibição das Probabilidades de Erro

```python
print(
    f'Usuário {u_idx+1}:'
)
```

## Explicação

Os resultados são exibidos para cada usuário e para cada valor de variância.

---

# Plotagem da BER

```python
plt.figure(figsize=(10, 6))
```

## Explicação

A figura é criada para exibir a curva BER × variância do ruído.

---

# Plotagem das Curvas

```python
plt.semilogy(
    error_probabilities[u_idx, :],
    marker='o',
    label=f'Usuário {u_idx+1}'
)
```

## Explicação

O gráfico utiliza escala logarítmica no eixo Y.

A função:

```python
semilogy()
```

é adequada para análise de BER.

---

# Configuração do Gráfico

```python
plt.title(
    'Probabilidade de Erro de Bit (BER)'
)

plt.xlabel(
    'Variância do Ruído'
)

plt.ylabel(
    'BER'
)
```

## Explicação

O gráfico apresenta:

- BER dos usuários;
- comportamento em diferentes níveis de ruído.

---

# Grade do Gráfico

```python
plt.grid(
    True,
    which="both",
    ls="--"
)
```

## Explicação

A grade facilita a visualização em escala logarítmica.

---

# Configuração dos Ticks

```python
plt.xticks(
    range(len(sigmas_sq)),
    [str(s) for s in sigmas_sq]
)
```

## Explicação

Os valores do eixo X são configurados para exibir as variâncias utilizadas.

---

# Configuração do Eixo Y

```python
y_ticks = [
    1e-4,
    1e-3,
    1e-2,
    1e-1,
    0.5,
    1e0
]
```

## Explicação

Os ticks do eixo Y são ajustados manualmente para melhor visualização da BER.

---

# Resultado Esperado

À medida que a variância do ruído aumenta:

- a BER cresce;
- o desempenho do sistema piora;
- aumenta a probabilidade de erro de detecção.

---

# Resultado do Gráfico

<p align="center">
  <img src="assets5/Q2P5.png" width="850">
</p>

---

# Resultado Final

Ao executar o código, são obtidos:

- simulação Monte Carlo do sistema CDMA;
- recuperação dos bits transmitidos;
- cálculo da BER;
- análise do impacto do ruído AWGN;
- comparação de desempenho entre os usuários.

---

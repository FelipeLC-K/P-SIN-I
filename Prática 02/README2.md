<p align="center">
  <img src="assets2/bannercefet.png" width="100%">
</p>

<h1 align="center">PROSIN</h1>

<p align="center">
  
  Prática 02
  
</p>

# PROSIN — Processamento de Sinais


- **Professor:** Rafael da Silva Chaves
- **Instituição:** Centro Federal de Educação Tecnológica Celso Suckow da Fonseca- CEFET/RJ
- **Dupla:** Lucas de Farias dos Santos e Luís Felipe Chaves de Oliveira
- **Semestre:** 2026.1

# Prática 2 — Amostragem

# Bibliotecas Utilizadas

```python
import numpy as np
from scipy.fft import fft, fftfreq
import matplotlib.pyplot as plt
from IPython.display import Audio, display
from scipy.signal import chirp, resample_poly
from scipy.io import wavfile
import pandas as pd
```

## Explicação

* `numpy as np` → utilizado para cálculos numéricos, operações matemáticas vetoriais e manipulação de arrays.
* `scipy.fft.fft` → utilizado para calcular a Transformada Rápida de Fourier (FFT), permitindo analisar o conteúdo espectral dos sinais.
* `scipy.fft.fftfreq` → utilizado para gerar o eixo de frequências correspondente à FFT.
* `matplotlib.pyplot as plt` → utilizado para geração e visualização de gráficos.
* `IPython.display.Audio` → utilizado para reproduzir sinais de áudio diretamente no notebook.
* `IPython.display.display` → utilizado para exibir elementos multimídia, como players de áudio.
* `scipy.signal.chirp` → utilizado para gerar sinais chirp com frequência variável ao longo do tempo.
* `scipy.signal.resample_poly` → utilizado para realizar reamostragem de sinais, alterando a frequência de amostragem.
* `scipy.io.wavfile` → utilizado para leitura e manipulação de arquivos de áudio no formato `.wav`.
* `pandas as pd` → utilizado para manipulação e análise de dados em tabelas e estruturas organizadas.
---

# Questão 1

# Função para Cálculo do Espectro

```python
def calculate_spectrum(signal, sampling_frequency, single_sided=True):

    N = len(signal)
    T = 1.0 / sampling_frequency

    yf = fft(signal)

    if single_sided:
        xf = fftfreq(N, T)[:N//2]
        amplitudes = 1.0/N * np.abs(yf[0:N//2])

    else:
        xf = fftfreq(N, T)
        amplitudes = 1.0/N * np.abs(yf)

    return xf, amplitudes
```

## Explicação

Essa função calcula o espectro de frequência de um sinal utilizando a FFT (*Fast Fourier Transform*).

### Parâmetros da Função

- `signal` → sinal no domínio do tempo.
- `sampling_frequency` → frequência de amostragem do sinal.
- `single_sided` → define se será utilizado espectro unilateral.

---

# Número de Amostras

```python
N = len(signal)
```

## Explicação

Obtém a quantidade total de amostras do sinal.

---

# Espaçamento Temporal

```python
T = 1.0 / sampling_frequency
```

## Explicação

Calcula o intervalo entre amostras do sinal.

---

# Aplicação da FFT

```python
yf = fft(signal)
```

## Explicação

A FFT converte o sinal do domínio do tempo para o domínio da frequência.

---

# Cálculo do Eixo de Frequências

```python
xf = fftfreq(N, T)
```

## Explicação

A função `fftfreq()` gera o eixo correspondente às frequências presentes no espectro.

---

# Cálculo da Amplitude

```python
amplitudes = 1.0/N * np.abs(yf)
```

## Explicação

Calcula a magnitude das componentes espectrais do sinal.

---

# Definição dos Parâmetros do Sinal

```python
duracaox = 5
freq_amostragem = 44100
```

## Explicação

- `duracaox` → duração do sinal em segundos.
- `freq_amostragem` → frequência de amostragem utilizada.

A frequência de 44100 Hz é padrão em aplicações de áudio digital.

---

# Criação do Eixo Temporal

```python
t = np.linspace(
    0,
    duracaox,
    int(duracaox * freq_amostragem),
    endpoint=False
)
```

## Explicação

Cria o vetor temporal utilizado para gerar os sinais cossenoidais.

---

# Frequências Utilizadas

```python
frequencias = [500, 5000, 10000, 50000]
```

## Explicação

Foram gerados sinais com diferentes frequências para análise espectral:

- 500 Hz
- 5000 Hz
- 10000 Hz
- 50000 Hz

---

# Geração dos Sinais Cossenoidais

```python
cossine_waves = []

for freq in frequencias:
    wave = np.cos(2 * np.pi * freq * t)
    cossine_waves.append(wave)
```

## Explicação

Os sinais são gerados utilizando:

$$
x(t)=cos(2\pi f t)
$$

Onde:

- `f` → frequência do sinal.
- `t` → tempo.

---

# Plotagem dos Espectros

```python
fig, axs = plt.subplots(
    len(frequencias),
    1,
    figsize=(12, 4 * len(frequencias))
)

fig.suptitle(
    'Espectro de Sinais Cosseno',
    y=1.02,
    fontsize=16
)

for i, wave in enumerate(cossine_waves):

    frequencies_spec, amplitudes_spec = calculate_spectrum(
        wave,
        freq_amostragem
    )

    ax = axs[i]

    ax.plot(
        frequencies_spec,
        amplitudes_spec
    )

    ax.set_title(
        f'Espectro de Cosseno com Frequência = {frequencias[i]} Hz'
    )

    ax.set_xlabel('Frequência (Hz)')
    ax.set_ylabel('Amplitude')

    ax.grid(True)

    ax.set_xlim(0, frequencias[i] * 2)

plt.tight_layout()

plt.show()
```

## Explicação

Os gráficos mostram o espectro de frequência dos sinais gerados.

Cada pico observado no espectro corresponde à frequência dominante do sinal cossenoidal.

O comando:

```python
ax.set_xlim(0, frequencias[i] * 2)
```

limita a visualização do eixo de frequência para destacar os picos espectrais.

---

# Resultado Esperado

Ao executar o código, são obtidos:

- espectros de frequência dos sinais;
- identificação das componentes espectrais;
- visualização da FFT dos sinais cossenoidais;
- comparação entre diferentes frequências.

---

# Resultado dos Gráficos

<p align="center">
  <img src="assets2/Q1P2.png" width="850">
</p>

---

# Questão 2

# Geração e Análise Espectral de Chirps

## Definição dos Parâmetros

```python
f0 = 500
f1 = 10000

fs = 44100

duracaox = 5
```

## Explicação

Os parâmetros utilizados definem as características dos sinais chirp:

- `f0` → frequência inicial do sinal.
- `f1` → frequência final do sinal.
- `fs` → frequência de amostragem.
- `duracaox` → duração do sinal em segundos.

Neste experimento:

- frequência inicial = 500 Hz
- frequência final = 10000 Hz
- frequência de amostragem = 44100 Hz

---

# Criação do Vetor Temporal

```python
t_chirp = np.linspace(
    0,
    duracaox,
    int(duracaox * fs),
    endpoint=False
)
```

## Explicação

O vetor `t_chirp` representa o eixo temporal utilizado na geração dos sinais chirp.

A função:

```python
np.linspace()
```

gera amostras igualmente espaçadas ao longo do tempo.

---

# Geração dos Chirps

```python
linear_chirp = chirp(
    t_chirp,
    f0=f0,
    f1=f1,
    t1=duracaox,
    method='linear'
)

quadratic_chirp = chirp(
    t_chirp,
    f0=f0,
    f1=f1,
    t1=duracaox,
    method='quadratic'
)

logarithmic_chirp = chirp(
    t_chirp,
    f0=f0,
    f1=f1,
    t1=duracaox,
    method='logarithmic'
)
```

## Explicação

Foram gerados três tipos diferentes de sinais chirp.

### Chirp Linear

A frequência aumenta linearmente ao longo do tempo.

### Chirp Quadrático

A frequência varia de forma quadrática.

### Chirp Logarítmico

A frequência varia exponencialmente.

---

# Organização dos Sinais

```python
chirps = {
    'Linear Chirp': linear_chirp,
    'Quadratic Chirp': quadratic_chirp,
    'Logarithmic Chirp': logarithmic_chirp
}
```

## Explicação

Os sinais foram armazenados em um dicionário para facilitar o processamento e a geração dos gráficos.

---

# Plotagem dos Espectros

```python
fig, axs = plt.subplots(
    len(chirps),
    1,
    figsize=(12, 4 * len(chirps))
)

fig.suptitle(
    'Espectro de Chirps (Linear, Quadrático, Logarítmico)',
    y=1.02,
    fontsize=16
)

for i, (name, signal) in enumerate(chirps.items()):

    frequencies_spec, amplitudes_spec = calculate_spectrum(
        signal,
        fs
    )

    ax = axs[i]

    ax.plot(
        frequencies_spec,
        amplitudes_spec
    )

    ax.set_title(f'Espectro do {name}')

    ax.set_xlabel('Frequência (Hz)')
    ax.set_ylabel('Amplitude')

    ax.grid(True)

    ax.set_xlim(0, f1 * 1.5)

plt.tight_layout()

plt.show()
```

## Explicação

O código calcula e exibe o espectro de frequência dos sinais chirp utilizando FFT.

Cada gráfico apresenta:

- eixo horizontal → frequência;
- eixo vertical → amplitude espectral.

O comando:

```python
ax.set_xlim(0, f1 * 1.5)
```

limita a visualização do espectro para destacar a faixa de frequências de interesse.

---

# Resultado Esperado

Ao executar o código, são obtidos:

- espectros de frequência dos sinais chirp;
- comparação entre chirps lineares, quadráticos e logarítmicos;
- visualização da distribuição espectral dos sinais;
- análise da variação de frequência ao longo do tempo.

---

# Resultado dos Gráficos

<p align="center">
  <img src="assets2/Q2P2.png" width="850">
</p>

---

---

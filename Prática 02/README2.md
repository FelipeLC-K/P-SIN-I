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

# Questão 3

# Análise Espectral do Arquivo `handel.wav`

## Leitura do Arquivo de Áudio

```python
fs_handel_original, data_handel_original = wavfile.read('/content/handel.wav')
```

## Explicação

O arquivo de áudio `handel.wav` é carregado utilizando:

```python
wavfile.read()
```

A função retorna:

- `fs_handel_original` → frequência de amostragem do áudio.
- `data_handel_original` → vetor contendo as amostras do sinal.

---

# Normalização do Áudio

```python
normalized_data_handel = data_handel_original / np.max(np.abs(data_handel_original))
```

## Explicação

O sinal é normalizado para que sua amplitude fique entre:

$$
-1 \leq x(t) \leq 1
$$

Isso evita:

- clipping;
- distorções;
- problemas de escala durante o processamento.

---

# Cálculo do Espectro

```python
frequencies_handel, amplitudes_handel = calculate_spectrum(
    normalized_data_handel,
    fs_handel_original
)
```

## Explicação

A função `calculate_spectrum()` aplica a FFT ao sinal de áudio.

O resultado obtido contém:

- `frequencies_handel` → eixo de frequências.
- `amplitudes_handel` → amplitudes espectrais.

---

# Plotagem do Espectro

```python
plt.figure(figsize=(12, 6))

plt.plot(
    frequencies_handel,
    amplitudes_handel
)

plt.title('Espectro do Sinal de Áudio Handel (Normalizado)')

plt.xlabel('Frequência (Hz)')
plt.ylabel('Amplitude')

plt.grid(True)

plt.xlim(0, fs_handel_original / 2)

plt.tight_layout()

plt.show()
```

## Explicação

O gráfico apresenta o conteúdo espectral do áudio.

Cada componente do espectro indica a presença de determinadas frequências no sinal musical.

O comando:

```python
plt.xlim(0, fs_handel_original / 2)
```

limita a visualização até a frequência de Nyquist:

:contentReference[oaicite:0]{index=0}

onde:

- \(f_N\) → frequência de Nyquist;
- \(f_s\) → frequência de amostragem.

---

# Resultado Esperado

Ao executar o código, são obtidos:

- espectro de frequência do áudio;
- identificação das componentes espectrais;
- análise do conteúdo harmônico do sinal musical.

---

# Resultado do Gráfico

<p align="center">
  <img src="assets2/Q3P2.png" width="850">
</p>

---

# Questão 4

# Subamostragem do Arquivo `handel.wav`

## a) Criação da Função de Subamostragem

```python
def downsample_signal(signal, M):

    return signal[::M]
```

## Explicação

A função `downsample_signal()` realiza a subamostragem de um sinal discreto utilizando um fator \(M\).

O comando:

```python
signal[::M]
```

seleciona apenas uma amostra a cada \(M\) amostras do sinal original.

---

# Conceito Matemático

A nova frequência de amostragem após a subamostragem é dada por:

:contentReference[oaicite:0]{index=0}

Onde:

- \(f_s\) → frequência de amostragem original;
- \(f_s'\) → nova frequência de amostragem;
- \(M\) → fator de subamostragem.

---

# Objetivos da Subamostragem

A técnica de subamostragem permite:

- reduzir quantidade de dados;
- diminuir custo computacional;
- estudar efeitos de aliasing;
- modificar resolução temporal do sinal.

---

# b) Definição dos Fatores de Subamostragem

```python
M_factors = [2, 4, 8]

downsampled_signals = {}

down_sampling_frequencies = {}
```

## Explicação

Foram utilizados três fatores de subamostragem:

- \(M = 2\)
- \(M = 4\)
- \(M = 8\)

Quanto maior o valor de \(M\), menor será a nova frequência de amostragem.

---

# Aplicação da Subamostragem

```python
for i, M in enumerate(M_factors):

    downsampled_signal = downsample_signal(
        normalized_data_handel,
        M
    )

    new_fs = fs_handel_original / M
```

## Explicação

Para cada fator \(M\):

- o sinal é subamostrado;
- a nova frequência de amostragem é calculada.

---

# Armazenamento dos Resultados

```python
downsampled_signals[f'M={M}'] = downsampled_signal

down_sampling_frequencies[f'M={M}'] = new_fs
```

## Explicação

Os sinais subamostrados e suas respectivas frequências de amostragem são armazenados em dicionários para facilitar o acesso posterior.

---

# Cálculo do Espectro

```python
frequencies_ds, amplitudes_ds = calculate_spectrum(
    downsampled_signal,
    new_fs
)
```

## Explicação

A FFT é aplicada aos sinais subamostrados para analisar os efeitos da redução da frequência de amostragem no espectro.

---

# Plotagem dos Espectros

```python
ax = plt.subplot(len(M_factors), 1, i + 1)

ax.plot(
    frequencies_ds,
    amplitudes_ds
)

ax.set_title(
    f'Espectro do Sinal de Handel Subamostrado (M={M}, Fs={new_fs:.2f} Hz)'
)

ax.set_xlabel('Frequência (Hz)')
ax.set_ylabel('Amplitude')

ax.grid(True)

ax.set_xlim(0, new_fs / 2)
```

## Explicação

Os gráficos apresentam os espectros dos sinais subamostrados.

O comando:

```python
ax.set_xlim(0, new_fs / 2)
```

limita o eixo de frequência até a nova frequência de Nyquist:

:contentReference[oaicite:1]{index=1}

---

# Resultado Esperado dos Espectros

Ao executar o código, observa-se:

- redução da largura espectral;
- alteração da resolução temporal;
- possíveis efeitos de aliasing;
- mudança da frequência máxima representável.

---

# Resultado dos Gráficos

<p align="center">
  <img src="assets2/Q4P2.png" width="850">
</p>

---

# c) Reprodução dos Sinais Subamostrados

```python
print("Sinal Original:")

display(
    Audio(
        data=normalized_data_handel,
        rate=fs_handel_original
    )
)

for M in M_factors:

    signal = downsampled_signals[f'M={M}']

    fs_ds = down_sampling_frequencies[f'M={M}']

    print(f"Sinal Subamostrado M={M} (Fs={fs_ds:.2f} Hz):")

    display(
        Audio(
            data=signal,
            rate=fs_ds
        )
    )
```

## Explicação

Os sinais subamostrados são reproduzidos para comparação auditiva com o sinal original.

A redução da frequência de amostragem pode causar:

- perda de qualidade;
- redução da faixa espectral;
- distorções;
- aliasing.

---

# Áudios Gerados

## Sinal Original

[▶ Handel Original](assets2/handel_original.wav)

---

## Subamostragem M = 2

[▶ Handel M=2](assets2/handel_M2.wav)

---

## Subamostragem M = 4

[▶ Handel M=4](assets2/handel_M4.wav)

---

## Subamostragem M = 8

[▶ Handel M=8](assets2/handel_M8.wav)

---

# Resultado Final

Ao executar o código, são obtidos:

- sinais subamostrados;
- espectros de frequência correspondentes;
- comparação auditiva entre diferentes taxas de amostragem;
- análise dos efeitos da subamostragem no domínio da frequência.

---

<p align="center">
  <img src="assets/bannercefet.png" width="100%">
</p>

<h1 align="center">PROSIN</h1>

<p align="center">
  
  Prática 01
  
</p>

# PROSIN — Processamento de Sinais


- **Professor:** Rafael da Silva Chaves
- **Instituição:** Centro Federal de Educação Tecnológica Celso Suckow da Fonseca- CEFET/RJ
- **Dupla:** Lucas de Farias dos Santos e Luís Felipe Chaves de Oliveira
- **Semestre:** 2026.1

# Prática 1 — Geração e Análise de Sinais

## Objetivo

Esta prática tem como objetivo gerar sinais cossenoidais utilizando Python, visualizar seus gráficos e reproduzir os sinais em áudio. Além disso, são gerados sinais do tipo *chirp*, amplamente utilizados em sistemas de telecomunicações e processamento digital de sinais.

---

# Bibliotecas Utilizadas

```python
import numpy
import matplotlib.pyplot
from IPython.display import Audio, display
from scipy.signal import chirp, resample_poly
from scipy.io import wavfile
import pandas as pd
```

## Explicação

* `numpy` → utilizado para cálculos numéricos e manipulação vetorial.
* `matplotlib.pyplot` → utilizado para geração dos gráficos.
* `IPython.display.Audio` → utilizado para reproduzir os sinais em áudio.
* `scipy.signal.chirp` → utilizado para gerar sinais chirp.
* `wavfile` → utilizado para manipulação de arquivos `.wav`.
* `pandas` → utilizado para manipulação de dados.

---

# 1) Definição dos Parâmetros

```python
duracaox = 5
freq_amostragem = 44100
```

## Explicação

* `duracaox` define a duração do sinal em segundos.
* `freq_amostragem` define a frequência de amostragem do sinal.

A frequência de 44100 Hz é amplamente utilizada em aplicações de áudio digital.

---

Criação do Eixo do Tempo

```python
t = numpy.linspace(0, duracaox, int(duracaox * freq_amostragem), endpoint=False)
```

## Explicação

O vetor `t` representa o eixo temporal utilizado para gerar os sinais.

A função `linspace()` cria amostras igualmente espaçadas no intervalo especificado.

---

Geração das Frequências

```python
frequencias = [500, 5000, 10000]
```

## Explicação

Foram escolhidas três frequências diferentes para comparação:

* 500 Hz
* 5000 Hz
* 10000 Hz

---

Geração das Ondas Cossenoidais

```python
cossine_waves = []

for freq in frequencias:
  wave = numpy.cos(2 * numpy.pi * freq * t)
  cossine_waves.append(wave)
```

## Explicação

A equação utilizada para gerar os sinais é:

$$
x(t) = cos(2\pi f t)
$$

Onde:

* `f` representa a frequência do sinal.
* `t` representa o tempo.

Cada onda gerada é armazenada na lista `cossine_waves`.

---

Plotagem dos Gráficos

```python
for i, wave in enumerate(cossine_waves) :
  matplotlib.pyplot.figure(figsize=(12, 4))
  matplotlib.pyplot.plot(t, wave)
  matplotlib.pyplot.title(f'Sinal Cosseno com Frequência = {frequencias[i]} Hz')
  matplotlib.pyplot.xlabel('Tempo (s)')
  matplotlib.pyplot.ylabel('Amplitude')
  matplotlib.pyplot.grid(True)
  matplotlib.pyplot.xlim(0, 0.01)
  matplotlib.pyplot.show()
```
<p align="center">
  <img src="assets/Q1P1.png" width="700">
</p>

## Explicação

Os gráficos mostram o comportamento temporal dos sinais gerados.

O comando:

```python
matplotlib.pyplot.xlim(0, 0.01)
```

limita a visualização para os primeiros milissegundos do sinal, facilitando a observação dos ciclos da onda.

---

# 6) Reprodução dos Áudios

```python
for i, wave in enumerate(cossine_waves):
    normalized_wave = wave / numpy.max(numpy.abs(wave))
    print(f"Audios da onda de {frequencias[i]} Hz...")
    display(Audio(data=normalized_wave, rate=freq_amostragem))
```




















## Explicação

Os sinais são normalizados para evitar distorções de amplitude.

Em seguida, cada sinal é reproduzido utilizando o player de áudio do Jupyter Notebook.

---

# 7) Geração do Sinal Chirp

```python
linear_chirp = chirp(t, f0=f0, f1=f1, t1=duracaox, method='linear')
```

## Explicação

O sinal *chirp* é um sinal cuja frequência varia ao longo do tempo.

Neste caso:

* Frequência inicial: 500 Hz
* Frequência final: 10000 Hz

O método linear faz com que a frequência aumente linearmente durante o intervalo de tempo.

---

# Resultado Esperado

Ao executar o código, são obtidos:

* Gráficos das ondas cossenoidais.
* Reprodução sonora dos sinais.
* Geração do sinal chirp.
* Comparação entre diferentes frequências.

---

# Tecnologias Utilizadas

* Python
* NumPy

print("Áudio taca_convoluido:")
display(Audio(data=taca_convoluido_normalizado, rate=fb))

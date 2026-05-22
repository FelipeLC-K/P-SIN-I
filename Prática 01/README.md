<p align="center">
  <img src="assets1/bannercefet.png" width="100%">
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
Questão 1 

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
  <img src="assets1/Q1P1.png" width="700">
</p>


# 6) Reprodução dos Áudios

```python
for i, wave in enumerate(cossine_waves):
    normalized_wave = wave / numpy.max(numpy.abs(wave))
    print(f"Audios da onda de {frequencias[i]} Hz...")
    display(Audio(data=normalized_wave, rate=freq_amostragem))
```

[▶ Áudio 500 Hz](assets1/500hz.wav)
[▶ Áudio 500 Hz](assets1/5000hz.wav)
[▶ Áudio 500 Hz](assets1/10000hz.wav)

---

# Questão 2

# 2) Definição dos Parâmetros Iniciais

```python
f0 = 500
f1 = 10000
```

## Explicação

Os parâmetros utilizados definem:

- `f0` → frequência inicial do sinal chirp.
- `f1` → frequência final do sinal chirp.

Neste experimento, o sinal varia de:

- 500 Hz
- até 10000 Hz

---

# 2A) Geração dos Sinais Chirp

```python
# Geração do sinal chirp linear
linear_chirp = chirp(t, f0=f0, f1=f1, t1=duracaox, method='linear')

# Geração do sinal chirp quadrático
quadratic_chirp = chirp(t, f0=f0, f1=f1, t1=duracaox, method='quadratic')

# Geração do sinal chirp logarítmico
logarithmic_chirp = chirp(t, f0=f0, f1=f1, t1=duracaox, method='logarithmic')
```

## Explicação

O sinal chirp é um sinal cuja frequência varia ao longo do tempo.

Foram gerados três tipos diferentes:

### Chirp Linear
A frequência cresce linearmente.

### Chirp Quadrático
A frequência varia segundo uma função quadrática.

### Chirp Logarítmico
A frequência cresce exponencialmente.

---

# Plotagem dos Gráficos

```python
matplotlib.pyplot.figure(figsize=(15, 10))

matplotlib.pyplot.subplot(3, 1, 1)
matplotlib.pyplot.plot(t, linear_chirp)
matplotlib.pyplot.title('Chirp Linear')
matplotlib.pyplot.xlabel('Tempo (s)')
matplotlib.pyplot.ylabel('Amplitude')
matplotlib.pyplot.xlim(0, 0.1)
matplotlib.pyplot.grid(True)

matplotlib.pyplot.subplot(3, 1, 2)
matplotlib.pyplot.plot(t, quadratic_chirp)
matplotlib.pyplot.title('Chirp Quadrático')
matplotlib.pyplot.xlabel('Tempo (s)')
matplotlib.pyplot.ylabel('Amplitude')
matplotlib.pyplot.xlim(0, 0.1)
matplotlib.pyplot.grid(True)

matplotlib.pyplot.subplot(3, 1, 3)
matplotlib.pyplot.plot(t, logarithmic_chirp)
matplotlib.pyplot.title('Chirp Logarítmico')
matplotlib.pyplot.xlabel('Tempo (s)')
matplotlib.pyplot.ylabel('Amplitude')
matplotlib.pyplot.xlim(0, 0.1)
matplotlib.pyplot.grid(True)

matplotlib.pyplot.tight_layout()
matplotlib.pyplot.show()
```

## Explicação

Os gráficos mostram a variação temporal dos sinais chirp.

O comando:

```python
matplotlib.pyplot.xlim(0, 0.1)
```

limita a visualização para facilitar a observação da variação de frequência.

---

# Resultado dos Gráficos

<p align="center">
  <img src="assets1/Q2P1.png" width="800">
</p>

---

# 2B) Reprodução dos Áudios

```python
print("Áudios dos chirps:")

print("Chirp Linear...")
display(Audio(data=linear_chirp, rate=freq_amostragem))

print("Chirp Quadrático...")
display(Audio(data=quadratic_chirp, rate=freq_amostragem))

print("Chirp Logarítmico...")
display(Audio(data=logarithmic_chirp, rate=freq_amostragem))
```

## Explicação

Os sinais gerados são reproduzidos em áudio para permitir a percepção da variação de frequência ao longo do tempo.

---

# Áudios dos Chirps

[▶ Chirp Linear](assets1/chirp_linear.wav)

[▶ Chirp Quadrático](assets1/chirp_quadratico.wav)

[▶ Chirp Logarítmico](assets1/chirp_logaritmico.wav)

---

# Resultado Esperado

Ao executar o código, são obtidos:

- Gráficos dos sinais chirp.
- Reprodução sonora dos sinais.
- Comparação entre diferentes métodos de variação de frequência.
- Visualização do comportamento temporal dos chirps.

---

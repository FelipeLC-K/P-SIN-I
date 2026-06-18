<p align="center">
  <img src="assets3/bannercefet.png" width="100%">
</p>

<p align="center">

</p>

# PROSIN I — Processamento de Sinais I


- **Professor:** Rafael da Silva Chaves
- **Instituição:** Centro Federal de Educação Tecnológica Celso Suckow da Fonseca - CEFET/RJ
- **Dupla:** Lucas de Farias dos Santos e Luís Felipe Chaves de Oliveira
- **Semestre:** 2026.1

# Prática 3 — Transformada Z


# Questão 1: Resposta em Frequência e Diagrama de Polos e Zeros de um Filtro FIR


# Função de Transferência

O sistema analisado é dado por:

\[
H(z)=1+0.49z^{-2}+0.2401z^{-6}-0.0576z^{-8}-0.0282z^{-10}-0.0138z^{-12}
\]

Como não existe denominador diferente de 1, trata-se de um filtro FIR (Finite Impulse Response).

---

# Definição dos Coeficientes

```python
b = np.zeros(13)

b[0]  = 1.0
b[2]  = 0.49
b[6]  = 0.2401
b[8]  = -0.0576
b[10] = -0.0282
b[12] = -0.0138

a = [1.0]
```

## Explicação

Foi criado o vetor dos coeficientes do numerador considerando todos os atrasos entre:

```text
z⁰ até z⁻¹²
```

Os coeficientes inexistentes recebem valor zero.

Como o sistema é FIR, o denominador é simplesmente:

```python
a = [1.0]
```

---

# Cálculo da Resposta em Frequência

```python
ws, h = signal.freqz(
    b,
    a,
    worN=8192
)
```

## Explicação

A função `freqz()` calcula a resposta em frequência do filtro.

---

# Resposta de Magnitude

```python
20*np.log10(abs(h))
```

## Explicação

A magnitude foi convertida para decibéis (dB)

---

# Resposta de Fase

```python
np.unwrap(
    np.angle(h)
)
```

## Explicação

A função `angle()` calcula a fase da resposta em frequência.

Em seguida, `unwrap()` elimina as descontinuidades de ±π, produzindo uma curva contínua e mais fácil de interpretar.

---

# Cálculo dos Polos e Zeros

```python
z, p, k =
signal.tf2zpk(
    b,
    a
)
```

# Diagrama de Polos e Zeros

```python
plt.plot(
    np.real(z),
    np.imag(z),
    'o'
)

plt.plot(
    np.real(p),
    np.imag(p),
    'x'
)
```

## Explicação

O diagrama representa:

- círculos → zeros;
- cruzes → polos.

Também é desenhado o círculo unitário, utilizado como referência para análise de estabilidade.

---

# Círculo Unitário

```python
circle =
plt.Circle(
    (0,0),
    1,
    linestyle='--'
)
```

---

-----------------------------------------------------------------------------------

# Questão 2: Aplicação do Filtro H(z) ao Sinal de Áudio


# Leitura do Arquivo de Áudio

```python
sampling_rate, audio_data =
wavfile.read('/content/handel.wav')
```

## Explicação

O arquivo `handel.wav` é carregado juntamente com sua frequência de amostragem.

Esses dados serão utilizados como entrada do sistema digital representado por \(H(z)\).

---

# Conversão para Mono

```python
if audio_data.ndim > 1:
    audio_data = audio_data[:,0]
```

## Explicação

Caso o áudio seja estéreo, apenas um dos canais é utilizado.

Dessa forma, o processamento é realizado sobre um único sinal unidimensional.

---

# Normalização do Áudio

```python
audio_data =
audio_data /
np.max(np.abs(audio_data))
```

## Explicação

O sinal é normalizado para que sua amplitude fique compreendida entre:

```text
-1 e 1
```

Isso evita saturação durante o processamento e facilita a comparação entre os sinais.

---

# Aplicação do Filtro

```python
filtered_audio =
signal.lfilter(
    b,
    a,
    audio_data
)
```

## Explicação

A função `lfilter()` aplica o filtro digital definido pelos coeficientes:

```python
b
```

e

```python
a
```

obtidos na Questão 1.

O resultado é o sinal filtrado, correspondente à saída do sistema para a entrada de áudio.

---

# Cálculo do Espectro do Sinal Original

```python
freq_original,
Pxx_den_original =
signal.welch(
    audio_data,
    sampling_rate,
    nperseg=1024
)
```

## Explicação

O método de Welch é utilizado para estimar a densidade espectral de potência do sinal original.

---

# Cálculo do Espectro do Sinal Filtrado

```python
freq_filtered,
Pxx_den_filtered =
signal.welch(
    filtered_audio,
    sampling_rate,
    nperseg=1024
)
```

## Explicação

O mesmo procedimento é aplicado ao sinal filtrado, permitindo comparar diretamente os efeitos do filtro sobre o espectro do áudio.

---

# Comparação dos Espectros

```python
plt.semilogy(...)
```

## Explicação

Os espectros de potência são apresentados no mesmo gráfico utilizando escala logarítmica no eixo vertical.

---

----------------------------------------------------------------------------------------------


# Questão 3: Projeto do Filtro Inverso e Recuperação do Sinal de Áudio

---

# Construção do Filtro Inverso

O filtro original possui a forma:

\[
H(z)=\frac{B(z)}{A(z)}
\]

Como o denominador do filtro original é:

```python
a = [1.0]
```

o filtro inverso é definido por:

\[
H_{inv}(z)=\frac{A(z)}{B(z)}
\]

---

# Definição dos Coeficientes

```python
b_inv = a
a_inv = b
```

## Explicação

Os coeficientes do numerador e do denominador são invertidos para formar o filtro inverso.

Dessa forma, o sistema tenta desfazer o efeito produzido pelo filtro aplicado anteriormente ao sinal de áudio.

---

# Resposta em Frequência

```python
ws_inv, h_inv =
signal.freqz(
    b_inv,
    a_inv,
    worN=8192
)
```

## Explicação

A resposta em frequência do filtro inverso é calculada utilizando 8192 pontos, permitindo observar sua magnitude e sua fase ao longo da faixa de frequências.

Idealmente, a resposta do filtro inverso é complementar à resposta do filtro original.

---

# Magnitude da Resposta

```python
20*np.log10(abs(h_inv))
```

## Explicação

O gráfico de magnitude mostra o ganho do filtro inverso em cada frequência.

As frequências que foram atenuadas pelo filtro original tendem a ser amplificadas pelo filtro inverso, buscando recuperar o conteúdo espectral perdido.

---

# Resposta de Fase

```python
np.unwrap(
    np.angle(h_inv)
)
```

## Explicação

A fase do filtro inverso é apresentada de forma contínua utilizando `unwrap()`, permitindo analisar o atraso introduzido pelo sistema em cada componente de frequência.

---

# Cálculo dos Polos e Zeros

```python
z_inv,
p_inv,
k_inv =
signal.tf2zpk(
    b_inv,
    a_inv
)
```

## Explicação

São calculadas as posições dos polos e dos zeros do filtro inverso.

Como o filtro original é FIR, seus zeros tornam-se polos no filtro inverso, enquanto os polos do filtro original tornam-se zeros.

---

# Diagrama de Polos e Zeros

```python
plt.plot(
    np.real(z_inv),
    np.imag(z_inv),
    'o'
)

plt.plot(
    np.real(p_inv),
    np.imag(p_inv),
    'x'
)
```

---

# Recuperação do Sinal

```python
recovered_audio =
signal.lfilter(
    b_inv,
    a_inv,
    filtered_audio
)
```

## Explicação

O filtro inverso é aplicado ao sinal obtido na Questão 2.

O objetivo é cancelar os efeitos introduzidos pelo filtro original e reconstruir um sinal o mais próximo possível do áudio inicial.

---

# Comparação dos Espectros

```python
signal.welch(...)
```

## Explicação

O espectro do sinal recuperado é calculado utilizando o método de Welch e comparado com o espectro do sinal original.

Quanto maior a sobreposição entre as curvas, melhor foi a recuperação realizada pelo filtro inverso.

---

# Avaliação pelo Erro Quadrático Médio

```python
mse =
np.mean(
(original_trimmed -
 recovered_trimmed)**2
)
```

## Explicação

O Erro Quadrático Médio (MSE) mede a diferença entre o sinal original e o sinal recuperado.

---

# Avaliação pela Relação Sinal-Ruído

```python
snr_db =
10*np.log10(
signal_power /
noise_power
)
```


---

# Resultado Final

----------------------------------------------------------------------------------------------------

# Questão 4: Análise da Resposta em Frequência e do Diagrama de Polos e Zeros

---

# Função de Transferência

O sistema analisado é dado por

```text
H(z) = (1 - z⁻ᴸ)/(1 - a z⁻ᴸ)
```

onde foram considerados:

```python
a = {0.7, 0.9}
```

e

```python
L = {1, 4, 10}
```

---

# Valores Testados

```python
valores_a = [0.7, 0.9]
valores_L = [1, 4, 10]
```

## Explicação

Foram analisadas seis combinações diferentes:

- a = 0.7, L = 1;
- a = 0.7, L = 4;
- a = 0.7, L = 10;
- a = 0.9, L = 1;
- a = 0.9, L = 4;
- a = 0.9, L = 10.

---

# Construção dos Coeficientes

## Numerador

```python
b_coeffs = np.zeros(L_val + 1)

b_coeffs[0] = 1

b_coeffs[L_val] = -1
```

## Explicação

O numerador representa o termo

```text
1 − z⁻ᴸ
```


---

## Denominador

```python
a_coeffs = np.zeros(L_val + 1)

a_coeffs[0] = 1

a_coeffs[L_val] = -a_val
```

---

# Resposta em Frequência

```python
ws, h = signal.freqz(
    b_coeffs,
    a_coeffs,
    worN=8192
)
```

## Explicação

A função `freqz()` calcula a resposta em frequência do sistema em 8192 pontos.

---

# Gráfico da Magnitude

```python
plt.plot(
    ws/np.pi,
    20*np.log10(abs(h))
)
```

---

# Gráfico da Fase

```python
plt.plot(
    ws/np.pi,
    np.unwrap(np.angle(h))
)
```

---

# Cálculo dos Polos e Zeros

```python
z, p, k = signal.tf2zpk(
    b_coeffs,
    a_coeffs
)
```

---

# Diagrama de Polos e Zeros

```python
plt.plot(
    np.real(z),
    np.imag(z),
    'o'
)

plt.plot(
    np.real(p),
    np.imag(p),
    'x'
)
```

---

# Resultado Final

--------------------------------------------------

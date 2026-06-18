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

# Resultado Final

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

# Resultado Final

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


# Questão 5: Aplicação dos Sistemas da Questão 4 ao Sinal de Áudio


# Carregamento do Áudio

```python
sampling_rate, audio_data =
wavfile.read('/content/handel.wav')
```

## Explicação

O arquivo de áudio é carregado utilizando a biblioteca `scipy.io.wavfile`.

Caso o áudio seja estéreo, apenas um canal é utilizado na análise.

---

# Normalização do Sinal

```python
audio_data =
audio_data /
np.max(np.abs(audio_data))
```

## Explicação

O sinal é normalizado para o intervalo entre -1 e 1.

Isso evita problemas numéricos durante a filtragem e facilita a comparação entre os sinais.

---

# Valores Testados

```python
valores_a = [0.7, 0.9]

valores_L = [1, 4, 10]
```

## Explicação

Foram avaliadas seis configurações do filtro:

- a = 0.7, L = 1;
- a = 0.7, L = 4;
- a = 0.7, L = 10;
- a = 0.9, L = 1;
- a = 0.9, L = 4;
- a = 0.9, L = 10.

---

# Construção do Filtro

## Numerador

```python
b_coeffs =
np.zeros(L_val + 1)

b_coeffs[0] = 1

b_coeffs[L_val] = -1
```

## Denominador

```python
a_coeffs =
np.zeros(L_val + 1)

a_coeffs[0] = 1

a_coeffs[L_val] = -a_val
```

## Explicação

Os coeficientes implementam a função de transferência

```text
H(z)=
(1-z⁻ᴸ)
/(1-az⁻ᴸ)
```

para cada combinação de parâmetros.

---

# Filtragem do Áudio

```python
filtered_audio_q4 =
signal.lfilter(
    b_coeffs,
    a_coeffs,
    audio_data
)
```

## Explicação

O filtro é aplicado diretamente ao sinal de áudio utilizando a função `lfilter()`, produzindo um novo sinal filtrado.

---

# Cálculo do Espectro

```python
freq_filtered_q4,
Pxx_den_filtered_q4 =
signal.welch(
    filtered_audio_q4,
    sampling_rate,
    nperseg=1024
)
```

## Explicação

O método de Welch é utilizado para estimar a densidade espectral de potência do sinal filtrado.

O mesmo procedimento já havia sido realizado para o sinal original, permitindo uma comparação direta.

---

# Comparação dos Espectros

```python
plt.semilogy(
    freq_original,
    Pxx_den_original
)

plt.semilogy(
    freq_filtered_q4,
    Pxx_den_filtered_q4
)
```

---

# Resultado Final

------------------------------------------------


# Questão 6: Recuperação do Sinal de Áudio Utilizando os Sistemas da Questão 4

---

# Sistemas Avaliados

Foram analisadas todas as combinações:

```python
a ∈ {0.7, 0.9}

L ∈ {1, 4, 10}
```

resultando em seis sistemas diferentes.

---

# Construção do Filtro Original

Para cada combinação são definidos os coeficientes do sistema

```python
H(z)=
(1-z^{-L})
/
(1-az^{-L})
```

através dos vetores:

```python
b_coeffs_q4
```

e

```python
a_coeffs_q4
```

---

# Construção do Filtro Inverso

O filtro utilizado para recuperar o áudio é

```python
Hinv(z)=
1/H(z)
```

implementado por

```python
b_inv_q6 = a_coeffs_q4
a_inv_q6 = b_coeffs_q4
```

## Explicação

O numerador do filtro inverso corresponde ao denominador do filtro original e vice-versa.

---

# Resposta em Frequência

```python
ws_inv_q6, h_inv_q6 =
signal.freqz(
    b_inv_q6,
    a_inv_q6,
    worN=8192
)
```

## Explicação

É calculada a resposta em frequência do filtro inverso para verificar como ele compensa os efeitos introduzidos pelo sistema original.

---

# Cálculo dos Polos e Zeros

```python
z_inv_q6, p_inv_q6, k_inv_q6 =
signal.tf2zpk(
    b_inv_q6,
    a_inv_q6
)
```

---

# Diagrama de Polos e Zeros

```python
plt.plot(...)
```

---

# Aplicação do Filtro Original

Antes da recuperação, o áudio original é novamente filtrado utilizando o sistema correspondente.

```python
filtered_audio_q4_current =
signal.lfilter(
    b_coeffs_q4,
    a_coeffs_q4,
    audio_data
)
```

## Explicação

Esse passo reproduz o efeito do sistema da Questão 4 sobre o sinal de áudio.

---

# Recuperação do Áudio

```python
recovered_audio_q6 =
signal.lfilter(
    b_inv_q6,
    a_inv_q6,
    filtered_audio_q4_current
)
```

## Explicação

O filtro inverso é aplicado ao áudio filtrado com o objetivo de reconstruir o sinal original.

---

# Cálculo do Espectro

```python
signal.welch(
    recovered_trimmed_q6,
    sampling_rate,
    nperseg=1024
)
```

## Explicação

É calculada a densidade espectral de potência do áudio recuperado para comparação com o espectro original.

---


# Avaliação pelo Erro Quadrático Médio (MSE)

```python
mse_q6 =
np.mean(
(original_trimmed_q6 -
 recovered_trimmed_q6)**2
)
```

## Explicação

O MSE mede a diferença média entre o sinal recuperado e o sinal original.

---

# Avaliação pela Relação Sinal-Ruído (SNR)

```python
snr_db_q6 =
10*np.log10(
signal_power_q6 /
noise_power_q6
)
```

## Explicação

A SNR indica quanto do sinal útil foi preservado em relação ao erro introduzido durante o processo de recuperação.

---


# Resultado Final

------------------------------


# Questão 7: Aproximação FIR dos Filtros Inversos

---

# Ordens FIR Utilizadas

Foram avaliadas duas aproximações:

```python
fir_order_low = 100
```

e

```python
fir_order_high = 250
```

## Explicação

Essas ordens permitem comparar o efeito do comprimento do filtro FIR na qualidade da recuperação do sinal.

---

# Filtro Inverso da Questão 3

O filtro inverso utilizado é

```python
Hinv(z)=
1/H(z)
```

cujos coeficientes são definidos por

```python
b_inv_q3 = a
a_inv_q3 = b
```

## Explicação

Como o filtro original é FIR, seu inverso corresponde à troca entre numerador e denominador da função de transferência.

---

# Recuperação do Áudio Filtrado

Caso o sinal filtrado ainda não exista na memória, ele é novamente calculado:

```python
filtered_audio =
signal.lfilter(
    b,
    a,
    audio_data
)
```

## Explicação

Esse sinal representa o áudio após passar pelo sistema da Questão 2 e será utilizado na recuperação.

---

# Obtenção da Resposta ao Impulso

Para construir a aproximação FIR é aplicada uma entrada impulso ao filtro inverso.

```python
impulse_input_q3_low
```

e

```python
signal.lfilter(
    b_inv_q3,
    a_inv_q3,
    impulse_input_q3_low
)
```

## Explicação

A resposta ao impulso obtida é truncada após um número finito de amostras, tornando possível representar o filtro inverso como um FIR.

---

# Aplicação da Aproximação FIR

```python
recovered_audio_fir_q3_low =
signal.lfilter(
    impulse_response_q3_low,
    [1.0],
    filtered_audio
)
```

## Explicação

A resposta ao impulso calculada passa a ser utilizada como os coeficientes do novo filtro FIR responsável pela recuperação do áudio.

---

# Função de Análise

Toda a avaliação dos resultados é realizada pela função

```python
plot_analysis()
```

Ela executa:

- cálculo do espectro;
- comparação entre espectros;
- cálculo do MSE;
- cálculo da SNR;
- plotagem da resposta em frequência do FIR.

---

# Aproximação FIR dos Sistemas da Questão 6

Também foi realizada a aproximação FIR para todos os filtros inversos construídos na Questão 6.

Foram analisadas as combinações:

```python
a ∈ {0.7, 0.9}
```

e

```python
L ∈ {1, 4, 10}
```

---

# Construção dos Sistemas

Para cada combinação são definidos:

```python
b_coeffs_q4
```

e

```python
a_coeffs_q4
```

representando o filtro original

```python
H(z)
```

---

# Construção do Filtro Inverso

O filtro inverso é obtido por

```python
b_inv_q6 = a_coeffs_q4

a_inv_q6 = b_coeffs_q4
```

## Explicação

Os coeficientes do numerador e denominador são invertidos para implementar

```python
Hinv(z)
```

---

# Resposta ao Impulso do Filtro Inverso

A resposta ao impulso é calculada por

```python
signal.lfilter(
    b_inv_q6,
    a_inv_q6,
    impulse_input_q6_low
)
```

e

```python
signal.lfilter(
    b_inv_q6,
    a_inv_q6,
    impulse_input_q6_high
)
```

## Explicação

São obtidas aproximações FIR para duas ordens diferentes.

---

# Recuperação do Áudio

O sinal filtrado é recuperado utilizando:

```python
signal.lfilter(
    impulse_response_q6_low,
    [1.0],
    filtered_audio_q4_current
)
```

e

```python
signal.lfilter(
    impulse_response_q6_high,
    [1.0],
    filtered_audio_q4_current
)
```

## Explicação

A resposta ao impulso calculada passa a atuar diretamente como um filtro FIR.

---

# Avaliação pelo MSE

```python
mse =
np.mean(
(original_trimmed -
 recovered_trimmed)**2
)
```

---

# Avaliação pela SNR

```python
snr_db =
10*np.log10(
signal_power /
noise_power
)
```

---

# Resultado Final

----------------------------------------



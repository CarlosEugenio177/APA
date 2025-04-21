# 📚 Guia‑de‑Bolso — Projeto & Análise de Algoritmos

> **Objetivo**  
> Reconhecer rapidamente o padrão das questões mais comuns em provas de Análise de
> Algoritmos (Big‑O, comparação de funções, escolha de constantes, análise de laços)
> e aplicar o passo‑a‑passo correto sem perder ponto.

---

## 1. Comparar dois algoritmos e descobrir “a partir de que *n*”

| Passo | O que fazer | Por quê |
| :---: | ----------- | ------ |
| 1 | Escreva a desigualdade correta<br>`T_A(n) < T_B(n)` ou vice‑versa | Evita inverter sinal |
| 2 | Traga tudo p/ um lado<br>`f(n) = T_A − T_B < 0` | Vira problema de sinal |
| 3 | Tire fator comum / divida por `nᵏ` (positivo) | Simplifica o grau |
| 4 | • Sobrou **linear** → isola `n`<br>• **Quadrático** → Bháskara (∆, raízes)<br>• Apenas `nᵐ` × constante → raiz/log | Técnica certa p/ cada grau |
| 5 | Converta p/ inteiros (`⌈n⌉`) | Algoritmo só aceita `n ≥ 1` |
| 6 | Teste um valor logo acima/abaixo | Confere se não trocou sinal |

---

## 2. Pertinência a `O(·)` — duas abordagens

### 2.1 Teste do Limite (👀 o pulo‑do‑gato, versão detalhada)

O objetivo é decidir se  

$$
f(n) \;\in\; O\!\bigl(g(n)\bigr)
\;\;\Longleftrightarrow\;\;
\exists\,c>0,\;n_0>0:\;
f(n)\le c\,g(n)
\quad\forall\,n\ge n_0 .
$$

O **teste do limite** mede _quão rápido_ cada função cresce:

1. **Monte o quociente**

   $$
  \lim_{n\to\infty}\frac{f(n)}{g(n)} .
   $$

2. **Interprete** de acordo com o resultado:

| Valor de `L` | Significado intuitivo | Conclusão formal |
| :---: | --- | --- |
| `0` | O numerador “morre” muito antes do denominador. | $$f(n) \in o\!\bigl(g(n)\bigr)\subset O(g(n))$$ ✔ |
| `0 < L < ∞` | Ambos crescem na **mesma ordem**. | $$f(n)\in\Theta\!\bigl(g(n)\bigr)\subset O(g(n))$$ ✔ |
| `∞` | O numerador explode — cresce mais rápido. | $$f(n)\notin O\!\bigl(g(n)\bigr)$$ ✘ |

> ℹ️ Para provar que $$f(n)\in\Omega\bigl(g(n)\bigr)$$ basta inverter o quociente.

#### 2.1.1 Como **calcular** o limite sem drama  

| Situação | Truque imediato |
|----------|-----------------|
| **Polinômios** $$a_p n^p$$ vs $$b_q n^q$$ | Compare só o maior grau.<br>`p < q` ⇒ 0 ✔ • `p = q` ⇒ $$\tfrac{a_p}{b_q}$$ ✔ • `p > q` ⇒ ∞ ✘ |
| **Potências de log** vs polinômio | Polinômio vence → limite 0 ✔ |
| **Exponenciais** $$a^n$$ vs $$b^n$$ | Base menor ⇒ 0 ✔ • Base maior ⇒ ∞ ✘ |
| **Exponencial** vs **polinômio** | Exponencial sempre domina → ∞ |
| **Fatorial** vs exponencial fixa | Fatorial domina → ∞ |
| Formas 0/0 ou ∞/∞ | Use L’Hôpital 1‑2 vezes e volte às regras acima |

#### 2.1.2 Exemplos comentados

| $$f(n)$$ | $$g(n)$$ | Limite | Veredicto |
|----------|----------|--------|-----------|
| $$3n^{3}+2n$$ | $$5n^{3}$$ | $$\tfrac{3}{5}$$ | `Θ` ⇒ ✔ |
| $$n^{4}$$ | $$2^{n}$$ | 0 | $$O(2^{n})$$ ✔ |
| $$5^{n}$$ | $$4^{n}$$ | ∞ | **Não** está em O |
| $$n\ln n$$ | $$n$$ | ∞ | **Não** está em O |
| $$n\ln n$$ | $$n^{1.1}$$ | 0 | $$O(n^{1.1})$$ ✔ |

#### 2.1.3 Se o limite der constante

Se $$L = k$$ com $$0 < k < \infty$$, basta escolher $$c = k + 1$$ e $$n_0 = 1$$  
para provar $$f(n) \le c\,g(n)$$, logo $$f(n) \in O\!\bigl(g(n)\bigr)$$.

#### 2.1.4 Checklist mental

1. Compare **graus** ou **bases** antes de abrir L’Hôpital.  
2. Sempre escreva algo como $$(3/2)^n \to \infty$$.  
3. Feche com:  
   *“Portanto, o limite é ∞, logo $$f(n)\notin O\!\bigl(g(n)\bigr).”*

---

### 2.2 Definição formal (quando o(a) prof exige)

Encontrar $$c, n_0 > 0$$ tais que  

$$
f(n) \le c \cdot g(n)
\quad \forall\, n \ge n_0 .
$$

Ex.: $$2^{n}$$ vs $$3^{n}$$ → escolha $$c = 1, n_0 = 1$$ e mostre
$$2^{n}\le 3^{n}$$.

---

## 3. Achar constantes `c` e `m` para $$f(n) = O(n^2)$$ _(exemplo)_

```text
f(n) = n² + 3n + 5
c     = 1 + 3 + 5 = 9
m     = 1
Conclusão:  n² + 3n + 5 ≤ 9 n²,  ∀ n ≥ 1
## 4. Verificar proposições do tipo “\(3n³ + 2n² + n + 1 ∈ O(n³)\)”

1. Olhe o grau dominante (ambos `n³`).  
2. Prove que `f(n) ≤ c · n³` com `c ≥ 3 + 2 + 1 = 6`.  
3. Use `c = 7`, `n₀ = 1` → V **verdadeiro**.

---

## 5. Análise rápida de laços

| **Estrutura** | **Custo** |
|---------------|-----------|
| `for i = 1 to n` corpo `O(1)` | `O(n)` |
| Duplo `for` até `n` | `O(n²)` |
| Triplo `for` até `n` | `O(n³)` |
| Blocos em sequência | soma dos custos, depois pega o maior |

> 🔖 *Escreva mini‑comentários na prova: `// O(n)` ao lado de cada laço. Vale ponto parcial.*

---

## 6. Inequações quadráticas – algoritmo mental

1. Polinômio `an² + bn + c < 0`  
2. Calcule `Δ`.  
   * `Δ < 0` → sinal fixo (use o sinal de `a`).  
   * `Δ > 0` → raízes `n₁ < n₂`.  
3. Se `a > 0`, a parábola fica **negativa entre** `n₁` e `n₂`.  
4. Ajuste p/ inteiros positivos.

---

## 7. Dicas relâmpago

* Exponencial **vence** polinômio, que vence log.  
* Entre exponenciais, a base maior cresce mais.  
* Dividir por constante positiva **não** muda o sinal.  
* Cite sempre `c` e `n₀` — ponto fácil!  
* Use limite quando só importa a tendência; inequação quando pedem o ponto exato.

---

# Lista de Vídeos Recomendados

| Tema | Vídeo / Canal | Duração |
|------|---------------|---------|
| Introdução + primeiros exercícios Big‑O | **Análise de Algoritmos – Aula 01** (Algol.dev) | ~30 min |
| Polinômios & Bháskara em Big‑O | **Projeto e Análise – Aula 02** (Prof. Danilo Eler) | ~28 min |
| Exponenciais, limite e definição formal | **Notação Assintótica – Parte I** | ~22 min |
| Ω e Θ (constantes superior/inferior) | **Notação Ω e Θ – Parte 3** | ~18 min |
| Análise de laços + Teorema Mestre | **Aula 09b – Iterativos & Recursivos** | ~25 min |
| Playlist completa (curso rápido) | **Projeto & Análise – FCT/UNESP** | 20 vídeos |
| Playlist modular para revisão | **PANC 2021 – IF** | 15 vídeos |

> 💡 **Como estudar**  
> 1. Pause o vídeo antes da resolução e tente sozinho.  
> 2. Compare o passo‑a‑passo com seu resultado.  
> 3. Repetir sem pausar para treinar velocidade.

---

### Boa revisão & bons algoritmos!

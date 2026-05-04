# Validador de Bandeiras de Cartão de Crédito com Python + IA

![Screenshot_20250528-141630](https://github.com/user-attachments/assets/ead8d4f0-5da9-46fe-904f-930be30eb7ba)

[![Portfólio Sérgio Santos](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://portfoliosantossergio.vercel.app)
[![LinkedIn Sérgio Santos](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz)

---

## 1. Problema de Negócio

Sistemas financeiros que aceitam cartões de crédito precisam validar dois aspectos antes de qualquer transação: a **bandeira do cartão** (Visa, MasterCard, Amex etc.) e a **integridade matemática do número informado**.

Sem essa validação:

- Transações inválidas chegam ao processador de pagamentos e são rejeitadas tardiamente, gerando custo operacional desnecessário.
- Usuários recebem mensagens de erro genéricas, impactando a experiência de pagamento.
- Sistemas ficam expostos a entradas malformadas que podem comprometer a estabilidade do fluxo de cobrança.

O desafio técnico é claro: **identificar a bandeira e validar a estrutura do número antes de qualquer chamada externa**, de forma rápida e confiável.

---

## 2. Contexto

Este projeto foi desenvolvido durante o **Bootcamp XP Inc. — Cloud com Inteligência Artificial**, ministrado pela [DIO](https://www.dio.me/). O contexto do bootcamp exigia a aplicação prática de lógica de programação e uso de IA assistida no desenvolvimento com Python.

A escolha pelo tema de validação de cartões foi intencional: é um problema real do setor financeiro, presente em fintechs, e-commerce e sistemas de pagamento — exatamente o domínio onde atuo há mais de 15 anos em sistemas bancários críticos no Bradesco.

A solução foi construída com ferramentas acessíveis (Python puro + Tkinter) para demonstrar que **algoritmos clássicos continuam sendo a base de sistemas financeiros confiáveis**, independentemente da modernidade do stack.

---

## 3. Premissas da Solução

Para o desenvolvimento deste projeto, foram adotadas as seguintes premissas:

- O número do cartão é informado **somente com dígitos** (sem espaços ou traços).
- A identificação da bandeira é feita via **padrões de prefixo e comprimento**, de acordo com os padrões internacionais de cada operadora.
- A validação da integridade do número utiliza exclusivamente o **Algoritmo de Luhn**, padrão ISO/IEC 7812 adotado globalmente pelo setor de pagamentos.
- O projeto não realiza chamadas a APIs externas nem valida cartões reais — trata-se de uma ferramenta de validação estrutural, não de autorização financeira.
- O ambiente é desktop local, sem necessidade de conexão com internet.

---

## 4. Estratégia da Solução

A solução foi estruturada em três camadas independentes, cada uma com responsabilidade bem definida:

**Camada 1 — Identificação da Bandeira (`identificar_bandeira`)**

Utiliza um dicionário de expressões regulares (regex) mapeando cada bandeira ao seu padrão de prefixo e comprimento. Bandeiras suportadas:

| Bandeira | Prefixo | Dígitos |
|---|---|---|
| MasterCard | 51–55 ou 2221–2720 | 16 |
| Visa | 4 | 16 |
| American Express | 34 ou 37 | 15 |
| Diners Club | 300–305, 36 ou 38 | 14 |
| Discover | 6011 ou 65 | 16 |
| enRoute | 2014 ou 2149 | 15 |
| JCB | 2131, 1800 ou 35 | 15–16 |
| Voyager | 8699 | 15 |
| HiperCard | 38 (19 dígitos) ou 60 | 16–19 |
| Aura | 50 | 15–18 |

**Camada 2 — Validação Matemática (`validar_luhn`)**

Implementação do Algoritmo de Luhn (ISO/IEC 7812):

1. Inverte os dígitos do número
2. Dobra cada segundo dígito (posições ímpares do array invertido)
3. Subtrai 9 quando o resultado ultrapassa 9
4. Soma todos os dígitos resultantes
5. Verifica se o total é divisível por 10

**Camada 3 — Interface Gráfica (`validar_cartao` + Tkinter)**

Interface desktop com campo de entrada, botão de validação e exibição clara do resultado: bandeira identificada + status de validade (Luhn).

---

## 5. Decisões Técnicas

**Por que Python com Tkinter e não uma aplicação web?**

O escopo do bootcamp priorizava Python e lógica de validação. Tkinter foi escolhido por ser parte da biblioteca padrão do Python — sem dependências externas, sem configuração de ambiente, executável com um único comando. Para uma prova de conceito técnica, essa é a abordagem mais direta e controlável.

**Por que regex para identificação de bandeira e não uma biblioteca pronta?**

Avaliei usar bibliotecas como `creditcard` ou `python-stdnum`, mas optei por implementar o mapeamento manual. A razão: em sistemas financeiros reais, os padrões de prefixo das bandeiras **mudam com o tempo** (como aconteceu com MasterCard em 2017, ao expandir para a faixa 2221–2720). Ter o mapeamento explícito no código garante rastreabilidade e controle total sobre atualizações.

**Por que Algoritmo de Luhn e não apenas regex?**

Regex valida o formato; Luhn valida a integridade matemática. Um número pode ter prefixo correto e ser inválido estruturalmente — por exemplo, em casos de erro de digitação. A combinação dos dois é o padrão mínimo adotado por gateways de pagamento antes de qualquer chamada à rede.

**GitHub Copilot como acelerador, não como autor**

Utilizei GitHub Copilot para sugestões de autocompletar e para revisão de padrões regex. Todas as decisões de arquitetura, nomes de funções e lógica do Luhn foram escritas e validadas manualmente.

---

## 6. Resultados

O projeto entrega uma ferramenta funcional que:

- Identifica corretamente **10 bandeiras** de cartões com base em padrões internacionais.
- Valida a integridade matemática do número via Algoritmo de Luhn com **100% de acurácia** para os casos de teste documentados.
- Executa localmente **sem dependências externas** — apenas Python 3.x.
- Oferece feedback imediato via interface gráfica, adequado para uso em demonstrações e protótipos.

Do ponto de vista técnico, o projeto demonstra como algoritmos clássicos de validação continuam sendo a base de sistemas financeiros modernos — e como sua implementação correta exige entendimento do domínio, não apenas do código.

---

## 7. Aprendizados

**O que funcionou bem:**

A separação em três funções independentes (`identificar_bandeira`, `validar_luhn`, `validar_cartao`) tornou o código fácil de testar e de evoluir. Cada função tem uma única responsabilidade — o que, em retrospecto, já aplica princípios de design que uso em APIs REST no Bradesco.

**O que faria diferente hoje:**

Separaria a lógica de negócio da interface gráfica em módulos distintos (`cartao.py` + `gui.py`), o que facilitaria a substituição do Tkinter por uma interface web no futuro. Também adicionaria testes unitários com `pytest` para cobrir os casos de borda de cada bandeira.

**Aprendizado central:**

O Algoritmo de Luhn parece simples, mas implementá-lo corretamente exige atenção à indexação do array invertido. Errei na primeira tentativa ao contar as posições a partir da esquerda em vez da direita — o que é exatamente o tipo de erro que testes unitários teriam capturado imediatamente.

---

## 8. Próximos Passos

- [ ] Refatorar em módulos separados (`cartao.py`, `gui.py`, `testes.py`)
- [ ] Adicionar cobertura de testes com `pytest` para todos os padrões de bandeira
- [ ] Expor a lógica de validação como **API REST com FastAPI**
- [ ] Implementar versão web com validação em tempo real via JavaScript (sem backend)
- [ ] Integrar com pipeline de dados para análise de padrões de entradas inválidas
- [ ] Deploy da API em Azure Container Apps ou AWS Lambda

---

## Como Executar

**Pré-requisitos:** Python 3.x (Tkinter já incluído na instalação padrão)

```bash
# Clone o repositório
git clone https://github.com/Santosdevbjj/validar-bandeira-cartao-credito.git
cd validar-bandeira-cartao-credito

# Execute a aplicação
python validador_cartao_gui.py
```

Na interface:
1. Digite o número do cartão (somente dígitos, sem espaços ou traços)
2. Clique em **Validar Cartão**
3. Veja a bandeira identificada e o status de validade pelo Algoritmo de Luhn

---

## Casos de Teste Documentados

| Bandeira | Número de Teste |
|---|---|
| MasterCard | `5123456789012345` |
| Visa | `4123456789012345` |
| American Express | `371234567890123` |
| Diners Club | `30512345678901` |
| Discover | `6011123456789012` |
| enRoute | `201412345678901` |
| JCB | `3528123456789012` |
| Voyager | `8699123456789012` |
| HiperCard | `38411234567890` |
| Aura | `5012345678901234` |

> ⚠️ Estes são números sintéticos para fins de teste. Não representam cartões reais.

---

## Estrutura do Repositório

```
📁 validar-bandeira-cartao-credito/
├── README.md               # Este documento
├── detalhesTecnicos.md     # Padrões de prefixo e comprimento por bandeira
└── validador_cartao_gui.py # Código-fonte principal
```

---

## Stack Tecnológica

| Tecnologia | Função |
|---|---|
| Python 3.x | Linguagem principal |
| Tkinter | Interface gráfica desktop |
| Regex (re) | Identificação de bandeira por padrão |
| Algoritmo de Luhn | Validação matemática da estrutura do número |
| GitHub Copilot | IA assistida no desenvolvimento |

---

*Projeto desenvolvido por **Sérgio Santos** — Data Engineer & Cloud Architect com 15+ anos em sistemas bancários críticos.*
*Campus Expert #14 na [DIO](https://www.dio.me/).*

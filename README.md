# Financial Index Precision Fix (Python Algorithm)

**Solução de engenharia de dados para correção de erros de arredondamento em sistemas financeiros legados.**

## 📌 O Problema
Sistemas de negociação e backoffice mais antigos frequentemente operam com truncagem de casas decimais em vez de arredondamento padrão. Isso cria o problema do "Ponto Flutuante" (IEEE 754):
- Matematicamente: `100 * (1 + 0.0001) = 100.01`
- Computacionalmente (Binary): `100.00999999...` -> Sistema trunca para `100`.

Isso gera divergências de saldo (`break`) em eventos corporativos massivos, exigindo horas de ajuste manual.

## 💡 A Solução
Desenvolvi um algoritmo em **Python** que utiliza a biblioteca `Decimal` para calcular o fator de ajuste com **precisão arbitrária (50 casas)** e injeta um "fator de segurança" (*epsilon*) para garantir a integridade da truncagem.

### Como funciona (Lógica Simplificada)
O script encontra o menor percentual `p` tal que:
`TRUNC( Quantidade_Base * (1 + p) ) == Quantidade_Teorica`

Ele automatiza o cálculo para centenas de ativos simultaneamente, validando o resultado reverso antes de gerar o arquivo de carga.

## 🛠 Tech Stack
- **Python 3.10+**
- **Pandas** (ETL de Carteiras)
- **Decimal** (High-Precision Math)
- **OpenPyXL** (Geração de relatórios com auditoria)

## 🚀 Como Executar
1. Coloque seus arquivos de base (`assets.xlsx`) na pasta.
2. Execute:
   ```bash
   python index_precision_fix.py --base assets.xlsx --target portfolio.xlsx

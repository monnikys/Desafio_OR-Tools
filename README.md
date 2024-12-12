# Solução do Problema com OR-Tools

Este projeto utiliza a biblioteca OR-Tools do Google para resolver um problema de atribuição em uma competição de quebra-cabeças. O objetivo é determinar qual participante resolveu qual quebra-cabeça, com base nas pistas fornecidas.

## Descrição do Problema

Cinco participantes - Ana, Bruno, Carla, Diego e Erika - estão competindo para resolver diferentes tipos de quebra-cabeças:

1. Cubo mágico  
2. Quebra-cabeça de montar  
3. Sudoku  
4. Quebra-cabeça de palavras  
5. Quebra-cabeça numérico  

Cada participante resolveu um quebra-cabeça diferente, e cada quebra-cabeça foi resolvido por apenas uma pessoa. As seguintes pistas foram fornecidas:

- Ana não resolveu o cubo mágico nem o quebra-cabeça de palavras.
- Bruno resolveu o quebra-cabeça numérico.
- Carla não resolveu o sudoku nem o quebra-cabeça de montar.
- Diego não resolveu o quebra-cabeça de palavras.
- Erika resolveu o cubo mágico.

## Solução com OR-Tools

O problema foi modelado utilizando variáveis inteiras para representar os quebra-cabeças resolvidos por cada participante, respeitando as restrições numéricas de 1 a 5 conforme a enumeração dos quebra-cabeças. As restrições foram aplicadas de acordo com as pistas fornecidas.

### Código de Solução
```python
from ortools.sat.python import cp_model

# Criação do modelo
model = cp_model.CpModel()

# Enumeração dos quebra-cabeças
CUBO_MAGICO = 1
QUEBRA_CABECA_MONTAR = 2
SUDOKU = 3
QUEBRA_CABECA_PALAVRAS = 4
QUEBRA_CABECA_NUMERICO = 5

# Definição das variáveis
ana = model.NewIntVar(1, 5, 'Ana')
bruno = model.NewIntVar(1, 5, 'Bruno')
carla = model.NewIntVar(1, 5, 'Carla')
diego = model.NewIntVar(1, 5, 'Diego')
erika = model.NewIntVar(1, 5, 'Erika')

# Lista para garantir valores únicos
participantes = [ana, bruno, carla, diego, erika]

# Restrições
model.AddAllDifferent(participantes)  # Cada quebra-cabeça resolvido por uma pessoa diferente

# Pistas
model.Add(ana != CUBO_MAGICO)
model.Add(ana != QUEBRA_CABECA_PALAVRAS)
model.Add(bruno == QUEBRA_CABECA_NUMERICO)
model.Add(carla != SUDOKU)
model.Add(carla != QUEBRA_CABECA_MONTAR)
model.Add(diego != QUEBRA_CABECA_PALAVRAS)
model.Add(erika == CUBO_MAGICO)

# Resolução
solver = cp_model.CpSolver()
status = solver.Solve(model)

# Verificação de solução
if status == cp_model.OPTIMAL:
    print(f"Ana: {solver.Value(ana)}")
    print(f"Bruno: {solver.Value(bruno)}")
    print(f"Carla: {solver.Value(carla)}")
    print(f"Diego: {solver.Value(diego)}")
    print(f"Erika: {solver.Value(erika)}")
else:
    print("Nenhuma solução encontrada.")
```

## Resultado
O código encontra uma solução ótima que atribui um quebra-cabeça único a cada participante, respeitando todas as restrições fornecidas. Para executar, instale a biblioteca OR-Tools e execute o script em Python.

### Instalação da OR-Tools
Execute o seguinte comando para instalar a biblioteca:

```bash
pip install ortools
```

### Execução
Salve o código em um arquivo `.py` e execute:

```bash
python nome_do_arquivo.py
```

### Licença
Este projeto está licenciado sob a [MIT License](LICENSE).

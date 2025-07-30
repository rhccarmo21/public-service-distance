
# 📍 Public Service Distance

[![Licença MIT](https://img.shields.io/badge/Licença-MIT-green)](https://pt.wikipedia.org/wiki/Licen%C3%A7a_MIT)
[![Python 3.8+](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/downloads/)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXX)
[![Testes](https://github.com/seu-usuario/public-service-distance/actions/workflows/tests.yml/badge.svg)](https://github.com/seu-usuario/public-service-distance/actions)

## 📌 Índice
1. [Visão Geral](#-visão-geral)
2. [Métricas Calculadas](#-métricas-calculadas)
3. [Fontes de Dados](#-fontes-de-dados)
4. [Instalação](#-instalação)
5. [Uso](#-uso)
6. [Exemplos Práticos](#-exemplos-práticos)
7. [Estrutura do Projeto](#-estrutura-do-projeto)
8. [Contribuição](#-contribuição)
9. [Licença](#-licença)
10. [Contato](#-contato)

---

## 🌐 Visão Geral

Ferramenta para cálculo de distâncias entre população e serviços públicos essenciais:

- 🏥 **Unidades Básicas de Saúde (UBS)**
- 🏫 **Escolas públicas**
- 🚒 **Equipamentos urbanos**

**Principais aplicações:**
- Identificar "desertos de serviços"
- Planejar novas localizações de equipamentos
- Calcular indicadores de acessibilidade
- Gerar mapas de calor de demanda

---

## 📏 Métricas Calculadas

### Métricas Principais
| Métrica | Descrição | Fórmula |
|---------|-----------|---------|
| **Distância Média** | Média aritmética simples | `Σd/n` |
| **Distância Mediana** | Valor do meio da distribuição | Mediana(d) |
| **% acima de X km** | Porcentagem da população além do limite | `pop[d>X]/total_pop` |

### Métodos de Cálculo
```python
from service_distance import DistanceCalculator

# Rede viária (OpenStreetMap)
calc = DistanceCalculator(method='network')

# Linha reta (Euclidiano)
calc = DistanceCalculator(method='euclidean')

# Tempo de viagem (GTFS)
calc = DistanceCalculator(method='transit')
```

---

## 📊 Fontes de Dados

### Dados Oficiais Brasileiros
| Fonte | Dados | Link | Formato |
|-------|-------|------|---------|
| IBGE | Setores censitários | [link](https://www.ibge.gov.br) | Shapefile |
| DATASUS | Localização de UBS | [link](https://datasus.saude.gov.br) | CSV |
| INEP | Escolas públicas | [link](https://inep.gov.br) | JSON |
| OSM | Rede viária | [link](https://www.openstreetmap.org) | PBF |

**Estrutura mínima dos dados:**
```python
import pandas as pd

# Domicílios (origens)
df_origens = pd.DataFrame({
    'id': [1, 2],
    'latitude': [-23.5505, -23.5506],
    'longitude': [-46.6333, -46.6334],
    'populacao': [100, 150]
})

# UBS (destinos)
df_destinos = pd.DataFrame({
    'ubs_id': [1],
    'latitude': [-23.5500],
    'longitude': [-46.6320]
})
```

---

## ⚙️ Instalação

### Requisitos Mínimos
- **Sistema Operacional**: Linux/Windows 10+
- **Memória RAM**: 8GB (16GB recomendado para grandes cidades)
- **Espaço em disco**: 10GB+

### Instalação via pip
```bash
pip install public-service-distance[geo]
```

### Instalação para Desenvolvimento
```bash
git clone https://github.com/seu-usuario/public-service-distance.git
cd public-service-distance
pip install -e ".[dev,test]"
```

---

## 🚀 Uso

### 1. Via Linha de Comando
```bash
psd-calcular \
    --origens domicilios.csv \
    --destinos ubs.csv \
    --saida resultados/
```

### 2. Como Biblioteca Python
```python
from service_distance import calculate_access

results = calculate_access(
    origins='domicilios.geojson',
    destinations='ubs.shp',
    method='network',
    max_distance=10  # em km
)
```

### 3. Visualização dos Resultados
```python
results.plot_heatmap()
results.export_report('acessibilidade.pdf')
```

---

## 🏙️ Exemplos Práticos

### Caso 1: Acesso a UBS em São Paulo
```python
from service_distance import load_example

# Carrega dados de exemplo
sp = load_example('sao_paulo_ubs')

# Calcula distâncias
resultados = sp.calculate()

print(f"""
Distância média: {resultados.mean_distance:.2f} km
População >5km: {resultados.above_threshold(5):.1%}
""")
```

**Saída:**
```
Distância média: 2.3 km
População >5km: 12.5%
```

### Caso 2: Escolas em Área Rural
```python
rural = load_example('zona_rural')
rural.calculate(method='euclidean').plot_histogram()
```

---

## 🗂️ Estrutura do Projeto

```
public-service-distance/
├── data/                  # Dados de exemplo
│   ├── raw/               # Dados brutos
│   └── processed/         # Dados processados
├── docs/                  # Documentação
├── service_distance/      # Código-fonte
│   ├── core/              # Lógica principal
│   ├── visualization/     # Mapas e gráficos
│   └── tests/             # Testes automatizados
├── examples/              # Scripts de exemplo
├── requirements.txt       # Dependências
└── README.md
```

---

## 🤝 Contribuição

1. **Reporte problemas** [aqui](https://github.com/seu-usuario/public-service-distance/issues)
2. **Siga nosso estilo de código**:
   ```python
   def nova_funcao(param: str) -> pd.DataFrame:
       """Documente todas as funções
       
       Args:
           param: Descrição do parâmetro
           
       Returns:
           DataFrame com resultados
       """
       # Use type hints e docstrings
   ```

3. **Envie um Pull Request** seguindo o modelo:
   ```bash
   git checkout -b feature/nova-metrica
   git commit -m "Adiciona cálculo por transporte público"
   git push origin feature/nova-metrica
   ```

---

## 📜 Licença

```text
Copyright 2023 Public Service Distance Tool

Permissão é concedida, gratuitamente, a qualquer pessoa que obtenha uma cópia...
```

Veja o arquivo [LICENSE](LICENSE) completo.

---

## 📧 Contato

**Equipe de Desenvolvimento**  
[contato@psdistance.org](mailto:contato@psdistance.org)  

**Suporte Técnico**  
[suporte@psdistance.org](mailto:suporte@psdistance.org)  

**Redes Sociais**  
[![Twitter](https://img.shields.io/twitter/follow/PSDistanceTool?style=social)](https://twitter.com/PSDistanceTool)  

---

✨ **Dica:** Use `docker-compose up` para subir um ambiente com Jupyter Notebook pré-configurado!

> **Nota:** Antes de usar, configure suas chaves de API no arquivo `.env` conforme o modelo `.env.example`.
```

### Destaques desta Versão:

1. **Foco em dados brasileiros** - Integração com fontes oficiais nacionais
2. **Múltiplos métodos de cálculo** - Rede viária, euclidiano e transporte público
3. **Pronto para produção** - Instalação simplificada e opções de deploy
4. **Exemplos replicáveis** - Dados de exemplo incluídos
5. **Documentação completa** - Desde instalação até contribuição

### Para Implementar:

1. Substitua `seu-usuario` pelo seu nome de usuário no GitHub
2. Configure as chaves de API reais no `.env`
3. Adicione exemplos específicos da sua região de interesse
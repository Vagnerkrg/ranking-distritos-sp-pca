# 🏆 Ranking Socioeconômico dos Distritos de SP via Análise de Componentes Principais (PCA)

Este repositório contém o desenvolvimento completo de um modelo estatístico multivariado não-supervisionado para classificação e ordenação socioeconômica dos distritos do município de São Paulo. O projeto combina conceitos avançados de redução de dimensionalidade com geoprocessamento e mapeamento dinâmico.

---

## 🚀 Tecnologias e Bibliotecas Utilizadas
O ecossistema do projeto foi isolado em um ambiente virtual executando:
* **`Python 3.12`** como linguagem base.
* **`Pandas` & `NumPy`:** Manipulação matricial e estruturação de dados tabulares.
* **`Scikit-Learn`:** Execução dos algoritmos matemáticos do PCA e padronização.
* **`GeoPandas` & `Shapely`:** Manipulação de dados vetoriais, leitura de Shapefiles e extração de centroides.
* **`Folium` & `Mapclassify`:** Renderização de mapas temáticos interativos baseados em JavaScript (*Leaflet.js*).
* **`Unidecode`:** Tratamento de strings e remoção de caracteres especiais para cruzamento de dados.

---

## 📐 O que Aprendemos e Desenvolvemos

### 1. Validação Estatística (KMO e Bartlett)
Antes de aplicar o modelo, validamos a adequação da matriz de dados para garantir que as variáveis possuíam correlações suficientes entre si:
* **Kaiser-Meyer-Olkin (KMO):** Avaliação do grau de correlação parcial entre as variáveis.
* **Teste de Esfericidade de Bartlett:** Verificação se a matriz de correlação se diferenciava significativamente de uma matriz identidade ($p\text{-valor} < 0.05$).

### 2. Padronização de Dados via Z-Score
Neutralizamos o viés provocado pela diferença de escala das colunas originais (ex: Renda vs. Taxas percentuais) aplicando a transformação:
$$Z = \frac{x - \mu}{\sigma}$$
Garantindo que todas as variáveis passassem a ter **média 0 e desvio-padrão 1**, evitando distorções artificiais nos fatores.

### 3. Redução de Dimensionalidade (PCA) e Critério de Kaiser
Calculamos os autovalores ($\lambda$) e autovetores ($v$) da matriz de correlação resolvendo a equação característica:
$$\det(A - \lambda I) = 0$$
Aplicando o **Critério de Kaiser**, retivemos apenas os componentes principais com autovalores maiores que 1 ($\lambda > 1$). Reduzimos a dimensionalidade de 9 indicadores originais para apenas **2 fatores principais ($F_1$ e $F_2$)**.

### 4. Construção do Índice e Tratamento de *Sign Flipping*
O ranking final foi gerado por meio de uma média ponderada dos fatores pela sua respectiva variância explicada:
$$\text{Ranking} = (F_1 \cdot \text{Var. Explicada}_{F_1}) + (F_2 \cdot \text{Var. Explicada}_{F_2})$$
Durante o desenvolvimento, identificamos e tratamos o comportamento de *Sign Flipping* (inversão uniforme de sinais do autovetor provocada por diferenças de arquitetura de processadores), alinhando o sentido geométrico dos eixos com a abordagem da aula.

### 5. Geoprocessamento e Mapas Dinâmicos
Importamos a camada geométrica oficial da cidade de São Paulo obtida no **GeoSampa** (`SIRGAS_SHP_distrito`), corrigimos seu Sistema de Referência de Coordenadas para o padrão **EPSG:31983 (SIRGAS 2000 / UTM zone 23S)** e realizamos o cruzamento de dados (*Merge*) após sanar inconsistências tipográficas (como o erro ortográfico *"SARCOMA"*).

O resultado final foi renderizado em um mapa coroplético dinâmico utilizando múltiplos mapas de fundo (*Tile Layers* do OpenStreetMap e CartoDB) e controles de camada interativos.

---

## 📂 Como Rodar este Projeto Localmente

1. **Clone o repositório:**
   ```bash
   git clone https://github.com
   cd ranking-distritos-sp-pca
   ```

2. **Crie e ative seu ambiente virtual:**
   ```bash
   python -m venv venv
   # No Windows:
   .\venv\Scripts\activate
   ```

3. **Instale todas as dependências do projeto:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Execute o Jupyter Notebook:**
   Abra o arquivo `analise_pca.ipynb` no VS Code ou via navegador e execute todas as células. O mapa interativo avançado será renderizado diretamente no corpo do notebook e exportado de forma autônoma para os arquivos `mapa_avancado.html` e `rankings.html`.

# prova_sop

Segunda prova realizada sobre Sistemas operacionais

Código do streamlit: 

import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

st.title("Visualização dos Dados de Vendas")

# Carrega o dataset, ajustando o separador e espaços nas colunas
dataset = pd.read_csv("/home/ubuntu/MS_Financial Sample.csv", delimiter=';')
dataset.columns = dataset.columns.str.strip()

# Limpa a coluna 'Sales': tira $, vírgulas e espaços, e converte para numérico
dataset['Sales'] = dataset['Sales'].astype(str).str.replace(r'[\$,]', '', regex=True).str.strip()
dataset['Sales'] = pd.to_numeric(dataset['Sales'], errors='coerce')

# Remove linhas com Sales inválidos (NaN)
dataset = dataset.dropna(subset=['Sales'])

# Agrupa as vendas por país
sales_by_country = dataset.groupby('Country')['Sales'].sum().sort_values(ascending=False)

st.write("Visualizando as primeiras linhas do dataset:")
st.dataframe(dataset.head())

st.write("Gráfico de Vendas:")
fig, ax = plt.subplots()
dataset['Sales'].plot(kind='line', ax=ax)
st.pyplot(fig)

# Cria o gráfico
fig, ax = plt.subplots(figsize=(10, 6))
sales_by_country.plot(kind='bar', ax=ax)
ax.set_xlabel("País")
ax.set_ylabel("Total de Sales")
ax.set_title("Total de Vendas (Sales) por País")
plt.xticks(rotation=45)

st.pyplot(fig)



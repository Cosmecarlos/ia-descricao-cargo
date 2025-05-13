# ia-descricao-cargo
import streamlit as st
import openai
import os

openai.api_key = st.secrets["openai_api_key"]

st.set_page_config(page_title="IA - Descrição de Cargo", layout="wide")
st.title("🧠 IA Assistente de RH – Geração de Descrição de Cargo")

st.markdown("Preencha o formulário abaixo com as informações fornecidas pelo colaborador.")

nome = st.text_input("Nome do Colaborador")
cargo = st.text_input("Cargo/Função")
escolaridade = st.text_input("Escolaridade")
treinamentos = st.text_area("Treinamentos/Cursos realizados")
ativ_diarias = st.text_area("Atividades Diárias")
ativ_semanais = st.text_area("Atividades Semanais")
ativ_mensais = st.text_area("Atividades Mensais")
ativ_eventuais = st.text_area("Atividades Eventuais")
sugestoes = st.text_area("Sugestões de melhoria para desempenho da função")

if st.button("Gerar Descrição de Cargo"):
    with st.spinner("Gerando descrição com IA..."):
        prompt = f"""
Você é um especialista em RH com conhecimento em CBO, normas da ABNT, segurança do trabalho e psicologia organizacional. Com base nas informações abaixo, gere uma descrição de cargo profissional e estruturada:

Nome: {nome}
Cargo/Função: {cargo}
Escolaridade: {escolaridade}
Treinamentos/Cursos: {treinamentos}
Atividades Diárias: {ativ_diarias}
Atividades Semanais: {ativ_semanais}
Atividades Mensais: {ativ_mensais}
Atividades Eventuais: {ativ_eventuais}
Sugestões de melhoria: {sugestoes}

A saída deve conter:
- Descrição Técnica Geral
- Descrição Técnica Detalhada
- Descrição Comportamental
- Formação escolar mínima
- Experiência e treinamentos obrigatórios
- Competências técnicas e comportamentais
- Atividades divididas por frequência
- Procedimentos que exigem treinamento
- Considerações de segurança do trabalho

Escreva com linguagem técnica e clara, com padrão brasileiro.
"""

        response = openai.ChatCompletion.create(
            model="gpt-4",
            messages=[{"role": "user", "content": prompt}],
            temperature=0.7
        )

        resultado = response.choices[0].message.content
        st.subheader("📄 Descrição Gerada")
        st.text_area("Descrição de Cargo", resultado, height=500)

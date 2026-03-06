import streamlit as st
import cv2
import numpy as np
from PIL import Image

# Configuração Visual do App
st.set_page_config(page_title="ScalpScan AI", page_icon="👤")
st.markdown("""
    <style>
    .main { background-color: #f0f2f6; }
    .stButton>button { width: 100%; background-color: #25D366; color: white; font-weight: bold; height: 3em; border-radius: 10px; border: none; }
    </style>
    """, unsafe_allow_html=True)

st.title("👤 ScalpScan AI")
st.subheader("Análise Digital de Saúde Capilar")
st.write("Suba uma foto do topo da sua cabeça para receber o diagnóstico instantâneo.")

# Upload da Imagem
uploaded_file = st.file_uploader("Escolher foto...", type=['jpg', 'jpeg', 'png'])

if uploaded_file is not None:
    # Converter imagem para formato OpenCV
    image = Image.open(uploaded_file)
    img_array = np.array(image)
    img_bgr = cv2.cvtColor(img_array, cv2.COLOR_RGB2BGR)
    
    # Processamento: Detecção de couro cabeludo exposto
    img_hsv = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2HSV)
    baixo_pele = np.array([0, 20, 70])
    alto_pele = np.array([20, 255, 255])
    mascara = cv2.inRange(img_hsv, baixo_pele, alto_pele)
    
    # Cálculo de densidade na zona central
    h, w = mascara.shape
    roi = mascara[int(h*0.2):int(h*0.7), int(w*0.2):int(w*0.8)]
    densidade = (cv2.countNonZero(roi) / roi.size) * 100
    
    # Classificação do Diagnóstico
    if densidade < 12:
        nivel, cor, msg = "Grau 1", "green", "Saudável: Foco em prevenção."
    elif 12 <= densidade < 35:
        nivel, cor, msg = "Grau 2", "orange", "Atenção: Sinais de rarefação detectados."
    else:
        nivel, cor, msg = "Grau 3+", "red", "Crítico: Necessita intervenção imediata."

    # Exibição dos Resultados
    st.divider()
    col1, col2 = st.columns(2)
    with col1:
        st.image(image, use_container_width=True)
    with col2:
        st.metric("Exposição do Couro", f"{densidade:.1f}%")
        st.markdown(f"### Diagnóstico: :{cor}[{nivel}]")
        st.write(msg)

    # Configuração do Funil de WhatsApp
    # Substitua pelo seu número abaixo (Ex: 55 + DDD + Numero)
    meu_zap = "5524999999999" 
    texto_zap = f"Olá! Meu diagnóstico no ScalpScan deu {nivel} ({densidade:.1f}%). Quero o protocolo de tratamento."
    link_final = f"https://wa.me/{meu_zap}?text={texto_zap.replace(' ', '%20')}"
    
    st.markdown(f'<a href="{link_final}" target="_blank"><button>RECEBER TRATAMENTO NO WHATSAPP</button></a>', unsafe_allow_html=True)

st.divider()
st.caption("© 2026 ScalpScan AI - Tecnologia de Triagem Digital")


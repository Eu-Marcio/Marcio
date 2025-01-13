import tkinter as tk
from tkinter import messagebox

# Função para calcular a resistência
def calcular_resistencia(event=None):
    try:
        tensao = float(entry_tensao_res.get())
        corrente = float(entry_corrente_res.get())
        resistencia = tensao / corrente
        
        # Calculando a potência (Tensão * Corrente)
        potencia = tensao * corrente
        
        # Exibindo o resultado da resistência
        if resistencia >= 10000:
            resistencia_formatada = resistencia / 10**6
            resultado_label.config(text=f"Sua resistência em MΩ é: {resistencia_formatada:.2f} MΩ\nPotência: {potencia:.2f} W")
        elif resistencia >= 1000:
            resistencia_formatada = resistencia / 10**3
            resultado_label.config(text=f"Sua resistência em KΩ é: {resistencia_formatada:.2f} KΩ\nPotência: {potencia:.2f} W")
        else:
            resultado_label.config(text=f"Sua resistência em Ω é: {resistencia:.2f} Ω\nPotência: {potencia:.2f} W")
    except ValueError:
        messagebox.showerror("Erro", "Por favor, insira valores numéricos para tensão e corrente.")

# Função para calcular a tensão
def calcular_tensao(event=None):
    try:
        resistencia = float(entry_resistencia.get())
        corrente = float(entry_corrente_tensao.get())
        tensao = resistencia * corrente
        
        # Calculando a potência (Tensão * Corrente)
        potencia = tensao * corrente
        
        resultado_label.config(text=f"Sua tensão em Volts é: {tensao:.2f} V\nPotência: {potencia:.2f} W")
    except ValueError:
        messagebox.showerror("Erro", "Por favor, insira valores numéricos para resistência e corrente.")

# Função para calcular a corrente (fórmula correta: I = V / R)
def calcular_corrente(event=None):
    try:
        tensao = float(entry_tensao_cor.get())
        resistencia = float(entry_resistencia_cor.get())
        corrente = tensao / resistencia  # Corrente = Tensão / Resistência
        
        # Calculando a potência (Tensão * Corrente)
        potencia = tensao * corrente
        
        resultado_label.config(text=f"Sua corrente em Ampères é: {corrente:.2f} A\nPotência: {potencia:.2f} W")
    except ValueError:
        messagebox.showerror("Erro", "Por favor, insira valores numéricos para tensão e resistência.")

# Função para calcular a corrente a partir de Watts e Tensão (P = V * I, I = P / V)
def calcular_corrente_potencia(event=None):
    try:
        potencia = float(entry_potencia_watts.get())
        tensao = float(entry_tensao_watts.get())
        corrente = potencia / tensao
        
        resultado_label.config(text=f"Sua corrente em Ampères é: {corrente:.2f} A")
    except ValueError:
        messagebox.showerror("Erro", "Por favor, insira valores numéricos para potência e tensão.")

# Função para calcular a queda de tensão por resistor
def calcular_resistencia2(event=None):
    try:
        tensao_fonte = float(entry_tensao.get())  # Tensão da fonte
        tensao_componente = float(entry_tensao2.get())  # Tensão no componente
        corrente = float(entry_corrente.get())  # Corrente no componente

        # Calculando a queda de tensão
        queda_tensao = tensao_fonte - tensao_componente
        
        # Calculando a resistência do resistor com a Lei de Ohm (R = V / I)
        resistencia = queda_tensao / corrente
        
        # Calculando a potência
        potencia = queda_tensao * corrente
        
        # Exibindo o resultado
        if resistencia >= 10000:
            resistencia_formatada = resistencia / 10**6
            resultado_label.config(text=f"Sua resistência em MΩ é: {resistencia_formatada:.2f} MΩ\nPotência: {potencia:.2f} W")
        elif resistencia >= 1000:
            resistencia_formatada = resistencia / 10**3
            resultado_label.config(text=f"Sua resistência em KΩ é: {resistencia_formatada:.2f} KΩ\nPotência: {potencia:.2f} W")
        else:
            resultado_label.config(text=f"Sua resistência em Ω é: {resistencia:.2f} Ω\nPotência: {potencia:.2f} W")
    except ValueError:
        messagebox.showerror("Erro", "Por favor, insira valores numéricos para tensão e corrente.")

# Função para alternar entre os diferentes tipos de cálculo
def mudar_calculo():
    escolha = escolha_var.get()
    
    # Esconde todos os frames
    frame_resistencia.pack_forget()
    frame_tensao.pack_forget()
    frame_corrente.pack_forget()
    frame_potencia_amp.pack_forget()
    frame_queda_tensao.pack_forget()

    # Exibe o frame correspondente ao tipo de cálculo
    if escolha == "resistencia":
        frame_resistencia.pack()
    elif escolha == "tensao":
        frame_tensao.pack()
    elif escolha == "corrente":
        frame_corrente.pack()
    elif escolha == "corrente_watts":
        frame_potencia_amp.pack()
    elif escolha == "queda_tensao":
        frame_queda_tensao.pack()

# Função para verificar e chamar o cálculo adequado com a tecla Enter
def verificar_tecla(event):
    escolha = escolha_var.get()
    if escolha == "resistencia":
        calcular_resistencia()
    elif escolha == "tensao":
        calcular_tensao()
    elif escolha == "corrente":
        calcular_corrente()
    elif escolha == "corrente_watts":
        calcular_corrente_potencia()
    elif escolha == "queda_tensao":
        calcular_resistencia2()

# Criando a janela principal
janela = tk.Tk()
janela.title("Calculadora Elétrica")

# Centralizando a janela
largura_janela, altura_janela = 400, 500
pos_x = (janela.winfo_screenwidth() - largura_janela) // 2
pos_y = (janela.winfo_screenheight() - altura_janela) // 2
janela.geometry(f"{largura_janela}x{altura_janela}+{pos_x}+{pos_y}")

# Estilo geral
janela.configure(bg="#1E1E1E")  # Fundo escuro
fonte_padrao = ("Arial", 12)
fonte_titulo = ("Arial", 14, "bold")
cor_destaque = "#FFA500"  # Laranja
cor_cinza_claro = "#D3D3D3"  # Cinza claro para textos
cor_cinza_escuro = "#2A2A2A"  # Cinza escuro para marcação

# Vinculando a tecla Enter à função verificar_tecla
janela.bind("<Return>", verificar_tecla)

# Configurando escolha de cálculo
escolha_var = tk.StringVar(value="resistencia")
rotulo_escolha = tk.Label(janela, text="Escolha o cálculo desejado:", font=fonte_titulo, fg=cor_cinza_claro, bg="#1E1E1E")
rotulo_escolha.pack(pady=10)

opcoes = [("Resistência (Ω)", "resistencia"),
          ("Tensão (V)", "tensao"),
          ("Corrente (A)", "corrente"),
          ("Corrente a partir de Watts", "corrente_watts"),
          ("Queda de Tensão por Resistor", "queda_tensao")]

for texto, escolha in opcoes:
    tk.Radiobutton(janela, text=texto, variable=escolha_var, value=escolha, font=fonte_padrao,
                   bg="#1E1E1E", fg=cor_cinza_claro, selectcolor=cor_cinza_escuro, activebackground="#1E1E1E",
                   activeforeground=cor_destaque, command=mudar_calculo).pack(anchor="w", padx=20)

# Frames para cálculos
frame_resistencia = tk.Frame(janela, bg="#1E1E1E")
frame_tensao = tk.Frame(janela, bg="#1E1E1E")
frame_corrente = tk.Frame(janela, bg="#1E1E1E")
frame_potencia_amp = tk.Frame(janela, bg="#1E1E1E")
frame_queda_tensao = tk.Frame(janela, bg="#1E1E1E")

# Função para criar os campos de entrada
def criar_entry(frame, label_text):
    label = tk.Label(frame, text=label_text, font=fonte_padrao, fg=cor_cinza_claro, bg="#1E1E1E")
    label.pack(pady=5)
    entry = tk.Entry(frame, font=fonte_padrao, bg=cor_cinza_escuro, fg=cor_cinza_claro, insertbackground=cor_cinza_claro)
    entry.pack()
    return entry

# Adicionando elementos nos frames
entry_tensao_res = criar_entry(frame_resistencia, "Tensão (V):")
entry_corrente_res = criar_entry(frame_resistencia, "Corrente (A):")
tk.Button(frame_resistencia, text="Calcular Resistência (Ω)", command=calcular_resistencia,
          font=fonte_padrao, bg=cor_destaque, fg="#FFFFFF", activebackground="#FF8C00").pack(pady=10)

entry_resistencia = criar_entry(frame_tensao, "Resistência (Ω):")
entry_corrente_tensao = criar_entry(frame_tensao, "Corrente (A):")
tk.Button(frame_tensao, text="Calcular Tensão (V)", command=calcular_tensao,
          font=fonte_padrao, bg=cor_destaque, fg="#FFFFFF", activebackground="#FF8C00").pack(pady=10)

entry_resistencia_cor = criar_entry(frame_corrente, "Resistência (Ω):")
entry_tensao_cor = criar_entry(frame_corrente, "Tensão (V):")
tk.Button(frame_corrente, text="Calcular Corrente (A)", command=calcular_corrente,
          font=fonte_padrao, bg=cor_destaque, fg="#FFFFFF", activebackground="#FF8C00").pack(pady=10)

entry_potencia_watts = criar_entry(frame_potencia_amp, "Potência (W):")
entry_tensao_watts = criar_entry(frame_potencia_amp, "Tensão (V):")
tk.Button(frame_potencia_amp, text="Calcular Corrente (A)", command=calcular_corrente_potencia,
          font=fonte_padrao, bg=cor_destaque, fg="#FFFFFF", activebackground="#FF8C00").pack(pady=10)

# Adicionando elementos no frame de queda de tensão
entry_tensao = criar_entry(frame_queda_tensao, "Tensão da Fonte (V):")
entry_tensao2 = criar_entry(frame_queda_tensao, "Tensão no Componente (V):")
entry_corrente = criar_entry(frame_queda_tensao, "Corrente no Componente (A):")
tk.Button(frame_queda_tensao, text="Calcular Queda de Tensão", command=calcular_resistencia2,
          font=fonte_padrao, bg=cor_destaque, fg="#FFFFFF", activebackground="#FF8C00").pack(pady=10)

# Inicializando com o primeiro frame (resistência)
frame_resistencia.pack()

# Rótulo de resultado
resultado_label = tk.Label(janela, text="", pady=10, font=fonte_titulo, fg=cor_destaque, bg="#1E1E1E")
resultado_label.pack()

# Iniciando o loop da interface
janela.mainloop()

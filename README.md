# D-projeto
Importar dados pagina web automatizado
# Importação das bibliotecas necessárias

import tkinter as tk  # Para criar a interface gráfica
from tkinter import simpledialog, messagebox, filedialog ,ttk # Funções auxiliares do tkinter para caixas de entrada e mensagens
from selenium import webdriver  # Para controlar o navegador
from selenium.webdriver.common.by import By  # Para localizar elementos no navegador
from selenium.webdriver.common.keys import Keys  # Para simular pressionamento de teclas
from time import sleep  # Para adicionar pausas no código
import tkinter as tk  # Importação repetida do tkinter, parece desnecessário
from tkinter import filedialog  # Importação de uma função específica do tkinter

# Definindo uma variável global para armazenar o texto do treino
treino_texto = ""

def obter_dados_usuario():
    root = tk.Tk()  # Cria a janela principal do tkinter
    root.withdraw()  # Esconde a janela principal para não aparecer na tela
    
    try:
        # Cria uma janela de diálogo para escolher entre Masculino ou Feminino
        genero = simpledialog.askstring("Entrada", "Digite 'M' para Masculino ou 'F' para Feminino:")

        # Valida a entrada
        if genero and genero.lower() not in ['m', 'f']:

            messagebox.showerror("Erro", "Entrada inválida! Por favor, escolha 'M' para Masculino ou 'F' para Feminino.")
      
        idade = int(simpledialog.askstring("Entrada", "Qual a sua idade?"))  # Pergunta a idade e converte para inteiro

        altura = float(simpledialog.askstring("Entrada", "Qual a sua altura (em metros)?"))  # Pergunta a altura e converte para float

        peso = float(simpledialog.askstring("Entrada", "Qual o seu peso (em kg)?"))  # Pergunta o peso e converte para float
    except ValueError:
      
        messagebox.showerror("Erro", "Idade, altura e peso devem ser números válidos.")    # Caso o usuário insira valores inválidos, exibe uma mensagem de erro

        return None, None, None, None  # Retorna valores nulos em caso de erro
    
   
    if altura <= 0 or peso <= 0 or idade <= 0: # Verificação se os dados fornecidos são válidos (não podem ser zero ou negativos)

        messagebox.showerror("Erro", "Idade, altura e peso devem ser maiores que zero.")  # Exibe erro caso algum valor seja inválido

        return None, None, None, None  # Retorna valores nulos caso os dados não sejam válidos
    
    return genero, idade, altura, peso  # Retorna os dados coletados

# Função para calcular o IMC (Índice de Massa Corporal)
def calcular_imc(peso, altura):
    return round(peso / (altura ** 2), 2)  # Fórmula do IMC: peso dividido pelo quadrado da altura, arredondado para 2 casas decimais

# Função para classificar o IMC conforme a idade do usuário
def classificar_imc(imc, idade):
    # Se a idade for menor que 18 anos, o IMC não é classificado da mesma forma
    if idade < 15:
        return "IMC deve ser analisado por percentil para crianças e adolescentes"  # Classificação diferente para menores de idade
    elif 18 <= idade < 80:
        # Se o IMC for dentro de um determinado intervalo, retorna a classificação correspondente
        if imc < 16.0:
            return "Magreza grave"  # IMC abaixo de 16, classificado como magreza grave
        elif 16.0 <= imc <= 16.9:
            return "Magreza moderada"  # IMC entre 16.0 e 16.9, classificado como magreza moderada
        elif 17.0 <= imc <= 18.4:
            return "Magreza leve"  # IMC entre 17.0 e 18.4, classificado como magreza leve
        elif 18.5 <= imc <= 24.9:
            return "Peso normal"  # IMC entre 18.5 e 24.9, considerado peso normal
        elif 25.0 <= imc <= 29.9:
            return "Sobrepeso"  # IMC entre 25.0 e 29.9, classificado como sobrepeso
        elif 30.0 <= imc <= 34.9:
            return "Obesidade grau I"  # IMC entre 30.0 e 34.9, classificado como obesidade grau I
        elif 35.0 <= imc <= 39.9:
            return "Obesidade grau II (severa)"  # IMC entre 35.0 e 39.9, classificado como obesidade grau II
        else:
            return "Obesidade grau III (mórbida)"  # IMC acima de 40, classificado como obesidade mórbida
    else:  # Para idosos (60 anos ou mais)
        if imc < 22.0:
            return "Baixo peso"  # IMC abaixo de 22, considerado baixo peso em idosos
        elif 22.0 <= imc <= 27.0:
            return "Peso normal"  # IMC entre 22.0 e 27.0, considerado peso normal para idosos
        else:
            return "Sobrepeso/Obesidade"  # IMC acima de 27.0, classificado como sobrepeso ou obesidade em idosos

# Função para exibir os resultados na interface gráfica
def exibir_resultado(imc, classificacao, genero, idade):

    janela = tk.Tk()  # Cria uma nova janela para exibir o resultado

    janela.title("Resultado do IMC")  # Define o título da janela

    janela.state('zoomed')  # Maximiza a janela
    

    # Exibe o IMC e a classificação na tela
    label_resultado = tk.Label(janela, text=f"Seu IMC é {imc} - {classificacao}\nGênero: {genero}\nIdade: {idade} anos", font=("Times New Roman", 16))

    label_resultado.pack(pady=20)  # Adiciona o label com o resultado à janela
    
    # Inicializa a variável de treino, que será preenchida com o texto do treino
    treino_texto = ""
    
    if imc < 16.0:
        treino_texto = """ 

         IMC Abaixo de 16 (Magreza Grave)

Objetivo: Aumento de massa muscular e ganho de peso saudável.

Dia 1 - Superior (Peito e Tríceps)
Supino reto com barra: 4 séries de 8-10 repetições
Crucifixo com halteres: 3 séries de 12 repetições
Desenvolvimento de ombro com barra: 3 séries de 10 repetições
Tríceps no pulley: 3 séries de 12 repetições
Elevação lateral de ombro: 3 séries de 12 repetições

Dia 2 - Inferior (Pernas e Glúteos)
Agachamento com barra: 4 séries de 10 repetições
Leg press: 4 séries de 12 repetições
Flexão de pernas deitado: 4 séries de 12 repetições
Extensão de pernas: 3 séries de 12 repetições
Panturrilha no leg press: 4 séries de 15 repetições

Dia 3 - Superior (Costas e Bíceps)
Puxada na barra fixa (assistido): 4 séries de 8-10 repetições
Remada baixa: 4 séries de 10 repetições
Rosca alternada com halteres: 3 séries de 12 repetições
Rosca concentrada: 3 séries de 12 repetições
Encolhimento de ombros com barra: 3 séries de 15 repetições

Dia 4 - Inferior (Glúteos e Pernas)
Agachamento livre: 4 séries de 8-10 repetições
Avanço com halteres: 3 séries de 12 repetições (cada perna)
Stiff (levantamento terra romeno): 3 séries de 10 repetições
Panturrilha no leg press: 4 séries de 15 repetições
Elevação de quadril no solo: 4 séries de 12 repetições

Dia 5 - Full Body + Cardio Leve
Supino reto com barra: 3 séries de 10-12 repetições
Remada com halteres: 3 séries de 12 repetições
Agachamento com halteres: 3 séries de 12 repetições
Abdominais (prancha): 3 séries de 30 segundos
Caminhada ou bicicleta: 20-30 minutos (intensidade leve)
Observações:

O treino deve ser bem focado em musculação e alimentação rica em proteínas e calorias.
Evite exercícios de alta intensidade cardiovascular para não queimar muitas calorias.

Objetivo: Aumento calórico para ganho de massa muscular e peso saudável.

Dia 1 a 7

Café da manhã
Omelete com 3 ovos, queijo e aveia
1 copo de leite integral com cacau
1 banana com pasta de amendoim

Almoço
150g de arroz branco
150g de peito de frango grelhado
1 concha de feijão
Salada de abacate e tomate com azeite

Jantar
Macarrão integral ao pesto com frango desfiado
1 taça de iogurte natural com mel e granola

        """
    elif 16.0 <= imc <= 18.4:
        treino_texto = """ 
          IMC de 16,0 a 18,4 (Magreza Leve)

Objetivo: Aumento de massa muscular, com foco em fortalecimento e ganho de peso.

Dia 1 - Superior (Peito e Ombros)
Supino reto com barra: 4 séries de 8-10 repetições
Desenvolvimento de ombro com halteres: 3 séries de 12 repetições
Crucifixo com halteres: 3 séries de 12 repetições
Elevação lateral de ombro com halteres: 3 séries de 12 repetições
Tríceps no pulley: 3 séries de 12 repetições

Dia 2 - Inferior (Pernas e Glúteos)
Agachamento com barra: 4 séries de 8-10 repetições
Leg press: 4 séries de 12 repetições
Flexão de pernas deitado: 4 séries de 12 repetições
Avanço com halteres: 3 séries de 12 repetições (cada perna)
Panturrilha no leg press: 3 séries de 15 repetições

Dia 3 - Superior (Costas e Bíceps)
Puxada na barra fixa (se possível, assistido): 4 séries de 8-10 repetições
Remada baixa: 4 séries de 10-12 repetições
Rosca alternada com halteres: 3 séries de 12 repetições
Rosca concentrada: 3 séries de 12 repetições
Encolhimento de ombros com barra: 3 séries de 15 repetições

Dia 4 - Inferior (Glúteos e Pernas)
Agachamento livre: 4 séries de 8-10 repetições
Avanço com halteres: 3 séries de 12 repetições (cada perna)
Stiff (levantamento terra romeno): 3 séries de 10 repetições
Panturrilha no leg press: 4 séries de 15 repetições
Elevação de quadril no solo: 4 séries de 12 repetições

Dia 5 - Full Body + Cardio
Supino reto com barra: 3 séries de 12 repetições
Remada com halteres: 3 séries de 12 repetições
Agachamento com halteres: 3 séries de 12 repetições
Abdominais (elevação de pernas): 3 séries de 15 repetições
Cardio moderado (bicicleta ou caminhada rápida): 20-30 minutos

Observações:

Concentre-se em treinos compostos para trabalhar grandes grupos musculares.
A alimentação deve ser rica em carboidratos e proteínas para promover o ganho de massa.

Objetivo: Aumento calórico leve para ganho de peso e fortalecimento muscular.

Dia 1 a 7

Café da manhã
1 tapioca com queijo e frango desfiado
1 copo de vitamina de banana com aveia e leite integral
6 castanhas-do-pará

Almoço
150g de arroz integral
100g de carne vermelha magra
1 concha de feijão preto
Salada variada com azeite e sementes
Jantar

Purê de batata-doce com frango desfiado
1 taça de iogurte natural com mel
1 fatia de pão integral com queijo cottage

        """
    elif 18.5 <= imc <= 24.9:
        treino_texto = """ 

        4IMC de 18,5 a 24,9 (Peso Normal)

Objetivo: Manutenção da saúde e tonificação muscular.

Dia 1 - Superior (Peito e Tríceps)
Supino reto com barra: 3 séries de 10-12 repetições
Crucifixo com halteres: 3 séries de 12 repetições
Desenvolvimento de ombro com barra: 3 séries de 12 repetições
Tríceps no pulley: 3 séries de 12 repetições
Elevação lateral de ombro: 3 séries de 15 repetições

Dia 2 - Inferior (Pernas e Glúteos)
Agachamento com barra: 4 séries de 10-12 repetições
Leg press: 3 séries de 12 repetições
Flexão de pernas deitado: 4 séries de 12 repetições
Avanço com halteres: 3 séries de 12 repetições (cada perna)
Panturrilha no leg press: 3 séries de 15 repetições

Dia 3 - Superior (Costas e Bíceps)
Puxada na barra fixa: 4 séries de 8-10 repetições
Remada baixa: 4 séries de 12 repetições
Rosca alternada com halteres: 3 séries de 12 repetições
Rosca concentrada: 3 séries de 12 repetições
Encolhimento de ombros com barra: 3 séries de 15 repetições

Dia 4 - Inferior (Glúteos e Pernas)
Agachamento com barra: 4 séries de 12 repetições
Leg press: 3 séries de 12 repetições
Flexão de pernas deitado: 3 séries de 12 repetições
Avanço com halteres: 3 séries de 12 repetições
Panturrilha no leg press: 3 séries de 15 repetições

Dia 5 - Full Body + Cardio Leve
Supino reto com barra: 3 séries de 12 repetições
Remada com halteres: 3 séries de 12 repetições
Agachamento com halteres: 3 séries de 12 repetições
Abdominais (prancha): 3 séries de 30 segundos
Caminhada ou bicicleta: 20-30 minutos (intensidade leve)

Observações:

Mantenha uma rotina equilibrada entre musculação e cardio para a manutenção da forma física e resistência.
Evite excesso de cardio se seu objetivo for manter ou ganhar um pouco de massa muscular.

Objetivo: Manutenção do peso e saúde geral.

Dia 1 a 7

Café da manhã
1 fatia de pão integral com abacate e ovo cozido
1 copo de café com leite sem açúcar
1 porção de frutas variadas

Almoço
120g de arroz integral
100g de frango grelhado
1 concha de feijão
Salada com folhas verdes, tomate, azeite e chia

Jantar
Omelete com queijo e legumes
1 copo de suco de laranja natural
4 castanhas

        """
    elif 25.0 <= imc <= 29.9:
        treino_texto = """ 

        IMC de 25,0 a 29,9 (Sobrepeso)

Objetivo: Perda de gordura e aumento de massa magra.

Dia 1 - Superior (Peito e Tríceps)
Supino reto com barra: 4 séries de 10 repetições
Crucifixo com halteres: 3 séries de 12 repetições
Desenvolvimento de ombro com barra: 3 séries de 12 repetições
Tríceps no pulley: 3 séries de 12 repetições
Elevação lateral de ombro: 3 séries de 15 repetições

Dia 2 - Inferior (Pernas e Glúteos)
Agachamento com barra: 4 séries de 12 repetições
Leg press: 3 séries de 12 repetições
Flexão de pernas deitado: 3 séries de 12 repetições
Avanço com halteres: 3 séries de 12 repetições
Panturrilha no leg press: 3 séries de 15 repetições

Dia 3 - Cardio + Superior
Cardio moderado (bicicleta ou caminhada inclinada): 30 minutos
Supino reto com barra: 3 séries de 12 repetições
Remada com halteres: 3 séries de 12 repetições
Desenvolvimento de ombro com barra: 3 séries de 12 repetições
Rosca direta com barra: 3 séries de 12 repetições

Dia 4 - Inferior (Pernas e Glúteos)
Agachamento com barra: 4 séries de 12 repetições
Leg press: 3 séries de 12 repetições
Flexão de pernas deitado: 3 séries de 12 repetições
Avanço com halteres: 3 séries de 12 repetições (cada perna)
Panturrilha no leg press: 4 séries de 15 repetições

Dia 5 - Full Body + Cardio Intervalado
Agachamento com halteres: 3 séries de 12 repetições
Supino reto com barra: 3 séries de 12 repetições
Remada com halteres: 3 séries de 12 repetições
Abdominais (prancha): 3 séries de 30 segundos
Cardio intervalado (HIIT): 20 minutos

Observações:

A combinação de musculação com cardio ajuda na perda de gordura, preservando a massa magra.
Mantenha uma alimentação equilibrada, priorizando alimentos com baixo índice glicêmico.

Objetivo: Redução do percentual de gordura com alimentação balanceada.

Dia 1 a 7

Café da manhã
Omelete com 2 ovos, espinafre e tomate
1 copo de café sem açúcar
1 fatia de pão integral com queijo branco

Almoço
100g de arroz integral
100g de peito de frango grelhado
1 concha de feijão
Salada de folhas verdes e azeite

Jantar
Sopa de legumes com frango desfiado
1 porção de iogurte natural com sementes de chia
1 xícara de chá verde

        """
    elif 30.0 <= imc <= 34.9:
        treino_texto = """ 

         IMC de 30,0 a 34,9 (Obesidade Grau I)

Objetivo: Perda de gordura com foco em queima de gordura e fortalecimento muscular.

Dia 1 - Cardio Leve + Superior
Caminhada ou bicicleta ergométrica (30 minutos)
Supino com barra: 3 séries de 12 repetições
Remada com halteres: 3 séries de 12 repetições
Elevação lateral com halteres: 3 séries de 12 repetições
Tríceps no pulley: 3 séries de 12 repetições

Dia 2 - Inferior (Pernas e Glúteos)
Agachamento com barra: 4 séries de 12 repetições
Leg press: 3 séries de 12 repetições
Flexão de pernas deitado: 3 séries de 15 repetições
Panturrilha no leg press: 4 séries de 15 repetições
Avanço com halteres: 3 séries de 12 repetições (cada perna)

Dia 3 - Cardio Leve + Superior
Cardio (bicicleta ou caminhada inclinada): 30-40 minutos
Supino com barra: 3 séries de 12 repetições
Remada com halteres: 3 séries de 12 repetições
Desenvolvimento de ombro com halteres: 3 séries de 12 repetições
Tríceps no pulley: 3 séries de 12 repetições

Dia 4 - Inferior (Pernas e Glúteos)
Agachamento com barra: 4 séries de 12 repetições
Leg press: 3 séries de 12 repetições
Flexão de pernas deitado: 3 séries de 12 repetições
Avanço com halteres: 3 séries de 12 repetições
Panturrilha no leg press: 3 séries de 15 repetições

Dia 5 - Full Body + Cardio
Supino reto com barra: 3 séries de 12 repetições
Remada com halteres: 3 séries de 12 repetições
Agachamento com halteres: 3 séries de 12 repetições
Abdominais (elevação de pernas): 3 séries de 15 repetições
Cardio (20-30 minutos)

Observações:

Priorize o treino cardiovascular para ajudar na queima de gordura, mas sem exagerar para evitar lesões.
Um plano alimentar com déficit calórico deve ser seguido para garantir a perda de peso de forma saudável.

Objetivo: Perda de gordura com dieta controlada e nutritiva.

Dia 1 a 7

Café da manhã
1 omelete de claras com espinafre e tomate
1 copo de chá verde ou café sem açúcar
1 fatia de pão integral com cottage

Almoço
100g de arroz integral ou quinoa
100g de peixe grelhado
Salada de folhas verdes, tomate e azeite
1 concha pequena de feijão

Jantar
Creme de abóbora com frango desfiado
1 copo de chá de camomila
1 punhado de amêndoas


        """
    elif 35.0 <= imc <= 39.9:
        treino_texto = """ 

       IMC de 35,0 a 39,9 (Obesidade Grau II - Severa)

Objetivo: Perda de peso gradual, com foco na melhoria cardiovascular e força.

Treino:

Frequência: 3-4 vezes por semana
Foco: Atividades de baixo impacto com treinos de musculação leves.
Exemplo de treino:

Dia 1 (Cardio + Superior):
Caminhada rápida ou bicicleta ergométrica (30-40 minutos)
Supino reto com halteres (leve): 3 séries de 12 repetições
Remada com halteres (leve): 3 séries de 12 repetições
Elevação lateral de ombro com halteres (leve): 3 séries de 12 repetições
Tríceps no pulley (leve): 3 séries de 12 repetições

Dia 2 (Inferior + Cardio):
Agachamento com halteres (leve): 3 séries de 12 repetições
Leg press (leve): 3 séries de 12 repetições
Cardio (banda ou bicicleta): 20-30 minutos

Dia 3 (Cardio + Superior Leve)
Caminhada rápida ou bicicleta ergométrica (30-40 minutos)
Supino com halteres (leve): 3 séries de 12 repetições
Remada com halteres (leve): 3 séries de 12 repetições
Elevação lateral de ombro com halteres (leve): 3 séries de 12 repetições
Tríceps no pulley (leve): 3 séries de 12 repetições

Dia 4 (Inferior Leve + Cardio Leve)
Agachamento com halteres (leve): 3 séries de 12 repetições
Leg press (leve): 3 séries de 12 repetições
Cardio (banda ou bicicleta): 20-30 minutos

Dia 5 (Full Body Leve)
Agachamento com halteres (leve): 3 séries de 12 repetições
Supino com halteres (leve): 3 séries de 12 repetições
Remada com halteres (leve): 3 séries de 12 repetições
Abdominais (elevação de pernas): 3 séries de 12 repetições
Cardio (20-30 minutos)

Observações:

O foco aqui é no condicionamento físico e aumento gradual da intensidade dos treinos. O acompanhamento médico é fundamental

Objetivo: Redução de gordura corporal com uma dieta hipocalórica rica em fibras e proteínas para saciedade.

Dia 1 a 7

Café da manhã
Omelete com 2 claras de ovo, espinafre e queijo cottage
1 copo de café sem açúcar ou chá verde
1 fatia de pão integral com pasta de amendoim natural

Almoço
100g de arroz integral ou quinoa
120g de filé de frango ou peixe grelhado
Salada variada (alface, rúcula, tomate, cenoura) com azeite e sementes de girassol
1 concha pequena de feijão preto

Jantar
Creme de abóbora ou sopa de legumes com frango desfiado
1 copo de chá de camomila ou hortelã
1 punhado de nozes ou amêndoas


        """
    else:
        treino_texto = """ 
        
    IMC Acima de 40 (Obesidade Grau III - Mórbida)
    
Objetivo: Perda de peso significativa, com foco em exercícios de baixo impacto e melhora da saúde cardiovascular.

Treino:

Frequência: 3 vezes por semana
Foco: Atividades leves de baixo impacto, com treino de força para a prevenção de perda muscular.
Exemplo de treino:

Dia 1 (Cardio + Superior):
Caminhada ou bicicleta ergométrica (20-30 minutos)
Supino com halteres (leve): 3 séries de 12 repetições
Remada com halteres (leve): 3 séries de 12 repetições
Elevação lateral com halteres (leve): 3 séries de 12 repetições

Dia 2 (Inferior + Cardio):
Agachamento com halteres (leve): 3 séries de 12 repetições
Leg press (leve): 3 séries de 12 repetições
Cardio (banda ou bicicleta): 20-30 minutos

Dia 3 (Cardio Leve + Superior Leve)
Caminhada ou bicicleta ergométrica (20-30 minutos)
Supino com halteres (leve): 3 séries de 12 repetições
Remada com halteres (leve): 3 séries de 12 repetições
Elevação lateral com halteres (leve): 3 séries de 12 repetições
Tríceps no pulley (leve): 3 séries de 12 repetições

Dia 4 (Inferior Leve + Cardio Leve)
Agachamento com halteres (leve): 3 séries de 12 repetições
Leg press (leve): 3 séries de 12 repetições
Cardio (banda ou bicicleta): 20-30 minutos

Dia 5 (Full Body Leve + Cardio Leve)
Agachamento com halteres (leve): 3 séries de 12 repetições
Supino com halteres (leve): 3 séries de 12 repetições
Remada com halteres (leve): 3 séries de 12 repetições
Abdominais (elevação de pernas): 3 séries de 12 repetições
Cardio (20-30 minutos)

Observações:

O treino deve ser leve no início, com ênfase na adaptação do corpo e fortalecimento das articulações.
O acompanhamento médico e nutricional é essencial para garantir uma perda de peso saudável.

Objetivo: Perda de peso significativa com dieta hipocalórica, rica em proteínas magras, fibras e gorduras boas para promover saciedade e evitar picos de glicose.

Dia 1 a 7

Café da manhã
1 omelete com 2 claras e 1 gema, espinafre e queijo cottage
1 copo de chá verde ou café sem açúcar
1 fatia de pão integral com abacate

Almoço
100g de arroz integral ou quinoa
120g de peixe grelhado ou frango desfiado
Salada grande com folhas verdes, pepino, cenoura e azeite
1 concha pequena de feijão

Jantar
Sopa de legumes com carne magra desfiada
1 copo de chá de ervas (camomila, hortelã ou gengibre)
1 punhado de nozes ou castanhas

        """
    
    # Usando o widget Canvas para rolagem
    canvas = tk.Canvas(janela) # Cria o widget Canvas onde o conteúdo será exibido

    scroll = tk.Scrollbar(janela, orient="vertical", command=canvas.yview)# Cria uma barra de rolagem vertical para o canvas

    canvas.config(yscrollcommand=scroll.set)# Configura o Canvas para usar a barra de rolagem criada
    
    frame = tk.Frame(canvas)# Cria um frame dentro do Canvas, que será usado para agrupar os widgets

    scroll.pack(side="right", fill="y") # Posiciona e faz o Scrollbar preencher a altura (y) da janela

    canvas.pack(side="left", fill="both", expand=True)# Posiciona o Canvas à esquerda, fazendo-o se expandir para preencher a janela

    canvas.create_window((0, 0), window=frame, anchor="nw") # Cria uma janela dentro do Canvas, onde o frame será posicionado
    
 # Adicionando o texto com as informações sobre o treino dentro do Frame
    label_treino = tk.Label(frame, text=treino_texto, font=("Courier", 14), justify="left") # Cria um Label para exibir o texto do treino

    label_treino.pack(pady=10)# Adiciona o label ao frame com um espaçamento de 10 pixels
    
 # Atualiza o tamanho do Canvas após adicionar o conteúdo
    frame.update_idletasks()# Atualiza as informações do frame após o conteúdo ser adicionado

    canvas.config(scrollregion=canvas.bbox("all")) # Ajusta a área de rolagem do Canvas para englobar o conteúdo do frame

    # Função para salvar o resultado em um arquivo
    def salvar_resultado():
        # Cria a janela para salvar o arquivo

        arquivo = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Text files", "*.txt")])# Abre o diálogo para o usuário escolher o local e nome do arquivo

        if arquivo:# Se o usuário escolher um arquivo para salvar

            with open(arquivo, "w") as f:# Abre o arquivo no modo de escrita
                f.write(f"IMC: {imc}\nClassificação: {classificacao}\nGênero: {genero}\nIdade: {idade} anos\n\nTreino:\n{treino_texto}")  # Escreve os dados no arquivo

            messagebox.showinfo("Sucesso", "Resultado salvo com sucesso!") # Exibe uma mensagem de sucesso

 # Criação de um botão para o usuário salvar o resultado
    botao_salvar = tk.Button(janela, text="Salvar Resultado", command=salvar_resultado, font=("Comic Sans MS", 16))# Cria o botão

    botao_salvar.pack(pady=20)# Adiciona o botão à janela com um espaçamento de 20 pixels

    janela.mainloop()# Inicia a interface gráfica e mantém a janela aberta até o usuário fechá-la

# Obtém os dados do usuário
genero, idade, altura, peso = obter_dados_usuario()# Coleta os dados do usuário

if genero and idade and altura and peso:# Se os dados forem válidos

    imc = calcular_imc(peso, altura)# Calcula o IMC do usuário

    classificacao = classificar_imc(imc, idade)# Classifica o IMC com base na idade do usuário

    exibir_resultado(imc, classificacao, genero, idade)# Exibe os resultados para o usuário


#codigo feito por Allan Viana



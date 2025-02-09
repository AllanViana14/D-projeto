from selenium import webdriver #Importa a biblioteca Selenium que é usada para automatizar navegadores web.
from selenium.webdriver.common.by import By #Importa a classe By, que permite identificar elementos na página de várias formas, como por ID, classe, etc.
from selenium.webdriver.support.ui import WebDriverWait #Importa a classe WebDriverWait, que permite aguardar a ocorrência de uma condição específica antes de prosseguir com o código.
from selenium.webdriver.support import expected_conditions as EC #Importa expected_conditions (EC), que contém várias condições esperadas, como esperar que um elemento seja clicável ou visível.
from selenium.webdriver.common.action_chains import ActionChains #Importa ActionChains, que permite realizar ações mais complexas, como mover o mouse, arrastar e soltar, ou realizar várias ações encadeadas.
from time import sleep # Importa a função sleep do módulo time, usada para pausar a execução do código por um número determinado de segundos.

# Inicializar o WebDriver (Edge)
navegador = webdriver.Edge()

try:
    # Maximizar a janela do navegador
    navegador.maximize_window()

    # Adicionar um pequeno delay
    sleep(0.50)

    # Abrir a página desejada
    navegador.get("http://vmpwin1348.boticario.net/APP/web/Login")

    # Adicionar um pequeno delayz
    sleep(0.50)

    # Esperar até que o campo de usuário esteja visível e preencher com o nome de usuário
    campo_usuario = WebDriverWait(navegador, 10).until(
        EC.visibility_of_element_located((By.XPATH, '/html/body/div[2]/div/div/form/div[3]/input'))
    )
    campo_usuario.send_keys('allan.viana.t_knapp@grupoboticario.com.br')

    # Adicionar um pequeno delay
    sleep(0.50)

    # Esperar até que o campo de senha esteja visível e preencher com a senha
    campo_senha = WebDriverWait(navegador, 10).until(
        EC.visibility_of_element_located((By.XPATH, '/html/body/div[2]/div/div/form/div[4]/input'))
    )
    campo_senha.send_keys('Boticario@202419901515')

    # Adicionar um pequeno delay
    sleep(0.50)

    # Localizar o botão de login pelo XPath e clicar
    botao_login = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="btnSubmit"]'))
    )
    botao_login.click()

    # Adicionar um pequeno delay
    sleep(0.50)

    # Localizar o botão de serviços pelo XPath e clicar
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '/html/body/div[1]/div[2]/div[2]/ul/li[3]'))
    )
    botao_servicos.click()

    # Adicionar um pequeno delay
    sleep(0.50)

    # Localizar o botão de serviços pelo XPath e clicar
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '/html/body/div[1]/div[2]/div[2]/ul/li[3]/ul/li[3]/a/span'))
    )
    botao_servicos.click()

    # Adicionar um pequeno delay
    sleep(0.75)

    # Localizar o botão data seta
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="TIMESTAMP_START_B-1Img"]'))
    )
    botao_servicos.click()

    # Adicionar um pequeno delay
    sleep(0.50)

    # Localizar o botão data today
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="TIMESTAMP_START_DDD_C_BT"]'))
    )
    botao_servicos.click()

    # Adicionar um pequeno delay
    sleep(0.50)

    # Localizar o botão data today
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="TIMESTAMP_START_DDD_C_BO"]'))
    )
    botao_servicos.click()

    # Adicionar um pequeno delay
    sleep(0.50)

    # Localizar o botão data 2
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="TIMESTAMP_END_B-1Img"]'))
    )
    botao_servicos.click()

    # Adicionar um pequeno delay
    sleep(0.50)

    # Localizar o botão data today 2
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="TIMESTAMP_END_DDD_C_BT"]'))
    )
    botao_servicos.click()

    # Adicionar um pequeno delay
    sleep(0.50)

    # Localizar o botão data today campo horas
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="TIMESTAMP_END_DDD_C_TE_B-3Img"]'))
    )
    botao_servicos.click()

    # Adicionar um pequeno delay
    sleep(0.50)

    # Preencher o campo de horas com '23:59:00'
    campo_hora = WebDriverWait(navegador, 10).until(
        EC.visibility_of_element_located((By.XPATH, '//*[@id="TIMESTAMP_END_DDD_C_TE_I"]'))
    )
    campo_hora.send_keys('013000')

    # Adicionar um pequeno delay
    sleep(0.50)

    # Localizar o botão data apertar OK
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="TIMESTAMP_END_DDD_C_BO"]'))
    )
    botao_servicos.click()

    sleep(1)

    # Localizar o serviço específico
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="Value"]/option[10]'))
    )
    botao_servicos.click()

    sleep(0.50)

    # Localizar o botão de pesquisa
    botao_servicos = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="grow"]/div/div[2]/div/div[4]/div/div/button'))
    )
    botao_servicos.click()

    sleep(0.50)

    # Localizar o elemento onde deseja clicar com o botão direito do mouse
    elemento_direito = WebDriverWait(navegador, 10).until(
        EC.presence_of_element_located((By.XPATH, '//*[@id="GridView_DXDataRow8"]/td[5]'))
    )

    # Usar ActionChains para realizar o clique com o botão direito
    acao = ActionChains(navegador)
    acao.context_click(elemento_direito).perform()

    # Adicionar um pequeno delay
    sleep(0.50)

    # Localizar o botão de seta exportação para XLS 
    opcao_exportar = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="GridView_DXContextMenu_Rows_DXI8_T"]/span')) 
    )
    opcao_exportar.click()
    sleep(0.50)
    
 # Localizar o botão de exportação para XLS do
    opcao_exportar = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="GridView_DXContextMenu_Rows_DXI8_PImg"]')) 

    )
    sleep(0.50)
    opcao_exportar.click()
 # Localizar o botão de exportação para XLS (supondo que seja a primeira opção do menu de contexto)
    opcao_exportar = WebDriverWait(navegador, 10).until(
        EC.element_to_be_clickable((By.XPATH, '//*[@id="GridView_DXContextMenu_Rows_DXI8i5_T"]/span')) 

    )
    opcao_exportar.click()

    
    # Adicionar um pequeno delay para garantir a conclusão da exportação
    sleep(15)

finally:
    # Fechar o navegador
    navegador.quit()
    
